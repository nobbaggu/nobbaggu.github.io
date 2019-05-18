---
title: (Java) 참조 자료형
date: 2018-12-15T11:48:46+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - 자료형
  - 참조
---
클래스는 하나의 자료형이 될 수 있다.

우리가 문자열을 선언할 때 흔히 쓰는 String 자료형도 int, char, double처럼 기본 자료형이 아닌 참조 자료형이다. 누군가 JDK에 만들어 놓은것을 사용할 뿐이다.

아무튼 클래스가 하나의 자료형이 될 수 있고 그것을 참조 자료형이라고 부른다는 것이 중요하다. 여기에서 객체지향 프로그래밍의 맛보기를 알 수 있다.

~~~ java
package classpart;

public class Subject {
    int midScore;
    int finScore;

    public Subject(int mid, int fin) {
        midScore = mid;
        finScore = fin;
    }
}
~~~

&nbsp;

~~~ java
package classpart;

public class Student {
   int stuID;
   String stuName;
   Subject math = new Subject(50,90);
   Subject physics = new Subject(80,40);
}
~~~

&nbsp;

어떤 학생의 학번, 이름, 수강과목의 중간,기말 점수를 저장하고 싶을 때 Student 클래스안에 모두 선언하면 복잡할 것이다. 이렇게 할 경우 수학의 중간,기말과 물리의 중간,기말 총 4개를 더 추가해야한다. 하지만 수강과목을 하나의 클래스로 외부에 선언하고 Student 클래스에 Subject 참조 자료형의 변수를 선언하고 객체를 만들어주면 편리하다.

이처럼 객체 상호간의 유기작용을 프로그래밍 하는것이 객체지향 프로그래밍의 핵심이다.