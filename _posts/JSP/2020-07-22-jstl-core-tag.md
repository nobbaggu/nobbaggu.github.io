---
title: (JSP) 17 - JSTL 코어태그
date: 2020-07-22T20:00:00+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - JSP
  - JSTL
  - 코어
  - 태그
---

## 0. JSTL이란? ##
----

+ 커스텀 태그(Custom Tag)
	+ JSP는 새로운 태그를 사용자가 만들어서 추가할 수 있는 기능을 지원한다.

+ JSTL
	+ JSP Standard Tag Library
	+ 논리판단, 반복, 포맷 처리 등 자주 쓰는 기능을 커스텀 태그로 만들어 모아놓은 표준 라이브러리
	
+ JSTL 태그 종류
	+ 코어
		+ 변수지원, 흐름제어, URL 처리
		+ 접두어 c
	+ XML
		+ XML 코어, 흐름제어, XML 변환
		+ 접두어 x
	+ 국제화
		+ 지역, 메시지 형식, 숫자 및 날짜 형식
		+ 접두어 fmt
	+ 데이터베이스
		+ SQL
		+ 접두어 sql
	+ 함수
		+ 컬렉션, String 등 자료 처리
		+ 접두어 fn
	
+ 위에서 코어, 국제화, 함수 태그를 많이 사용한다.

+ JSTL jar 파일을 WEB-INF/lib 디렉토리에 복사해야한다.
	+ http://search.maven.org/#browse%7C-658715035
	
<br>

## 1. 코어 태그 ##
----

+ 코어 태그 사용을 위해서는 taglib 디렉티브를 추가해야한다.

~~~ jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
~~~

### a. 변수 지원 태그 ##
	
+ set(JSP EL 변수 설정)

~~~ jsp
<c:set var="변수명" value="값" scope="영역" />
~~~

+ var : EL 변수 이름
+ value : 변수 값. 표현식, EL, 텍스트를 사용해서 지정
+ scope : 변수를 저장할 영역. page, request, session, application 중 하나. 지정하지 않으면 기본값으로 page 영역에 저장

~~~ jsp
<c:set var="name" value="이순신" />
<c:set var="mem" value="<%= member %>" scope="request" />
<c:set var="memberName" value="${mem.name}" />
~~~

<br>

+ set(프로퍼티 값 설정)
 
~~~ jsp
<c:set target="대상" property="프로퍼티 이름" value="값" />
~~~

+ target : 대상 객체. 표현식이나 EL 변수 대입. 텍스트는 안된다. 자바빈, Map이 대상이어야한다.
+ property : 설정할 프로퍼티. 내부적으로 자바빈의 경우 setProperty 메소드를, Map은 put을 사용해서 설정된다.
+ value : 값

~~~ jsp
<c:set target="${member}" property="name" value="김삿갓" />
<c:set target="<%=myMap%>" property="key" value="value" />
~~~

<br>

+ remove : JSP 변수 제거

~~~ jsp
<c:remove var="변수 이름" />
<c:remove var="변수 이름" scope="request" />
~~~

+ scope는 변수를 삭제할 영역 지정. 명시하지 않으면 모든 영역에서 해당 변수 삭제

<br>
	
### b. 흐름제어 ###

+ if : 조건문

~~~ jsp
<c:if test="조건">
...
</c:if>
~~~

+ test 속성에는 true/false 혹은 true/false를 리턴할 수 있는 수식이 온다.
	+ true면 몸체 실행, false면 노실행
	
~~~ jsp
<c:set var="name" value="이순신" scope="request" />
<c:set var="age" value="30" />

<c:if test="${age > 20}">
	<p>${name}은 20살보다 많다.</p>
</c:if>	
~~~

<br>

+ choose : 다중 조건 처리
	+ Java의 if-else if와 같은 기능
	+ when, otherwise와 같이 사용
	
~~~ jsp
<c:choose>
		<c:when test="${age > 10 && age <=20}">
			<p>${name}은 10살보다 많고 20살보다 작거나같다.</p>
		</c:when>
		<c:when test="${age > 20 && age <= 30}">
			<p>${name}은 20살보다 많고 30살보다 작거나같다.</p>
		</c:when>
		<c:when test="${age > 30 && age <= 40}">
			<p>${name}은 30살보다 많고 40살보다 작거나같다.</p>
		</c:when>
		<c:otherwise>
			<p>${name}은 도대체 몇살이냐</p>
		</c:otherwise>
	</c:choose>
~~~

+ 조건이 맞는 when이 실행되면 이후의 when들과 otherwise는 실행되지 않는다.

+ otherwise 태그는 필수는 아니다.

<br>

+ forEach : 배열, 컬렉션, Map 자료형에 대한 반복처리

~~~ jsp
<%--자료의 각 item에 대해 반복--%>
<c:forEach var="변수" items="자료">
...
</c:forEach>

<%--i가 1부터 10까지 1씩 증가하면서 반복--%>
<c:forEach var="i" begin="1" end="10">
...
</c:forEach>

<%--i가 1부터 10까지 2씩 증가하면서 반복--%>
<c:forEach var="i" begin="1" end="10" step="2">
...
<c:forEach>

<%--자료의 3번 인덱스부터 9번 인덱스까지만--%>
<c:forEach var="i" items="자료" begin="3" end="9">
...
</c:forEach>

<%--varStatus 속성을 사용해 인덱스값을 가져올 수도 있따.--%>
<c:forEach var="item" items="${list}" varStatus="status">
	${status.index + 1}번째 항목 : ${item.name}
</c:forEach>
~~~

+ varStatus 속성은 index 이외에도 다른 속성값들을 가지고 있다.
	+ index : 현재 인덱스
	+ count : 루프 실행 횟수
	+ begin : begin 속성값
	+ end : end 속성값
	+ step : step 속성값
	+ first : 현재 실행이 첫 번째 실행인 경우 true
	+ last : 현재 실행이 마지막 실행인 경우 true
	+ current : 컬렉션 중 현재 루프에서 사용할 객체

~~~ jsp
<c:set var="list" value="${[1,2,3,4,5,6,7,8,9,10]}" />
<c:set var="sum" value="0" />

<c:forEach var="number" items="${list}" begin="0" end="4" step="2">
	${sum = sum + number; ''}
</c:forEach>

<p>첫 번째부터 5번째 원소까지의 합은 ${sum} 입니다.</p>
~~~

~~~ jsp
<c:set var="map" value="<%=new HashMap<>()%>" />

<c:set target="${map}" property="name" value="김삿갓" />
<c:set target="${map}" property="age" value="30" />
<c:set target="${map}" property="addr" value="서울" />

<c:forEach var="entry" items="${map}">
	<p>${entry.key} ${entry.value}</p>
</c:forEach>
~~~

+ forTokens
	+ 문자열을 토크나이즈하여 각 토큰에 대한 반복
	+ 구분자 지정 가능
	
~~~ jsp
<c:forTokens var="token" items="문자열" delims="구분자">
...
</c:forTokens>
~~~

+ forEach 태그와 동일하게 begin, end, step, varStatus 속성을 제공.

~~~ jsp
<c:set var="str" value="봄,여름,가을,겨울" />
<c:forTokens var="token" items="${str}" delims=",">
	<p>${token}</p>
</c:forTokens>

<c:set var="str2" value="자바 C 파이썬 Go Ruby" />
<c:forTokens var="token" items="${str2}" delims=" ">
	<p>${token}</p>
</c:forTokens>
~~~

<br>
	
### c. URL 처리 ###

+ url
	+ URL을 생성해준다.
	
~~~ jsp
<c:url value="URL" var="varName" scope="영역" />

<c:url value=URL" var="varName">
	<c:param name="이름" value="값" />
</c:url>
~~~

+ param 태그로 파라미터를 추가할 수 있다.

+ 절대경로
	+ 완전한 URL

+ 상대경로
	+ 웹 애플리케이션 내에서의 상대경로. "/"로 시작
	+ 현재 페이지에대한 상대경로도 지정 가능
		+ ex) ../view/hello.jsp

~~~ jsp
<%--절대 URL. 파라미터 1개 전달--%>
<c:url value="search.naver.com/search.naver">
	<c:param name="query" value="자바" />
</c:url>

<%--출력하지 않고 google 변수에 저장--%>
<c:url value="www.google.com" var="google" />
구글주소: ${google}

<%--웹 애플리케이션에서의 상대경로--%>
<c:url value="/view/list.jsp" />

<%--현재 페이지에서 상대경로--%>
<c:url value="../view2/list2.jsp" />
~~~

<br>

+ redirect
	+ 지정한 페이지로 리다이렉트 시켜주는 기능을 한다.
	
~~~ jsp
<c:redirect url="URL" context="컨텍스트경로" />

<c:redirect url="URL" context="컨텍스트경로">
	<c:param name="이름" value="값" />
</c:redirect>
~~~

+ url속성 값이 슬래시(/)로 시작할 경우 컨텍스트 경로가 추가된다.
	+ ex) chap01 애플리케이션의 view.jsp 페이지일 경우 url="/view.jsp", context="/chap01"

+ param태그를 통해 파라미터를 전달해줄 수 있다.

+ redirect 태그 이후의 코드는 실행되지 않는다.
