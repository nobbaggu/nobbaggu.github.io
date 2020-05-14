---
title: (Java) 53. Scanner 클래스
date: 2018-12-31T14:41:34+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - Java
  - scanner
  - stream
  - System.in
  - 스트림
  - 입력
  - 자바
---
Scanner 클래스는 java.util 패키지의 클래스로 입력 스트림을 제공한다. Scanner 클래스로 만든 입력 스트림은 정수, 실수, 문자 등 다양한 자료를 읽어들일 수 있다. 간편하게 사용할 수 있어 자주 사용하는 클래스이므로 알아두면 편리하다.

&nbsp;

# 1. Scanner 클래스

* * *

Scanner 클래스의 참조자료에 객체를 만들어 저장해두고 다양한 메소드를 호출하여 사용할 수 있다.

~~~ java
package stream;

import java.util.Scanner;

public class SystemInTest {

   public static void main(String[] args) {
      
      Scanner scanner = new Scanner(System.in); //System.in = 표준 입력 스트림 ->키보드 입력을 받기위해 System.in을 생성자 매개변수로 전달
      
      System.out.print("Name: ");
      String name = scanner.nextLine(); //문자열 입력 메소드
      System.out.print("Address: ");
      String addr = scanner.nextLine(); //문자열 입력 메소드
      System.out.print("Age: ");
      int age = scanner.nextInt(); //정수 입력 메소드
      
      System.out.println("Name: "+name+ ", Age: "+age+ ", Address: "+addr);
   }

}
~~~

실행결과

Name: John


Address: Seoul


Age: 29


Name: John, Age: 29, Address: Seoul


 

Scanner 객체를 만들 때 생성자의 매개변수로 System.in, 즉 표준 입력 스트림을 전달해주었는데, 이는 키보드로 입력을 하기 위해서이다. 이외에도 파일이나 문자열 입력도 받을 수 있다.

이것을 하기위해 System.in 표준입력스트림의 read() 메소드를 사용했다면 while문을 써서 읽어들여야 했을 것이다. 또한 예외처리까지 해주어야 해서 코드가 더 복잡해지고 과정이 번거로울 것이다.