---
title: (JSP) 4 - request 기본 객체
date: 2020-07-17T20:37:19+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - response
  - 기본객체
---

+ 거의 사용되지 않는 객체
	+ HTTP 헤더에 내용 추가
	+ 다른 페이지로 리다이렉트(redirect)
	
+ 리다이렉트 시키기
	+ `response.sendRedirect()` 메소드 사용
	
<br>

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>

<!DOCTYPE html>
<html>
<head>
	<title>리다이렉트페이지</title>
</head>
<body>
	
<%
	String redirect = request.getParameter("redirect");
	if(redirect != null && redirect.equals("yes")) {
		response.sendRedirect("/chap03/form.jsp");
	}
%>

hello

</body>
</html>
~~~