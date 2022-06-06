---
toc: true
layout: post
description: PKIX path building failed, unable to find valid certification path to requested target 에러 해결법.
categories: [Logstash]
title: PKIX path building failed error
sticky_rank: 14
---

# 문제 상황

Logstash와 Elasticsearch를 연동하는 과정에서 다음과 같은 에러가 발생하였습니다.

![]({{site.baseurl}}/images/2022-05-14-pkix-path-building-failed/error1.png)

`PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target`

이는 Elasticstack 8.0 버전부터 default로 보안 설정이 적용되어 생기는 문제로 보안 관련 처리를 해주어야 제대로 동작합니다.

# 해결 방법

두 가지 방법이 있습니다.
1. SSL 자격 확인을 하지 않도록 설정하는 방법
2. SSL 자격 인증서를 등록하는 방법

1번 방법에 대한 해결법은 [이 게시글](https://knu-cd2.github.io/blog/logstash/2022/05/14/logstash-to-elasticsearch.html)을 참고해주세요.

2번 방법에 대한 해결법을 설명해드리겠습니다.

## Elasticsearch 인증서 생성

다음 명령어를 입력하여 `.cer 인증서`를 생성해줍니다.

```shell
echo -n | openssl s_client -connect localhost:9200 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > ./ca_logstash.cer
```

생성된 파일을 `${LS_HOME}/config/certs`폴더로 이동시킵니다.

## conf 파일 수정

config 파일을 다음과 같이 `cacert`과 `ssl`을 추가하여 변경합니다.

```conf
...

output {
    elasticsearch {
        index => "output"
        hosts => ["https://192.168.0.6:9200", "https://192.168.0.14:9200", "https://192.168.0.15:9200"]
        user => "elastic"
        password => "yourpassword"
        ssl => true
        cacert => "/home/c1-elastic/elastic/logstash-8.1.0/config/certs/ca_logstash.cer"
    }
    stdout {}
}
```

{% include alert.html text="Elasticsearch가 다중 노드로 구성되어 있다면 hosts에 해당 노드를 모두 추가하여야 합니다." %}

수정 후 다시 실행하여 봅시다.

```shell
./bin/logstash -f config/test.conf
```

![]({{site.baseurl}}/images/2022-05-14-pkix-path-building-failed/done1.png)

사진과 같이 정상적으로 실행되었음을 확인할 수 있습니다.