---
title: (Java) 다운캐스팅
date: 2018-12-18T20:06:09+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - downcasting
  - Java
  - 다운캐스팅
  - 자바
  - 형변환
---
상위 클래스로 형 변환된 인스턴스는 상위 클래스에 없는 멤버변수와 메소드는 사용할 수 없다. 다만 같은 메소드가 재정의 된 경우 가상 메소드에 의해 자신의 메소드를 사용할 뿐이었다. 그런데 필요에 의해 다시 하위 자료형으로 형 변환되어 멤버변수나 메소드를 사용해야 할 경우가 있을 수 있다. 다시 하위 자료형으로 형 변환하는 것을 다운 캐스팅(down casting)이라고 한다.

&nbsp;

# 1. 다운 캐스팅

* * *

상위 클래스를 상속받는 하위 클래스가 여러개인 경우 다시 하위 클래스로 돌아오기 전에 하위 클래스들 중 어떤 클래스의 객체인지 확인해야 한다. 예를 들어 고객이라는 상위 클래스를 상속받은 VIP고객, Gold고객, Silver고객이 있다고 하자. 고객 자료형으로 선언된 VIP고객의 인스턴스가 다시 하위 자료형으로 갈 때 Gold 고객이나 Silver고객 자료형으로 다운캐스팅이 되어서는 안된다. 따라서 어떤 클래스의 인스턴스인지 확인해야 하는데 이를 해주는 것이 instanceof 예약어이다.

~~~ java
package inheritance;

public class Animal {
   public void move() {
      System.out.println("animal moves");
   }
}
~~~

~~~ java
package inheritance;

public class Human extends Animal{
   public void move() {
      System.out.println("human walks");
   }
   
   public void readBook() {
      System.out.println("human reads a book");
   }
}
~~~

~~~ java
package inheritance;

public class Tiger extends Animal{
   public void move() {
      System.out.println("tiger runs");
   }
   
   public void hunting() {
      System.out.println("tiger hunts");
   }
}
~~~

~~~ java
package inheritance;

public class Eagle extends Animal {
   public void move() {
      System.out.println("eagle flys");
   }
   
   public void spreadWings() {
      System.out.println("eagle spreads wings");
   }
}
~~~

Animal 상위 클래스를 상속받는 Human, Tiger, Eagle 클래스가 있다. 각 클래스는 Animal의 move() 메소드를 재정의하고 추가적으로 자신만의 메소드를 하나 더 만들었다.

~~~ java
package inheritance;
import java.util.ArrayList;

public class AnimalTest {

   ArrayList<Animal> aniList = new ArrayList<Animal>();
   
   public static void main(String[] args) {
      AnimalTest aTest = new AnimalTest();
      aTest.addAnimal();
      aTest.testCasting();
   }
   
   public void addAnimal() {
      aniList.add(new Human());
      aniList.add(new Tiger());
      aniList.add(new Eagle());
      
      for(Animal ani : aniList) {
         ani.move();
      }
   }
   
   public void testCasting() {
      for(int i=0; i<aniList.size(); i++) {
         Animal ani = aniList.get(i);
         if(ani instanceof Human) {
            Human h = (Human)ani;
            h.readBook();
         }
         else if(ani instanceof Tiger) {
            Tiger t = (Tiger)ani;
            t.hunting();
         }
         else if(ani instanceof Eagle) {
            Eagle e = (Eagle)ani;
            e.spreadWings();
         }
         else {
            System.out.println("No such a type!");
         }
      }
   }
}
~~~

AnimalTest라는 클래스를 하나 만들고 Animal자료형을 요소로 가지는 ArrayList를 하나 만들었다. 그리고 세 가지 하위클래스 각각의 객체를 생성하는 add()메소드와 참조변수가 참조하는 객체의 참조자료형을 확인한 후 맞는 자료형으로 변경한 후 각 하위 클래스만이 가지는 고유의 메소드를 호출한다. 이 때 if else 문을 사용했다.

실행결과

human walks


tiger runs


eagle flys


human reads a book


tiger hunts


eagle spreads wings

instanceof 예약어를 사용하여 참조변수가 어떤 하위 클래스의 객체를 참조하는지 매칭하여 다운캐스팅을 하였다. 다운캐스팅 된 객체를 새로운 참조변수에 저장하고 이 참조변수를 통해서 각 객체의 고유 메소드를 호출할 수 있었다.