---
title: (Java) Java 입출력
date: 2018-12-31T14:16:26+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - byte
  - char
  - input()
  - InputStream
  - Java
  - output
  - OutputStream
  - PrintStream
  - stream
  - System.in.read()
  - 기반
  - 문자
  - 바이트
  - 보조
  - 스트림
  - 입력
  - 입출력
  - 자바
  - 출력
  - 표준
---
입출력이란 **데이터 이동의 방향**을 말한다. 입력은 프로그램으로 데이터가 들어오는 것, 출력은 데이터를 내보내는 것이라고 할 수 있다. 대표적으로 모니터, 키보드, 마우스부터 네트워크 등 모든 것이 데이터를 읽고 써야 제 기능을 한다. 이 때 장치마다 입출력을 제각각 구현해야한다면 비생산적이고 장치간의 호환성도 떨어질 것이다.

Java는 장치에 의존하지 않는 **일관적인 데이터의 입출력을 위해 스트림이라는 것을 제공**한다. 스트림은 데이터의 이동 통로이다. 스트림은 시냇물, 개울을 뜻한다. 데이터가 이동하는 모습이 물이 흐르는 것과 비슷하다하여 붙여진 이름이다.

<a href="https://SWnomad.com/?attachment_id=1066" rel="attachment wp-att-1066"><img class="aligncenter wp-image-1066" src="/images/2018/09/1-14.jpg" alt="" width="393" height="197" srcset="/images/2018/09/1-14.jpg 519w, /images/2018/09/1-14-300x150.jpg 300w" sizes="(max-width: 393px) 100vw, 393px" /></a>

# 1. 스트림의 세 가지 분류 방식

* * *

## 1) 입력 vs 출력

스트림은 데이터를 오직 한 방향으로만 보낼 수 있도록 제공된다. 따라서 하나의 스트림으로 입력과 출력을 동시에 할 수 없어 따로 만들어 주어야한다.

입력 스트림 : FileInputStream, FileReader, BufferedInputStream, BufferedReader 등

출력 스트림 : FileOutputStream, FileWriter, BufferedOutputStream, BufferedWriter 등

&nbsp;

## 2) 바이트 vs 문자

Java의 스트림은 기본적으로 데이터를 바이트(byte)단위로 묶어 이동시킨다. 그러나 문자를 나타내는 char 자료형은 2byte이며 이를 1byte씩 이동시킨다면 문자가 깨지는 현상이 발생한다. 따라서 문자 데이터를 위해 별도로 2byte씩 데이터를 이동시키는 문자 스트림을 따로 제공한다.

클래스 이름이 Stream으로 끝나면 byte 단위로 데이터를 처리하는 스트림이고 Reader, Writer로 끝나면 문자 스트림 클래스이다.

바이트 스트림 : FileInputStream, FileOutputStream, BufferedInputStream, BufferedOutputStream 등

문자 스트림: FileReader, FileWriter, BufferedReader, BufferedWriter

&nbsp;

## 3) 기반 vs 보조

데이터를 직접 source로부터 읽어들이거나 target에 쓰는 기능을 제공하기 위해 직접 연결되는 기반스트림 이외에도 항상 다른 스트림에 포함하여 생성되어 보조역할을 하는 보조 스트림이 있다.

기반 스트림 : FileInputStream, FileOutputStream, FileReader, FileWriter 등

보조 스트림 : InputStreamReader, OutputStreamWriter, BufferedInputStream, BufferedOutputStream 등

&nbsp;

&nbsp;

# 2. 표준 I/O

* * *

표준 입출력은 키보드와 모니터를 대상으로 데이터를 읽고 쓰는 가장 기본적인 스트림이다. 이 스트림은 프로그램 시작과 동시에 자동으로 생성되므로 따로 만들어 줄 필요가 없다.

우리가 지금까지 많이 쓰던 System.out.println()을 한 번 뜯어보자. System 클래스에는 static 변수 out이 선언되어있다. out은 System의 하위클래스인 PrintStream 클래스 객체를 참조한다. 따라서 System.out을 통해 PrintStream의 println()메소드를 호출한 것이다. PrintStream은 표준 출력 스트림 클래스이다.

<table style="height: 109px;" width="687">
  <tr>
    <td style="width: 221px;">
      자료형
    </td>
    
    <td style="width: 222px;">
      변수 이름
    </td>
    
    <td style="width: 222px;">
      설명
    </td>
  </tr>
  
  <tr>
    <td style="width: 221px;">
      static PrintStream
    </td>
    
    <td style="width: 222px;">
      out
    </td>
    
    <td style="width: 222px;">
      표준 출력
    </td>
  </tr>
  
  <tr>
    <td style="width: 221px;">
      static InputStream
    </td>
    
    <td style="width: 222px;">
      in
    </td>
    
    <td style="width: 222px;">
      표준 입력
    </td>
  </tr>
  
  <tr>
    <td style="width: 221px;">
      static OutputStream
    </td>
    
    <td style="width: 222px;">
      err
    </td>
    
    <td style="width: 222px;">
      표준 오류 출력
    </td>
  </tr>
</table>

out은 표준 출력을 위한 참조변수, in은 표준 입력을 위한 참조변수, err는 표준 오류 출력을 위한 참조변수이며 모두 System클래스 안에 정의되어 있다. System.err를 통해 빨간색의 오류 메세지를 출력할 수 있다.

예제를 하나 보자.

~~~ java
package stream;

import java.io.IOException;

public class SystemInTest {

   public static void main(String[] args) {
      
      int myData;
      
      while(true) {
         try {
            myData = System.in.read(); //1byte 읽어들임
            if(myData != '\n') { //엔터가 아니면
               System.out.print((char)myData); //하나 출력하고
               continue;//계속
            }
            else { //myData == '\n'(엔터)이면
               break; //while문 탈출
            }
         } catch (IOException e) {
            e.printStackTrace();
         }
         
      }
   }

}
~~~

실행결과

hello world!


hello world!


 

in은 InputStream클래스의 객체를 참조하는데, InputStream클래스는 바이트 단위 스트림이므로 1byte씩만 읽어들인다. 따라서 ASCII코드가 아닌 문자 형태로 출력하려면 char형으로 강제 형변환을 해주어야 한다.