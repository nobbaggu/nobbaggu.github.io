---
title: (Java) 33. 추상 클래스
date: 2018-12-18T21:10:27+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - abstract
  - Java
  - 메소드
  - 자바
  - 추상
  - 추상메소드
  - 추상클래스
  - 클래스
---
추상이란 것은 명확하지 않음을 뜻한다. 따라서 추상 클래스는 명확하지 않은 클래스라고 유추할 수 있다. 맞는 말이다. 추상 클래스는 메소드를 명확하게 구현해놓지 않았다. 추상클래스를 상속받는 클래스에서 구체적으로 구현해 사용할 수 있도록 가능성만 남겨놓은 클래스를 추상클래스라고 한다.

&nbsp;

# 1. 추상 클래스

* * *

추상 클래스를 선언할 때에는 class 대신 abstract class 라고 선언한다.

~~~ java
public abstract class Computer{
   ...
}
~~~

추상 클래스는 항상 추상 메소드를 가지고 있다. **추상 메소드란 코드를 구현해놓지 않고 메소드의 반환형, 이름, 매개변수의 갯수와 자료형만 선언해놓은 메소드이다.**

~~~ java
public abstract void display();
~~~

위와 같이 추상 메소드는 메소드의 코드 구현부 { }를 가지지 않는다. 단순히 코드 구현만 하지 않은게 아니라 코드 구현부인 메소드의 몸체 부분을 가지고 있지 않는 것이다.

~~~ java
public void display(){
   //추상메소드가 구현 코드가 없다고 해서 이 메소드가 추상 케소드라고 생각할 수 있다.
   //하지만 이것은 추상 메소드가 아니다.
   //단지 코드 구현부가 없어 아무 명령어도 실행하지 않는 일반적인 메소드일 뿐이다.
}
~~~

위와 같은 메소드는 추상 메소드가 아니다.

도대체 왜 코드 구현도 제대로 되어있지 않은 메소드를 정의하는 건지 생각 해보아야한다. 코드를 보며 이해해보자.

~~~ java
package abstractpractice;

//추상 클래스
public abstract class Computer {
   public abstract void display();
   public abstract void typing();
   public void turnOn() {
      System.out.println("System on");
   }
   public void turnOff() {
      System.out.println("System off");
   }
}
~~~

&nbsp;

~~~ java
package abstractpractice;
//하위 클래스에서 상위 클래스의 추상메소드 코드 구현
public class DeskTop extends Computer{

   @Override
   public void display() {
      System.out.println("Desktop display()");      
   }

   @Override
   public void typing() {
      System.out.println("Destop typing()");    
   }
}
~~~

&nbsp;

~~~ java
package abstractpractice;
//하위 클래스에서 상위 클래스의 추상메소드 코드 구현
public abstract class NoteBook extends Computer{

   @Override
   public void display() {
      System.out.println("NoteBook display()");  
   }

   @Override
   public void typing() {
      System.out.println("NoteBook typing()");
   }
}
~~~

Computer 클래스를 상속받는 DeskTop클래스와 NoteBook클래스가 있다. Computer 클래스에서는 turnOn()과 turnOff() 메소드는 명확하게 구현해놓았다. 데스크톱이나 노트북이나 전원 켜기/끄기 기능은 공통으로 가지고 있을 것이기 때문이다. 하지만 디스플레이와 타이핑은 데스크톱과 노트북마다 구현방식이 달라져야 하기때문에 Computer클래스에서 구현하지 않고 추상 메소드로 남겨놓은 것이다.

**하위 클래스에서 공통적으로 사용할 메소드는 코드를 구현하고 다르게 구현해야 하는 메소드는 추상 메소드로 남겨두는 것이다.**

&nbsp;

&nbsp;

# 2. 추상클래스의 성질

* * *

**1) 추상 클래스를 상속받은 모든 클래스는 상위 클래스의 추상메소드의 코드를 구현시켜야한다. 그렇지 않으면 다시 abstract 선언을 하여 추상 클래스로 남겨두어야 한다.**

~~~ java
package abstractpractice;

public abstract class NoteBook extends Computer{

   @Override
   public void display() {
      System.out.println("NoteBook display()");  
   }

// @Override
// public void typing() {
//    System.out.println("NoteBook typing()");
// }
}
~~~

NoteBook 클래스에서는 display() 메소드의 코드만 구현하는 걸로 하였다. 그럼 NoteBook 클래스는 추상 메소드를 상속받고 구현하지 않아 추상 메소드를 가지고 있는 것이다. **추상 메소드를 가지고 있으려면 추상 클래스여야 하므로** NoteBook 클래스는 다시 abstract 선언을 해주어야한다.

~~~ java
package abstractpractice;

public class MyNoteBook extends NoteBook {

   @Override
   public void typing() {
      System.out.println("NoteBook typing()");
   }
}
~~~

다시 NoteBook클래스를 상속받은 MyNoteBook클래스에서 typing()메소드를 구현하였다.

&nbsp;

**2) 추상 클래스는 객체(인스턴스)를 생성할 수 없다.**

~~~ java
package abstractpractice;

public class ComputerTest {

   public static void main(String[] args) {
      Computer c1 = new Computer(); //오류
      Computer c2 = new DeskTop();
      Computer c3 = new NoteBook(); //오류
      Computer c4 = new MyNoteBook();
   }
}
~~~

위 코드는 오류가 난다. Computer클래스와 NoteBook 클래스는 추상클래스라서 객체를 생성할 수 없기 때문이다. 만약 추상클래스가 객체를 생성하면 객체는 메소드를 사용할 수 있어야한다. 그러나 추상 클래스는 코드가 구현되어있지 않은 메소드를 가지고있다. 이러한 이유로 추상 클래스는 객체를 생성할 수 없는 것이다.