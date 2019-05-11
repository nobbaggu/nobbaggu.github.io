---
title: 1. 준비
date: 2019-04-24T13:00:00+09:00
author: SWnomad
layout: post
categories: 안드로이드
tags:
  - android
  - project
  - 안드로이드
  - 프로젝트
  - hello
---
안드로이드는 자바를 기반으로 개발한다. 따라서 안드로이드 개발을 위해서는 JDK(Java Development Kit)와 JRE(Java Runtime Environment)가 필요하다. 마지막으로 Android Studio를 설치하면 된다.

설치가 끝났다면 첫 프로젝트를 만들어보자.

&nbsp;
### 1. Start a new Project 클릭

![start_new_project](/images/android/1/start_new_project.png){: width="70%" height="70%"}

&nbsp;
### 2. empty_activity 선택

![empty_activity](/images/android/1/empty_activity.png){: width="70%" height="70%"}

&nbsp;
### 3. 앱 이름 설정
 
![application_name](/images/android/1/app_name.png){: width="70%" height="70%"}

프로젝트 이름은 앱의 이름이 된다. 패키지 명은 다른 앱과 구분되는 고유한 값으로 유일해야 한다. 보통 도메인 이름 형식을 사용한다.

&nbsp;
### 4. 프로젝트 sync

![syncing](/images/android/1/syncing.png){: width="70%" height="70%"}

프로젝트를 생성하면 구성하는데 몇초 가량의 시간이 걸린다.

&nbsp;
### 5. 프로젝트 생성

![hello_android](/images/android/1/hello_android.png){: width="70%" height="70%"}

프로젝트가 완성된 화면이다.

앱을 실행해볼 수 있는 방법은 2가지가 있는데, 하나는 에뮬레이터(Emulator)이다. 에뮬레이터는 컴퓨터에 가상의 안드로이드 OS 단말을 설치하는 것이다. 그러나 에뮬레이터는 너무 심하게 느리고 무거워 개인적으로는 사용하지 않는다.

두 번째 방법은 내가 가지고 있는 안드로이드 단말기를 USB를 통해 연결하여 테스트하는 방법이다. 실제 단말에서 테스트 하는것이 더 정확하고 빠르게 테스트 할 수 있어 이 방법을 추천한다.

먼저 Shift + 10을 누르거나 오른쪽 상단의 ▶을 누르면 앱이 실행된다. 아직 아무 기기도 등록되어있지 않을 것이다.

먼저 컴퓨터와 단말기의 연결을 위한 드라이버를 설치해야 한다.

http://local.sec.samsung.com/comLocal/support/down/kies_main.do?kind=usb

위 링크를 타고 들어가면 삼성 갤럭시 S 시리즈 단말기 연결을 위한 드라이버를 설치할 수 있다.

이제 자신의 단말기를 **개발자 모드**로 바꾸어야 하는데, 설정 - 디바이스 정보(휴대전화 정보) - 소프트웨어 정보 - 빌드 번호가 나올 것이다. 이 때 빌드 번호를 터치하다보면 '[개발자 모드]를 실행하였습니다.' 라는 메세지가 나올 때 까지 터치하면 된다. 단말기마다 조금씩 다르므로 이 부분은 구글에서 개발자 모드 실행에 관해서 검색하면 된다.

이제 컴퓨터에 단말기를 연결하면 'USB 디버깅을 허용하시겠습니까?' 라는 메세지가 나오는데, 확인을 하면 된다.

다시 Shift+F10을 눌러 실행하면 다음과 같이 내 디바이스가 등록이 된 것을 볼 수 있다.

![hello_android](/images/android/1/device_connect.png){: width="60%" height="60%"}

OK를 누르면 단말기에 앱이 자동으로 설치되고 실행된다.