---
title: Object 클래스
date: 2018-12-23T00:56:38+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - class
  - equals
  - hashCode
  - Java
  - method
  - object
  - toString
  - 메소드
  - 자바
  - 클래스
  - 해쉬코드
---
모든 클래스의 최상위 클래스는 Object 클래스이다. 컴파일러는 모든 클래스의 뒷부분에 'extends Object'를 자동으로 추가하여 컴파일한다. 따라서 우리는 Object 클래스의 모든 메소드를 호출하여 사용할 수 있었다.

&nbsp;

# 1. Object 클래스란?

* * *

java.lang 패키지의 클래스이며 자바의 모든 클래스들의 최상위 클래스이다. Object 클래스 뿐만 아니라 java.lang 패키지의 모든 클래스는 컴파일 과정에서 자동으로 import문으로 추가하게 되어있다. 그 유명한 String 클래스도 java.lang 패키지의 하위 클래스이다. 그래서 우리가 지금까지 String 클래스를 사용하여 문자열을 표현할 수 있었던 것이다.

~~~ java
import java.lang.*; // 컴파일러가 java.lang 패키지의 모든 하위 클래스 import
~~~

~~~ java
// 컴파일러가 자동으로 extends Object 구문 추가
class Student{                       class Student extends Object {
   int studentID;          ->           int studentID;
   String studentName;                  String studentName;
}                                    }


~~~

&nbsp;

&nbsp;

# 2. Object 클래스 대표적인 메소드들

* * *

## 1) String toString()

String타입의 toString() 메소드는 객체의 정보를 반환한다. 기본적으로 객체를 System.out.println함수 안에 집어넣으면 .toString()이 자동으로 붙는다.

~~~ java
package objectClass;

public class Book {
   int bookNumber;
   String bookTitle;
   
   Book(int bookNumber, String bookTitle){
      this.bookNumber = bookNumber;
      this.bookTitle = bookTitle;
   }
}
~~~

&nbsp;

~~~ java
package objectClass;

public class ToStringEx {

   public static void main(String[] args) {
      Book book1 = new Book(100, "Titanic");
      
      System.out.println(book1);
      System.out.println(book1.toString());     
   }
}
~~~

실행결과

objectClass.Book@15db9742


objectClass.Book@15db9742


 

Object.class 파일에 정의되어있는 toString() 메소드의 원형은 다음과 같다.

~~~ java
public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }
~~~

그래서 지금까지 객체의 정보를 출력하면  _클래스 이름@해쉬코드값_  이 출력된 것이었다.

~~~ java
package objectClass;

public class ToStringEx {

   public static void main(String[] args) {
      Book book1 = new Book(100, "Titanic");
      
      System.out.println(book1);
      System.out.println(book1.toString());
      
      String str = new String("test");
      System.out.println(str);
      Integer i1 = new Integer(100);
      System.out.println(i1);
   }

}
~~~

그러나 str과 il은 클래스이름@해쉬코드 값이 출력되는게 아니라 문자열과 정수값이 출력된다. 모든 클래스는 Object클래스를 상속받는다고 했다. String과 Integer 클래스도 마찬가지다. 이 두 클래스들은 Object 클래스를 상속받고 toString() 메소드를 재정의한다.

~~~ java
//String 클래스의 toString() 오버라이딩
public String toString() {
        return this;
}
~~~

&nbsp;

~~~ java
//Integer 클래스의 toString() 오버라이딩
public String toString() {
        return toString(value);
}
~~~

&nbsp;

그렇다면 우리가 직접 Book 클래스에서 toString()을 재정의 해보자.

~~~ java
package objectClass;

public class Book {
   int bookNumber;
   String bookTitle;
   
   Book(int bookNumber, String bookTitle){
      this.bookNumber = bookNumber;
      this.bookTitle = bookTitle;
   }
}
~~~

~~~ java
package objectClass;

public class ToStringEx {

   public static void main(String[] args) {
      Book book1 = new Book(100, "Titanic");
      
      System.out.println(book1);
   }

}
~~~

실행결과

100,Titanic


 

## 2) boolean equals(Object obj)

equals() 메소드는 두 객체를 비교하여 같은 객체면 true, 다른 객체면 false를 반환한다.

~~~ java
package objectClass;

public class ToStringEx {

   public static void main(String[] args) {
      Book book1 = new Book(100, "Titanic");
      Book book2 = new Book(200, "LionKing");
      Book book3 = book1; //book3 참조변수에 book1을 대입
      
      System.out.println(book1);
      System.out.println(book2);
      System.out.println(book3);
      
      System.out.println(book1.equals(book2));
      System.out.println(book1.equals(book3));
   }

}
~~~

실행결과

objectClass.Book@15db9742


objectClass.Book@6d06d69c


objectClass.Book@15db9742


false


true

book3 참조변수에 book1을 대입하였다. 따라서 이 두 참조변수는 같은 인스턴스를 가르킬 것이다. 그리고 book2에는 다른 인스턴스를 생성하여 저장하였다. 따라서 book1.equals(book2)는 false, book1.equals(book3)는 true를 반환한다.

String 클래스와 Integer 클래스에서는 equals()메소드가 이미 재정의 되어있다. 먼저 코드를 보고 결과를 보자.

~~~ java
package objectClass;

public class ToStringEx {

   public static void main(String[] args) {
      String str1 = new String("hello");
      String str2 = new String("hello");
      
      Integer int1 = new Integer(100);
      Integer int2 = new Integer(100);
      
      System.out.println(str1==str2);
      System.out.println(str1.equals(str2));
      System.out.println(int1==int2);
      System.out.println(int1.equals(int2));
   }

}
~~~

실행결과

false


true


false


true


 

==은 말 그대로 두 객체의 주소값을 비교한다. 그러나 Integer와 String 클래스에서 equals() 메소드가 두 문자열 자체 혹은 정수 자체를 비교하여 같으면 true를 반환하도록 재정의 되어있다.

~~~ java
package objectClass;

public class Book {
   int bookNumber;
   String bookTitle;
   
   Book(int bookNumber, String bookTitle){
      this.bookNumber = bookNumber;
      this.bookTitle = bookTitle;
   }

   @Override
   public boolean equals(Object obj) {
      
      if(obj instanceof Book) { //매개변수의 자료형이 같다면 비교 시작
         Book book = (Book)obj; //obj객체를 Book타입으로 변환하여 book에 저장
         if(this.bookNumber == book.bookNumber) { //bookNumber가 같으면 true, 다르면 false 반환
            return true;
         }
         else {
            return false;
         }
      }
      else //매개변수의 자료형이 다르면 false 반환
      {
         return false;
      }
   }

}
~~~

Book 클래스에서 책 번호가 같으면 같은 것으로, 다르면 다른 것으로 인식되도록 직접 equals() 메소드를 재정의 하여보았다.

&nbsp;

## 3) int hashCode() 메소드

hashCode()는 객체가 저장된 힙 메모리의 주소를 반환한다.

~~~ java
package objectClass;

public class ToStringEx {

   public static void main(String[] args) {
      Book book1 = new Book(100, "hello");
      Book book2 = new Book(100, "hi");
      
      System.out.println(book1.hashCode());
      System.out.println(book2.hashCode());
   }
}
~~~

실행결과

366712642


1829164700


 두 객체는 서로 다른 객체이므로 hashCode값이 다르다. Object 클래스의 toString() 메소드는 클래스이름@메모리 주소 를 반환하였었다. 이 때 메모리 주소는 hashCode값을 16진수로 변환한 값이었다.

&nbsp;