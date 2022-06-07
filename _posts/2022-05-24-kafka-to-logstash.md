---
toc: true
layout: post
description: 카프카 클러스터와 로그스태시 연동
categories: [Logstash]
title: "[Logstash] Dataflow, Kafka Cluster to Logstash"
sticky_rank: 16
---

# 카프카-로그스태시 데이터 파이프라인 구축

__카프카 클러스터로부터 로그스태시를 이용해 데이터를 가져오는 방법__

---

## logstash에서 conf 파일 설정

* 사용할 `conf` 파일의 `input` 부분을 아래와 같이 구성하면 간단하게 완료됩니다.

```conf
input {
    kafka {
        bootstrap_servers => "59.23.xxx.xxx:{port},59.23.xxx.xxx:{port}"
        topics => ["{topic name}"]
        consumer_threads => 1
    }
}
```

* 클러스터를 구성한 경우 `bootstrap_servers`에 모든 IP:PORT를 적어주면 됩니다.

* 여기서 주의할 점은 하나의 `""` 안에 적어주어야 합니다. 아래와 같이 작성하면 오류가 발생합니다.

```
(error case1)
bootstrap_servers => "59.23.xxx.xxx:{port}", "59.23.xxx.xxx:{port}"

(error case2)
bootstrap_servers => ["59.23.xxx.xxx:{port}", "59.23.xxx.xxx:{port}"]
```

* 이 외에도 데이터를 불러올 때 다양한 옵션들이 많습니다. 다양한 옵션들은 logstash 공식 문서에서 확인하실 수 있습니다.