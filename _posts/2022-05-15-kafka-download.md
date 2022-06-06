---
toc: true
layout: post
description: 카프카 다운로드 및 설정
categories: [Kafka]
title: [Kafka] Kafka download and setting
sticky_rank: 16
---

# 카프카 다운로드 및 설정

__서버에 카프카 다운로드 후 로컬과 통신을 설정하는 과정입니다.__

___

## 카프카 다운로드

![]({{site.baseurl}}/images/kafka/kafkaset1.JPG)
`https://kafka.apache.org/downloads` <br/>
위 사이트로 가서 카프카 다운로드 링크를 복사하도록 하겠습니다.

<br/>

![]({{site.baseurl}}/images/kafka/kafkaset2.JPG)
```
wget https://dlcdn.apache.org/kafka/3.1.0/kafka_2.13-3.1.0.tgz
```
앞에 숫자는 Scalar 버전이고, 뒤에 숫자는 카프카 버전입니다.
저는 Scalar는 2.13버전, 카프카는 3.1.0 버전으로 진행하겠습니다.

<br/>

![]({{site.baseurl}}/images/kafka/kafkaset3.JPG)
![]({{site.baseurl}}/images/kafka/kafkaset4.JPG)
```
tar -xzvf kafka_2.13-3.1.0.tgz
```
`tar` 명령어를 통해 압축을  풀어줍니다.

<br/>

-------

## 자바 설치

카프카를 구동하기 위해서는 자바가 필요합니다. 자바도 설치해보도록 하겠습니다.

![]({{site.baseurl}}/images/kafka/kafkaset5.JPG)
```
sudo apt install openjdk-8-jdk-headless
```
저의 경우 자바 8 버전을 설치하였습니다. 

<br/>

![]({{site.baseurl}}/images/kafka/kafkaset6.JPG)
설치가 완료되었고 테스트 할겸 java와 javac의 버전도 출력해보았습니다.

<br/>

-------

## 카프카 사용 메모리 설정

이 부분은 무료 클라우드 인스턴스를 사용하거나 온프래미스 구축시 장비의  메모리가 부족하신 분들만 진행하시면 됩니다.

1. `vi ~/.bashrc` 명령어를 사용해 배쉬쉘에 들어간 다음에

2. `export KAFKA_HEAP_OPTS="-Xmx400m -Xms400m"` 를 입력후 저장해줍니다.

3. `source ./bashrc`를 해준다음 `echo $KAFKA_HEAP_OPTS`를 입력해 환경변수가 잘 지정되었는지 확인 해줍니다.

저 같은 경우 메모리 8G 라즈베리파이를 이용했기에 이 과정은 넘어갔습니다.

<br/>

-----

## server.properties 설정

카프카를 로컬에서 통신하기 위해 server.properties를 설정해보겠습니다.

![]({{site.baseurl}}/images/kafka/kafkaset7.JPG)
```
cd kafka_2.13-3.1.0
vim config/server.properties
```

<br/>

![]({{site.baseurl}}/images/kafka/kafkaset8.JPG)
그 다음 `advertised.listeners`를 찾아서 주석을 풀어주고 설정을 합니다.

저 같은 경우 여러대의 라즈베리파이가 외부아이피 하나에 연결되어 있기에 기존의 9092포트가 아닌 30014로 설정하였습니다. 아이피 부분은 클라우드의 인스턴스나 본인이 구축한 온프레미스 환경의 외부아이피를 입력하시면 됩니다.

<br/>

![]({{site.baseurl}}/images/kafka/kafkaset9.JPG)
그 후 포트포워딩도 진행하였습니다. 로컬과 통신을 위해 새로 추가한 규칙은 순위 16의 규칙입니다.

<br/>

![]({{site.baseurl}}/images/kafka/kafkaset10.JPG)
![]({{site.baseurl}}/images/kafka/kafkaset11.JPG)
추가적으로 카프카를 구동하기전에 `server.properties` 에서 `log.dirs`에 설정된 값에 해당하는 디렉토리가 없다면 생성해주도록 합니다.

<br/>

----

## 카프카 실행

카프카를 실행하기 위해서는 주키퍼를 실행한 다음 카프카를 실행하여야 합니다. 주키퍼의 경우 다운받은 카프카에 함께 포함되어 있습니다.

우선 주키퍼를 실행해보도록 하겠습니다.
```
bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
```

그 다음으로 카프카를 실행해보도록 하겠습니다.
```
bin/kafka-server-start.sh -daemon config/server.properties
```
`-daemon` 옵션은 백그라운드로 동작하도록 해주는 옵션입니다.

![]({{site.baseurl}}/images/kafka/kafkaset12.JPG)
실행을 한 후에 `jps`를 커맨드라인에 입력하면 위와 같이 `QuorumPeerMain`(주키퍼) 와 `Kafka`가 동작 중인 것을 확인 할 수 있습니다.

<br/>

---

## 로컬과 카프카 통신 확인

저의 로컬 환경은 Window10 에서 WSL을 활용하였습니다.

![]({{site.baseurl}}/images/kafka/kafkaset13.JPG)
WSL에서도 서버에 설치한 카프카와 자바의 버전과 동일하게 카프카와 자바를 설치해줍니다. 위에서 설치시 활용한 커맨드를 그대로 입력하면 됩니다

<br/>

그 후 정상적으로 통신이 가능한지 테스트를 진행합니다.
```
bin/kafka-broker-api-versions.sh --bootstrap-server 59.23.xxx.xxx:30014
```

![]({{site.baseurl}}/images/kafka/kafkaset14.JPG)
통신이 정상적으로 잘 되는 것을 확인 할 수 있습니다.

<br/>

---
