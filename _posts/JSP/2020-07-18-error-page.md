---
title: (JSP) 6 - 에러 처리와 에러 페이지
date: 2020-07-18T14:00:00+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - 에러처리
  - 에러페이지
  - errorPage
---

+ 에러가 발생했을 때 사용자에게 보여 줄 페이지를 에러 페이지라 한다.

+ 브라우저에서 디폴트로 띄워주는 에러 페이지는 사용자가 보기 어렵고 사이트 신뢰성을 떨어뜨리며 코트가 일부 노출되어 보안에도 좋지 않다.
		
<br>

## 1. 에러 페이지 ##
----

+ page 디렉티브에서 `errorPage` 속성 사용
	+ 해당 jsp 페이지에 에러가 났을 때 속성값으로 지정한 jsp 페이지를 띄워준다.


~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ page errorPage="/myErrorPage.jsp" %>

<html>
<head>
	<title>로그인 페이지</title>
</head>
<body>
	<%
		String name = request.getParameter("name").toUpperCase();
		String age = request.getParameter("age");
	%>

	이름: <%= name %>
	나이: <%= age %> 

</body>
</html>
~~~

<br>

+ 에러 페이지 작성 방법
	+ page 디렉티브의 `isErrorPage` 속성값으로 `true` 지정
	+ `isErrorPage` 속성값이 `true`인 페이지는 `exception` 객체 사용 가능

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ page isErrorPage = "true" %>

<html>
<head>
	<title>에러 페이지</title>
</head>
<body>

<p>
에러가 발생했습니다.<br>
<p>

에러 타입: <%= exception.getClass().getName() %> <br>

에러 메시지: <%=exception.getMessage() %>

</body>
</html>
~~~

+ 위 에러페이지를 설정해주었을 때 뜨는 결과

~~~ text
에러가 발생했습니다.
에러 타입: java.lang.NullPointerException
에러 메시지: null
~~~

+ 실제로는 위와 같이 자바 문법을 보여주는 것도 사용자에게는 의미가 없을 것이다.

+ 유명한 검색사이트나 블로그 플랫폼 등에서 글이 더 이상 존재하지 않을 때 뜨는 페이지 같은 것을 생각하면 될 것이다.

<br>

## 2. 응답 상태 코드별로 에러 페이지 다르게 지정하기 ##
----

+ WEB-INF/web.xml에 설정 추가
	+ 에러 코드별로 에러 페이지 URI를 설정할 수 있다.

~~~ xml
<web-app ...>
	...
	<error-page>
		<error-code>에러코드</error-code>
		<location>에러페이지 URI</location>
	</error-page>
	...
</web-app>
~~~
