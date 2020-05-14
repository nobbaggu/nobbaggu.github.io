---
title: (Java) 26. 객체 배열
date: 2018-12-16T17:56:17+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - array
  - 객체
  - 배열
  - 자료형
  - 참조
---
# 1. 객체 배열

* * *

~~~ java
package classpart;

public class Student {
    int studentID;
    String name;
    
    public Student() {};
   
    public int getStudentID() {
      return studentID;
   }
   public void setStudentID(int studentID) {
      this.studentID = studentID;
   }
   public String getName() {
      return name;
   }
   public void setName(String name) {
      this.name = name;
   }
    
    public void showInfo() {
       System.out.println(studentID+", "+name);
    }
}
~~~

위와 같이 Student 클래스를 생성했다.

멤버 변수로는 이름과 학번이 있고 각각에 대해 get(), set() 메소드를 지원한다. 또한 학생 정보를 출력해주는 showInfo()메소드를 만들었다.

이제 Student를 하나의 참조 자료형으로 사용한 Student 자료형 배열을 만들어보자.

~~~ java
package classpart;

public class Test {
    public static void main(String[] args) {
       Student[] stus = new Student[3]; //Student형의 인스턴스를 저장할 배열공간만 확보했을 뿐 아직 인스턴스가 만들어지지 않았다.
       
       //배열 요소마다 인스턴스 생성
       stus[0] = new Student();
       stus[1] = new Student();
       stus[2] = new Student();
       
       stus[0].setName("John");
       stus[0].setStudentID(1001);
       stus[1].setName("Kelly");
       stus[1].setStudentID(1002);
       stus[2].setName("Faul");
       stus[2].setStudentID(1003);
       
       for(int i=0; i<stus.length; i++) {
          stus[i].showInfo();
       }
    }
}
~~~

가장 처음에 Student[] stus = new Student[3]; 를 써서 Student 타입의 길이가 3인 배열 stus를 만들었다.

주의해야 할 건 아직 인스턴스가 만들어지지 않았단 것이다. stus배열의 각각의 요소는 아직 아무런 객체의 주소값도 가지고 있지 않다. 출력해보면 null이 나온다.

따라서 이후에 각각의 요소마다 인스턴스를 생성해주고 이후의 코드를 진행해야한다.

~~~ java
package classpart;

public class Test {
    public static void main(String[] args) {
       Student[] stus = new Student[3]; //Student형의 인스턴스를 저장할 배열 공간만 확보했을 뿐 아직 인스턴스가 만들어지지 않았다.
       
       for(int i=0; i<stus.length; i++) {
          System.out.println(stus[i]);
       }
       //배열 요소마다 인스턴스 생성
       stus[0] = new Student();
       stus[1] = new Student();
       stus[2] = new Student();
       
       for(int i=0; i<stus.length; i++) {
          System.out.println(stus[i]);
       }
    }
}
~~~

실행결과

null


null


null


classpart.Student@15db9742


classpart.Student@6d06d69c


classpart.Student@7852e922

결과를 보면 각각의 배열요소마다 인스턴스를 생성하여 저장하기 전 아무런 객체도 참조하지 않는 null 상태이다. 혹시나 실수로 인스턴스 생성을 까먹으면 NullPointException 컴파일 오류가 난다.

&nbsp;