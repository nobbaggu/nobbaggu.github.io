---
title: (JSP) 9 - <jsp:forward> 액션태그
date: 2020-07-18T17:00:00+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - forward
  - 액션태그
---

## 1. <jsp:forward> 액션태그 사용법 ##
----

+ 하나의 JSP 페이지에서 다른 JSP 페이지로 이동

+ ex) fromPage.jsp 페이지에서 toPage.jsp 페이지로 요청의 흐름을 이동

~~~ jsp
<jsp:forward page="/view/toPage.jsp" />
~~~

+ **응답 결과는 fromPage.jsp가 아닌 toPage.jsp가 생성한 응답 결과가 웹 브라우저에 전달된다.**

+ **fromPage.jsp에서 사용하던 request, response 기본객체는 toPage.jsp에 그대로 전달된다.**

+ 그럼 fromPage.jsp에서 처리할 것이지 굳이 왜 흐름을 제어하는가?
	+ 구조적/기능별로 페이지를 모듈화할 수 있다.
	+ ex) 조건에 따라 다른 화면을 보여주어야 한다면 조건문과 forward 태그를 사용하면 기능별 화면을 각각의 JSP 페이지로 관리할 수 있다.
	
<br>

fromPage.jsp

~~~ jsp
<!DOCTYPE html>
<%@ page contentType="text/html; charset=utf-8" %>

<html>
<head>
	<title>from페이지</title>
</head>
<body>
	<p>from 페이지의 내용</p>
	<jsp:forward page="/toPage.jsp" />
</body>
</html>
~~~

<br>
toPage.jsp

~~~ jsp
<!DOCTYPE html>
<%@ page contentType="text/html; charset=utf-8" %>

<html>
<head>
	<title>to페이지</title>
</head>
<body>
	<p>to페이지의 내용</p>
</body>
</html>
~~~

<br>

출력결과

~~~ text
to페이지의 내용
~~~

+ **fromPage.jsp의 내용은 출력되지 않았다.**
	+ 응답 결과는 toPage.jsp 페이지를 사용한다는 것을 알 수 있다.
	+ forward 태그를 만나서 페이지를 이동하기 전에 출력 버퍼의 내용은 버리기 때문이다.
	+ 따라서 forward 태그를 사용할 때는 buffer 속성을 none으로 하면 안된다.(저장 없이 바로 출력하기 때문)
	
+ 브라우저의 URL 주소는 응답화면이 나와도 fromPage.jsp 그대로이다.
	+ 요청 흐름의 제어는 웹 컨테이너가 하고 사용자는 다른 JSP가 요청을 처리했다는 사실을 모르도록 한다.
	
<br>

## 2. 활용 ##
----

+ forward 액션태그는 조건에 따라 다른 화면을 보여주어야 할 경우에 사용된다.
	+ 액션 태그를 사용하지 않고 스크립트와 html 태그만 사용하면 뒤섞여서 코드가 매우 복잡해진다.
	
+ 조건에 따른 페이지 이동 예시

~~~ jsp
<!DOCTYPE html>
<%@ page contentType="text/html; charset=utf-8" %>

<html>
<head>
	<title>select</title>
</head>
<body>
	
	<%
		String code = request.getParameter("code");
		String viewPage = null;

		siwtch(code) {
			case 1:
				viewPage = "/view/page1.jsp";
				break;
			case 2:
				viewPage = "/view/page2.jsp";
				break;
			case 3:
				viewPage = "/view/page3.jsp";
				break;
			default:
				viewPage = "/view/pageDefault.jsp";
		}
	%>

	<jsp:forward page="<%= viewPage %>" />
</body>
</html>
~~~

<br>

## 3. 요청 흐름이 이동할 때 데이터 전달 ##
----

+ include 액션태그와 마찬가지로 \<jsp:param\> 태그 사용 가능

+ request 객체의 `setAttribute()`, `getAttribute()` 사용 가능
	
+ 보통은 위 두 방법 중 request 기본 객체를 사용해서 데이터를 전달한다.
	+ param 태그는 String 값 밖에 전달할 수 없으나 request 객체에는 어떤 타입이든 객체로 전달 가능하다.
