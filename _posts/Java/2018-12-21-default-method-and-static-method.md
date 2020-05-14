---
title: (Java) 37. default메소드와 static메소드
date: 2018-12-21T15:50:13+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - default
  - implements
  - interface
  - method
  - static
  - 디폴트
  - 메소드
  - 인터페이스
  - 정적
---
자바8 이전에는 인터페이스는 상수와 추상메소드 이외의 요소는 가질 수 없었다. 그러나 인터페이스를 구현한 클래스들에 공통적으로 사용할 수 있는 메소드가 필요한 경우 혹은 객체를 만들지 않고도 호출 간으한 메소드가 필요한 경우가 있다.

이러한 경우 편의성을 제공하기 위해 Java8부터는 디폴트 메소드와 정적 메소드 기능을 추가하게 된다.

&nbsp;

# 1. default 메소드

* * *

디폴트 메소드는 인터페이스에서 코드를 구현한 메소드이다. 추상 메소드만 가질 수 있던 자바8 이전에는 인터페이스를 구현한 여러 클래스에서 공통된 기능의 메소드가 필요하면 각 클래스마다 같은 코드를 여러 번 구현해야했다. 그러나 디폴트 메소드는 인터페이스를 구현한 모든 클래스가 상속받은 것과 같이 기본적으로 사용할 수 있는 메소드이다.

~~~ java
package interfaceprac;

public interface Calc {
   
   double PI = 3.14;
   int ERROR = -999999999;
   
   default int add(int num1, int num2) {
      return num1+num2;
   }
   
   default int subtract(int num1, int num2) {
      return num1-num2;
   }
   
   int mul(int num1, int num2);
   double div(int num1, int num2);
}
~~~

add(), subtract() 두 메소드는 default 예약어를 통해 선언되었다. 인터페이스의 메소드이지만 코드가 구현되어 있다. 그리고 Calc 인터페이스를 구현한 모든 클래스에서는 add(), subtract() 메소드를 기본적으로 사용할 수 있다.

~~~ java
package interfaceprac;

public class MyCalc implements Calc{

   @Override
   public int mul(int num1, int num2) {
      return num1*num2;
   }

   @Override
   public double div(int num1, int num2) {
      return (num2==0) ? ERROR : (double)num1/num2;
   }
   
}
~~~

인터페이스의 추상 메소드 mul()과 div()의 코드를 정의 하였다.

~~~ java
package interfaceprac;

public class CalcTest {
   public static void main(String[] args) {
      MyCalc cal = new MyCalc();
      System.out.println(cal.add(5, 2));
      System.out.println(cal.subtract(5, 2));
      System.out.println(cal.mul(5, 2));
      System.out.println(cal.div(5, 2));
   }
}
~~~

mul(), div()와 같이 인터페이스를 구현한 클래스에서 만든 메소드뿐만 아니라 인터페이스에서 만든 default 메소드 역시 사용 가능하다.

추가적으로, **디폴트 메소드는 인터페이스를 구현한 클래스에서 재정의(오버라이딩) 가능하다.**

&nbsp;

&nbsp;

# 2. static 메소드

* * *

정적 메소드는 static 선언을 하여 만든 메소드이다. 클래스 내에서 static 변수와 static 메소드는 인스턴스의 생성 여부와 상관 없이 사용 가능 하였다.

인터페이스에서도 같다. 인스턴스를 만들지 않고도 호출 가능한 메소드의 사용을 위해 Java8부터 인터페이스에서도 static 메소드 기능을 지원하는 것이다.

~~~ java
package interfaceprac;

public interface Calc {
   
   double PI = 3.14;
   int ERROR = -999999999;
   
   static int add(int num1, int num2) {
      return num1+num2;
   }
}
~~~

~~~ java
package interfaceprac;

public class CalcTest {
   public static void main(String[] args) {
      System.out.println(Calc.add(1, 2));
   }
}
~~~

&nbsp;