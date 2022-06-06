---
toc: true
layout: post
description: 카프카 실행 시 에러가 날 때
categories: [Kafka]
title: ERROR Exiting Kafka due to fatal exception (kafka.Kafka$)
sticky_rank: 17
---

# 카프카 실행 에러

---

## 에러 로그

```
ERROR Exiting Kafka due to fatal exception (kafka.Kafka$)
java.nio.file.NoSuchFileException: -
        at sun.nio.fs.UnixException.translateToIOException(UnixException.java:86)
        at sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:102)
        at sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:107)
        at sun.nio.fs.UnixFileSystemProvider.newByteChannel(UnixFileSystemProvider.java:214)
        at java.nio.file.Files.newByteChannel(Files.java:361)
        at java.nio.file.Files.newByteChannel(Files.java:407)
        at java.nio.file.spi.FileSystemProvider.newInputStream(FileSystemProvider.java:384)
        at java.nio.file.Files.newInputStream(Files.java:152)
        at org.apache.kafka.common.utils.Utils.loadProps(Utils.java:668)
        at kafka.Kafka$.getPropsFromArgs(Kafka.scala:52)
        at kafka.Kafka$.main(Kafka.scala:86)
        at kafka.Kafka.main(Kafka.scala)
```
카프카를 실행 했을 때 위와 같은 오류가 뜨면서 실행이 되지 않았습니다.

<br/>

---

## 에러 당시 상황

카프카를 단일 노드로 실행하고 테스트하다가 클러스터를 구성해서 실행하니깐 저런 에러가 뜬 것으로 예측됩니다.

---

## 해결 방법

{% include info.html text="해당 방법이 정확한 방법이 아닐 수 있습니다" %}

__before__
![]({{site.baseurl}}/images/kafka/kafkaerror11.JPG)

<br/>

__after__
![]({{site.baseurl}}/images/kafka/kafkaerror12.JPG)

<br/>

저같은 경우 카프카 폴더 안 `config` 폴더에 들어있는 `server.properties` 파일에서 `log.dirs` 옵션을 `log.dir`로 바꿔서 실행하니 해결이 되었습니다.