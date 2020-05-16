---
title: (Java) 57 - 객체 직렬화
date: 2018-12-31T20:28:32+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - Java
  - ObjectInputStream
  - ObjectOutputStream
  - serializable
  - serialize
  - 역직렬화
  - 자바
  - 직렬화
---
인스턴스 정보를 전송하기 위해 직렬화를 시킨다. 이후 전송받아 내용을 복원하는 것을 역직렬화라고 한다. 즉, 직렬화란 인스턴스 참조변수를 스트림을 통해 전송하기 위해 하는 일이다.

이 때 사용하는 클래스가 ObjectInputStream과 ObjectOutputStream이다.

~~~ java
ObjectInputStream(InputStream in)
ObjectOutputStream(OutputStream out)
~~~

ObjectInputStream은 InputStream 객체를 생성자의 매개변수로 받는다. ObjectOutputStream은 OutputStream 객체를 생성자의 매개변수로 받는다.

&nbsp;

# 1. 직렬화 / 역직렬화

* * *

Person 클래스를 정의하고 인스턴스를 만들어 직렬화를 시킨다음 스트림으로 출력하여 파일에 저장하고 다시 읽어보자.

~~~ java
package stream;

import java.io.Serializable;

public class Person implements Serializable { //직렬화를 해야하므로 Serializable 인터페이스 구현
   String name;
   int age;
   
   public Person() {}
   
   public Person(String name, int age) {
      this.name = name;
      this.age = age;
   }
   
   @Override
   public String toString() {
      return name+","+age;
   }
}
~~~

&nbsp;

~~~ java
package stream;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;

public class SerializationTest {

   public static void main(String[] args) throws ClassNotFoundException { //역직렬화 과정에서 Class 찾지 못할 수 있으므로 ClassNotFoundException 던지기
      
      Person per1 = new Person("John", 23);
      Person per2 = new Person("Kelly", 39);
      
      try(FileOutputStream fos = new FileOutputStream("serial.out")){
         
         ObjectOutputStream oos = new ObjectOutputStream(fos); //생성자 매개변수로 FileOutputStream 객체 전달
         oos.writeObject(per1); //ObjectOutputStream을 통해 "serial.out" 파일에 쓰기
         oos.writeObject(per2);
         
      }catch(IOException e) {
         System.out.println(e);
      }
      
      try(FileInputStream fis = new FileInputStream("serial.out")){
         
         ObjectInputStream ois = new ObjectInputStream(fis); //생성자 매개변수로 FileInputStream 객체 전달
         
         Person p1 = (Person)ois.readObject(); //반환형이 Object형이므로 Person으로 강제 형변환
         Person p2 = (Person)ois.readObject();
         
         System.out.println(p1);
         System.out.println(p2);
         
      }catch(IOException e) {
         System.out.println(e);
      }
            
   }

}
~~~

실행결과

John,23


Kelly,39

"serial.out" 파일을 열어보면 우리가 알 수 없는 형태로 객체가 저장되어 있는 것을 볼 수 있다. 우리가 다시 이것을 문자 형태로 읽으려면 역질렬화를 해야하는데, 이것을 ObjectInputStream 클래스를 통해 할 수 있다.

&nbsp;

# 2. 직렬화 제외

* * *

직렬화 과정에서는 모든 인스턴스 변수가 직렬화된다. 직렬화에서 제외하고 싶은 변수는 앞에 transient 예약어를 붙이면 된다. 이 예약어가 붙은 변수는 저장될 때 그 자료형의 기본값으로 저장된다. 가령 int형 멤버변수는 0, String 참조변수 값은 null로 저장되는 것이다.

위에서 사용한 코드에서 String name을 transient String name으로 변경하고 다시 실행시켜보자.

~~~ java
transient String name;
int age;
~~~

실행결과

null,23


null,39


 이름이 자동으로 null로 저장된다.

&nbsp;

&nbsp;