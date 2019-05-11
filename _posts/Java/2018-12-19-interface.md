---
title: 인터페이스
date: 2018-12-19T21:32:44+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - implements
  - interface
  - Java
  - 인터페이스
  - 자바
---
인터페이스는 멤버 변수로는 상수(final 변수), 메소드는 추상 메소드만을 포함한다. 따라서 당연히 인터페이스 자체만으로는 인스턴스를 만들어 쓸 수 없다. 그렇다면 자체적으로 아무런 기능도 하지 못하는 인터페이스는 왜 쓸까?

&nbsp;

# 1. 인터페이스 만들기

* * *

~~~ java
package interfaceprac;

public interface Calc {
   
   //final로 선언하지 않아도 컴파일 과정에서 상수로 변환
   double PI = 3.14;
   int ERROR = -999999999;
   
   //abstract로 선언하지 않아도 컴파일 과정에서 추상메소드로 변환
   int add(int num1, int num2);
   int subtract(int num1, int num2);
   int multiply(int num1, int num2);
   int divide(int num1, int num2);
}
~~~

인터페이스에 구현된 변수는 모두 상수로, 메소드는 추상 메소드로 컴파일 과정에서 변환이 된다.

&nbsp;

&nbsp;

# 2. 클래스에서 인터페이스 구현

* * *

추상 클래스에서는 객체를 만들기 위해서 추상클래스를 상속받는(extends) 클래스를 만든 후 메소드 코드를 작성하여 객체를 만들어 사용하는 것과 같은 과정이 필요하다.

인터페이스는 이 과정을 조금 다르게 표현한다. 상속이 아니라 구현한다(implements)고 한다. 인터페이스를 구현하는 클래스를 만드는 것이다.

~~~ java
package interfaceprac;

public class MyCalc implements Calc{

   @Override
   public int add(int num1, int num2) {
      return num1+num2;
   }

   @Override
   public int subtract(int num1, int num2) {
      return num1-num2;
   }
   
}
~~~

Calc 인터페이스를 구현한 MyCalc 클래스이다.

&nbsp;

&nbsp;

# 3. 인터페이스를 구현한 클래스의 성질

* * *

**1) 인터페이스 자료형으로 형 변환(upcasting) 가능**

**2) 인터페이스 자료형의 참조변수에 저장되었을 시 클래스에 추가적으로 구현한 메소드는 사용 불가능**

**3) 인터페이스를 구현한 클래스 객체가 호출하는 메소드는 클래스에서 구현된 메소드(인터페이스에서는 구현된 메소드가 없으므로 당연한 이야기지만...)**

&nbsp;

이는 상위 클래스를 상속받은 하위 클래스, 혹은 추상 클래스를 상속받은 하위 클래스와 완전 동일하다.

일단 **인터페이스의 형 변환이 가능한 점과 인터페이스를 구현한 여러개의 클래스마다 자신의 메소드를 호출한다는 점은 다형성이 가능케 하는 부분이다.**

&nbsp;