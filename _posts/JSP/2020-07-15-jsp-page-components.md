---
title: (JSP) 1 - JSP 페이지의 구성요소
date: 2020-07-15T18:37:19+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - 디렉티브
  - 스크립트 요소
  - 기본 객체
  - jsp
  - 액션태그
  - 스크립트릿
  - 표현식
  - 선언부
  - 페이지
---

## 1. JSP 페이지 구성요소 ##
----
	
+ 디렉티브(directive)
	+ JSP페이지의 설정 정보 지정
	+ `<%@ 디렉티브이름 속성1="값1" 속성2="값2" ... %>`
	+ page, taglib, include 세 가지의 디렉티브가 있다.
		+ page: JSP 페이지에 대한 정보 지정(문서 타입, 출력버퍼 크기, 에러 페이지 등)
		+ taglib: 태그 라이브러리 지정
		+ include: JSP 페이지의 특정 영역에 다른 문서를 포함시킨다.

+ 스크립트 요소
	+ JSP 페이지의 내용을 동적으로 생성하기 위해 사용
		+ 스크립트릿 : 자바 코드 실행
			+ `<% 자바 코드 %>`
			+ `<% Calendar cal = Calendar.getInstance() %>`
		+ 표현식 : 값 출력
			+ `<%= 값 %>`
			+ `<%=cal.get(Calendar.YEAR) %>`년 입니다.
		+ 선언부 : 자바 메서드 생성
		
+ 기본 객체
	+ 웹 애플리케이션을 만드는데 필요한 기본 기능 제공
	+ request, response, session, application, page 등의 기본 객체가 존재
	+ request, response, session 기본 객체를 가장 많이 사용
	
+ 표현 언어
	+ 스크립트 요소로 인해 JSP 코드가 복잡해진다.
	+ 표현 언어를 사용하면 간결하게 JSP 코드 작성 가능
	+ `a*b=${param.a*param.b}`
		+ 스크립트 표현식을 사용하면 훨씬 길어진다.
	
+ 표준 액션 태그
	+ 간단하게 태그를 사용해서 기능을 사용할 수 있게 해준다.
	+ ex) `<jsp:include page="header.jsp" flush="true" />`
		+ jsp 페이지 코드 포함시키기
	+ 커스텀 태그: 개발자가 직접 만든 액션태그
	+ 표준 태그 라이브러리(JSTL): 자주 사용하는 액션 태그 라이브러리

<br>

## 2. page 디렉티브 ##
----

+ page 디렉티브 작성 예시
	+ `<%@ page contentType="text/html; charset=utf-8" %>
	+ `<%@ page import="java.util.Date" %>
	
+ page 디렉티브의 주요 속성들
	+ contentType
		+ JSP 문서의 MIME 타입과 캐릭터 인코딩 지정
	+ import
		+ JSP 페이지에서 사용할 자바 클래스 지정
	+ session
		+ session 사용여부 지정
	+ buffer
		+ 출력버퍼 크기 지정
	+ autoFlash
		+ 버퍼가 다 찼을 경우 자동으로 출력 스트림으로 보낼지 여부 지정
	+ info
		+ JSP 페이지에 대한 설명
	+ errorPage
		+ JSP 페이지 실행도중 에러가 발생할 경우 보여줄 페이지 지정
	+ isErrorPage
		+ 해당 JSP 페이지가 에러때 보여줄 페인지인지 아닌지 여부
	+ pageEncoding
		+ 소스코드의 캐릭터 인코딩 지정
	+ isELIgnored
		+ 표현 언어를 해석할지 단순 문자열로 처리할지 지정
		+ true: 문자열로 처리 / false: 해석
	+ deferredSyntaxAllowedAsLiteral
		+ #{가 단순 문자열로 사용되는 것을 허용할지 여부 지정
	+ trimDirectiveWhitespaces
		+ 출력 결과에서 템플릿 텍스트의 공백문자 제거할지 여부 지정
	
+ 자주 사용하는 속성
	+ **contentType** 속성
		+ type만 지정해줘도되고 charset도 같이 지정할 수 있다.
			+ charset을 지정하지 않으면 기본값으로 ISO-8859-1을 사용한다.(영어와 서유럽 문자만 포함)
			+ ISO-8859-1을 사용할 경우 한글을 포함한 지원하지 않는 문자들은 깨진다.
			+ 한글 및 대부분의 문자를 지원해주는 utf-8이 가장 많이 사용된다.
		
		+ text/html, text/xml, application/json 등등
			+ 지정하지 않을 경우의 기본값은 text/html

	+ **import** 속성
		+ import로 클래스를 불러두면 모든 경로를 입력할 필요 없이 클래스 명만 명시해서 사용할 수 있다.
		+ `<%@ import="java.util.Date" %>`
		+ `<%@ import="java.util.Date, java.util.Date" %>`
		+ `<%@ import="java.util.*" %>
		
	+ **trimDirectiveWhitespaces** 속성
		+ 불필요하게 생성되는 공백문자 제거
		+ ex) 디렉티브 작성 부분은 문서에서 공백으로 나타난다.
		+ `<%@ page trimDirectiveWhitespaces="true" %>`
			+ true로 설정하여 공백문자 제거 설정
			
	+ **pageEncoding** 속성
		+ JSP 파일의 코드를 작성할 캐릭터셋을 명시해준다.
		+ pageEncoding 속성값이 지정되지 않은 경우에는 contentType의 charset 값을 이용해서 소스코드를 해석한다.
		+ 둘 다 없을 경우 ISO-8859-1 캐릭터셋이 적용된다.
		+ contentType의 charset과 pageEncoding 속성의 차이점
			+ contentType의 charset은 http 응답으로 오는 페이지의 인코딩
			+ pageEncoding의 속성값은 작성한 JSP 문서의 인코딩 방식을 명시해 준 것
			+ charset은 utf-8, pageEnCoding 값은 euc-kr 와 같이 두개를 서로 다른 방식으로 설정할 수도 있다.
			
<br>

## 3. 스크립트 요소 ##
----

+ JSP 프로그래밍에서 로직을 수행하기 위한 요소

+ 스크립트릿(scriptlet)
	+ `<% 자바 코드 %>`
	+ 자바 코드를 작성해서 기능을 구현
	
+ 표현식(expression)
	+ 어떤 값을 출력결과에 포함시킬 수 있다.
	+ `<%= 값 %>`
	+ ex1) `<%= 1+2+3 %>`
		+ 리터럴을 그대로 대입
	+ ex2) `<%= sum %>`
		+ sum이라는 Java 변수를 대입
	+ ex3) `<%= multiply(2,3) %>`
		
+ 선언부(declaration)
	+ `<%! 메소드 작성 %>`
		
<br>
