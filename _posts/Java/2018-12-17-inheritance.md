---
title: (Java) 상속
date: 2018-12-17T20:55:13+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - extends
  - inheritance
  - Java
  - super()
  - 부모
  - 상속
  - 상위
  - 업캐스팅
  - 자바
  - 자식
  - 클래스
  - 하위
---
상속은 객체지향 프로그래밍에서 핵심적인 역할을 하는 기능이다. 객체지향 프로그램의 재사용성과 확장성을 넓히는 첫 번째 키가 바로 상속이다.

&nbsp;

# 1. 상속이란?

* * *

클래스는 '상속(inheritance)' 라는 것을 할 수 있다. 이 때 상속 해주는 클래스를 '부모 클래스', '상위 클래스' 등으로 부르고 상속받는 클래스를 '자식 클래스' 혹은 '하위 클래스' 라고 부른다. 먼저 상속이란 것은 어떤 문법으로 이루어지는지 보자.

~~~ java
package inheritance;

public class Customer {
   protected int customerID;
   protected String customerName;
   protected String customerGrade;
   int bonusPoint;
   double bonusRatio;
   
   public Customer() {
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

위와 같은 고객 클래스가 있다. 그런데 VIP고객 클래스를 하나 더 만들고 싶다. 상식적으로 VIP고객은 고객의 부분집합이므로 고객 클래스가 가지고있는 멤버변수와 메소드를 기본적으로 가지고 있어야 한다. 그리고 추가적인 기능을 넣고싶을 때 아래와 같이 하면된다.

~~~ java
package inheritance;

public class VIPCustomer extends Customer{
   private int agentID; // 담당 상담원 멤버변수 추가
   double saleRatio; // 할인율 멤버변수 추가
   
   public VIPCustomer() {
      customerGrade = "VIP";
      bonusRatio = 0.05; // 보너스 적립율 0.05로 상승
      saleRatio = 0.1; // 할인율 0.1 적용
   }
   
   public int agentID() {
      return agentID; // 담당 상담원 ID 반환
   }
   
   public int calcPrice(int price) {
      bonusPoint += price*bonusRatio;
      return price-(int)(price*saleRatio); //가격 할인 추가
   }
}
~~~

extends 예약어를 사용해서 VIPCustomer 클래스가 Customer 클래스를 상속받도록 하였다. 주의해야 할 것은 부모 클래스에서 멤버변수를 protected라고 선언했다는 것이다. **private로 선언하면 다른 클래스에서 사용하는 것을 막으므로 상속받은 클래스에서 사용할 수 없게된다. protected 예약어는 상속받는 하위 클래스에게만 접근을 허용하는 접근제어자이므로 알아두자.**

상속받은 하위 클래스는 기본적으로 상위 클래스의 멤버변수와 메소드를 사용할 수 있다. main 메소드에서 실행해보자.

~~~ java
package inheritance;

public class CustomerTest {
   public static void main(String[] args) {
      Customer customerJohn = new Customer();
      VIPCustomer customerKelly = new VIPCustomer();
      
      customerJohn.setCustomerID(10010);
      customerJohn.setCustomerName("John");
      customerJohn.bonusPoint = 1000;
      
      customerKelly.setCustomerID(10020);
      customerKelly.setCustomerName("Kelly");
      customerKelly.bonusPoint = 10000;
      
      System.out.println(customerLee.showCustomerInfo());
      System.out.println(customerKim.showCustomerInfo());
   }
}
~~~

실행결과

John's grade is SILVER and bonus point is 1000


Kelly's grade is VIP and bonus point is 10000.</pre>

&nbsp;

&nbsp;

# 2. 하위 클래스 생성 과정

* * *

하위 클래스의 생성자 안에는 컴파일러가 자동으로 super(); 라는 문장을 추가한다.

~~~ java
public VIPCustomer() {
      super(); //컴파일러가 자동으로 추가
      customerGrade = "VIP";
      bonusRatio = 0.05;
      saleRatio = 0.1;
}
~~~

super란 상위 클래스를 참조하는 지시자이다. this 예약어가 클래스 자신을 지칭하는 것과 같은 것이다.

사실 하위 클래스 객체가 만들어질 때는 상위클래스 객체가 먼저 만들어지면서 멤버변수와 메소드가 힙 메모리에 생성된다. 그리고 이어서 나머지 하위 클래스의 추가적인 멤버변수와 메소드가 생성되는 것이다.

Customer 클래스와 VIPCustomer 생성자 안에 다음의 문장 출력문을 추가해보자.

~~~ java
public Customer() {
      customerGrade = "SILVER"; // 기본 등급
      bonusRatio = 0.01; // 기본 적립율
     System.out.println("Customer instance made!");
}
~~~

~~~ java
public VIPCustomer() {
      customerGrade = "VIP";
      bonusRatio = 0.05; // 보너스 적립율 0.05로 상승
      saleRatio = 0.1; // 할인율 0.1 적용
      System.out.println("VIPCustomer instance made!");
}
~~~

이어서 main 함수에 아래의 문장을 통해서 VIPCustomer 인스턴스를 하나 생성하면

~~~ java
VIPCustomer customerKelly = new VIPCustomer();
~~~

실행결과

Customer instance made!


VIPCustomer instance made

두 개의 문장이 출력된다. 다시 말하자면 자식 클래스 객체를 생성할 때에는 부모 클래스 객체가 먼저 하나 생성이 되고(super(); 를 통해) 자식 클래스의 나머지 멤버변수와 메소드를 메모리에 추가로 할당하는 것이다.

&nbsp;

&nbsp;

# 3. 하위 클래스의 형 변환(up-casting)

* * *

~~~ java
package inheritance;

public class CustomerTest {
   public static void main(String[] args) {
      Customer customerJohn = new VIPCustomer(10010, "John", 1);
      System.out.println(customerJohn.agentID()); // 오류 발생
   }
}
~~~

VIPCustomer 인스턴스를 생성하여 customerJohn 참조변수에 저장했다. 그런데 customerJohn 참조변수는 Customer 타입이다.

**결론부터 말하자면 자식 클래스의 인스턴스는 자료형을 부모클래스 자료형으로 형 변환하여 선언할 수 있다.**

이렇게 생성된 객체를 참조하는 **참조변수는 부모클래스 자료형이므로 생성된 객체의 모든 멤버변수와 메소드에 접근할 수 있지는 않다. 부모 클래스에 포함이 된 멤버변수와 메소드만 사용할 수 있다.**

그렇다면 부모 클래스의 객체를 자식 클래스 자료형으로 형 변환하여 선언하는 것도 가능할까? 불가능하다. 상위 클래스 객체가 하위 클래스의 모든 기능을 다 가지고 있지 않기 때문이다.