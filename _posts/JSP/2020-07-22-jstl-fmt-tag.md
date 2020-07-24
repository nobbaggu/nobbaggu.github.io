---
title: (JSP) 18 - JSTL 국제화 태그
date: 2020-07-23T20:00:00+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - JSP
  - JSTL
  - 국제화
  - 태그
---

+ 국제화 태그는 크게 아래의 세 가지 기능으로 분류된다.
	+ 로케일 지정
	+ 메시지 처리
	+ 숫자 및 날짜 포맷팅
	
+ 하나하나 알아보자.

<br>

## 1. 로케일 지정 태그 ##
----

+ 국제화 태그 사용을 위해 JSP 페이지에서 taglib 디렉티브를 추가해주어야한다.

~~~ jsp
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
~~~

+ 접두어 fmt로 시작한다.

<br>

+ \<fmt:setLocale\>
	+ 국제화 태그들이 사용할 로케일을 지정한다.
	
+ HTTP 요청 내용에는 ACCEPT-LANGUAGE라는 헤더 정보가 있다.
	+ 이 헤더는 클라이언트가 어떤 언어를 이해할 수 있는지 나타낸다.
	+ JSTL의 국제화 태그들은 이 헤더의 값을 이용해서 언어별로 알맞은 처리를 한다.
		+ ex) 인사말을 여러나라 언어로 저장해둔다음 ACCEPT-LANGUAGE 헤더 값을 보고 알맞은 나라의 인사말을 출력해준다.
		
+ setLocale 태그는 ACCEPT-LANGUAGE 값을 무시하고 직접 로케일을 지정할 때 사용한다.

+ 사용 예시

~~~ jsp
<fmt:setLocale value="ko" scope="request" />
<fmt:setLocale value="ko_kr" />
<fmt:setLocale value="en_us" scope="request" />
~~~

+ value
	+ 로케일을 '언어코드_국가코드' 형식으로 지정
	+ 언어코드는 필수며 국가코드는 필수가 아니다.

+ scope
	+ 지정한 로케일이 영향을 미치는 범위
	
+ 일반적으로 웹 브라우저가 전송한 ACCEPT-LANGUAGE HTTP 헤더 정보를 사용하기 때문에 setLocale 태그는 거의 사용되지 않는다.

<br>

+ \<fmt:requestEncoding\>
	+ 요청 파라미터의 캐릭터 인코딩 지정
	+ `request.setCharacterEncoding()` 과 동일하다.
	
<br>

## 2. 메시지 처리 태그 ##
----

+ 리소스 번들
	+ 로케일별로 다른 문자를 출력해주기 위한 리소스 집합
	+ 메시지 처리 태그에서 사용
	+ WEB-INF/classes에 작성해야한다.

<br>

+ 리소스 번들 만들기
	
webapps/mywebapplication/XXX/WEB-INF/classes/resource/message.properties

~~~ text
TITLE = Learn JSP 2.3
GREETING = Hi! I am KSK
VISITOR = Your ID is {0}
~~~

webapps/mywebapplication/XXX/WEB-INF/classes/resource/message_ko.properties

~~~ text
TITLE = JSP 2.3을 배워보자
GREETING = 안녕하세요 김삿갓입니다
VISITOR = 당신의 ID는 {0} 입니다
~~~

<br>

+ 메시지 처리태그
	+ \<fmt:bundle\>
		+ 태그 몸체에서 사용할 리소스 번들 지정
	+ \<fmt:message\>
		+ 메시지 출력
	+ \<fmt:setBundle\>
		+ 특정 메시지 번들을 사용할 수 있도록 로딩

~~~ jsp
<fmt:setLocale value="en" />

<fmt:bundle basename="resource.message">
	<fmt:message key="GREETING" />
</fmt:bundle>

<fmt:bundle basename="resource.message">
	<fmt:message key="VISITOR">
		<fmt:param value="3" />
	</fmt:message>
</fmt:bundle>
~~~

+ 여긴 한국이므로 HTTP의 ACCEPT_LANGUAGE 헤더값은 ko이다.
	+ setLocale 태그로 로케일을 영미권으로 지정했다.
	+ 리소스 번들의 GREETING 값을 읽어오면 로케일로 지정된 값과 매칭되는 리소스파일에서 읽어온다.
	+ \{0\}과 같은 기호는 파라미터를 받을 수 있는 자리이다. \{1\}, \{2\} 등 여러개 있으면 파라미터가 순서대로 들어온다.
	
<br>

## 3. 숫자 처리 태그 ##
----

+ \<fmt:formatNumber\>
	+ 숫자를 양식에 맞춰 출력
	
+ 속성들
	+ value : 숫자 지정
	+ type : 양식 지정. number, percent, currency. 기본값은 number
	+ pattern : 출력 패턴 지정
	+ currencyCode : type이 currency일 때 통화코드 지정. "원"을 넣으면 KRW
	+ currentSymbol : 통화를 표현할 때 사용할 기호 표시
	+ groupingUsed : 콤마(,)와 같이 단위 구분 사용 여부 지정(true/false). 기본값은 true
	+ var : 결과를 저장할 변수명 지정. 없으면 바로 출력
	+ scope : 변수를 저장할 영역을 지정. 기본값은 page
	
~~~ jsp
<c:set var="price" value="10000" />

숫자: <fmt:formatNumber value="${price}" type="number" />
통화: <fmt:formatNumber value="${price}" type="currency" currencyCode="KRW" />
패턴: <fmt:formatNumber value="${price}" pattern="00000000.00" />
~~~

<br>

+ \<fmt:parseNumber\>
	+ 문자를 숫자로 변환
	
+ 속성들
	+ value : 파싱할 문자열
	+ type : 양식 지정. number, currency, percentage
	+ pattern : 출력 패턴 지정
	+ parseLocale : 로케일 지정
	+ integerOnly : 정수만 파싱할지 여부 지정. 기본값은 false
	+ var : 결과를 저장할 변수명 지정. 없으면 바로 출력
	+ scope : 변수를 저장할 영역. 기본값은 page
	
~~~ jsp
<fmt:parseNumber value="1,100.12" pattern="0,000.00" var="num" />
${num}
~~~

<br>

## 4. 날짜/시간 처리 태그 ##
----

+ \<fmt:formatDate\>
	+ 날짜 정보를 담고있는 객체를 포맷팅하여 출력할 때 사용
	
+ 속성들
	+ value : java.util.Date 타입 객체 지정.
	+ type : time(시간만), date(날짜만), both(둘 다) 포맷팅 할지 지정
	+ dateStyle : 날짜 포맷 지정(default, short, medium, long, full)
	+ timeStyle : 시간 포맷 지정(default, short, medium, long, full)
	+ pattern : 양식 지정. java.text.DateFormat에 있는 양식
	+ timeZone : 시간대 지정. String 또는 java.util.TimeZone 객체 지정.
	+ var : 파싱 결과 저장할 변수명
	+ scope : 변수 저장할 영역. 기본값은 page
	
~~~ jsp
<c:set var="now" value="<%= new java.util.Date() %>" />

<fmt:formatDate value="${now}" type="date" dateStyle="full" />
<fmt:formatDate value="${now}" type="both" dateStyle="full" timeStyle="short" />
~~~

<br>

+ \<fmt:parseDate\>
	+ 문자열로 표시된 날짜와 시간 값을 java.util.Date로 파싱해 포맷팅하여 출력
	
+ 속성들
	+ formatDate 태그와 같으나 parseLocale(로케일 지정) 속성만 추가
	
~~~ jsp
<fmt:parseDate value="2020-03-11 22:13:45" pattern="yyyy-MM-dd HH:mm:ss" />
~~~

<br>

+ \<fmt:timeZone\>
	+ 날짜를 시간대별로 처리해준다.
	+ formatDate, parseDate와 묶어서 사용
	
~~~ jsp
<fmt:formatDate value="<%=new java.util.Date() %>" type = "both" dateStyle="medium" timeStyle="full" />

<%--홍콩 시간대 설정 --%>
<fmt:timeZone value="Hongkong">
	<fmt:formatDate value="<%=new java.util.Date() %>" type = "both" dateStyle="medium" timeStyle="full" />
</fmt:timeZone>
~~~

출력결과

~~~ text
2020. 7. 23 오후 10시 22분 42초 KST
2020. 7. 23 오후 9시 22분 42초 HKT
~~~

<br>

+ \<fmt:setTimeZone\>
	+ 날짜를 시간대별로 처리해준다.
	+ timeZone 태그와는 다르게 페이지 전체에 적용된다.
	
~~~ jsp
<fmt:setTimeZone value="Hongkong" />

<fmt:formatDate value="<%=new java.util.Date() %>" type = "both" dateStyle="medium" timeStyle="full" />
~~~
