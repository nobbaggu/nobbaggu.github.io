---
title: (Java) 다형성
date: 2018-12-18T00:44:31+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - extends
  - Java
  - 가상 메소드
  - 다형성
  - 메소드
  - 상속
  - 오버라이딩
  - 자바
---
다형성은 클래스의 형 변환과 가상 메소드 개념의 장점을 최대한 활용하는 객체지향의 매우 핵심적인 개념이다.

결국 다형성이란 것은 하위 클래스 객체를 상위 클래스 자료형으로 변환 가능하다는 사실, 그러나 참조자료형이 호출하는 메소드는 인스턴스 객체에 좌우된다는 사실이 핵심이자 전부이다. 다만 이것을 최대한 활용하는 개념인 것이다.

&nbsp;

# 1. 다형성이란?

* * *

다형성이란 상위 클래스와 이를 상속받는 여러 하위클래스들이 같은 이름의 메소드를 호출하면서도 객체의 자료형마다 재정의 된 코드를 실행할 수 있는 편리한 방법이다. 코드를 통해 무슨 말인지 보자.

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
}
~~~

~~~ java
package inheritance;

public class Tiger extends Animal{
   public void move() {
      System.out.println("tiger runs");
   }
}
~~~

~~~ java
package inheritance;

public class Eagle extends Animal {
   public void move() {
      System.out.println("eagle flys");
   }
}
~~~

Animal이라는 상위 클래스와 이를 상속하는 Human, Tiger, Eagle 클래스가 있다. 그리고 각각 move() 메소드를 재정의하고 있다.

~~~ java
package inheritance;

public class AnimalTest {

   public static void main(String[] args) {
      AnimalTest animalTest = new AnimalTest();
      Human human = new Human();
      Tiger tiger = new Tiger();
      Eagle eagle = new Eagle();
      
      animalTest.moveAnimal(human);
      animalTest.moveAnimal(tiger);
      animalTest.moveAnimal(eagle);
   }
   //매개변수로 Animal 자료형 참조변수를 넣어준다.
   public void moveAnimal(Animal animal) {
      animal.move();
   }

}
~~~

위 코드를 실행시키면 다음과 같은 결과가 나온다.

human walks


tiger runs


eagle flys


 

moveAnimal 메소드의 매개변수의 자료형은 Animal이다. 따라서 하위 클래스인 Human, Tiger, Eagle 모든 객체가 올 수 있다. 하위 클래스 객체의 상위 클래스 자료형으로의 형 변환을 이용한 것이다. 그런데 더 놀라운 것은 각각의 참조변수가 참조하는 객체마다 각자 자신이 참조하는 다른 메소드가 실행된다는 것이다.

매개변수의 자료형을 상위 클래스의 자료형으로 퉁쳐서 코드를 한 줄만 구현하고도 전달되는 객체마다 서로 다른 메소드가 실행될 수 있다는 것이다. 또 다른 포유류를 상속받는 몇개의 클래스가 추가되더라도 이 부분은 달라질 필요가 없는 것이다.

&nbsp;

&nbsp;

# 2. 배열과 다형성

* * *

하나의 상위 클래스를 상속받는 여러 개의 하위 클래스 객체들이 여러 개 있을 때는 같은 상위 클래스 타입 자료형으로 선언할 수 있으므로 배열로 관리하는 것이 편리하다. 다음 코드를 보자.

~~~ java
package inheritance;

//상위 Customer 클래스
public class Customer {
   protected int customerID;
   protected String customerName;
   protected String customerGrade;
   int bonusPoint;
   double bonusRatio;
   
   public Customer() {
      initCustomer();
   }
   //객체 생성과 동시에 고객번호와 이름 필수로 정의
   public Customer(int customerID, String customerName) {
      this.customerID = customerID;
      this.customerName = customerName;
      initCustomer();
   }
   
   public int calcPrice(int price) {
      bonusPoint += price * bonusRatio; // 보너스 포인트 적립
      return price;
   }
   
   private void initCustomer() {
      customerGrade = "SILVER";
      bonusRatio = 0.01;
   }
   
   public String showCustomerInfo() {
      return customerName+"'s grade is "+customerGrade+" and bonus point is "+bonusPoint;
   }

   public int getCustomerID() {
      return customerID;
   }

   public void setCustomerID(int customerID) {
      this.customerID = customerID;
   }

   public String getCustomerName() {
      return customerName;
   }

   public void setCustomerName(String customerName) {
      this.customerName = customerName;
   }

   public String getCustomerGrade() {
      return customerGrade;
   }

   public void setCustomerGrade(String customerGrade) {
      this.customerGrade = customerGrade;
   }
}
~~~

~~~ java
package inheritance;
//Customer를 상속하는 VIPCustomer
public class VIPCustomer extends Customer{
   private int agentID;
   double saleRatio;
   
   // 상위클래스의 생성자가 고객번호와 이름을 매개변수로 받으므로 이를 super()에 전달하고 담당 직원번호 정의
   public VIPCustomer(int customerID, String customerName, int agentID) {
      super(customerID, customerName);
      customerGrade = "VIP";
      bonusRatio = 0.05;
      saleRatio = 0.1;
      this.agentID = agentID;
   }
   
   public int agentID() {
      return agentID;
   }
   
   //메소드 재정의
   public String showCustomerInfo() {
      return super.showCustomerInfo()+". The agentID is "+agentID;
   }
   
   //메소드 재정의
   @Override //컴파일러에게 메소드 오버라이딩임을 알려줌
   public int calcPrice(int price) {
         bonusPoint += price*bonusRatio;
      return price-(int)(price*saleRatio);
   }
}
~~~

~~~ java
package inheritance;
//Customer를 상속받는 GoldCustomer
public class GoldCustomer extends Customer{
   
   double saleRatio;
   
   public GoldCustomer(int customerID, String customerName) {
      super(customerID, customerName);
      customerGrade = "GOLD";
      bonusRatio = 0.02;
      saleRatio = 0.1;
   }
   //메소드 재정의
   public int calcPrice(int price) {
      bonusPoint += price*bonusRatio;
      return price-(int)(price*saleRatio);
   }
}
~~~

&nbsp;

VIPCustomer와 GoldCustomer는 Customer를 상속하고 메소드를 재정의 하였다.

~~~ java
package inheritance;
import java.util.ArrayList;

public class CustomerTest {
   public static void main(String[] args) {
      ArrayList<Customer> customers = new ArrayList<Customer>();
      //배열 요소의 자료형이 Customer이므로 상속받는 클래스 객체가 추가될 수 있다.
      customers.add(new Customer(10010, "Kelly"));
      customers.add(new VIPCustomer(10020, "John", 12334));
      customers.add(new GoldCustomer(10030, "Tomson"));
      
      System.out.println(customers.get(0).calcPrice(10000));
      System.out.println(customers.get(1).calcPrice(10000));
      System.out.println(customers.get(2).calcPrice(10000));
      
      System.out.println(customers.get(0).showCustomerInfo());
      System.out.println(customers.get(1).showCustomerInfo());
      System.out.println(customers.get(2).showCustomerInfo());
   }
}
~~~

실행결과

10000


9000


9000


Kelly's grade is SILVER and bonus point is 100


John's grade is VIP and bonus point is 500. The agentID is 12334


Tomson's grade is GOLD and bonus point is 200


 

배열의 자료형을 Customer로 선언하였지만 이를 상속하는 모든 클래스의 객체를 추가할 수 있었다. 더군다나 각 객체가 메소드를 호출하면 객체의 자료형에 따른 서로 다른 메소드가 호출된다.

만약 상속고 다형성을 사용하지 않고 모든 등급의 변수와 메소드를 하나의 클래스 파일에 때려넣으면 bonusPoint라던지 saleRatio, 심지어 모든 메소드도 if else문을 사용하여 고객 등급마다 서로 다른 내용을 구현해야 할 것이다. 이렇게 되면 코드가 복잡해지고 새로운 등급이 추가되는 경우 코드 수정도 어려워진다.

상속과 다형성은 단순히 멤버변수나 메소드의 재사용 가능의 유무만 따지는 것이 아니다. 하나의 클래스가 다른 클래스에 포함되는 하위 개념 혹은 일반적인 클래스와 이것이 더욱 구체화되는 클래스의 관계에 사용하는 것이 맞다.