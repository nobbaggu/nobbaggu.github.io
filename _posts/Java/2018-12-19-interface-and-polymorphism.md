---
title: (Java) 36 - 인터페이스와 다형성
date: 2018-12-19T22:53:51+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - interface
  - Java
  - 다형성
  - 인터페이스
  - 자바
---
#   1. 인터페이스의 역할

* * *

인터페이스에는 코드가 작성되지 않은 추상 메소드만 있다. 그러나 인터페이스를 구현한 클래스에서 정의하는 메소드의 반환형과 이름, 매개변수의 타입 등 메소드의 형태를 알 수 있는 정보가 포함되어 있다.

인터페이스는 사용 설명서 같은 역할을 한다. 어떠한 인터페이스를 구현하는 클래스1, 클래스2, 클래스3이 있고 각각 기능에 따라 메소드 구현을 달리 하더라도 메소드의 형태에 관한 정보가 인터페이스에 있기 때문에 인터페이스가 구현된 코드를 보면 이를 구현하는 클래스의 사용 방법에 대해 알 수 있다.

&nbsp;

&nbsp;

# 2. 인터페이스와 다형성

* * *

콜센터 상담원 배분 프로그램을 만들려고 한다. 그런데 3가지의 서로 다른 배분 방식을 구현하고 싶다.

1. 순서대로 전화를 가져와 상담원 순서대로 배분

2. 순서대로 전화를 가져와 대기고객 수가 적은 상담원에 배분

3. 고객 등급 순으로 전화를 가져와 업무 능력이 높은 상담원에게 우선 배분

먼저 공통적인 기능의 메소드를 정의한 인터페이스부터 만들 수 있다.

~~~ java
package scheduler;

public interface Scheduler {
   public void getNextCall();
   public void sendCallToAgent();
}
~~~

이제 각각의 방법마다 인터페이스를 구현한 3가지의 클래스를 만들어본다.

~~~ java
package scheduler;

//배분 방식 1
public class RoundRobin implements Scheduler {

   @Override
   public void getNextCall() {
      System.out.println("get calls as ordered");
   }

   @Override
   public void sendCallToAgent() {
      System.out.println("send call to next ordered agent");
   }
   
}
~~~

~~~ java
package scheduler;
//배분 방식 2
public class LeastJob implements Scheduler {

   @Override
   public void getNextCall() {
      System.out.println("get calls as ordered");
   }

   @Override
   public void sendCallToAgent() {
      System.out.println("send calls to agents with least job");
   }
   
}
~~~

&nbsp;

~~~ java
package scheduler;
//배분 방식 3
public class PriorityAllocation implements Scheduler {

   @Override
   public void getNextCall() {
      System.out.println("get calls from prior customers first");
   }

   @Override
   public void sendCallToAgent() {
      System.out.println("give calls to better agents");
   }
   
}
~~~

이제 클래스의 객체를 만들어 실행해 볼 차례다.

~~~ java
package scheduler;
import java.io.IOException;

public class SchedulerTest {
   public static void main(String[] args) throws IOException {
      System.out.println("Select call allocation method");
      System.out.println("R : RoundRobin");
      System.out.println("L : LeastJob");
      System.out.println("P : Priority Allocation");
      System.out.print("Type here : ");
      
      int ch = System.in.read(); //문자를 입력받을 수 있는 함수
      Scheduler scheduler = null; //아무런 객체도 참조하지 않는 Scheduler 인터페이스 자료형의 참조변수 생성
      
      //문자를 입력받아 확인 후 객체 생성하여 scheduler 변수에 대입
      if(ch == 'R' || ch == 'r') {
         scheduler = new RoundRobin();
      }
      else if(ch == 'L' || ch == 'l') {
         scheduler = new LeastJob();
      }
      else if(ch == 'P' || ch == 'p') {
         scheduler = new PriorityAllocation();
      }
      else {
         System.out.println("fuck you");
         return;
      }
      
      //객체에 따른 메소드 호출
      scheduler.getNextCall();
      scheduler.sendCallToAgent();
   }
}
~~~

위에서 참조변수를 인터페이스 자료형인 Scheduler 타입으로 선언하고 일단 null값을 담아 아무런 객체도 참조하지 않도록 선언했다.

이후 입력된 문자에 따라 적절한 객체를 참조하도록 한 이후 메소드를 호출한다. 이 때 메소드는 인터페이스를 구현한 클래스에 해당하는 메소드가 호출된다.

이로써 인터페이스로 구현한 클래스 끼리도 다형성을 구현할 수 있음을 확인했다.