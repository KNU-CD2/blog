---
toc: true
layout: post
description: vscode를 이용한 ssh 원격 접속 방법
categories: [Ssh]
title: SSH connect with vscode
sticky_rank: 19
---

# 서론

원격 서버에 접속하여 사용할 때 `파일 시스템`을 한눈에 보고 싶을 때나, `vim` 명령어가 익숙하지 않아 불편할 때가 있습니다.

그럴 때 vscode의 원격 접속 확장 프로그램을 사용하면 편리하게 서버에 접속할 수 있습니다.

# 확장 프로그램 설치

{% include info.html text="vscode가 설치되어 있지 않다면 공식 사이트에서 다운로드하세요." %}

vscode를 실행시키고, 좌측 메뉴바에서 마켓플레이스에 들어가 `SSH`를 검색합니다.

![]({{site.baseurl}}/images/2022-05-16-vscode-ssh/program1.png)

위 사진에 해당하는 확장 프로그램을 설치해주세요.

# 원격 접속 세팅

설치 후 vscode 화면의 좌측 하단의 `> <` 버튼을 클릭하세요.

![]({{site.baseurl}}/images/2022-05-16-vscode-ssh/program2.png)

![]({{site.baseurl}}/images/2022-05-16-vscode-ssh/list1.png)

Connect to Host... > Configure SSH Hosts... > {save points} 순으로 선택하여 config 파일을 생성하고 작성합니다.

```
Host test_server
  HostName xxx.xxx.xxx.xxx
  User test_user
  Port xxxx
```

`Host`는 서버 정보 이름을 무엇으로 정할지 나타내는 것이고, HostName, User, Port는 접속할 원격 서버 정보를 작성합니다.

작성을 완료 하셨으면 다시 버튼을 클릭하여 아까 저장해둔 정보로 접속을 해봅시다.

![]({{site.baseurl}}/images/2022-05-16-vscode-ssh/done1.png)

서버의 비밀번호를 작성하면 접속에 성공합니다 !

비밀번호 작성하는 것이 귀찮아졌다면, [원격 접속 자동 로그인 게시글](https://knu-cd2.github.io/blog/ssh/2022/05/16/vscode-ssh-without-password.html)을 참고해주세요.
