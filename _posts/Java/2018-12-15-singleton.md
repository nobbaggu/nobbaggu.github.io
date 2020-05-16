---
title: (Java) 24 - 싱글톤 패턴
date: 2018-12-15T19:43:36+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - Java
  - singleton
  - 싱글톤
  - 자바
---
객체지향 프로그래밍에서 클래스의 인스턴스를 단 한 개만 만들어서 사용하는 디자인 패턴이다. 학교 객체와 학생들 객체가 있을 때, 학교는 1개이므로 인스턴스를 한 개만 만들면 된다. 이럴 경우 싱글톤 패턴을 사용한다.

싱글톤 패턴을 단계적으로 프로그래밍 해보자.

&nbsp;

## 1. private 생성자 생성

생성자가 없을 경우 컴파일러가 넣어주는 default 생성자는 항상 public이다. 싱글톤 패턴에서는 외부에서 인스턴스를 생성할 수 없도록 항상 private 생성자를 선언해준다.

~~~ java
package classpart;

public class School {
   private School() {}
}
~~~

&nbsp;

## 2. static을 사용하여 유일한 인스턴스 생성

단계1에서 private 생성자를 만들어놓았으므로 외부에서는 인스턴스를 생성할 수 없게 되었다. 따라서 미리 클래스 내부에서 인스턴스를 하나 만들어 주는 것이다.

~~~ java
package classpart;

public class School {
   private static School instance = new School(); // 클래스의 유일한 인스턴스
   private School() {}
}
~~~

&nbsp;

## 3. 외부에 공개되는 메소드 생성

클래스 내부에서 생성한 private 인스턴스를 외부에서 사용 가능하도록 메소드를 만들어준다.

~~~ java
package classpart;

public class School {
   private static School instance = new School(); // 클래스의 유일한 인스턴스
   private School() {}
   
   public static School getInstance() {
      if(instance == null) {
          instance = new School();
      }
      return instance;
   }
}
~~~

getInstance() 메소드는 static 메소드로 선언했다. 왜냐하면 인스턴스가 만들어지기 전부터 사용할 수 있어야 하기 때문이다.

## 4. 사용하기

~~~ java
package classpart;

public class Test {
    public static void main(String[] args) {
       School school1 = School.getInstance();
       School school2 = School.getInstance();
       
       System.out.println(school1==school2);
    }
}
~~~

실행결과

true

school1 참조변수와 school2 참조변수 모두 같은 인스턴스를 참조한다.