---
toc: true
layout: post
description: Logstash에서 Elasticsearch로 데이터 전송 및 저장하는 방법. PKIX path building failed 에러 해결법.
categories: [Logstash]
title: [Elastic Stack] How to connect Logstash with Elasticsearch
sticky_rank: 13
---

# Logstash에서 Elasticsearch로 데이터 저장하는 방법

데이터 파이프라인을 구축하기 위해 Logstash에서 가공한 데이터를 Elasticsearch로 전송하여 데이터를 저장하고자 합니다.

우선, Elasticsearch은 실행되어 있는 상태이여야 합니다. [다른 게시글](https://knu-cd2.github.io/blog/elasticsearch/kibana/2022/05/11/intergrating-elasticsearch-and-kibana.html)을 참고하여 실행합니다.

다음으로 config 파일 `test.conf`을 작성합니다. 이번 예제에서는 Elasticsearch에서 발생하는 로그를 받아와 이것을 Elasticsearch로 전송하여 인덱스 `output`을 생성하여 저장하고, 눈으로 확인하기 위해 표준 출력으로도 받아보겠습니다.

```conf
input {
    file {
        path => "/home/c1-elastic/elastic/elasticsearch-8.1.0/logs/gc.log"
        start_position => "beginning"
        sincedb_path => "/dev/null"
    }
}

output {
    elasticsearch {
        index => "output"
        hosts => ["https://127.0.0.1:9200"]
        user => "elastic"
        password => "yourpassword"
    }
    stdout {}
}
```

작성한 config 파일을 이용하여 로그스태시를 실행하여 봅시다.

```shell
./bin/logstash -f config/test.conf
```

# 에러 발생

![]({{site.baseurl}}/images/2022-05-14-logstash-to-elasticsearch/error1.png)

실행시 다음과 같은 에러가 발생합니다.

`PKIX path building failed: sun.security.provider.certpath.SunCertPathBuilderException: unable to find valid certification path to requested target`

이는 Elasticstack 8.0 버전부터 default로 보안 설정이 적용되어 생기는 문제로 보안 관련 처리를 해주어야 제대로 동작합니다.

# certification 에러 해결

에러 해결 방법에는 두 가지 방법이 있습니다.
1. SSL 자격 확인을 하지 않도록 설정하는 방법
2. SSL 자격 인증서를 등록하는 방법

여기서는 우선 1번을 이용한 해결 방법을 소개하도록 하겠습니다.

2번 방법에 대한 해결법은 [다음 게시물](https://knu-cd2.github.io/blog/logstash/2022/05/14/pkix-path-building-failed.html)을 참고해주세요.

## conf 파일 변경

elasticsearch 플로그인에 다음 두 옵션 `ssl_certificate_verification`과 `ssl`을 추가합니다.
```conf
...

output {
    elasticsearch {
        index => "output"
        hosts => ["https://127.0.0.1:9200"]
        user => "elastic"
        password => "yourpassword"
        ssl_certificate_verification = false
        ssl = true
    }
    stdout {}
}
```

수정 후 다시 실행하여 봅시다.

```shell
./bin/logstash -f config/test.conf
```

![]({{site.baseurl}}/images/2022-05-14-logstash-to-elasticsearch/done1.png)

정상적으로 실행되었습니다! output에서 표준 출력으로 잘 출력되었음을 확인할 수 있습니다.

# Elasticsearch에서 확인

실제 elasticsearch에도 인덱스가 만들어졌는지 확인해봅시다.

다음과 같이 명령어 `curl`를 날려보세요.

```shell
curl -u "elastic:yourpassword" -k https://localhost:9200/output?pretty
```

![]({{site.baseurl}}/images/2022-05-14-logstash-to-elasticsearch/done2.png)

사진과 같이 정상적으로 인덱스로 저장되었음을 확인할 수 있습니다.