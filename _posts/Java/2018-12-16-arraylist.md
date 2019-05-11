---
title: ArrayList 클래스
date: 2018-12-16T19:25:24+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - ArrayList
  - Java
  - 배열
  - 어레이 리스트
  - 자바
---
Java의 기본 문법에서 지원하는 배열에는 제약사항이 많다. 요소를 추가하려면 길이를 늘려야 하고 중간에 다른 값 하나를 추가하려하면 코드를 수정해야 한다. 이외에도 불편한 점이 많다.

java.util 패키지에는 객체 배열을 편리하게 다룰 수 있도록 해주는 ArrayList 클래스가 구현되어 있다. ArrayList 클래스는 주요 클래스로 자세히 다루어야 하지만 이번에는 일반 배열과 비교하는 목적으로 단지 사용법을 보도록 한다.

&nbsp;

# 1. ArrayList로 객체 배열 생성

* * *

~~~ java
ArrayList<Student> stus = new ArrayList<Student>();
~~~

<E> 형태의 자료형은 제네릭이라고 하는데, 고정되지 않고 유동적인 자료형을 다루기 위해 사용하는 개념으로 나중에 자세히 다룬다.

ArrayList클래스의 사용을 위해서는 컴파일러에게 ArrayList클래스의 위치를 알려주어야 한다. 코드의 가장 상단에 다음 구문을 추가해야한다.

~~~ java
import java.util.ArrayList;
~~~

java.util 패키지는 유용한 자료구조 & 알고리즘 클래스를 지원하는 패키지다.

&nbsp;

&nbsp;

# 2. ArrayList 배열 사용

* * *

~~~ java
package classpart;

public class Student {
    int studentID;
    String name;
    
    public Student() {};
    
    public Student(int studentID, String name) {
       this.studentID = studentID;
       this.name = name;
    }
   
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

&nbsp;

~~~ java
package classpart;
import java.util.ArrayList;

public class Test {
    public static void main(String[] args) {
       ArrayList<Student> stus = new ArrayList<Student>(); //ArrayList로 Studet타입 객체 생성하고 stus 참조변수가 참조
       
       //새로운 배열요소를 추가하기 위한 add()메소드
       stus.add(new Student(1001, "John"));
       stus.add(new Student(1002, "Kelly"));

       for(int i=0; i<stus.size(); i++) { //객체 배열의 길이를 반환하는 size() 메소드
          Student stu = stus.get(i); // 객체 배열의 index i 요소를 반환하는 get() 메소드
          stu.showInfo();
       }
    }
}
~~~

자주 사용하는 메소드는 기억해두면 편하다.

JavaDoc에서 ArrayList에 관한 설명을 한 번 읽어보는 것이 도움될 것이다.