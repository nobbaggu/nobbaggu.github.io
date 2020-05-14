---
title: (Java) 27. 배열 복사
date: 2018-12-16T18:54:18+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - arraycopy
  - Java
  - 객체
  - 배열
  - 복사
  - 자바
format: aside
---
# 1. arraycopy 메소드

* * *

~~~ java
arraycopy(src, srcPos, dest, destPos, length);
~~~

src : 복사할 배열

srsPos : 복사할 시작 index

dest : 붙여넣을 배열

destPos : 붙여넣을 시작 index

length : 붙여넣을 갯수

&nbsp;

~~~ java
package classpart;

public class Test {
    public static void main(String[] args) {
       Student[] stus1 = new Student[3];
       Student[] stus2 = new Student[3];
       
       stus1[0] = new Student();
       stus1[1] = new Student();
       stus1[2] = new Student();
       
       stus1[0].setName("John");
       stus1[0].setStudentID(1001);
       stus1[1].setName("Kelly");
       stus1[1].setStudentID(1002);
       stus1[2].setName("Paul");
       stus1[2].setStudentID(1003);
       
       //stus1을 stus2에 복사
       System.arraycopy(stus1, 0, stus2, 0, stus1.length);
       
       for(int i=0; i<stus2.length; i++) {
             stus2[i].showInfo();
       }
       
       System.out.println();
       
       //stus1의 첫 번째 객체 정보 변경
       stus1[0].setName("Mac");
       stus1[0].setStudentID(9999);
       
       //stus2의 모든 객체 정보 출력
       for(int i=0; i<stus2.length; i++) {
             stus2[i].showInfo();
       }
    }
}
~~~

실행결과

1001, John


1002, Kelly


1003, Paul




9999, Mac


1002, Kelly


1003, Paul


 

stus1의 첫 번째 요소가 가르키는 객체 정보를 변경했는데 stus2의 첫 번째 요소의 객체 정보도 변경되었다.

arraycopy() 메소드의 원리를 보면 이유를 알 수 있다.

어차피 객체 배열에 요소로 저장되어있는 것은 객체의 주소 정보이다. arraycopy() 메소드는 객체의 주소값을 복사해서 넘겨주는 메소드이다. 따라서 stus1과 stus2의 각 요소는 같은 객체를 가르키고 있는 참조변수인 것이다. 그래서 stus1로 객체에 접근해 정보를 변경하면 stus2를 통해 출력해도 바뀐 정보가 나오는 것이다.

이를 얕은 복사라고 한다.

&nbsp;

&nbsp;

# 2. 깊은 복사

* * *

~~~ java
package classpart;

public class Test {
    public static void main(String[] args) {
       Student[] stus1 = new Student[3];
       Student[] stus2 = new Student[3];

       stus1[0] = new Student();
       stus1[1] = new Student();
       stus1[2] = new Student();

       stus1[0].setName("John");
       stus1[0].setStudentID(1001);
       stus1[1].setName("Kelly");
       stus1[1].setStudentID(1002);
       stus1[2].setName("Paul");
       stus1[2].setStudentID(1003);

       stus2[0] = new Student();
       stus2[1] = new Student();
       stus2[2] = new Student();

       for(int i=0; i<stus1.length; i++) {
          stus2[i].setName(stus1[i].getName());
          stus2[i].setStudentID(stus1[i].getStudentID());
       }

       for(int i=0; i<stus2.length; i++) {
             stus2[i].showInfo();
       }

       System.out.println();

       stus1[0].setName("Assy");
       stus1[0].setStudentID(10000);

       for(int i=0; i<stus2.length; i++) {
             stus2[i].showInfo();         
       }
    }
}
~~~

실행결과

1001, John


1002, Kelly


1003, Paul




1001, John


1002, Kelly


1003, Paul


 

stus2 인스턴스도 따로 만들고 stus1의 get() 메소드로 정보만 출력해서 저장하였다. 이 경우는 stus1과 stus2의 각 요소가 서로 다른 인스턴스를 가르키기 때문에 stus1을 통해 객체 정보를 변경한다고 해서 stus2가 참조하는 객체는 영향을 받지 않는다.