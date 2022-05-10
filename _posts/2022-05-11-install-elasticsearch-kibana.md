---
toc: true
layout: post
description: ElasticSearch와 Kibana 설치하기
categories: [Elasticsearch, Kibana]
title: Download ElasticSearch & Kibana
---

# 리눅스 서버에 ElasticSearch와 Kibana 설치하기(8.1.0)

## 1. ElasticSearch 설치

https://www.elastic.co/kr/downloads/past-releases/elasticsearch-8-1-0 에 접속하시면

![]({{site.baseurl}}/images/install1.PNG)
위와 같은 화면이 나옵니다.

LINUX X86_64, LINUX AARCH64
둘 중 자신의 리눅스 서버에 맞는 프로그램을 설치해야 합니다.

설치버튼을 우클릭해서
'링크 주소 복사' 클릭하고 리눅스 터미널에

```shell
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.1.0-linux-aarch64.tar.gz
```

이렇게 입력합니다.

![]({{site.baseurl}}/images/install2.PNG)
그러면 사진과 같이 tar.gz파일 설치가 완료됩니다.

![]({{site.baseurl}}/images/install3.PNG)

그러고 이 알집을 풀어주기 위해

```shell
tar -zxvf elasticsearch-8.1.0-linux-aarch64.tar.gz
```

커맨드를 입력합니다.

![]({{site.baseurl}}/images/install4.PNG)

그러면 이렇게 elasticsearch-8.1.0 폴더가 생성되면서 ElasticSearch의 설치가 완료됩니다.

## 2. Kibana 설치

Kibana의 설치법은 ElasticSearch와 동일합니다.

https://www.elastic.co/kr/downloads/past-releases/kibana-8-1-0로 접속하시면

![]({{site.baseurl}}/images/install5.PNG)
여기서도 우클릭으로 링크 주소 복사 후 리눅스 터미널에 아래 커맨드를 입력시킵니다.

```shell
wget wget https://artifacts.elastic.co/downloads/kibana/kibana-8.1.0-linux--aarch64.tar.gz
```

![]({{site.baseurl}}/images/install6.PNG)
이렇게 tar.gz 파일 설치가 완료됩니다.

이 파일도

```shelll
tar -zxvf kibana-8.1.0-linux-aarch64.tar.gz
```

커맨드를 입력해 압축을 해제합니다.

![]({{site.baseurl}}/images/install4.PNG)
이렇게 ElasticSearch와 Kibana의 설치가 끝났습니다.
