---
toc: true
layout: post
description: on-premise os 설치
categories: [On-premise]
title: On-premise os install
---

# OS 설치

{% include alert.html text="64GB 이상의 SD카드의 해당 방식으로 진행시 오류가 발생할 수 있습니다." %}

## 1. 디스크 포맷 (선택 사항)

 이미지는 예시를 보여주기 위해 128GB SD 카드를 이용하였습니다.

> ### 1. SD Card Formatter

<br/>
![]({{site.baseurl}}/images/format1.png)
<br/>

>sdcard.org에서 다운 받을 수 있고, 간편하게 SD 카드를 포맷시킬 수 있는 프로그램입니다.

<br/>
![]({{site.baseurl}}/images/format2.JPG)
<br/>

>sd카드를 컴퓨터에 넣고 동작하면 위와 같이 자동적으로 인식이 됩니다.

<br/>
![]({{site.baseurl}}/images/format3.JPG)
<br/>

>Quick format을 실행하면 파일시스템도 알아서 잘 포맷해 줍니다.

<br/>

{% include info.html text="클러스터 사이즈의 경우 운영체제를 설치하면 다시 설정됩니다" %}

> ### 2. 윈도우에서 포맷 하는 경우

<br/>
![]({{site.baseurl}}/images/format4.JPG)
<br/>

> 파일시스템은 FAT32로 해야 됩니다. 그리고 블록사이즈는 OS를 설치하면 자동적으로 512로 변경이되기 때문에 블록 사이즈는 기존 설정대로하면 됩니다.

> 저는 지금 예시를 알려주기 위해 128GB SD 카드로 진행했기에 exFAT으로 표시되고 있습니다.

## 2. OS 설치

OS의 경우 ubuntu server 20.04 LTS 를 사용했습니다. ubuntu server의 경우 OS를 설치하면 ssh 파일을 생성하는 등 번거러운 과정 필요 없이 바로 ssh로 접속할 수 있고, 용량도 작기에 Desktop 대신 Server를 이용하였습니다.

<br/>
![]({{site.baseurl}}/images/os.png)
<br/>

버전의 경우 22.04 LTS나 향후 더 나올 최신 버전을 사용해도 상관 없을 것 같습니다

<br/>
![]({{site.baseurl}}/images/etcher1.png)
<br/>

os를 설치할 땐 etcher를 사용합니다.

<br/>
![]({{site.baseurl}}/images/etcher2.JPG)
<br/>

UI가 직관적이기 때문에 별도의 설명이 없어도 충분히 쉽게 사용 가능합니다.

<br/>
![]({{site.baseurl}}/images/etcher3.JPG)
<br/>

설치가 완료되면 자동적으로 디스크가 꺼내집니다.
