---
title: (Java) 38 - 인터페이스 활용
date: 2018-12-21T17:46:22+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - extends
  - interface
  - Java
  - 구현
  - 상속
  - 인터페이스
  - 자바
---
# 1. 여러 인터페이스를 구현한 클래스

* * *

~~~ java
package interfaceprac;

public interface Buy {
   
   public void buy();
   
   default void order() {
      System.out.println("buy order");
   }
}
~~~

&nbsp;

~~~ java
package interfaceprac;

public interface Sell {
   
   public void sell();
   
   default void order() {
      System.out.print("sell order");
   }
   
}
~~~

&nbsp;

~~~ java
package interfaceprac;

public class Customer implements Buy, Sell{

   @Override //Sell인터페이스의 sell()메소드 구현
   public void sell() {
      System.out.println("I sell!");
      
   }

   @Override //Buy인터페이스의 buy()메소드 구현
   public void buy() {
      System.out.println("I buy!");
   }
   
   @Override //Buy, Sell 인터페이스에 있는 order()메소드 오버라이딩
   public void order() {
      System.out.println("customer order");
   }
   
}
~~~

그럼 이제 위의 인터페이스, 클래스로 프로그램을 만들어 실행시켜보자.

~~~ java
package interfaceprac;

public class CustomerTest {
   public static void main(String[] args) {
      Customer customer = new Customer(); //customer타입 객체 생성
      
      Buy buyer = customer; //객체를 Buy형으로 변환
      Sell seller = customer; //객체를 Sell형으로 변환
      
      buyer.buy(); //Customer에서 구현한 buy()메소드
      buyer.order(); //Buy의 default메소드 order() ????
      
      seller.sell(); //Customer에서 구현한 sell()메소드
      seller.order(); //Sell의 default메소드 order() ????
      
      customer.order(); //Customer에서 재정의한 order() 메소드
   }
}
~~~

실행결과

I buy!


customer order


I sell!


customer order


customer order


 buy.order(), sell.order(), customer.order() 모두 같은 실행결과가 나온다. 모든 메소드가 Customer 클래스에서 재정의한 order() 메소드를 호출한다.

가상 메소드 원리를 생각하면 이는 당연한 결과이다. buyer, seller 참조변수의 자료형이 Buy, Sell형이라고 해도 두 참조변수는 customer 참조변수를 형 변환한 것일 뿐이다. 따라서 buyer, seller, customer 세 참조변수 모두 customer가 참조하는 객체를 참조하며, 그 객체는 new Customer()이다. 따라서 세 참조변수를 통해서 order() 메소드를 호출하면 모두 Customer의 order() 메소드가 실행되는 것이다.

&nbsp;

&nbsp;

# 2. 인터페이스 상속

* * *

클래스가 다른 클래스를 상속하는 것 처럼 인터페이스도 인터페이스를 상속할 수 있다.

클래스의 상속과는 다르게 인터페이스에서 상속받는 것은 메소드의 기능이 아닌 형태일 뿐이다. 따라서 인터페이스는 여러개의 인터페이스를 상속받을 수 있다.

이 때 두 인터페이스를 상속받은 하위 인터페이스도 자신만의 가상메소드를 가진다고 해보자. 이 인터페이스를 구현한 클래스는 상위 인터페이스 2개, 하위1터페이스 1개에 있는 모든 추상 메소드를 구현해야한다.

~~~ java
package interfaceprac;

public interface Tiger {
   public void tigerAttack();
}
~~~

&nbsp;

~~~ java
package interfaceprac;

public interface Lion {
   public void lionAttack();
}
~~~

&nbsp;

~~~ java
package interfaceprac;

public interface Liger extends Tiger, Lion {
   public void superAttack();
}
~~~

&nbsp;

~~~ java
package interfaceprac;

public class Pocketmon implements Liger {

   @Override
   public void tigerAttack() {
      System.out.println("tiger attack~!");
   }

   @Override
   public void lionAttack() {
      System.out.println("lion attack~!");
   }

   @Override
   public void superAttack() {
      System.out.println("super attack!~!~~!");
   }
   
}
~~~

Pocketmon클래스는 Liger인터페이스를 구현하였고 총 세 개의 추상메소드를 구현해야 한다.

~~~ java
package interfaceprac;

public class PocketmonTest {
   public static void main(String[] args) {
      Pocketmon pokemon = new Pocketmon();
      
      pokemon.tigerAttack();
      pokemon.lionAttack();
      pokemon.superAttack();
   }
}
~~~

&nbsp;