---
toc: true
layout: post
description: vscode를 이용한 ssh 원격 접속 자동 로그인 방법
categories: [Ssh]
title: SSH connect auto login with vscode
sticky_rank: 20
---

# 서론

vscode를 이용하여 원격 접속할 때 비밀번호를 매번 입력하는 과정이 귀찮습니다.

꼭 vscode를 이용할 때 뿐만 아니라 shell에서 접속할 때도 마찬가지입니다.

원격 접속을 할 때 비밀번호를 입력하지 않고 자동으로 인증되도록 설정하는 방법을 알아보겠습니다.

# SSH key 생성하기

SSH key는 `공개 키 (Public Key)`와 `비공개 키 (Private Key)`로 이루어져 있습니다.

비공개 키 `id_rsa`는 로컬 컴퓨터에 존재해야하며, 공개 키 `id_rsa.pub`는 원격 서버에 존재해야합니다.

비공개 키는 외부에 절대로 노출되어서는 안됩니다.

SSH key를 생성하고 공개 키를 원격 서버에 저장하는 과정을 알아보겠습니다.

## SSH key 생성

{% include info.html text="windows는 git bash를 이용하여 진행하면 동일하게 진행할 수 있습니다." %}

우선 SSH key가 이미 존재하는지 확인해봅시다.

```shell
cat ~/.ssh/id_rsa.pub
```

`ssh-rsa`로 시작하는 문자열이 출력된다면 다음 단계로 진행하세요.

없다면 다음을 명령어로 키를 생성합니다.

```shell
ssh-keygen -t rsa
```

생성할 때 모두 default로 설정하여도 좋습니다. 따로 입력하지 않고 Enter를 3번 눌러 진행하세요.

## 원격 서버에 저장

ssh-key가 생성되었다면 출력해보세요.

```shell
cat ~/.ssh/id_rsa.pub
```

`ssh-rsa`로 시작하는 문자열을 복사하여 원격 서버의 `~/.ssh/authorized_keys`에 등록하도록 하겠습니다.

```shell
vi ~/.ssh/authorized_keys
```

여기에 아까 복사한 ssh-rsa 문자열을 붙여넣고 저장합니다.

# vscode config 파일 수정

이제, vscode에서 이 키를 가지고 인증하도록 세팅해주기만 하면 됩니다.

다음과 같이 접속 config 파일을 수정하세요.

```
Host test_server
  HostName xxx.xxx.xxx.xxx
  User test_user
  Port xxxx
  IdentityFile ~/.ssh/id_rsa
```

Done!

이제 비밀번호 없이 원격 접속이 가능합니다.