---
title: (Java) 56 - 보조 스트림
date: 2018-12-31T19:14:46+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - BufferedInputStream
  - BufferedOutputStream
  - FilterInputStream
  - FilterOutputStream
  - InputStreamReader
  - Java
  - OutputStreamWriter
  - stream
  - 보조
  - 스트림
  - 자바
---
보조 스트림이란 입출력을 직접 하지 않고 기반 스트림에 추가적인 기능을 부여하는 스트림이다. 사용 방법은 보조스트림 객체를 만들 때 생성자로 기반스트림을 전달해주는 것이다.

보조 스트림은 2개 이상을 사용할 수도 있다. 이 때에는 보조스트림에 생성자로 보조스트림을 전달해주면서 전달된 보조스트림의 생성자에 매개변수로 기반스트림을 주는 식으로 할 수 있다.

보조 스트림의 최상위 클래스는 FilterInputStream과 FilterOutputStream이다. 두 추상클래스의 생성자는 스트림 1개를 매개변수로 전달받는다. 다른 생성자는 지원하지 않는다.

&nbsp;

# 1. InputStreamReader / OutputStreamWriter

* * *

데이터를 바이트 단위로만 전송할 수 있을 때 한글과 같이 2byte가 필요한 데이터가 있다. 이런 경우에 사용할 수 있는 보조스트림이 InputStreamReader와 OutputStreamWriter이다. 이 두 스트림은 byte단위 데이터를 문자로 변환해주는 보조 스트림이다.

~~~ java
package stream;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;

public class InputStreamReaderTest {

   public static void main(String[] args) {
      
      try(InputStreamReader isr = new InputStreamReader(new FileInputStream("hello.txt"))){
         
         int i;
         while((i = isr.read()) != -1) {
            System.out.print((char)i);
         }
      } catch(IOException e) {
         System.out.println(e);
      }
   }

}
~~~

실행결과

"자바를자바라"

원래같은 경우라면 FileInputStream은 byte단위로 데이터를 읽기 때문에 한글 문자를 제대로 읽어들일 수 없다. 따라서 보조스트림으로 InputStreamReader 클래스를 사용하였다.

&nbsp;

OutputStreamWriter의 사용법 역시 이와 비슷하다.

&nbsp;

# 2. BufferedInputStream / BufferedOutputStream

* * *

BufferedInputStream과 BufferedOutputStream은 버퍼링 기능을 제공하는 보조 스트림이다. 따라서 1byte씩 데이터를 전송하는 스트림보다 더 빨리 입출력을 실행한다. 내부적으로 8192byte 크기의 배열을 가지고 있다.

BufferedInputStream은 생성자의 매개변수로 InputStream 하위클래스를, BufferedOutputStream은 OutputStream 하위클래스를 받는다. 또한 BufferedReader는 Reader, BufferedWriter은 Writer 의 하위 클래스를 생성자의 매개변수로 받는다.

~~~ java
package stream;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class StreamReaderWriterTest {

   public static void main(String[] args) {
      try (FileInputStream fis = new FileInputStream("test.jpg")){
         FileOutputStream fos = new FileOutputStream("copy.jpg");
         
         long t1 = System.currentTimeMillis(); //시간 측정 시작
         
         int i;
         while((i = fis.read()) != -1) { //FileInputStream으로 읽어서
            fos.write(i); //FileOutputStream으로 쓰기
         }
         
         long t2 = System.currentTimeMillis(); //시간 측정 끝
         
         System.out.println(t2-t1);
         
      } catch (IOException e) {
         e.printStackTrace();
      }
      
   }

}

~~~

실행결과

5584


 byte단위 스트림으로 파일을 읽어 내용을 쓰는 방식으로 복사하였는데 5.584초가 걸린다.

Buffered 스트림 하위 클래스를 사용하여 속도가 개선되는지 확인해보자.

~~~ java
package stream;

import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class StreamReaderWriterTest {

   public static void main(String[] args) {
      try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream("test.jpg"))){
         BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copy.jpg")) ;
         
         long t1 = System.currentTimeMillis(); //시간 측정 시작
         
         int i;
         while((i = bis.read()) != -1) { //BufferedInputStream으로 읽어서
            bos.write(i); //BufferedOutputStream으로 복사
         }
         
         long t2 = System.currentTimeMillis(); //시간 측정 끝
         
         System.out.println(t2-t1);
         
      } catch (IOException e) {
         e.printStackTrace();
      }
      
   }

}
~~~

실행결과

39


 0.039초가 걸린다. 아주 심하게 차이가 난다.