---
toc: true
layout: post
description: 파일비츠와 카프카 클러스터 연동
categories: [Filebeat]
title: "[Filebeat] Dataflow, Filebeat to Kafka Cluster"
sticky_rank: 21
---

# 파일비츠-카프카 데이터 파이프라인 구축

---

## yml 파일 수정

* `yml` 파일 수정을 통해 카프카 클러스터로 데이터 전송을 진행해보겠습니다.

* `filebeat.yml` 파일을 수정하는 방식으로 진행하겠습니다.

* `vim filebeat.yml` 한 후 아래 내용을 파일의 적당한 위치에 추가해줍니다.(저 같은 경우 파일 제일 끝에 추가하였습니다)

```yml
output.kafka:
  hosts: ["59.23.xxx.xxx:{port}","59.23.xxx.xxx:{port}"]

  topics: "{topic-name}"
  partition.round_robin:
    reachable_only: false

  required_acks: 1
  compression: gzip
  max_message_bytes: 1000000
```

* 실행 방법 : `./filebeat -e`

---

## 클러스터 IP 및 Port

* 두 개 이상의 카프카를 이용하면 `hosts`의 대괄호 안쪽에 콤마를 이용해 계속 추가해나가면 됩니다.

* 포트의 경우 카프카에서 리스너의 기본 포트인 `9092`를 사용하거나 포트포워딩을 진행했다면 포트포워드한 포트를 `{port}` 자리에 대체하면 됩니다.
