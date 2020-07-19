---
title: (JSP) 8 - include 디렉티브
date: 2020-07-18T16:00:00+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - include
  - 디렉티브
---

## 1. 사용 방법 ##
----

~~~ jsp
<%@ include file="포함할 JSP 파일" %>
~~~

+ 일반적으로 포함시킬 파일의 확장자는 jspf로 한다.
	+ jsp fragment(조각) 라는 뜻
	+ jsp 확장자로 해도 상관없다.

+ 컴파일 전에 포함할 페이지의 코드를 복사해서 해당 위치에 삽입한다.

<br>

+ 예제

main.jsp

~~~ jsp
<!DOCTYPE html>
<%@ page contentType="text/html; charset=utf-8" %>

<html>
<head>
	<title>main</title>
</head>
<body>
<%@ include file="sub.jspf" %>
<%= str + " " + str %>

</body>
</html>
~~~

<br>
sub.jspf

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>

<p>
	<% String str = "안녕하쎄여"; %>
	<%= str %>
</p>
~~~

<br>
출력결과

~~~ jsp
안녕하쎄여

안녕하쎄여 안녕하쎄여
~~~

<br>

+ sub.jspf에서 선언한 변수 `str`은 어떻게 main.jsp 파일에서 그대로 사용할 수 있었을까?
	+ 코드를 그대로 복사해서 끼워넣고 컴파일 하는 것이기 때문이다. 한 파일 내에 선언한 것과 다름 없다.

<br>

## 2. include 액션 태그와의 차이 ##
----

+ 페이지를 포함시키는 방식이 다르다.
	+ include 액션태그
		+ 포함 페이지로 흐름이 이동하여 해당 코드를 실행
	+ include 디렉티브
		+ 컴파일 전에 포함할 페이지의 코드를 복사해서 해당 위치에 삽입
	
+ 사용 목적의 차이
	+ include 액션태그
		+ 레이아웃의 구성 요소를 모듈화하여 재사용
	+ include 디렉티브
		+ 다수의 JSP 페이지에서 사용하는 변수 선언(session 객체의 사용자 정보 등)
		+ 저작권 표시와 같이 많은 페이지에서 사용되는 간단한 문장

<br>

## 3. 코드조각 자동 포함 기능 ##
----

+ 헤더와 푸터는 모든 페이지에서 공통적인 경우가 많은데 모든 페이지에 헤더, 푸터를 포함하는 jspf 파일을 불러오는 것도 고생스럽다.

+ WEB-INF/web.xml을 통해 설정 자동화 가능

~~~ jsp
<jsp-config>
	<jsp-property-group>
		<url-pattern>/view/*</url-pattern>
		<include-prelude>/common/variable.jspf</include-prelude>
		<include-coda>/common/footer.jspf</include-coda>
	</jsp-property-group>
</jsp-config>
~~~

+ url-pattern
	+ 프로퍼티를 적용할 JSP 파일의 URI 패턴
	
+ include-prelude
	+ JSP 파일의 앞에 삽입
	
+ include-coda
	+ JSP 파일의 뒤에 삽입