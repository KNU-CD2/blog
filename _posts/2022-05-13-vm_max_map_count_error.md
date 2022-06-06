---
toc: true
layout: post
description: max virtual memory areas vm.max_map_count [65530] is too low 에러 해결법
categories: [Elasticsearch]
title: [Elastic Stack] How to handle vm.max_map_count [65530] error in elasticsearch
sticky_rank: 7
---

# 문제 상황

초기 엘라스틱서치 실행 후 다음과 같이 `max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]` 라고 오류가 발생할 때가 있습니다.

![]({{site.baseurl}}/images/2022-05-13-vm/error1.png)

# 해결법

위와 같이 에러가 발생하였다면 다음과 같이 입력하여 `vm.max_map_count`를 `262144`로 변경하면 됩니다.

```shell
sudo sysctl -w vm.max_map_count=262144
```

## 왜 이런 에러가 발생할까?
- Elasticsearch는 `MMap FS`라는 타입의 디렉터리를 사용하여 샤드 인덱스를 저장합니다.
- 이 타입은 파일을 메모리에 매핑하여 저장하는데, 매핑되는 파일과 동일한 크기의 가상 메모리 주소 공간이 필요합니다.
- 하지만, 운영 체제의 기본 제한이 너무 낮으므로 `out of memory`를 유발합니다.
- 충분한 가상 메모리 주소 공간을 허용하라고 경고하기 때문에 이 에러가 발생한다고 알 수 있습니다.


## Reference
- https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html
- https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules-store.html#file-system