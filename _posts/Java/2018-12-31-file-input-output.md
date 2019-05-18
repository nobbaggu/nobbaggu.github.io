---
title: (Java) File 입출력
date: 2018-12-31T16:16:54+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - file
  - FileInputStream
  - FileOutputStream
  - input()
  - Java
  - output
  - stream
  - 스트림
  - 입력
  - 자바
  - 출력
  - 파일
---
InputStream은 byte단위로 데이터를 읽는 스트림 중 최상위 클래스이다. InputStream은 추상클래스이며 여러 추상메소드를 가지고 있다. InputStream을 상속받은 하위 클래스는 추상메소드를 재정의하여 사용한다.

InputStream 클래스의 하위 클래스 중 가장 많이 사용 하는 입력 스트림 중 하나가 FileInputStream이다. FileInputStream은 파일 자료를 읽어들이는 스트림이다.

비슷한 출력 클래스로는 FileOutputStream 클래스가 있다.

&nbsp;

# 1. FileInputStream

* * *

현재 프로젝트 폴더에 hello.txt 라는 파일을 생성하고 아무 내용이나 적어본다.

Let's play baseball in the park!

위와 같은 내용을 적고 이 파일을 읽어보겠다.

~~~ java
package stream;

import java.io.FileInputStream;

import java.io.IOException;

public class FileInputStreamTest {

   public static void main(String[] args) {
      FileInputStream fis = null; //FileInputStream 자료형 참조변수 생성
      
      try {
         fis = new FileInputStream("hello.txt"); //새로운 파일 객체 생성하여 fis에 대입
         System.out.println((char)fis.read());
         System.out.println((char)fis.read());
         System.out.println((char)fis.read());
         System.out.println((char)fis.read());
      } catch (IOException e) {
         System.out.println(e);
      } finally {
         try {
            fis.close();
         } catch (IOException e) {
            System.out.println(e);
         } catch(NullPointerException e) {
            System.out.println(e);
         }
      }
   }

}
~~~

실행결과

L


e


t


'


 4개 까지 읽어들였다.

FileInputStream의 read() 메소드는 반환형이 int이다. 그리고 더 이상 읽을 내용이 없을 때 -1을 반환한다. 이 성질을 이용하여 파일의 내용을 끝까지 읽어들일 수 있도록 코드를 변환해보자.



~~~ java
package stream;

import java.io.FileInputStream;

import java.io.IOException;

public class FileInputStreamTest {

   public static void main(String[] args) {
      FileInputStream fis = null;
      
      try {
         fis = new FileInputStream("hello.txt");
         int ch;
         while((ch = fis.read()) != -1) {
            System.out.println((char)ch);
         }
      } catch (IOException e) {
         System.out.println(e);
      } finally {
         try {
            fis.close();
         } catch (IOException e) {
            System.out.println(e);
         } catch(NullPointerException e) {
            System.out.println(e);
         }
      }
      
   }

}
~~~

더 이상 읽을 내용이 없을 때 read() 메소드가 -1을 반환하는 성질을 이용하였다. while 반복문을 통해서 read() 메소드가 -1을 뱉어낼 때 까지 파일의 내용을 1byte씩 출력한 트릭이다.

read(byte[] b) 메소드는 매개변수로 전달된 byte[] 배열의 크기 만큼씩 읽어들인다.

~~~ java
package stream;

import java.io.FileInputStream;

import java.io.IOException;

public class FileInputStreamTest {

   public static void main(String[] args) {
      
      try(FileInputStream fis = new FileInputStream("hello.txt")) { //자동으로 close() 메소드 호출을 위한 try with source 방식 스트림 생성
         
         byte[] bs = new byte[10];
         int i;
         while((i = fis.read(bs))!=-1) {
            for(byte b : bs) {
               System.out.print((char)b);
            }
            System.out.println();
         }
      } catch (IOException e) {
         System.out.println(e);
      }
      
   }

}
~~~

hello.txt의 내용을 "abcdefghijklmnopqrstuvwxyz"로 바꾸고 실행시켜보면 다음과 같은 결과가 나온다.

실행결과

abcdefghij


klmnopqrst


uvwxyzqrst


 그런데 마지막에 qrst가 한 번 더 출력되었다. 26개의 알파벳을 세 번째 읽을때는 6byte 의 내용만 남아있고 이것을 읽어들여 bs배열 앞 6byte에만 저장하였기 때문이다. 기존의 마지막 4byte 내용이 그대로 남아있어 bs를 출력하면 위와같은 결과가 나온 것이다.

&nbsp;

# 2. FileOutputStream

* * *

FileOutputStream은 바이트 단위 출력의 최상위 스트림인 OutputStream 추상 클래스를 상속받는 클래스이다. 파일 출력이라함은 파일에 데이터를 쓴다는 말이다. 가장 자주 쓰는 메소드는 FileInputStream의 read() 메소드와 반대의 기능을 하는 write() 메소드이다.

~~~ java
FileOutputStream fos = new FileOutputStream("test.txt");
~~~

"test.txt"라는 파일이 없으면 새로 생성한다. 만약 있다하더라도 기존 내용은 삭제되고 새로 쓴다. 기존 파일의 뒷부분에 내용을 이어서 쓰려면 생성자에 매개변수로 넣어주면 된다.

~~~ java
FileOutputStream fos = new FileOutputStream("test.txt", true);
~~~

FileOutputStream의 생성자는 여러가지를 지원하는데, (String name)만 써줄 수도있고 (String name, boolean append)로 두 개를 넣어줄 수도 있다. boolean append는 내용을 덧붙일 것인지 말 것인지 여부를 결정하는 매개변수이고 default값은 false이다.

~~~ java
package stream;

import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamTest {

   public static void main(String[] args) {
      try(FileOutputStream fos = new FileOutputStream("test.txt")) {
         fos.write(97); //'a'
         fos.write(98); //'b'
         fos.write(99); //'c'
      } catch (IOException e) {
         System.out.println(e);
      }
   }

}
~~~

프로젝트 폴더에 들어가보면 "test.txt"파일이 생성된 것을 볼 수 있고 열어서 내용을 확인해보면 내용이 "abc"로 있을 것이다. FileOutputStream은 정수를 해당 아스키코드에 해당하는 문자 값으로 자동으로 변환하여 저장한다.

이제는 뒤에 내용을 덧붙여서 써보겠다.

~~~ java
package stream;

import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamTest {

   public static void main(String[] args) {
      try(FileOutputStream fos = new FileOutputStream("test.txt", true)) {
         fos.write(100); //'d'
         fos.write(101); //'e'
         fos.write(102); //'f'
      } catch (IOException e) {
         System.out.println(e);
      }
   }

}
~~~

스트림의 생성자 두 번째 매개변수로 true를 넣어주었다.

wirte() 메소드에 매개변수를 넣어주어 조금 더 세부적인 기능을 조절할 수 있다.

write(byte[] b, int off, int len) 메소드는 b크기 배열의 index off부터 길이 len만큼의 데이터를 쓰는 메소드이다.

~~~ java
package stream;

import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamTest {

   public static void main(String[] args) {
      try(FileOutputStream fos = new FileOutputStream("test.txt")) {
         byte[] bs = new byte[26];
         byte data = 'a'; //문자 'a'의 아스키 코드 저장
         for(int i=0; i<bs.length; i++) {
            bs[i] = data;
            data++;
         }
         
         fos.write(bs, 3, 10);
      } catch (IOException e) {
         System.out.println(e);
      }
   }

}
~~~

길이 26의 byte 배열인 bs에는 'a'부터 'z'까지 26개의 알파벳이 저장되어있다. bs의 3번째 index부터 10개만 파일에 쓰기위해 fos.write(bs, 3, 10); 과 같이 메소드를 호출했다. 코드를 실행시키고 파일을 열어보면 파일에 다음과 같이 내용이 저장되어있다.

defghijklm

&nbsp;