---
toc: true
layout: post
description: 파일비츠 설치
categories: [Filebeat]
title: "[Filebeat] Filebeat Download and Setting"
sticky_rank: 20
---

# 파일비츠 다운로드 및 설치
__파일 비츠를 다운로드하고 실행해보는 과정입니다.__

----

## 파일비츠 다운로드

* 본인 환경에 맞게 다운로드를 진행하면 됩니다. 저같은 경우 라즈베리파이4를 이용했기에 `arm64(aarch64)`로 진행했습니다.

```
wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.1.0-linux-arm64.tar.gz
```
* `wget`을 이용해 파일비츠를 다운로드 합니다

```
tar -zxvf filebeat-8.1.0-linux-arm64.tar.gz
```
* `tar`를 이용해 압축을 풀어줍니다.

----

## filebeats.yml 설정

* 압축을 푼 파일비츠 폴더로 이동해 `vim test.yml`, test 이름을 가진 .yml 파일을 작성합니다.

* `test.yml` 파일은 아래와 같은 내용으로 작성합니다.
```yml
filebeat.inputs:
  - type: log
    enabled: true
    paths:
      - /home/{유저 홈디렉토리 명}/test/*.log

output.console:
    enabled: true
```

* `test` 폴더에는 `Hello Beats!`가 적힌 `test.log` 파일이 들어있습니다.

* yml을 작성 완료한 후에 `./filebeat -c test.yml -e` 로 실행시켜줍니다.

![]({{site.baseurl}}/images/2022-05-26-filebeat-download/beat.jpg)
* 위와 같이 `Hello Beats!` 가 메세지로 나타나는 것을 확인 할 수 있습니다.

----

## filebeat 실행시 옵션에 대해서

* `-e` 옵션은 에러 로그를 콘솔에 보여줍니다.(좀 더 정확하게는 `stderr`)

* `-c {yml 파일명}`은 파일비츠를 실행시킬 yml 파일을 지정할 수 있습니다. 만약 지정하지 않는다면 `filebeat.yml` 파일이 default로 지정됩니다.
