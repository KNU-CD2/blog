---
toc: true
layout: post
description: 카프카 클러스터 구성
categories: [Kafka]
title: "[Kafka] Deploy Kafka Cluster"
sticky_rank: 18
---

# 카프카 클러스터 구성하기

라즈베리파이에 올라간 카프카의 클러스터를 구성하는 방법입니다.

---

## zookeeper 설정 및 실행

```
cd kafka
vim config/zookeeper.properties
```
위 커맨드를 입력해서 zookeeper 설정 파일을 열어줍니다.그리고 클러스터를 구성할 정보를 입력해줍니다.
```
initLimit=5
syncLimit=2

server.1={IP 주소}:2888:3888
server.2={IP 주소}:2888:3888
server.3={IP 주소}:2888:3888
```

![]({{site.baseurl}}/images/kafka/kafka1.JPG)
저 같은 경우 라즈베리파이가 공유기 하나에 연결되어 있어서 내부 IP로 진행하였습니다. 위 사진에선 컴퓨터 한대에서만 설정한 것 처럼 보이는데 __모든 컴퓨터에 다 설정해주어야 합니다.__

<br/>

```
1번 컴퓨터에서
echo 1 > /tmp/zookeeper/myid

2번 컴퓨터에서
echo 2 > /tmp/zookeeper/myid

3번 컴퓨터에서
echo 3 > /tmp/zookeeper/myid
```

![]({{site.baseurl}}/images/kafka/kafka2.JPG)
그 다음 dataDir의 경로에 해당 클러스터가 몇 번 클러스터인지 설정해줍니다. 만약 tmp나 zookeeper 폴더가 없다면 `mkdir` 커맨드로 생성해주도록 합니다. 그리고 __myid는 겹쳐서는 안됩니다.__
 
이렇게 하면 zookeeper 관련해서 클러스터 설정은 끝입니다.

<br/>

```
주키퍼 실행
bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
```
마지막으로 모든 컴퓨터에 해당 커맨드를 입력하여 zookeeper를 실행시켜 줍니다. -daemon 옵션을 주면 백그라운드로 실행하게 됩니다.

종료는 `bin/zookeeper-server-stop.sh` 커맨드를 날려서 종료하면 됩니다.

---

## Kafka 설정 및 실행

{% include info.html text="카프카 실행 시 에러가 나면 아래 링크를 참조해주세요" %}
[ERROR Exiting Kafka due to fatal exception (kafka.Kafka$)](https://knu-cd2.github.io/blog/kafka/2022/05/19/kafka-error1.html)

<br/>

```
vim config/server.properties
```
위 커맨드를 입력하여 카프카와 관련된 설정파일을 열어줍니다.

![]({{site.baseurl}}/images/kafka/kafka3.JPG)
`broker.id` 옵션을 찾아서 zookeeper에서 설정한 myid 값을 넘겨줍니다.

<br/>

![]({{site.baseurl}}/images/kafka/kafka4.JPG)
`advertised.listeners` 옵션을 찾아서 listen할 아이피와 포트를 설정해주도록 합니다. 일단은 외부와의 통신을 고려하지 않고 진행하기 때문에 위와 같이 설정하였습니다. 저 같은 경우 `listeners` 옵션 주석이 풀려있는데 이 옵션은 주석을 풀지 않고 그대로 두셔도 됩니다.

<br/>

![]({{site.baseurl}}/images/kafka/kafka5.JPG)
마지막으로 `zookeeper.connect`에 zookeeper에서 클러스터를 구성한 노드(컴퓨터)들의 IP 주소와 포트를 적어줍니다.

```
카프카 실행
bin/kafka-server-start.sh -daemon config/server.properties
```
그리고 모든 컴퓨터에서 카프카를 실행을 시켜줍니다.

종료하는 방법은 `bin/kafka-server-stop.sh` 커맨드를 날려서 종료해주면 됩니다.

<br/>

클러스터가 정상적으로 실행되는지 확인하기 위해 토픽을 생성하고 토픽 리스트를 보는 테스트를 해보겠습니다.

```
토픽생성
bin/kafka-topics.sh --create \
--bootstrap-server 192.168.0.11:9092,192.168.0.12:9092,192.168.0.13:9092 \
--replication-factor 3 \
--partitions 3 \
--topic cluster-test

토픽 리스트 확인
bin/kafka-topics.sh --list \
--bootstrap-server 192.168.0.11:9092,192.168.0.12:9092,192.168.0.13:9092
```

![]({{site.baseurl}}/images/kafka/kafka6.JPG)
위와 같이 `cluster-test` 이름을 가진 토픽이 생성되고 리스트에서 확인할 수 있는 것을 볼 수 있습니다.

<br/>

---

## 카프카 클러스터를 외부에서 컨트롤 할 경우

제 노트북의 윈도우 WSL(외부환경)에서 카프카 클러스터에 토픽 생성 등과 같이 컨트롤이 필요할 경우 설정에 대해 알려드리겠습니다.

```
vim config/server.properties
```
위 파일을 다시 연 다음에 listen을 담당하는 `advertised.listeners` 부분만 변경해주면 됩니다.

```
advertised.listeners=PLAINTEXT://{외부 IP}:{외부 포트}
```
![]({{site.baseurl}}/images/kafka/kafka7.jpg)
저 같은 경우 각각 외부에서 30011, 30012, 30013으로 설정하였습니다.

<br/>

![]({{site.baseurl}}/images/kafka/kafka8.JPG)
그 후 포트포워딩도 진행하였습니다. 포트포워딩을 위해 추가한 설정은 순위 13~15 입니다.

<br/>

---

## 로컬에서 테스트

마지막으로 제 노트북의 WSL환경에서 카프카 클러스터와 통신이 가능한지 테스트해보겠습니다. (테스트하기 위해서는 WSL환경에 자바와 카프카가 설치되어 있어야합니다.)


```
bin/kafka-topic.sh --list \
--bootstrap-server {외부IP}:{외부 포트},{외부IP}:{외부 포트},{외부IP}:{외부 포트}
```
![]({{site.baseurl}}/images/kafka/kafka9.jpg)
토픽 리스트를 확인하는 커맨드를 날려주면 정상적으로 통신을 해 토픽 리스트들을 받아온 것을 확인 할 수 있습니다.

<br/>

---
