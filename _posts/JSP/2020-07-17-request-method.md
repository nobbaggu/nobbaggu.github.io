---
title: (JSP) 3 - GET방식 POST방식 요청의 차이
date: 2020-07-17T19:37:19+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - GET
  - POST
---

+ JSP의 \<form\> 태그에서 요청 방식 지정
	+ 지정하지 않으면 default로 GET방식 사용

~~~ html
<form action="jsp페이지 경로" method="get">
	...
</form>
~~~

~~~ html
<form action="jsp페이지 경로" method="post">
	...
</form>
~~~

<br>

## 1. GET ##
----

+ URL 경로 뒤에 파라미터를 붙여서 전송
	+ http://localhost:8080/abc/asdf.jsp?age=23&gender=woman&address=seoul
	+ 물음표(?) 뒤에 파라미터를 붙여서 전송
	+ 파라미터 사이를 & 기호로 구분
	+ 파라미터이름=파라미터값

+ 파라미터 길이에 제한이 있다.
	+ 파라미터가 길어지면 URL도 길어진다.
	+ 길이 제한은 웹 브라우저나 서버에 따라 다르다.
	
+ Idempotent(멱등)의 성질을 가진다.
	+ 요청을 여러번 수행하더라도 항상 같은 결과 보장
	+ 따라서 주로 '조회' 기능에 사용된다.
	
+ 인코딩 규칙
	+ \<a\>태그 : 웹 페이지와 같은 인코딩 사용
	+ \<form\>태그 : 웹 페이지와 같은 인코딩 사용
	+ URL에 쿼리 문자열 붙여서 전송 : 웹 브라우저마다 다름

+ 디코딩 처리
	+ WAS마다 사용하는 기본 캐릭터셋이 다르다.
	+ ex) 톰캣8은 utf-8, 톰캣7은 ISO-8859-1 사용
	
+ HTTP 헤더 예시

~~~ text
GET /home.html HTTP/1.1
Host: localhost:8080/example_page.jsp?name=steve&message=hello
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/ *;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
...
~~~

<br>

## 2. POST ##
----

+ 요청 헤더의 데이터 영역에 파라미터를 넣어서 전송
	+ URL 뒤에 파라미터 정보가 붙지 않는다.
	
+ GET방식과 달리 파라미터 길이에 제한이 없다.

+ Non-Idempotent의 성질을 가진다.
	+ 서버에 여러번 요청함에따라 다른 응답을 가질 수 있다.
	+ 따라서 서버의 상태나 데이터를 변경시킬 때 사용한다.
	+ ex) 블로그등에 글을 게시할 때 글의 내용을 파라미터로 보내기
	
+ 인코딩 규칙
	+ POST 방식으로 요청할 때 웹 브라우저는 응답화면에 지정된 캐릭터셋을 사용해서 파라미터를 인코딩한다.

+ 디코딩 처리
	+ `request.setCharacterEncoding()`는 POST의 데이터 영역에 대한 디코딩 방식을 설정하는 메소드이다.
		+ 따라서 GET 방식으로 가져온 파라미터에는 적용되지 않는다.(파라미터가 URL에 붙어서 오기때문)
	+ 응답 페이지에서 `request` 객체로 파라미터 값을 읽어 디코딩 하기전에 캐릭터셋(해당 페이지의 contentType에서 지정한 charset)을 지정해주어야 한다.
	
+ HTTP 헤더 예시

~~~ text
POST /myform.html HTTP/1.1
Host: localhost:8080/example_page.jsp
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Content-Length: 128
...
name=steve&message=hello
~~~

+ 위 예시에서 요청 헤더의 데이터 영역에 파라미터가 있다.