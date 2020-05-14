---
title: (Java) 40. String 클래스
date: 2018-12-23T01:30:34+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - class
  - concat
  - Java
  - string
  - StringBuffer
  - StringBuilder
  - 자바
  - 클래스
---
# 1. String 클래스

* * *

~~~ java
String str1 = new String("hello"); // str1은 힙 메모리에 생성 된 String클래스의 객체를 참조
String str2 = "hello"; // str2는 상수pool에 저장되어있는 hello를 참조
~~~

문자열을 생성하는 방법에는 두 가지가 있다. 첫 번째는 문자열을 String클래스의 생성자의 매개변수로 넣어서 생성하는 것이고, 두 번째는 프로그램이 시작되면서 상수pool에 할당되는 문자열을 가르키게 변수에 저장하는 방식이다.

### 1) String클래스 생성자로 생성

~~~ java
package stringClass;

public class StringTest {
   
   public static void main(String[] args) {
      String str1 = new String("hello");
      String str2 = new String("hello");
      
      System.out.println(str1.equals(str2)); // 같은 문자열인지 판단
      System.out.println(str1==str2); // 같은 객체인지 판단
   }

}
~~~

실행결과

true


false


 같은 문자열이더라도 객체를 생성할 때 마다 힙 메모리에 새로운 공간이 할당되어 서로 다른 객체가 된다.

&nbsp;

### 2) 상수pool에 저장 된 문자열 리터럴 가르키도록 생성

~~~ java
package stringClass;

public class StringTest {
   
   public static void main(String[] args) {
      String str1 = "hello";
      String str2 = "hello";
      
      System.out.println(str1.equals(str2)); // 같은 문자열인지 판단
      System.out.println(str1==str2); // 같은 객체인지 판단
   }

}
~~~

실행결과

true


true


 이번에는 두 참조변수가 같은 메모리 공간을 가르킨다는 결과가 나왔다. "hello" 문자열이 프로그램이 시작되면서 상수pool에 저장이 되고 이제부터 이 문자열을 저장하는 모든 참조변수는 이 상수풀의 "hello"를 가르키도록 지정된다. 따라서 위와 같은 결과가 나온 것이다.

&nbsp;

&nbsp;

# 2. StringBuild와 StringBuffer

* * *

~~~ java
package stringClass;

public class StringTest {
   
   public static void main(String[] args) {
      String str1 = new String("Java");
      String str2 = new String("Android");
      
      //str1의 주소값
      System.out.println(System.identityHashCode(str1));
      
      //str1에 str2 이어붙이기
      str1 = str1.concat(str2);
      System.out.println(str1);
      
      //str1의 주소값
      System.out.println(System.identityHashCode(str1));
   }

}
~~~

문자열을 이어붙였다. concat() 메소드는 문자열을 이어붙일 수 있게 해주는 String클래스의 메소드이다.

&nbsp;

실행결과

366712642


str1


1829164700


 

결과를 보면 str1에 문자열을 이어붙이면서 새로 생성된 문자열은 다른 주소에 저장되어 있다. 여기서 알 수 있는것은 **문자열이 변경될 때에는 새로운 메모리공간에 문자열이 생기고 변수가 그것을 가르키도록 갱신된다는 것이다.** 이렇게 되면 메모리 낭비가 생겨버린다. 이렇게 되는 이유는 문자열은 한 번 선언이 되고나면 '불변'하기 때문이다.

이 때 메모리 낭비를 해결할 수 있도록 StringBuild와 StringBuffer 클래스가 있다. 두 클래스는 문자열 변경의 안정성에 관련한 기능이 다른데, 문자열을 변경할 때에는 두 클래스 중 아무거나 사용해도 된다. 그럼 StringBuklder 클래스를 사용해서 문자열을 변경하는 코드를 한 번 보겠다.

~~~ java
package stringClass;

public class StringTest {
   
   public static void main(String[] args) {
      String str1 = new String("Java");
      String str2 = new String("Android");
      String str3 = new String("IOS");

      StringBuilder builder = new StringBuilder(str1);
      System.out.println(System.identityHashCode(builder)); //이어붙이기 전 builder 주소
      
      builder.append(str2);
      builder.append(str3);
      System.out.println(builder);
      
      System.out.println(System.identityHashCode(builder)); //이어붙인 후 builder 주소
   }

}
~~~

실행결과

366712642


JavaAndroidIOS


366712642


 문자열 변환작업을 하면서도 메모리 주소값이 변하지 않았다. 메모리 낭비를 해결한 것이다.