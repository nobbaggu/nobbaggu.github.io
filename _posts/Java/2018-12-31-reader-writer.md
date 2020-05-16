---
title: (Java) 55 - Reader와 Writer
date: 2018-12-31T16:40:56+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - file
  - Java
  - reader
  - writer
  - 쓰기
  - 읽기
  - 자바
  - 파일
---
# 1. FileReader 클래스

* * *

FileInputStream과 FileOutputStream은 byte 단위로 데이터를 전송한다. 그런데 한글같은 경우는 표현하기 위해 2byte를 사용해야 한다. "hello.txt"파일에 한글로 내용을 적고 읽어보면 다음과 같이 된다.

~~~ java
package stream;

import java.io.FileInputStream;

import java.io.IOException;

public class FileInputStreamTest {

   public static void main(String[] args) {
      
      try(FileInputStream fis = new FileInputStream("hello.txt")) {
         
         int i;
         while((i = fis.read())!=-1) {
            System.out.print((char)i);
         }
      } catch (IOException e) {
         System.out.println(e);
      }
      
   }

}
~~~

실행결과

"??¹?¸???¹?¶?"

문자가 깨진다.

이런경우에 사용하는 것이 문자 단위로 데이터를 읽는 스트림의 최사위 클래스인 Reader 클래스이다. 가장 대표적인 하위클래스인 FileReader를 통해 다시 파일 내용을 읽어보겠다.

~~~ java
package stream;

import java.io.FileReader;
import java.io.IOException;

public class FileInputStreamTest {

   public static void main(String[] args) {
      
      try(FileReader fr = new FileReader("hello.txt")) {
         
         int i;
         while((i = fr.read())!=-1) {
            System.out.print((char)i);
         }
      } catch (IOException e) {
         System.out.println(e);
      }
      
   }

}
~~~

실행결과

"자바를자바라"

한글이 깨지지않고 잘 출력되는 것을 볼 수 있다.

&nbsp;

# 2. FileWriter 클래스

* * *

~~~ java
package stream;

import java.io.FileWriter;
import java.io.IOException;

public class FileOutputStreamTest {

   public static void main(String[] args) {
      try(FileWriter fw = new FileWriter("test.txt")) {
         
         char[] buf = {'B', 'C', 'D'};
         
         fw.write('A');
         fw.write('가');
         fw.write(buf, 1, 2);
         fw.write("자바를자바라");
         fw.write(65);
         
         
      } catch (IOException e) {
         System.out.println(e);
      }
   }

}
~~~

배열, 문자, 문자열은 물론 한글도 잘 써진다.

실행결과

A가CD자바를자바라A

&nbsp;