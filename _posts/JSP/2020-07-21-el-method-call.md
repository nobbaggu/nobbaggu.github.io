---
title: (JSP) 15 - EL 메소드 호출
date: 2020-07-21T20:00:00+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - JSP
  - EL
  - 표현언어
  - 연산자
---

## 1. EL 객체 메소드 호출 ##
----

+ 클래스 작성
	+ 섭씨로 온도를 설정하고 섭씨나 화씨로 온도를 읽어들이는 기능을 가진 클래스

~~~ java
package thermometer;

import java.util.HashMap;
import java.util.Map;

public class Thermometer {
	private Map<String, Double> locationCelsiusMap = new HashMap<>();
	
	public void setCelsius(String location, Double value) {
		locationCelsiusMap.put(location, value);
	}
	
	public Double getCelsius(String location) {
		return locationCelsiusMap.get(location);
	}
	
	public Double getFahrenheit(String location) {
		Double celsius = getCelsius(location);
		if(celsius == null) {
			return null;
		}
		return celsius*1.8+32.0;
	}
	
	public String getInfo() {
		return "온도계 변환기 1.1";
	}
}
~~~

<br>

+ 컴파일 후 아래 JSP 코드와 같이 import하여 사용한다.

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ page import="thermometer.Thermometer" %>

<%
	Thermometer thermometer = new Thermometer();
	request.setAttribute("t", thermometer);
%>

<!DOCTYPE html>
<html>
<head>
	<title>온도계 변화</title>
</head>
<body>
	<p>${t.setCelsius("서울", 27.3)}</p>
	<p>${t.getCelsius("서울")}도 / ${t.getFahrenheit("서울")}화</p>
</body>
</html>
~~~

<br>

+ `request` 기본객체에 t라는 이름으로 객체를 추가한다.

+ EL에서 속성의 이름을 통해 객체의 메소드를 호출할 수 있다.
	+ `requestScope.t.메소드()`와 같이 객체 영역을 명시하지 않아도 된다.
	+ 객체 이름을 명시하지 않으면 page, request, session, application 4개의 기본객체를 탐색한다.
	
<br>

## 2. EL에서 정적 메소드 호출하기1 ##
----

+ 이 방법은 설정할 것이 많고 더럽고 짜증난다.
	+ 2번째 방법이 간편하고 실제로 주로 사용하는 방법이다.
	+ 이 방법은 알고만 있자.

+ 정적 메소드 정의하기
	+ 아래 Java 클래스 작성 후 classes에 컴파일

~~~ java
package util;

import java.text.DecimalFormat;

public class FormatUtil {
	public static String number(long number, String pattern) {
		DecimalFormat format = new DecimalFormat(pattern);
		return format.format(number);
	}
}
~~~

<br>

+ TLD 파일
	+ Tag Library Descriptor의 약자
	+ 태그 라이브러리에 대한 설정 정보를 담고 있다.
	+ 확장자는 .tld
	
+ 첫 번째로 TLD 파일에 \<function\> 태그를 사용해서 사용할 함수와 함수가 포함된 클래스를 등록한다.
	+ \<name\> 태그는 EL에서 사용할 함수의 이름이다.
	+ \<function-class\> 태그는 함수가 정의된 클래스 이름이다. WEB-INF/classes 하위 폴더에서 찾는다.
	+ \<function-signature\> 태그는 함수가 실행될 메소드를 지정해준다.

~~~ xml
<?xml version="1.0" encoding="utf-8" ?>

<taglib xmlns="http://java.sun.com/xml/ns/javaee" 
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
		http://java.sun.com/xml/ns/javaee/web-jsptaglibrary_2_1.xsd"
	version="2.1">

	<description>EL에서 함수실행</description>
	<tlib-version>1.0</tlib-version>
	<short-name>ELfunctions</short-name>

	<function>
		<description>숫자 포맷팅</description>
		<name>formatNumber</name>
		<function-class>util.FormatUtil</function-class>
		<function-signature>
			java.lang.String number(long, java.lang.String)
		</function-signature>
	</function>

</taglib>
~~~

<br>

+ 두 번째로 web.xml에 TLD 파일을 등록해준다.

~~~ xml
...
<jsp-config>
	...
	<taglib>
		<taglib-uri>
			/WEB-INF/tlds/el-functions.tld
		</taglib-uri>
		<taglib-location>
			/WEB-INF/tlds/el-functions.tld
		</taglib-location>
	</taglib>
	...
</jsp-config>
...
~~~

+ \<taglib-uri\>
	+ JSP에서 TLD 파일을 참조할 때 사용할 일종의 식별자

+ \<taglib-location\>
	+ 위 식별자가 가르키는 TLD 파일의 실제 위치(여기서는 같게 사용했다.)
	
<br>

+ 세 번째로 JSP에서 아래와 같이 태그 라이브러리를 사용할 수 있다.

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ taglib prefix="elfunc" uri="WEB-INF/tlds/el-functions.tld" %>

<%
	request.setAttribute("price", 12345);
%>

<!DOCTYPE html>
<html>
<head>
	<title>EL함수 호출</title>
</head>
<body>
	<p>
		오늘은 <b>${elfunc:formatNumber(price, "#,##0")}</b> 입니다.
	</p>
</body>
</html>
~~~

+ \<taglib\>
	+ uri에 해당하는 TLD 파일을 web.xml에서 매칭해보고 해당 파일의 실제 위치를 찾는다.
	+ 해당 파일의 위치를 알기 때문에 TLD에 등록된 함수들을 사용할 수 있다.
	+ prefix는 EL에서 간편하게 참조하기 위한 접두어이다.
	
+ 함수를 호출할 때 TLD에 등록한 이름을 사용한다.

<br>

출력결과

~~~ text
오늘은 12,345 입니다.
~~~

<br>

## 3. EL에서 정적 메소드 호출하기2 ##
----

+ 그냥 일반 클래스처럼 import 후 사용할 수 있다.

~~~ jsp
<%@ page contentType="text/html; charset=utf-8" %>
<%@ page import="util.FormatUtil" %>

<%
	request.setAttribute("price", 12345);
%>

<!DOCTYPE html>
<html>
<head>
	<title>EL 함수 호출 2</title>
</head>
<body>
	<p>
		오늘은 <b>${FormatUtil.number(price, "#,##0")}</b> 입니다.
	</p>
</body>
</html>
~~~
