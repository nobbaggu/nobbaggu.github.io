---
title: (Java) 30 - 메소드 오버라이딩
date: 2018-12-17T22:15:22+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - Java
  - override
  - overriding
  - 가상
  - 메소드
  - 영역
  - 오버라이딩
  - 자바
  - 코드
---
# 1. 메소드 오버라이딩이란?

* * *

상위 클래스를 상속받은 하위 클래스는 상위 클래스의 메소드를 재정의하여 사용할 수 있다. 이 때 메소드의 반환형, 매개변수의 갯수와 자료형, 메소드의 이름은 동일해야 한다. 그렇지 않으면 컴파일러가 다른 메소드로 인식해버린다.

~~~ java
package inheritance;

public class Customer {
   protected int customerID;
   protected String customerName;
   protected String customerGrade;
   int bonusPoint;
   double bonusRatio;
   
   //객체 생성과 동시에 고객번호와 이름 필수로 정의
   public Customer(int customerID, String customerName) {
      this.customerID = customerID;
      this.customerName = customerName;
      customerGrade = "SILVER"; // 기본 등급
      bonusRatio = 0.01; // 기본 적립율
   }
   
   public int calcPrice(int price) {
      bonusPoint += price * bonusRatio; // 보너스 포인트 적립
      return price;
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
   
   
   public String showVIPInfo() {
      return showCustomerInfo()+". The agentID is "+agentID;
   }

   @Override //컴파일러에게 메소드 오버라이딩임을 알려줌
   public int calcPrice(int price) {
         bonusPoint += price*bonusRatio;
      return price-(int)(price*saleRatio); //할인율 적용
   }
}
~~~

VIPCustomer 클래스에서는 calcPrice() 메소드를 재정의 하였다. 메소드의 반환형과 이름, 매개변수의 갯수와 자료형이 일치한다. 그러나 반환해주는 가격의 계산식 할인율을 적용하여 다시 정의하였다.

메소드 위에 @Override는 컴파일러에게 이 메소드가 상위 클래스의 메소드를 오버라이딩했음을 알려준다. 적어주지 않아도 상관은 없는데 메소드 오버라이딩을 의도함을 알려줌으로써 오버라이딩 규칙에 어긋나는 문법을 발견하면 에러를 발생시켜 프로그래머에게 알려주어 코드 오류를 방지해주므로 해주는 것이 좋다.

&nbsp;

&nbsp;

# 2. 가상 메소드

* * *

~~~ java
package inheritance;

public class CustomerTest {
   public static void main(String[] args) {
      
      Customer customerKim = new VIPCustomer(10020, "Kim", 12345); //Customer형의 참조변수에 VIPCustomer의 객체주소 저장
      customerKim.bonusPoint = 10000;
      
      //Customer의 calcPrice()가 실행될까 VIPCustomer의 calcPrice()가 실행될까?
      System.out.println(customerKim.calcPrice(10000));
   }
}
~~~

위에서는 VIPCustomer() 객체를 가르키는 customerKim의 참조변수가 Customer 자료형으로 형변환 되었다.

이 때 Customer클래스의 메소드가 실행될까 아니면 VIPCustomer클래스의 메소드가 실행될까?

결과를 알기 위해서는 가상 메소드라는 개념에 대해 알아야한다.

클래스로 객체를 생성할 때 마다 멤버변수를 힙 메모리에 저장한다. 그러나 메소드는 실행해야 할 명령어이기 때문에 메모리의 코드영역에 저장된다. 인스턴스를 여러개 생성해도 하나의 같은 메소드를 참조한다. 즉, 같은 클래스의 객체를 참조하는 참조변수는 여러개라도 모두 같은 메소드의 주소를 저장하고있다. 이를 가상메소드 방식이라고 한다.

그런데 하위 클래스에서 상위 클래스의 메소드를 오버라이드하면 가상 메소드 영역에 새로운 공간이 할당되고 거기에 재정의 된 메소드가 새로 만들어진다. 따라서 하위 클래스의 객체를 참조하는 참조변수는 메소드가 호출되었을 때 이 메소드를 참조한다.

따라서 customerKim 참조변수는 VIPCustomer 클래스의 재정의 된 메소드를 참조한다. 실행시켜서 정말 그런지 확인해봐야겠다.

실행결과

9000


  정말로 할인 된 가격정보를 출력한다.