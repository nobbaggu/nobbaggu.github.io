---
title: (JSP) 5 - out, application 기본객체
date: 2020-07-18T13:00:00+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - out
  - 기본객체
---

+ request, response 객체 이외에 JSP에서 자주 사용되는 2가지 객체인 out, application 기본 객체에 대해 알아본다.

<br>

## 1. out 기본객체 ##
----

+ JSP 페이지가 생성하는 모든 내용이 웹브라우저로 전송되는 것을 담당한다.

+ 모든 비스크립트 요소들(HTML 태그, 텍스트, 표현식의 결과값)은 out 객체로 전달되어 `out.println()` 메소드에 의해 출력된다.

~~~ jsp
<body>

hello

<% out.println("hello"); %>

<% String str = "hello"; %>

<%= str %>

</body>
~~~

+ 문법은 다르지만 텍스트 hello와 마지막의 표현식 str도 모두 `out` 객체에 전달되어 `println()` 메소드로 출력된 것이다.

<br>

+ 조건문이나 반복문 등의 경우 `out` 객체를 사용하면 코드가 좀 더 깔끔해진다.

if-else 블록과 스크립트를 구분하기 위해 스크립트릿이 많이 들어가 더러운 코드 

~~~ jsp
<% if(num1 > 10) { %>
<%= message1 %>
<% } else if (num1 > 5) { %>
<%= message2 %>
<% } %>
~~~

<br>
out객체 사용한 경우
~~~ jsp
<%
if(num1 > 10) {
	out.println(message1);
} else if(num1 > 5) {
	out.println(message2);
}
%>
~~~

<br>

+ out 객체의 출력 메소드
	+ `print()`
		+ 데이터 출력
	+ `println()`
		+ 출력 후 줄바꿈
	+ `newLine()`
		+ 줄바꿈

<br>

## 2. application 기본객체 ##
----

+ 웹 애플리케이션 전반에 걸쳐 사용되는 정보를 가지고 있는 객체
	+ 초기 설정 정보, 서버 정보, 자원
	+ 웹 애플리케이션에 포함된 모든 JSP 페이지가 공유한다.
	
+ 초기화 파라미터 읽기
	+ 웹 애플리케이션 전체에 걸쳐 사용할 수 있도록 서블릿 규약에 정의된 값을 읽어올 수 있다.
	+ WEB-INF/web.xml에 정의된 파라미터
	+ `String getInitParameter(String name)`
	+ `Enumeration<String> getInitParameterNames()`;
	
~~~ xml
<context-param>
	<description>설명(선택)</description>
	<param-name>이름</param-name>
	<param-value>값</param-value>
</context-param>
~~~

<br>

+ 서버 정보 읽기
	+ `String getServerInfo()`
		+ 서버 정보
	+ `String getMajorVersion()`
		+ 서버가 지원하는 서블릿 규약의 메이저 버전(정수 부분)
	+ `String getMinorVersion()`
		+ 서버가 지원하는 서블릿 규약의 마이너 버전(소수 부분)
		
+ 로그 메시지 기록
	+ 웹 컨테이너가 사용하는 로그 파일에 로그 메시지 기록
	+ `void log(String name)`
		+ 로그 남기기
	+ `void log(String name, Throwable throwable)`
		+ 로그, 익셉션 정보 함께 남기기
		
+ 자원 접근
	+ `String getRealPath(String path)`
		+ 웹 애플리케이션 내에 있는 자원의 시스템 상에서의 절대 경로 리턴
	+ `URL getResource(String Path)`
		+ 웹 애플리케이션 내에 있는 자원에 접근할 수 있는 URL 객체 리턴
		+ `URL`의 `openStream()` 메소드를 사용해 스트림 생성해서 자원을 읽을 수 있다.
	+ `InputStream getResourceAsStream(String realPath)`
		+ 전달받은 절대 경로의 자원을 스트림으로 변환해서 리턴
