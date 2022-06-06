---
toc: true
layout: post
description: Elasticsearch에서 다중노드 환경의 클러스터 환경 구축 방법
categories: [Elasticsearch]
title: "[Elasticsearch] How to deploy a multi-node elasticsearch cluster"
sticky_rank: 10
---

# 엘라스틱서치 다중노드 클러스터 환경 구축

## 1. 인증서 생성

클러스터에 포함된 노드들은 서로 transport module을 이용하여 통신을 한다.
통신을 할 때 보안을 위해 TLS를 사용하는데 이를 위해 인증서를 생성하자.

{% include info.html text="이 프로젝트에서는 서로 다른 2개의 서버에서 각각 노드 하나씩 생성하여 클러스터를 구성할 것이다." %}

첫번째 서버에서 인증서를 생성하고 이 인증서를 각 서버에 복사하는 과정을 진행할 것이다.
작동중인 elasticsearch는 모두 종료하고 진행한다.

CA를 생성한다.
filename은 default인 elastic-stack-ca.p12로 진행한다.

```shell
./bin/elasticsearch-certutil ca
```

이 CA를 이용해 certificate and private key를 생성한다.
filename은 default인 elastic-certificates.p12로 하고, 비밀번호는 기억해둔다.

```shell
./bin/elasticsearch-certutil cert --ca elastic-stack-ca.p12
```

생성된 elastic-certificates.p12를 config 폴더 안에 certs 폴더를 생성하고 거기에 복사한다.
그리고, 이 파일을 각 서버에 전송하여야 한다. (서버에도 certs 폴더를 생성해야함)
linux의 scp 명령어를 이용해 파일을 전송하자.

```shell
scp -P {포트번호} /home/{저장 경로}/elasticsearch-8.1.0/config/certs/elastic-certificates.p12  {호스트 이름}@{공개 서버 주소}:/home/c2-elastic/elastic/elasticsearch-8.1.0/config/certs/
```

## 2. elasticsearch.yml 파일 설정

다음과 같이 elasticsearch.yml 파일을 각 서버마다 작성한다.
주석처리 되어있는 것을 해제하여 사용하여도 좋고, 첫 줄에 추가적으로 작성하여도 된다.
노드 이름은 각 서버마다 다르게 지정하여야 한다.

```yml
cluster.name: csp-cluster # 동일하기
node.name: c1-elastic # 서버마다 다르게

network.host: ["_local_", "_site_"]
http.port: 9200
# 클러스터에 포함된 노드의 서버 주소. 주의: public IP가 아닌 private IP를 작성한다.
# hostname -I 명령어로 private IP 확인 가능함
discovery.seed_hosts: ["192.168.0.6", "192.168.0.14"]
cluster.initial_master_nodes: ["c1-elastic", "c2-elastic"] # 클러스터에 포함된 노드의 이름

xpack.security.enabled: true
xpack.security.enrollment.enabled: true

xpack.security.transport.ssl:
  enabled: true
  verification_mode: certificate
  client_authentication: required
  keystore.path: certs/elastic-certificates.p12
  truststore.path: certs/elastic-certificates.p12

transport.host: [_local_, _site_] # 이것은 첫번째 서버에서만 작성하면 됨
```

## 3. keystore에 비밀번호 저장

다음 명령어로 Elasticsearch keystore에 비밀번호를 저장한다.
각 서버마다 실행한다.

```shell
./bin/elasticsearch-keystore add xpack.security.transport.ssl.keystore.secure_password
./bin/elasticsearch-keystore add xpack.security.transport.ssl.truststore.secure_password
```

## 4. HTTP 인증서 생성

첫번째 서버에서 다음 명령어로 Elasticsearch HTTP certificate를 생성한다.

```shell
./bin/elasticsearch-certutil http
```

1. Generate a CSR? N
2. Use an existing CA? y
3. CA Path: /home/c2-elastic/elastic/elasticsearch-8.1.0/elastic-stack-ca.p12
4. For how long should your certificate be valid?: 5y
5. Generate a certificate per node? n
6. Enter all the hostnames that you need: ubuntu (hostname 명령어로 확인 가능)
7. Enter all IP addresses that you need: 192.168.0.6 192.168.0.14 (private IP 작성)
8. Do you wish to change any of these options? n

생성된 elasticsearch-ssl-http.zip 파일을 압축해제 하면 elasticsearch 폴더와 kibana 폴더가 생성된다.
elasticsearch 폴더 내의 http.p12 파일은 config/certs 폴더에 이동하고, 이 파일을 각 서버에 전송한다. (scp 명령어 활용)
kibana 폴더 내의 elasticsearch-ca.pem 파일은 키바나 폴더의 config/certs 폴더에 이동한다.

elasticsearch.yml 파일에 다음을 추가한다.

```yml
xpack.security.http.ssl:
  enabled: true
  keystore.path: certs/http.p12
```

## 4. keystore에 비밀번호 저장

다음 명령어로 Elasticsearch keystore에 비밀번호를 저장한다.
각 서버마다 실행한다.

```shell
./bin/elasticsearch-keystore add xpack.security.http.ssl.keystore.secure_password
```

## 5. kibana.yml 파일 설정

키바나를 실행시킬 서버에서 다음과 같이 yml 파일을 설정한다.

```yml
server.host: 0.0.0.0
server.publicBaseUrl: "http://192.168.0.6:5601"
server.name: "csp-kibana"

elasticsearch.hosts: ["https://192.168.0.6:9200", "https://192.168.0.14:9200"]

elasticsearch.ssl.certificateAuthorities:
  ["/home/c1-elastic/elastic/kibana-8.1.0/config/certs/elasticsearch-ca.pem"]
elasticsearch.serviceAccountToken: # 초기 셋팅시 자동으로 생성된 서비스 토큰값 작성
```

# Reference

- https://www.elastic.co/guide/en/elasticsearch/reference/8.1/security-basic-setup.html#encrypt-internode-communication
- https://www.elastic.co/guide/en/elasticsearch/reference/8.1/security-basic-setup-https.html#encrypt-kibana-http
- https://www.youtube.com/watch?v=id8L4fiCnQE&t=939s
