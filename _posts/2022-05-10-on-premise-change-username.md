---
toc: true
layout: post
description: on-premise 유저 이름 바꾸기
categories: [On-premise]
title: "[On-premise] username change"
sticky_rank: 4
---

# 유저 이름 바꾸기

서버 초기에 설치하면 유저이름이 `ubuntu`로 되어있습니다. 하지만 여러개의 클러스터를 운용할 때, 모든 클러스터 유저가 `ubuntu`로 되어 있으면 헷갈리기에 변경을 하도록 하겠습니다.

`ubuntu` 유저명을 바꾸는 대신 새로 유저를 생성하는 방법을 써도 됩니다.

<br/>

------

<br/>

## root로 ssh 접속하기
  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change1.JPG)
  일반 유저로 접속 후 `sudo passwd root`를 입력한 후 본인이 설정하고자 하는 비밀번호를 입력합니다. 해당 커맨드는 os 처음 설치시, root 비밀번호를 설정하는 커맨드입니다.

  <br/>

  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change2.JPG)  
  `sudo vim /etc/ssh/sshd_config` 를 입력합니다. root를 통한 ssh 접속은 기본적으로 거부되기 때문에 해당 파일의 설정을 변경해주어야 합니다.

  <br/>

  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change3.JPG)
  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change4.JPG)
  `PermitRootLogin` 속성을 찾아서 주석을 풀고 `prohibit-password`는 지우고 `yes`로 바꿔줍니다.
  vim을 사용하실 경우 `:\PermitRootLogin` 을 입력하면 쉽게 찾으실 수 있습니다.

  <br/>

  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change5.JPG)
  변경 사항을 반영하기 위해 `systemctl restart sshd`를 입력합니다.

  <br/>

  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change6.JPG)
  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change7.JPG)
  해당 과정을 거치면 ssh를 root 계정을 통해서 접속이 가능해지게 됩니다.

<br/>

----

<br/>

## 사용자 이름 바꾸기

  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change8.JPG)
  `usermod -l newname oldname` 을 입력합니다 소문자 L입니다. 저의 경우 `ubuntu`를 `simul`로 바꾸어 보겠습니다.

  <br/>

  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change9.JPG)
  `usermod -m -d /home/newname newname`을 입력합니다. __둘 다 newname으로 적은건 오타가 아닙니다!__
  {% include alert.html text="위 커맨드 입력을 실수하면 홈디렉토리가 사라질 수 있습니다." %}

  <br/>

  추가적으로 초기에 그룹 이름도 `ubuntu`로 되어 있는데, 그룹 이름도 바꾸고 싶은 경우 `groupmod -m newname oldname` 커맨드를 입력해주시면 됩니다. 저 같은 경우 이 부분은 생략하였습니다.

<br/>

-----------

<br/>

## 바뀐 사용자로 로그인
  
  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change10.JPG)
  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change11.JPG)
  바뀐 사용자로 로그인 한 다음에는 이제 다시 루트 로그인을 막아야합니다.

  <br/>

  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change12.JPG)
  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change13.JPG)
  `sudo vim /etc/ssh/sshd_config` 를 입력합니다. 그 다음 `PermitRootLogin` 부분 주석 처리를 합니다.

  <br/>

  ![]({{site.baseurl}}/images/2022-05-10-on-premise-change-username/change14.JPG)
  `systemlctl restart sshd`를 하면 정상적으로 root 로그인은 막히게 됩니다.

