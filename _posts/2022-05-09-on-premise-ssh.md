---
toc: true
layout: post
description: on-premise 초기 접속 설정
categories: [On-premise]
title: On-premise init connection with ssh
sticky_rank: 3
---

{% include info.html text="모니터 없이 진행 가능합니다\n
하지만 iptime 공유기를 기준으로 설명합니다." %}

# SSH를 통한 접속

-------

## Hardware 설정

`Network--(LAN)--Iptime--(LAN)--Raspberry`<br/>
 위와 같이 Iptime 공유기와 라즈베리파이를 연결합니다.

------

## DHCP로 할당된 IP 확인
  모니터 없이 초기 설정을 하기 때문에 iptime 관리자 페이지를 통해서 연결된 라즈베리파이가 어떤 내부아이피를 할당 받았는지 확인하는 과정이 필요합니다.

  설치한 Iptime 공유기에 유/무선으로 데스크톱이나 노트북을 연결한 다음에 192.168.0.1 로 접속하면 관리자 페이지로 들어갈 수 있습니다.

  ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/ssh1.JPG)
  ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/ssh2.JPG)
  ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/ssh3.JPG)
  로그인을 한 후에  관리도구를 클릭합니다.
  
  <br/>

  ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/ssh4.JPG)
  그 다음으로 `고급 설정 -> 네트워크 관리 -> 내부 네트워크` 설정으로 들어가면 라즈베리파이가 할당된 내부 아이피를 확인 할 수 있습니다.

  저의 경우 여러대의 라즈베리파이가 연결되어 있기에 내부 네트워크도 여러개인 것을 확인 할 수 있습니다.

-------

## 포트포워딩

 저처럼 여러대의 라즈베리파이를 이용할 경우 외부아이피의 포트 22 하나로 여러대의 라즈베리파이에 ssh를 통해 접속할 수 없습니다. 그래서 포트포워딩을 설정해야합니다.

 ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/ssh5.JPG)
 `고급설정 -> NAT/라우터관리 -> 포트포워드 설정` 으로 이동합니다. 저의 경우 위와 같이 포트포워딩이 설정되어 있습니다.
 
 <br/>

 ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/ssh6.JPG)
 새로 포트포워드 규칙을 만들려면
 1. `새규칙 추가` 클릭 
 2. `규칙이름`에 원하는대로 규칙이름을 작성
 3. 라즈베리파이가 할당된 내부 IP 주소를`내부 IP주소` 칸에 입력하고
 4. 외부포트 범위를 입력, 단일 포트를 쓸 경우 앞부분만 쓰고 `~` 뒤에 부분은 안써도 됩니다.
 5. 내부포트도 입력합니다.
 6. `적용`을 클립합니다.
 
 <br/>

 ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/ssh7.JPG)
 예를 들어 위와 같이 설정 했을 때, 외부 아이피 : `59.23.xxx.xxx` 외부 포트 : `20004` 으로 접속하게 되면 `192.168.0.6`이 할당된 라즈베리파이의 22번 포트로 접속이 되게 됩니다.

---------

## 접속 확인

 ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/putty1.jpg)
 윈도우에서 접속확인을 하기 위해 `putty`라는 프로그램을 이용해서 ssh 접속을 시도해보겠습니다. ubuntu server를 처음 설치하면 유저이름은 `ubuntu`로 설정되어 있습니다.
 
 <br/>

 ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/putty2.jpg)
 `Accept`를 클릭합니다.
 
 <br/>

 ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/putty3.jpg)
 ubuntu server 처음 설치시에는 비밀번호도 `ubuntu`로 설정되어있습니다. 타이핑을 해줍시다.

 __타이핑할 때 입력이 안되는거처럼 보여도 입력이 정상적으로 되는 중입니다. 그냥 입력하고 엔터를 누르면 정상적으로 작동합니다.__
 
 <br/>
 
 ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/putty4.JPG)
 ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/putty5.jpg)
 처음 접속시 비밀번호를 바꿔야합니다. 비밀번호를 바꾸면 일단 한번 꺼지는데 다시 접속을해 바꾼 비밀번호를 입력합니다.
 
 <br/>
 
 ![]({{site.baseurl}}/images/2022-05-09-on-premise-ssh/putty6.JPG)
 위와 같이 정상적으로 접속되는 것을 확인 할 수 있습니다.
