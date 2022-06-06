---
toc: true
layout: post
description: ElasticSearch에서 curl 사용하기
categories: [Elasticsearch]
title: Using curl with ElasticSearch
sticky_rank: 9
---

# ElasticSearch에서 curl 사용하기

Elasticsearch는 REST API를 사용하기 때문에
curl을 이용해서 간편하게 데이터를 조회하고, 추가하거나 삭제할 수 있습니다.
간단한 사용 예시를 보여드리겠습니다.

## 1. curl 에러

```shell
curl localhost:9200
```

이 커맨드를 실행하면 클러스터의 상태 정보를 알 수 있습니다.
먼저 이 커맨드를 콘솔에 입력해보겠습니다.

![]({{site.baseurl}}/images/curl1.PNG)

Elasticsearch가 실행 중이지 않으면
Connection refuesed 에러가 발생하므로 Elasticsearch를 꼭 가동해주세요.

다시 Elasticsearch가 가동 중인 커맨드를 입력하겠습니다.

![]({{site.baseurl}}/images/curl2.PNG)

이번에는 Empty reply from server 라며
클러스터의 상태정보를 알려주지 않습니다.

Elasticsearch 버전 7에서는 이러한 커맨드로 확인이 가능했지만,
버전 8이 되면서 보안설정이 생겨 단순한 curl 커맨드로는
Elasticsearch에 접근할 수 없습니다.

## 2. ID, Password를 이용한 curl

이번에는 Elasticsearch의 ID와 Password가 추가된 curl을 입력하겠습니다.

```shell
curl -u "elastic:changeme" -k https://localhost:9200
```

ID: elastic
Password: changeme
ID와 Password는 사용 중인 ID와 Password를 입력하셔야 합니다.

![]({{site.baseurl}}/images/curl3.PNG)

아까와는 다르게 클러스터의 상태를 조회할 수 있습니다.

## 3. http_ca.crt 인증서를 이용한 curl

인증서와 Elasticsearch의 ID와 Password를 추가해 curl을 이용할 수 있습니다.

```shell
 curl --cacert /home/c2-kafka/elastic/elasticsearch-8.1.0/config/certs/http_ca.crt -u "elastic:kibana" -X GET https://localhost:9200
```

curl 커맨드 중간에 http_ca.crt의 경로를 입력해야 합니다.
http_ca.crt의 상대경로가 아닌 절대경로를 입력하시면 됩니다.

![]({{site.baseurl}}/images/curl4.PNG)

이 커맨드 또한 아까 전과 같은 클러스터의 상태 정보를 알아낼 수 있습니다.
