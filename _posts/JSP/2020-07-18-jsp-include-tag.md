---
title: (JSP) 7 - \<jsp:include\>
date: 2020-07-18T15:00:00+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - include
  - 액션태그
---

## 1. 사용법 ##
----

+ jsp의 내용을 추가하고 싶은 부분에 아래와 같이 태그를 사용해서 내용 추가

~~~ jsp
<jsp:include page="header.jsp" />
~~~

~~~ jsp
<jsp:include page="/top.jsp" flush="true" />
~~~

<br>

예시

main.jsp

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>

<!DOCTYPE html>
<html>
<head>
	<title>main</title>
</head>
<body>

<p>내용1</p>

<jsp:include page="sub.jsp" />

<p>내용2</p>

</body>
</html>
~~~

<br>

sub.jsp

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>

<p>
내용 끼워넣기~
</p>
~~~

<br>

결과

~~~ text
내용1

내용 끼워넣기~

내용2
~~~

<br>

+ 실제 개발에서는 웹페이지의 상단, 푸터, 메뉴 등 항상 같은 레이아웃의 부분을 가지는 경우가 대부분이다.
	+ 각각의 화면마다 같은 코드를 가지고 있고 내용이 변해서 하나하나 다 변경해야 한다면?
	+ include 액션태그를 사용하면 중복 코드를 손쉽게 처리할 수 있다.

<br>

## 2. <jsp:param> 태그로 포함할 페이지에 파라미터 추가 ##
----

+ \<jsp:include\> 태그 안에 \<jsp:param\> 태그를 넣어주면 포함하려는 페이지에 파라미터를 추가할 수 있다.

+ \<jsp:param\>을 통해 전달하기 전 `request.setCharacterEncoding()` 메소드를 통해 캐릭터 인코딩을 설정해주어야 영문/숫자 이외의 한글 데이터가 잘 전달된다.

~~~ jsp
<jsp:include page="/sub.jsp">
	<jsp:param name="param1" value="value1" />
	<jsp:param name="param2" value="value2" />
</jsp:include>
~~~

예제

main.jsp

~~~ jsp
<!DOCTYPE html>
<%@ page contentType="text/html; charset=utf-8" %>

<% request.setCharacterEncoding("utf-8"); %>

<html>
<head>
	<title>main</title>
</head>
<body>

<p>내용1</p>

<jsp:include page="sub.jsp">
	<jsp:param name="name" value="김삿갓" />
	<jsp:param name="age" value="30" />
</jsp:include>

<p>내용2</p>

</body>
</html>
~~~

<br>
sub.jsp

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>

<p>
이름: <%= request.getParameter("name") %> <br>
나이: <%= request.getParameter("age") %>
</p>
~~~

<br>
main.jsp 출력결과

~~~ text
내용1

이름: John
나이: 30

내용2
~~~

<br>

## 3. <jsp:include> 태그가 다른 페이지를 포함하는 방식 ##
----

+ 실행 흐름 제어
	+ \<jsp:include\> 태그를 만나면 요청 흐름이 포함된 페이지로 이동
	+ 포함된 페이지에서의 실행 흐름이 끝나면 출력버퍼에 내용을 저장 후 원래 페이지로 실행 흐름이 돌아온다.
	