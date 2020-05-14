---
title: (Java) 47. 내부 클래스
date: 2018-12-27T21:28:08+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - class
  - inner
  - Java
  - local
  - static
  - 내부
  - 익명
  - 인스턴스
  - 자바
  - 정적
  - 지역
  - 클래스
---
내부 클래스는 클래스 안에 있는 클래스이다. 중첩 클래스라고도 부른다.

변수에는 인스턴스 변수(멤버 변수), static 변수, 지역 변수(메소드 내부 변수)가 있다. 이와 같이 내부 클래스에도 인스턴스 내부 클래스, static 내부 클래스, 지역 내부 클래스 등이 있다. 여기에 추가적으로 익명 클래스도 있다.

&nbsp;

# 1. 인스턴스 내부클래스

* * *

인스턴스 변수의 선언 위치와 같은 곳에 선언한다.

인스턴스 내부 클래스는 이 클래스를 감싸고 있는 클래스에서만 사용하기 위해 만드는 것이 보통이다. 따라서 감싸고 있는 클래스 이외의 다른 클래스에서의 사용을 막기 위해서 내부 클래스를 private로 선언하는 것이 일반적이다.

또한 인스턴스 내부클래스는 외부클래스 생성 후 사용할 수 있다.

~~~ java
package innerclass;

class OutClass {
   private int num = 10;
   private static int sNum = 20;
   
   private InClass inClass; //내부 클래스 참조 변수 선언만 해놓은 후
   
   public OutClass() {
      inClass = new InClass(); //외부 클래스 생성자 만들어진 이후 생성
   }
   
   class InClass{
      int inNum = 100;
      //static int sInNum = 200; //외부클래스 객체가 먼저 만들어져야 InClass객체가 만들어질 수 있으므로 static변수는 불가능
      
      void inTest() {
         System.out.println("OutClass num = "+num+ "(instance variable of outer class"); //외부 클래스 인스턴스 변수 사용 가능
         System.out.println("OutClass sNum = "+sNum+ "(static variable of outer class"); //외부 클래스 static 변수 사용 가능
      }
   }
   
   //외부 클래스에서는 내부클래스의 메소드 호출 가능
   public void usingClass() {
      inClass.inTest();
   }
   
}
~~~

&nbsp;

~~~ java
package innerclass;

public class InnerTest {

   public static void main(String[] args) {
      
      OutClass outClass = new OutClass();
      System.out.println("Call inner class method using outter class");  
      outClass.usingClass();
      
   }

}
~~~

실행결과

Call inner class method using outter class


OutClass num = 10(instance variable of outer class


OutClass sNum = 20(static variable of outer class


 

실행결과를 보면 알 수 있듯 내부 클래스 내부에서는 외부 클래스의 인스턴스 변수와 정적 변수 모두 사용할 수 있다.

&nbsp;

&nbsp;

# 2. static 내부클래스

* * *

인스턴스 내부클래스는 외부클래스 생성 이전에 사용할 수 없고 static변수의 선언도 불가능했다.

외부클래스 생성 이전에도 사용이 필요한 경우 static 내부클래스를 사용하면 된다.

~~~ java
package innerclass;

class OutClass {
   private int num = 10;
   private static int sNum = 20;
   
   static class InStaticClass{
      int inNum = 100;
      static int sInNum = 200;
      
      void inTest() {
         System.out.println("InStaticClass inNum = "+inNum+"(instance variable of inner class)");
         System.out.println("InStaticClass sInNum = "+sInNum+"(static variable of inner class)");
         System.out.println("OutClass sNum = "+sNum+"(static variable of outer class)");
         
         //내부클래스 객체가 외부클래스 객체와 num보다 빨리 생성 가능하므로 외부 인스턴스 변수 사용 불가능
         //System.out.println("OutClass sNum = "+num+"(instance variable of outer class)");
      }
      
      static void sTest() {
         //static 메소드 이므로 외부클래스, 내부클래스 static변수 사용 가능
         System.out.println("InStaticClass sInNum = "+sInNum+"(static variable of inner class)");
         System.out.println("OutClass sNum = "+sNum+"(static variable of outer class)");
      }
      
   }
   
}
~~~

&nbsp;

~~~ java
package innerclass;

public class InnerTest {

   public static void main(String[] args) {
      
      //외부클래스 객체 생성 없이 static 내부클래스 객체 생성
      OutClass.InStaticClass sInClass = new OutClass.InStaticClass();
      
      System.out.println("=====Call method of InStaticClass=====");
      sInClass.inTest();
      
      System.out.println();
      
      System.out.println("=====Call static method of InStaticClass=====");
      //클래스 이름으로 static메소드 사용 가능
      OutClass.InStaticClass.sTest();
      
   }

}
~~~

실행결과

=====Call method of InStaticClass=====


InStaticClass inNum = 100(instance variable of inner class)


InStaticClass sInNum = 200(static variable of inner class)


OutClass sNum = 20(static variable of outer class)




=====Call static method of InStaticClass=====


InStaticClass sInNum = 200(static variable of inner class)


OutClass sNum = 20(static variable of outer class)

static 내부클래스의 일반 메소드는 외부 클래스의 일반 변수를 사용할 수 없다. 왜냐하면 static 내부클래스의 인스턴스가 만들어지면 일반 메소드를 사용할 수 있는데 이 시기가 외부클래스의 생성, 그에 따른 외부 클래스의 멤버변수의 생성 시기보다 빠를 수 있기 때문이다.

당연히 static 내부클래스의 static메소드는 클래스 생성 이전부터 메모리에 로드되므로 외부, 내부클래스에 상관없이 오직 static변수만 사용할 수 있다.

&nbsp;

&nbsp;

# 3. 지역 내부클래스

* * *

지역변수는 함수(메소드)내에서만 정의하여 사용하는 변수이다. 지역 내부클래스는 이름대로 메소드 내부에 선언한다. 당연히 메소드 내부에서만 사용 가능하다.

~~~ java
package innerclass;

import java.lang.Runnable;

public class Outer {
   int outNum = 10;
   static int sNum = 20;
   
   Runnable getRunnable(int i) {
      int num = 100;
      
      //getRunnable 메소드 내부에 Runnable 인터페이스를 구현하는 MyRunnable 클래스 정의
      class MyRunnable implements Runnable{
         int localNum = 10;
         
         @Override
         public void run() {
            //num = 200; //오류
            //i = 20;  //오류
            System.out.println("i = " +i);
            System.out.println("num = "+num);
            System.out.println("localNum = "+localNum);
            System.out.println("outNum= "+outNum);
            System.out.println("Outer.sNum ="+Outer.sNum);
         }
      }
      
      return new MyRunnable(); //getRunnable 메소드의 반환값(객체)
   }
}
~~~

&nbsp;

~~~ java
package innerclass;

public class LocalInnerTest {
   public static void main(String[] args) {
      Outer out = new Outer(); //외부 클래스 객체 생성
      Runnable runner = out.getRunnable(1); //getRunnable()호출이 끝
      runner.run(); //여기서 getRunnable(1)의 매개변수 1이 사용(출력)됨
   }
}
~~~

실행결과

i = 1


num = 100


localNum = 10


outNum= 10


Outer.sNum =20


 

Runnable 인터페이스는 스레드를 구현하는 인터페이스이다. Runnable 인터페이스를 구현한 모든 클래스는 run() 추상메소드를 구현하여야 한다. 위 예제에서는 MyRunnable이라는 클래스를 getRunnable() 메소드 내부에 정의하고 이 메소드가 MyRunnable 클래스를 반환한다.

위에서 눈여겨 볼 점은 getRunnable() 메소드는 MyRunnable() 객체를 반환하면서 호출이 끝난다. 그런데 원래대로라면 매개변수 i도 메모리에서 같이 사라져야한다. 그런데 출력이 된다는 것은 아직 메모리에 남아있단 것이다.

그 이유는 i가 컴파일러에의해 상수로 변했기 때문이다. 하나 기억해 둘 것은 **지역 내부클래스에서 사용되는 지역변수는 상수로 처리된다**는 것이다.

&nbsp;

&nbsp;

# 4. 익명 내부클래스

* * *

지역 내부클래스를 다시보면 MyRunnable이라는 클래스 이름을 getRunnable() 메소드가 객체를 반환할 때 밖에 사용하지 않는다. 이럴 경우 이름을 생략하고 익명 클래스로 정의할 수 있는데, 2가지 방법이 있다.

### 방법1) : MyRunnable 클래스 이름 생략하고 클래스 바로 생성

~~~ java
return new Runnable(){ //익명 내부클래스로 Runnable 인터페이스 구현
         int localNum = 10;
         
         @Override
         public void run() {
            System.out.println("i = " +i);
            System.out.println("num = "+num);
            System.out.println("localNum = "+localNum);
            System.out.println("outNum= "+outNum);
            System.out.println("Outer.sNum ="+Outer.sNum);
         }
      };
~~~

&nbsp;

### 방법2) 인터페이스형 참조변수에 클래스 객체를 생성해 대입

~~~ java
Runnable runner = new Runnable(){ //익명 내부클래스로 Runnable 인터페이스 구현
   int localNum = 10;
   
   @Override
   public void run() {
      System.out.println("num = "+num);
      System.out.println("localNum = "+localNum);
      System.out.println("outNum= "+outNum);
      System.out.println("Outer.sNum ="+Outer.sNum);
   }
};
~~~

익명 내부클래스는 지역 내부클래스와 성질이 같으므로 비슷하게 사용할 수 있다. 지역변수 또한 지역 내부클래스에서 상수화 되었듯이 익명 내부클래스 내에서도 상수화 된다.