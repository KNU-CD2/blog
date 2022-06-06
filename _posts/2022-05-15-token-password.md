---
toc: true
layout: post
description: ElasticSearch Toekn, Kibana Password 초기화하기
categories: [Elasticsearch, Kibana]
title: [Elastic Stack] Reset ElasticSearch Token and Kibana Password
sticky_rank: 8
---

# ElasticSearch Toekn, Password 초기화하기

Elasticsearch 첫 화면에서 토큰 또는 비밀번호를 분실하시는 경우가 있을 수 있습니다.
Elasticsearch와 Kibana를 삭제 후 재설치 하시면
토큰과 비밀번호를 재발급 받으실 수 있습니다.

하지만 이러한 방법은 너무 번거롭기에, 삭제하지 않고
토큰과 비밀번호를 재발급 받을 수 있는 방법을 알려드리겠습니다.

## 1. ElasticSearch 토큰 초기화하기

먼저 Elasticsearch를 실행시켜야 합니다.
그리고 Elasticsearch 폴더의 bin 폴더로 가서

```shell
./elasticsearch-create-enrollment-token -s kibana
```

위의 커맨드를 실행시키면 됩니다.

![]({{site.baseurl}}/images/2022-05-15-token-password/token1.PNG)

그러면 사진과 같이 토큰이 초기화됩니다.

![]({{site.baseurl}}/images/inter6.PNG)
이 토큰을 Kibana 초기 접속화면에 입력하시면 키바나 사용이 가능합니다.

## 2. Kibana 비밀번호 초기화하기

Kibana의 비밀번호를 분실하실 때도
Elasticsearch를 실행시키고, Elasticserach 폴더의
bin 폴더로 가서

```
/elasticsearch-reset-password -u elastic
```

위의 커맨드를 실행시키면

![]({{site.baseurl}}/images/2022-05-15-token-password/token2.PNG)

사진과 같이 터미널에 비밀번호를 출력할 것인지 묻습니다.
그냥 y를 입력하면

![]({{site.baseurl}}/images/2022-05-15-token-password/token3.PNG)

터미널에 비밀번호가 나타납니다.
이 비밀번호를 Kibana에 입력합니다.(초기 Username: elastic)

![]({{site.baseurl}}/images/2022-05-15-token-password/token5.PNG)

Kibana 로그인이 완료되었습니다.

![]({{site.baseurl}}/images/2022-05-15-token-password/token6.PNG)
비밀번호를 변경하고 싶으시면 우측상단에 프로필 창을 클릭합니다.

![]({{site.baseurl}}/images/2022-05-15-token-password/token7.PNG)
사진과 같이 Kibana의 비밀번호를 원하는 비밀번호로 수정하실 수 있습니다!

## 3. Elasticsearch를 실행시키지 않았을 때

![]({{site.baseurl}}/images/2022-05-15-token-password/token8.PNG)
Elasticsearch를 실행시키지 않고 커맨드를 날리면
ERROR: Failed to determine the health of the cluster. 라는 에러가 발생합니다.

그러므로 꼭 Elasticsearch를 실행시키고 커맨드를 날려주셔야 합니다.
