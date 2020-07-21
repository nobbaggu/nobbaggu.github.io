---
title: (JSP) 13 - EL 기본
date: 2020-07-20T19:00:00+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - JSP
  - EL
  - 표현언어
---

## 1. 표현 언어란? ##
----

+ 값을 표현하는데 사용하는 스크립트 언어
	+ JSP의 스크립트 요소(스크립트릿, 표현식, 선언부)를 보완해준다.
	
+ 표현언어가 제공하는 기능들
	+ JSP의 네 가지 기본객체(page, request, session, application)가 제공하는 영억의 속성 사용
	+ 수치,관계,논리 연산자 제공
	+ 자바 메소드 호출
	+ 쿠키, 기본 객체의 속성 등 JSP를 위한 표현 언어의 기본 객체 제공
	+ 람다식을 이용한 함수 정의와 실행
	+ 스트림 API를 통한 컬렉션 처리
	+ 정적 메소드 실행
	
+ 표현 언어는 JSP 스크립트 요소보다 표현이 간결하고 직관적이다.
	+ 따라서 실제 프로젝트에서는 일반적으로 표현 언어를 사용한다.

+ 표현언어(Expression Language)를 짧게 EL이라고 부른다.
	
~~~ jsp
<%--표현식--%>
<%= member.getAddress().getZipCode() %>

<%--표현 언어--%>
${member.address.zipcode}
~~~

<br>

## 2. 데이터 타입 ##
----

+ EL은 5가지 타입을 제공한다.
	+ 불리언(Boolean) : true 혹은 false
	+ 정수 : java의 Long
	+ 실수 : java의 double. 3.24e3과 같은 exponent 표현 가능
	+ 문자열 : 작은따옴표('') 혹은 큰따옴표("")로 감싼다.
	+ 널 : null
	
+ ex) ${10}, ${"hello"}, ${3.1e10}, {null}, {true}

<br>

## 3. EL의 기본 객체 ##
----

+ JSP와 마찬가지로 EL에서 간편하게 사용할 수 있는 기본 객체들이 있다.

+ **pageContext**
	+ JSP의 page 기본객체와 동일
	
+ **pageScope**
	+ `pageContext` 기본객체의 \<속성(attribute), 값(value)\>들이 저장된 Map
	
+ **requestScope**
	+ `request` 기본객체의 \<속성(attribute), 값(value)\>들이 저장된 Map

+ **sessionScope**
	+ `session` 기본객체의 \<속성(attribute), 값(value)\>들이 저장된 Map

+ **applicationScope**
	+ `application` 기본객체의 \<속성(attribute), 값(value)\>들이 저장된 Map

+ **param**
	+ `request` 기본객체에 있는 파라미터 \<이름, 값\>들이 저장된 Map
	+ `request.getParameter("이름")`과 동일

+ **paramValues**
	+ `request` 기본객체에 있는 \<이름, 값 배열\>이 저장된 Map
	+ `request.getParameterValues("이름")`과 동일

+ **header**
	+ 요청 헤더의 \<헤더이름, 값\>을 저장한 Map
	+ `request.getHeader("이름")`과 동일

+ **headerValues**
	+ 요청 헤더의 \<헤더이름, 값 배열\>을 저장한 Map
	+ `request.getHeaders("이름")`과 동일

+ **cookie**
	+ \<쿠키이름, 쿠키객체\>가 저장된 Map
	+ `request.getCookies()`로부터 구한 Cookie 배열로부터 매핑을 생성한다.

+ **initParam**
	+ 초기화 파라미터의 \<이름, 값\>이 저장된 Map
	+ `application.getInitParameter("이름")`과 동일
	
<br>

+ 예제
	+ 파일명 : useELObject.jsp
	+ 요청 URL : http://localhost:8080/chap11/useELObject.jsp?name=김삿갓

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>

<% request.setCharacterEncoding("utf-8"); %>
<% request.setAttribute("age", 18); %>

<!DOCTYPE html>
<html>
<head>
	<title>EL객체</title>
</head>
<body>
	<p>${pageContext.request.requestURI}</p>
	<p>${param.name}</p>
	<p>${requestScope.age}</p>
	<p>${cookie.JSESSIONID.name} : ${cookie.JSESSIONID.value}</p>
	
	<p><%= request.getParameter("gender") %></p>
	<p>${param.gender}</p>
</body>
</html>
~~~

<br>

출력결과

~~~ text
/chap11/useELObject.jsp
김삿갓
18
JSESSIONID : E70F13FD3EF87DAD899ABDA9286A2734
null
~~~

<br>

+ EL은 값이 없으면 null을 출력하는 표현식과 다르게 아무것도 출력하지 않는다.

<br>

## 4. 객체 탐색 ##
----

+ 중요

+ EL에서 객체명 없이 속성을 사용할 때
	+ pageScope, requestScope, sessionScope, applicationScope를 차례대로 탐색
	
~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ page import="thermometer.Thermometer" %>

<%
	Thermometer thermometer = new Thermometer();
	Thermometer thermometer2 = new Thermometer();
	request.setAttribute("t", thermometer);
	session.setAttribute("a", thermometer2);
%>

<!DOCTYPE html>
<html>
<head>
	<title>온도계 변화</title>
</head>
<body>
	<p>${t.setCelsius("서울", 27.3)}</p>
	<p>${t.getCelsius("서울")}도 / ${t.getFahrenheit("서울")}화</p>
	<p>${a.setCelsius("서울", 21.1)}</p>
	<p>${a.getCelsius("서울")}도 / ${a.getFahrenheit("서울")}화</p>
</body>
</html>
~~~

+ t와 a는 각각 request객체와 session 기본객체에 저장되어있는 속성이며 속성값으로 각자가 `Thermometer` 객체를 가진다.

+ 기본객체를 명시하지 않고 속성을 사용할 경우 네 개의 영역을 차례대로 탐색하므로 't', 'a'와 같이 이름만 사용할 수가 있다.
