---
title: (디자인패턴) 13 - 프록시 패턴(Proxy Pattern)
date: 2020-06-29T18:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 프록시
  - proxy
  - 패턴
  - pattern
---

## 1. 프록시 패턴이란? ##
----

+ 프록시(Proxy)
	+ '대리인' 이라는 뜻을 가진 영어 단어
	
+ 프록시 객체
	+ 대리인 객체
	+ 실제 서비스 동작을 실행하는 객체의 대리 객체

<br>

![proxy_object_operation](https://nobbaggu.github.io/images/designpattern/proxypattern/proxy_object_operation.jpg){: width="50%" height="50%"}

<br>

+ 프록시 패턴
	+ 클라이언트가 실제 서비스 로직을 수행하는 객체(real object)가 아닌 이와 비슷한 프록시 객체의 메소드를 호출해 서비스를 요청하는 디자인 패턴
	+ 프록시 객체가 여러가지 부가적인 작업을 처리 후 실제 객체의 메소드를 호출하여 서비스 수행

<br>

![class_diagram](https://nobbaggu.github.io/images/designpattern/proxypattern/class_diagram.jpg){: width="50%" height="50%"}

<br>

### 프록시 패턴의 종류 및 사용목적 ###

+ 프록시 패턴은 목적과 형태는 다양한 형태로 존재하지만 결국은 기본 프록시 디자인을 따른다.

<br>

**1) 원격 프록시(remote proxy)**
	+ 네트워크로 연결된 원격 서버에 존재하는 객체를 사용하고자 할 경우 클라이언트가 로컬 프록시 객체에 요청하면 로컬 프록시 객체가 원격 통신을 통해 원격에 있는 객체의 메소드를 호출하고 결과를 받아오는 작업
	+ ex) 서버 앞단에 프록시 서버를 두면 요청이 올 경우 프록시 캐시에 존재하는 데이터는 프록시 객체가 그냥 돌려주고 없는 경우에만 실제 서버에 있는 서비스 객체로 데이터를 요청
	+ 다른 종류와 달리 클라이언트 보조 프록시(스텁-stub)과 서버 보조 프록시(스켈레톤-skeletion) 두 개를 사용한다.
	+ Java에서 프록시 객체 생성을 위해서 JDK에 포함되어있는 rmic 툴을 사용하면 간단하게 원격 프록시를 구현할 수 있다.
	
![remote_proxy](https://nobbaggu.github.io/images/designpattern/proxypattern/remote_proxy.jpg){: width="50%" height="50%"}

<br>

**2) 보호 프록시(protection proxy)**
	+ 프록시 객체가 권한이 있는 사용자인지 판단 후 true일 경우만 실제 객체의 메소드 호출
	+ java.lang.reflect 패키지를 사용하면 보호 프록시를 간단히 구현할 수 있다.
	
<br>

**3) 가상 프록시(virtual proxy)**
	+ 객체 생성 및 초기화 비용이 큰 객체를 대신하는 프록시
	+ 진짜 객체가 필요하기 전, 생성 도중에 진짜 객체를 대신한다.
	+ 진짜 객체 생성이 완료되면 진짜 객체에 요청을 직접 전달한다.
	+ ex) 뷰어에서 아이콘 객체가 용량이 큰 이미지의 로딩이 완료되기 전 기본 이미지를 사용하는 프록시 객체를 대신 보여준다.

<br>

+ 프록시 패턴 정의
	+ 객체에 대한 접근을 제어하기 위한 용도로 대리 객체를 제공하는 디자인 패턴
	+ 접근을 제어하는 목적은 크게 원격에 있는 객체와의 중개, 실제 객체 보호, 생성 비용이 큰 객체를 대신하기 위한 가상 객체 제공이 있다.

<br>

## 2. 정리 ##
----

+ 이외에도 프록시 패턴에는 여러가지 변형 응용 구조들이 존재한다.
	+ 방화벽 프록시, 캐싱 프록시, 동기화 프록시 등이 대표적이다.
	
+ 왠만한 프록시들은 프레임워크에서 지원하므로 웬만하면 그것을 쓰도록 하자.
