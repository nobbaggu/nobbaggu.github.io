---
title: (JSP) 13(2) - EL 연산자
date: 2020-07-19T16:00:00+09:00
author: nobbaggu
layout: post
categories: JSP
tags:
  - JSP
  - EL
  - 표현언어
  - 연산자
---

## 1. 수치 연산자 ##
----
+ \+: 덧셈
+ \-: 삘셈
+ \*: 곱셈
+ / 또는 div: 나눗셈
+ % 또는 mod: 나머지

<br>

+ 문자열이 숫자로 변환 가능하면 자동으로 변환해서 연산한다.
	+ 변환 불가능하면 exception(예외)를 던진다.
	+ 문자열이 null이면 0으로 취급한다.
	
~~~ jsp
${5/2}
${5mod3}
${"10"+20}
~~~

<br>

## 2. 비교 연산자 ##
----

+ == 또는 eq
+ != 또는 ne
+ < 또는 lt
+ > 또는 gt
+ <= 또는 le
+ >= 또는 ge

+ 문자열 비교의 경우 `String.compareTo()` 메소드 사용

~~~ jsp
<%--아래 두 식은 같은 의미를 가진다.--%>
${str=="2004"}
str.compareTo("2004")==0
~~~

<br>

## 3. 논리 연산자 ##
----

+ && 또는 and
+ || 또는 or
+ ! 또는 not
	
<br>

+ empty 연산자
	+ 검사할 객체가 빈 객체인지 검사한다.
	+ `empty 객체`
	+ null이거나 원소가 없는 빈 Collection인 경우 true 리턴
	+ 나머지 경우에는 false 리턴
	
<br>

## 4. 비교 선택 연산자 ##
----

+ `수식 ? 값1 : 값2`
+ 수식이 true면 값1, false면 값2 리턴

<br>

## 5. 문자열 연결 ##
----

+ `+=` 연산자
+ ex) ${"str" += "ing"} 결과는 string
	
<br>

## 5. 컬렉션 ##
----

+ List
	+ ${\[1,2,5,10\]}
	+ <c:set var="myList" values="${\[1,2,5,10\]}" />
	+ ${muList\[2\]} -> 5
+ Map
	+ ${{"name":"홍길동", "age":20}}
	+ <c:set var="myMap" values="${{"name":"홍길동", "age":20}}" />
	+ ${mem.name} -> 홍길동
	+ ${mem.age} -> 20
+ Set
	+ <c:set var="mySet" values="${{"가", "나", "다"}}" />
	+ ${mySet}
		
<br>
Map, List 혼합 예제

~~~ jsp
<c:set var="codes" values="${[{code:'001', label:'1번'}, {code:'002', label:'2번'}]}" />
${codes[0].label}
~~~

<br>

## 6. 세미콜론 연산자 ##
----

+ 세미콜론(;) 앞에있는 결과를 출력하지 않는다.
	
~~~ jsp
${1+2; 3+4}
<%--7만 출력한다.--%>
~~~

<br>

## 7. 할당 연산자 ##
----

+ `${var1=10}`
+ 주의할점은 할당 연산자 자체도 출력해버린다는 것이다.
	+ 위에서 var1에 10을 할당하면서 10을 출력한다.
	+ 이 때는 세미콜론 연산자로 무시하면 된다.
		
~~~ jsp
${var1=10;} <%--출력 무시--%>
${var1}
~~~
