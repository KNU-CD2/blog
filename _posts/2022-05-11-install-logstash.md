---
toc: true
layout: post
description: Logstash 설치 방법
categories: [Logstash]
title: Download Logtash
---

# 리눅스 서버에 Logstash 8.1.0 설치하는 방법

## wget을 통한 Logstash 설치
`wget`은 `Web Get`의 약어로 웹 상의 파일을 다운로드 받을 때 사용하는 리눅스 명령어로 비 상호작용 네트워크 다운로더입니다.

https://www.elastic.co/kr/downloads/past-releases/logstash-8-1-0

위 사이트에서 제공하는 파일을 다음과 같이 wget 명령어로 리눅스 서버에 설치합니다.

```shell
wget https://artifacts.elastic.co/downloads/logstash/logstash-8.1.0-linux-aarch64.tar.gz
```

![]({{site.baseurl}}/images/2022-05-11-install-logstash/logstash1.png)

이렇게 logstash의 압축파일을 다운로드 받았습니다. 이 파일을 압축 해제하기 위해 다음과 같이 입력합니다.

```shell
tar -zxvf logstash-8.1.0-linux-aarch64.tar.gz
```

![]({{site.baseurl}}/images/2022-05-11-install-logstash/logstash2.png)

이렇게 로그스태시 설치는 완료하였습니다.

다음은 실행을 통해 정상적으로 설치가 완료되었는지 확인을 해 봅시다.

## 실행을 통한 설치 확인

설치된 폴더로 이동하여 다음과 같이 명령어를 입력해봅시다.

```shell
bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

이것은 input filter를 표준 입력으로, output filter를 표준 출력으로 처리하도록 하는 명령어입니다.
실행 후 `Pipelines running` 이라는 메세지가 뜨면 아무 문자나 입력해봅시다.

![]({{site.baseurl}}/images/2022-05-11-install-logstash/logstash3.png)

다음과 같이 정상적으로 설치가 되었고, 입출력되는 것을 확인하였습니다.