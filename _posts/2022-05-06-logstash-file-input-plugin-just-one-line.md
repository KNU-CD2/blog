---
toc: true
layout: post
description: Logstash file input plugin 사용시 한 줄만 읽어지는 문제
categories: [Logstash]
title: [Elastic Stack] How to use file input plugin to read multiple lines in logstash.
sticky_rank: 15
---

# 로그스태시 file input 플러그인 사용시 한 줄만 읽어지는 문제

## 문제 상황

다음과 같이 log 파일을 임시로 생성하고, 이것을 logstash의 file input plugin을 이용하여 읽고 stdout으로 출력하려고 시도하였다.

`filter-example.log`

```log
[2020-01-02 14:17] [ID1] 192.10.2.6 9500 [INFO] - connected.
[2020/01/02 14:19:25]   [ID2] 218.25.32.70 1070 [warn] - busy server.
```

하지만 파일의 첫번째 line만 출력되고, 두번째 line은 출력되지 않는 문제가 발생하였다.

## 원인

로그스태시의 File input plugin은 기본적으로 파일을 처음 읽으면 `sincedb`라는 파일에 읽고 있는 위치를 기록한다.
따라서 로그스태시를 재시작하면 sinedb를 참조하여 파일의 tail, 즉 끝부터 읽는다.
실제 서비스를 운영할 때는 새로 들어오는 데이터에 대해서만 처리해주면 되지만, 테스트할 때는 재시작할 때 파일의 첫 위치부터 읽기를 원한다.

## 해결 방법

1. mode(읽기모드)를 read 로 설정한다. 그러나 이것만 설정 시 파일이 한번 읽히면 삭제되는 문제가 발생한다.
2. file_completed_action을 log로 설정한다. 파일을 읽고 나서 삭제하지 않고 기록하자.
3. file_completed_log_path를 지정한다. 해당 파일을 읽었다고 기록하는 로그를 작성해야한다. 공식 문서에 따르면, 이 파일이 매우 커질 수 있으므로 관리를 주의해야 한다고 한다.

`logstash-test.conf`

```conf
input {
	file {
		path => "/Users/squareyun/Documents/utility/logstash-8.1.1/config/filter-example.log"
		start_position => "beginning"
		mode => "read"
		file_completed_action => "log"
		file_completed_log_path => "/Users/squareyun/Documents/utility/logstash-8.1.1/config/test.log"
		sincedb_path => "/dev/null"
	}
}

output {
    stdout { }
}
```

## Reference

- https://www.elastic.co/guide/en/logstash/current/plugins-inputs-file.html#plugins-inputs-file-mode
