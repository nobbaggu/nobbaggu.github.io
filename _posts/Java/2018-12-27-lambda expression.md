---
title: 람다식
date: 2018-12-27T23:30:50+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - Java
  - 람다식
  - 자바
---
자바는 어떤 기능을 구현하기 위해 메소드를 정의하여 사용한다. 그런데 이를 위해서는 클래스가 먼저 있어야하고 클래스를 기반으로 객체를 만들어서 메소드를 호출해야만한다. C언어처럼 함수의 구현과 호출만으로는 기능을 구현할 수 없다.

그런데 자바8부터는 함수형 프로그래밍 방식인 '람다식'을 지원한다.

&nbsp;

# 1. 람다식 표현방법

* * *

일반적인 함수의 정의를 람다식으로 바꾸면 다음과 같이 쓸 수 있다.

~~~ java
//C언어에서의 함수 선언
int add(int x, int y) {                            //자바 람다식 표현
   return x+y;                            →       (int x, int y) -> {return x+y};
}
~~~

&nbsp;

람다식은 (매개변수) ->{실행문}; 의 형태로 정의하여 사용한다.

&nbsp;

&nbsp;

# 2. 문법

* * *

### 1) 매개변수 자료형 생략 가능

~~~ java
(int x, int y) -> {return x+y};      →      (x, y) -> {return x+y};
~~~

위와 같이 매개변수의 자료형을 생략할 수 있다.

&nbsp;

### 2) 매개변수 하나인 경우 괄호 생략 가능

~~~ java
(x) -> {return 2*x};         →       x -> {return 2*x};
(str) -> {System.out.println(str)}    →    str -> {System.out.println};
~~~

매개변수가 둘 이상인 경우는 괄호 생략 불가능하다.

&nbsp;

### 3) 실행문이 하나인 경우 중괄호 생략 가능

~~~ java
str -> {System.out.println};      →     str -> System.out.println;
<del>x -> {return 2*x};       →     x -> return 2*x;</del> //return문은 중괄호 생략 불가
~~~

실행문이 한 문장이면 중괄호가 생략 가능하다. 그러나 return으로 반환값을 반환해야 하는 경우는 안된다.

&nbsp;

### 4) 코드 구현부가 return문 한개라면 return 예약어 생략 가능

~~~ java
(x, y) -> {return x+y};    →     (x, y) -> x+y
str -> return str.length();    →     str -> str.length()
~~~

코드 구현부가 return문 한개인 경우 return문을 생략하고 식만 쓸 수 있다.

&nbsp;

# 3. 람다식 사용

* * *

~~~ java
//람다식이 구현할 함수를 정의한 인터페이스 정의
public interface MyNumber {
   int sumCalc(int x, int y);
}
~~~

&nbsp;

~~~ java
public class Hello {

   public static void main(String[] args) {
      MyNumber add = (x,y) -> x+y; //메소드를 구현할 람다식을 인터페이스형 참조변수에 저장
      System.out.println(add.sumCalc(10,20)); //참조변수로 메소드 호출
   }

}
~~~

실행결과

30


 

람다식을 구현하기 위해서는 인터페이스를 만들고 추상메소드를 선언해야 한다. 이후 인터페이스형 참조변수에 추상메소드를 구현할 람다식을 저장하고 참조변수로 메소드를 호출하여 람다식을 사용할 수 있다.

람다식을 구현하기 위해서는 인터페이스 안에 메소드가 단 1개만 있어야한다. 2개이상의 메소드가 있으면 람다식이 구현할 메소드가 무엇인지 모호해진다.

람다식을 구현한 메소드에는 특별히 @FunctionalInterface라는 annotation을 사용할 수 있다. 이 애노테이션은 함수형 인터페이스라는 것을 알려주고 실수로 추가적인 메소드를 선언하면 컴파일 과정에서 오류를 잡아내어준다.

~~~ java
<strong>@FunctionalInterface</strong>
public interface MyNumber { //오류 발생
   int sumCalc(int x, int y);
   int minusCalc(int x, int y);
}
~~~

필수는 아니지만 함수형 인터페이스에는 @FunctionalInterface 애노테이션을 사용하여 혹시모를 실수를 방지하는 것이 좋다.

&nbsp;

&nbsp;

# 4. 객체지향 vs 람다식

* * *

람다식을 사용하면 객체지향에서 사용되는 코드보다 적은 양의 코드로 같은 기능을 구현할 수 있는 경우가 있다.

다음 인터페이스를 구현하는 경우를 생각해보자.

~~~ java
@FunctionalInterface
public interface MyNumber {
   int sumCalc(int x, int y);
}
~~~

&nbsp;

### 1) 객체지향으로 구현

~~~ java
//인터페이스 구현 클래스 생성
public class MyNumberImpl implements MyNumber {
   public int sumCalc(int x, int y) {
      return x+y;
   }
}
~~~

&nbsp;

~~~ java
public class TestMyNumber {
   public static void main(String[] args) {
      MyNumberImpl calc = new MyNumberImpl(); //객체 생성
      System.out.println(calc.sumCalc(10, 20));// 메소드 호출
   }
}
~~~

&nbsp;

### 2) 람다식으로 구현

~~~ java
public class MyNumberLambda {
   public static void main(String[] args) {
      MyNumber add = (x,y)->x+y; //인터페이스형 참조변수에 메소드를 구현할 람다식 저장
      System.out.println(add.sumCalc(10, 20)); //참조변수로 메소드 호출
   }
}
~~~

위의 경우는 람다식으로 구현하는 것이 훨씬 간결해진다.

무조건 간결하게 되는 것이 가능한 경우라고해서 람다식을 사용하는 것이 옳은 것은 아니지만 람다식을 적재적소에 잘 사용하면 간결하고 가독성 높은 코드를 작성하는데 도움을 준다.

&nbsp;

# 5. 람다식의 비밀

* * *

객체지향 프로그래밍인 자바가 객체 생성 없이 메스드를 호출할 수 있었던 것 이유가 무엇일까?

사실 객체 생성 없이 호출했던 것이 아니다. 람다식으로 메소드를 구현하여 호출하면 컴퓨터가 알아서 익명 클래스를 만들고 객체를 생성해서 람다식을 실행시켰던 거다.

~~~ java
//익명 클래스 객체 생성
MyNumber add = new MyNumber(){
   @Override
   public int sumCalc(int x, int y){
      return x+y;
   }
};
~~~

&nbsp;

**람다식은 인터페이스형의 참조변수에 저장될 수 있는 걸 알았다. 그렇다면 이를 응용하면 참조변수 자료형으로 매개변수로 전달도 될 것이고 참조변수 자료형의 반환형을 가진 메소드의 반환값으로 사용될 수도 있다.**