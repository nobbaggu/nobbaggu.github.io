---
title: (JSP) 2 - request 기본 객체
date: 2020-07-17T18:37:19+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - request
  - 기본객체
---

+ JSP 페이지에서 가장 자주 사용되는 객체

+ 클라이언트(사용자 혹은 웹 브라우저)의 요청 정보를 가지고 있는 객체
	+ request 객체 메소드로 요청 정보의 각 항목들 호출

+ 어떤 요청 정보를 가지고 있나?
	+ 웹 브라우저 정보
	+ 요청 파라미터
	+ 요청 헤더
	+ 클라이언트 쿠키
	+ 속성 처리

<br>

## 1. 클라이언트 및 서버 정보 관련 메서드 ##
----

+ `getRemoteAddr()`
	+ 클라이언트 IP를 반환해준다.
+ `getContentLength()`
	+ 클라이언트가 전송한 요청 정보의 길이를 반환. 알 수 없는 경우 -1을 반환한다.
+ `getCharacterEncoding()`
	+ 요청을 전송할 때 사용된 문자 인코딩
+ `getContentType()`
	+ 요청을 전송할 때 사용한 컨텐츠의 타입
+ `getProtocol()`
	+ 요청한 프로토콜(http, https)
+ `getMethod()`
	+ 요청 방식(GET, POST)
+ `getRequestURI()`
	+ 요청 URL의 경로(서버 이름 뒷부분)
+ `getContextPath()`
	+ JSP페이지가 속한 웹 애플리케이션의 컨텍스트 경로
+ `getServerName()`
	+ 연결할 때 URL에 사용한 서버 이름(ip주소 혹은 도메인 네임)
+ `getServerPort()`
	+ 서버 포트번호

<br>
		
+ 예시

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>

<!DOCTYPE html>
<html>
<head>
	<title>클라이언트 및 서버 정보</title>
</head>
<body>
	<%=request.getRemoteAddr() %>
	<%=request.getContentLength() %>
	<%=request.getCharacterEncoding() %>
	<%=request.getContentType() %>
	<%=request.getProtocol() %>
	<%=request.getMethod() %>
	<%=request.getRequestURI() %>
	<%=request.getContextPath() %>
	<%=request.getServerName() %>
	<%=request.getServerPort() %>
</body>
</html>
~~~

+ 결과

0:0:0:0:0:0:0:1
-1
null
null
HTTP/1.1
GET
/chap03/requestInfo.jsp
/chap03
localhost
8080
	
<br>

## 2. 요청 파라미터 처리 메소드 ##
----

+ `String getParameter(String name)`
	+ 파라미터 명을 통해 파라미터 값 읽기 -> 없을경우 null

+ `String[] getParameterValues(String name)`
	+ 이름이 name인 파라미터 값들을 배열로 받기 -> 없을경우 null
	
+ `Enumeration getParameterNames()`
	+ 파라미터 목록 읽기
	
+ `Map getParameterMap()`
	+ 파라미터들의 <이름, 값> 을 Map 형태로 가져오기
	
+ 예시

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>

<!DOCTYPE html>
<html>
<head>
	<title>보내기</title>
</head>
<body>
	
	<form action="/chap03/viewParameter.jsp" method="post">
		<label for="name">이름 </label>
		<input id="name" type="text" name="name" size="10"><br>
		
		<label for="addr">주소</label>
		<input id="addr" type="text" name="addr" size="30"><br>

		<input type="checkbox" name="pet" value="dog">강아지
		<input type="checkbox" name="pet" value="cat">고양이
		<input type="checkbox" name="pet" value="pig">돼지

		<input type="submit" value="전송">

		<br>
		<br>
	</form>

</body>
</html>
~~~

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ page import="java.util.Enumeration" %>
<%@ page import="java.util.Map" %>

<%
	request.setCharacterEncoding("utf-8");
%>

<!DOCTYPE html>
<html>
<head>
	<title>요청 파라미터 출력</title>
</head>
<body>

<b>getParameter() 메소드</b><br>

name= <%= request.getParameter("name") %><br>
address= <%= request.getParameter("addr") %><br>

<br>
<b>getParameterValues() 메소드</b><br>
<%
	String[] values = request.getParameterValues("pet");
	if(values != null) {
		for(int i = 0; i < values.length; i++) {
%>
	<%= values[i] %><br>
<%
		}
	}
%>

<br>
<b>getParameterNames() 메소드</b><br>

<%
	Enumeration enums = request.getParameterNames();
	while(enums.hasMoreElements()) {
%>
	<%= enums.nextElement() %><br>
<%
	}
%>

<br>
<b>getParameterMap() 메소드</b><br>

<%
	Map paramMap = request.getParameterMap();
%>

	<%= ((String[]) paramMap.get("name"))[0] %><br>
	<%= ((String[]) paramMap.get("addr"))[0] %><br>
	<%= ((String[]) paramMap.get("pet"))[0] %><br>
	<%= ((String[]) paramMap.get("pet"))[1] %><br>

</body>
</html>
~~~

+ 결과

~~~ text
getParameter() 메소드
name= 이말년
address= 서울시 강남구

getParameterValues() 메소드
dog
cat

getParameterNames() 메소드
name
addr
pet

getParameterMap() 메소드
이말년
서울시 강남구
dog
cat
~~~ text

<br>

+ 주의사항
	+ `checkbox`에서 체크하지 않은 파라미터는 전송하지 않는다.
		+ `radio`도 마찬가지다.
	+ `getParameterMap()`의 각 `value`는 배열이다.
		+ 각각의 파라미터 이름에 해당하는 값들이 배열로 전송된다.
	+ `request.setCharacterEncoding("utf-8")`
		+ `request` 객체에서 받아온 파라미터를 디코딩할 때 여기서 지정한 캐릭터셋을 사용한다.