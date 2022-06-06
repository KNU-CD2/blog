---
toc: true
layout: post
description: ElasticSearch와 Kibana 연동하기
categories: [Elasticsearch, Kibana]
title: [Elastic Stack] Intergrating ElasticSearch & Kibana
sticky_rank: 6
---

# ElasticSearch와 Kibana 연동하기(8.1.0)

Elastic Stack이 7버전에서 8버전으로 바뀌면서 많은 부분이 수정되었습니다.
Elasticsearch와 Kibana 연동 부분에서도 많은 변화가 있어 기존 버전 7에서의 연동방식와 차이가 있습니다.
이 글에서는 버전 8의 Elasticsearch와 Kibana 연동 방법을 설명해드리겠습니다.

## 1. ElasticSearch 실행하기

먼저 Elasticsearch를 실행시켜는 법을 알려드리겠습니다.

![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter1.PNG)
설치한 Elasticsearch 폴더 내부에 많은 파일과 폴더가 있는 걸 알 수 있습니다.
Elasticsearch를 실행시킬려면 bin에 있는 elasticsearch를 실행시키면 됩니다.

```shell
./bin/elasticsearch
```

![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter2.PNG)

Elasticsearch를 최초로 실행시키면 이렇게 토큰과 Kibana에 접속할 비밀번호를 보여줍니다. 이를 잘 보관해두셔야 Kibana와 연동이 가능합니다.
이제 Kibana를 실행시켜 Elasticserach와 Kibana를 연동시켜 보겠습니다.

## 2. Kibana 외부접속

로컬 컴퓨터에서 자신만 Kibana에 접속하시는 분들은 상관 없으시겠지만, Elasticsearch Stack을 사용하면 아마 Kibana에서 외부접속은 필요하실 겁니다.
외부접속을 할려면 kibana폴더 내부에 config 폴더에 kibana.yml 이 있습니다.

```shell
vi config/kibana.yml
```

커맨드를 입력해서 kibana.yml 파일을 수정하겠습니다.

![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter4.PNG)
초기에는 server.host: "localhost"가 주석처리 되어 있습니다. 주석을 지우고

```shell
server.host: 0.0.0.0
```

으로 수정하시면 외부접속이 가능합니다.

## 3. Kibana 실행하기

![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter3.PNG)
설치한 Kibana 폴더 내부입니다. Kibana도 bin폴더에 있는 kibana를 실행하면 됩니다.
Kibana를 실행하기 전 위에서 최초로 구동시킨 Elasticsearch가 실행 중이어야 합니다.

```shell
./bin/kibana
```

![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter5.PNG)

그럼 터미널에서 이렇게 코드값을 주고 Kibana가 실행됩니다.

Kibana의 port는 5601로 설정되어 있습니다.
localhost:5601 또는 ServerIP:5601 로 접속하면,
![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter6.PNG)

이렇게 Kibana에서 토큰을 입력하라고 합니다.
그러면 아까 Elasticsearch에서 발행된 토큰을 입력해줍니다.
![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter7.PNG)

토큰이 입력되면 kibana는 코드를 입력하라 합니다.
![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter8.PNG)

Kibana를 실행시킬 때 코드를 입력해줍니다.
![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter13.PNG)

![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter9.PNG)
이렇게 Elasticsearch와 Kibana의 연동이 끝납니다.

![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter10.PNG)
로그인 창이 나오는데요. 초기에는
Username: elastic
Password는 Elasticsearch를 최초로 실행시킬 때 나오는 password를 입력해줍니다.
![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter11.PNG)

![]({{site.baseurl}}/images/2022-05-11-intergrating-elasticsearch-and-kibana/inter12.PNG)
이제 Kibana에 접속이 완료되었습니다.
