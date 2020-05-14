---
title: (Java) 21. 객체 지향 예제 - 학생, 버스, 지하철 프로그램
date: 2018-12-15T16:19:27+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - Java
  - OOP
  - 객체지향
  - 자바
---
객체 지향 프로그래밍에 대해 배운 것들을 사용해 학생, 버스, 지하철 세 가지의 객체가 서로 상호작용하는 것을 프로그래밍 한 예제다.

~~~ java
package classpart;

public class Student {
    String name;
    int money;
    
    public Student(String name, int money) {
       this.name = name;
       this.money = money;
    }
    
    public void takeBus(Bus bus) {
       this.money -= 1000;
       bus.take(1);
    }
    
    public void takeSubway(Subway subway) {
       this.money -= 1500;
       subway.take(1);
    }

   public String getName() {
      return name;
   }

   public int getMoney() {
      return money;
   }
   
   public void showInfo() {
      System.out.println(this.getName()+" has "+this.getMoney());
   }
}
~~~

&nbsp;

~~~ java
package classpart;

public class Bus {
   int busNum;
   int intake = 0;
   int passenger;
   
   public Bus(int num) {
      this.busNum = num;
   }
   
   public void take(int passenger) {
      this.passenger++;
      this.intake+=1000;
   }

   public int getBusNum() {
      return busNum;
   }

   public int getIntake() {
      return intake;
   }

   public int getPassenger() {
      return passenger;
   }
   
   public void showInfo() {
      System.out.println("Bus "+this.getBusNum()+" got "+this.getPassenger()+" passengers and earned "+this.getIntake()+" won");
   }
}
~~~

&nbsp;

~~~ java
package classpart;

public class Subway {
   int subwayNum;
   int intake = 0;
   int passenger;
   
   public Subway(int num) {
      this.subwayNum = num;
   }
   
   public void take(int passenger) {
      this.passenger++;
      this.intake+=1500;
   }

   public int getsubwayNum() {
      return subwayNum;
   }

   public int getIntake() {
      return intake;
   }

   public int getPassenger() {
      return passenger;
   }
   
   public void showInfo() {
      System.out.println("Subway "+this.getsubwayNum()+" got "+this.getPassenger()+" passengers and earned "+this.getIntake()+" won");
   }
}
~~~

&nbsp;

학생 클래스에서 학생이 버스나 지하철을 타는 메소드를 실행하면 다음과 같은 일이 일어난다.

학생 객체가 takeBus나 takeSubway 메소드를 실행하면 학생의 돈이 1000원 혹은 1500원 빠져나가고 Bus나 Subway타입의 객체의 take()메소드가 한 번 실행된다.

버스나 지하철 클래스 입장에서는 다른 객체에서 호출되어 실행되면 승객수가 1 증가하고 돈이 1000원이나 1500원이 올라간다.

~~~ java
package classpart;

public class Test {
    public static void main(String[] args) {
       
       Student stu1 = new Student("John", 10000);
       Student stu2 = new Student("Kelly", 20000);
       Student stu3 = new Student("Harry", 14000);
       
       Bus bus100 = new Bus(100);
       Subway subway2 = new Subway(2);
       Subway subway7 = new Subway(7);
       
       stu1.takeBus(bus100);
       stu2.takeSubway(subway2);
       stu1.takeSubway(subway7);
       stu3.takeSubway(subway7);
       
       stu1.showInfo();
       stu2.showInfo();
       stu3.showInfo();
       bus100.showInfo();
       subway2.showInfo();
       subway7.showInfo();    
    }
}
~~~

실행결과

John has 7500


Kelly has 18500


Harry has 12500


Bus 100 got 1 passengers and earned 1000 won


Subway 2 got 1 passengers and earned 1500 won


Subway 7 got 2 passengers and earned 3000 won


 

지금까지 배운 객체와 클래스, 인스턴스, 생성자, 참조변수의 모든 개념을 사용하여 간단한 예제를 하나 구현해 보았다. 예제 코드에서는 서로 다른 세 개의 객체가 서로 영향을 주며 유기적인 작용을 한다. 이러한 프로그램을 구현해 봄으로써 객체지향 프로그래밍이 어떤 것인지 감을 잡을 수 있었다.