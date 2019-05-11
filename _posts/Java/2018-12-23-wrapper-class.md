---
title: Wrapper 클래스
date: 2018-12-23T15:14:04+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - class
  - int
  - Integer
  - intValue
  - Java
  - parseInt
  - valueOf
  - wrapper
  - 언박싱
  - 오토박싱
  - 자바
  - 클래스
---
기본 자료형은 정수, 실수, 문자를 지원한다. 그런데 정수나 실수, 문자의 객체형이 필요한 경우가 있다. 정수 객체의 매개변수 전달이나 반환형이 필요한 경우등이 있다.

자바는 이런 경우를 위해 기본 자료형과 같은 클래스를 지원하는데, 이러한 클래스들을 두고 Wrapper클래스라고 한다.

<a href="https://SWnomad.com/wrapper-%ed%81%b4%eb%9e%98%ec%8a%a4/%ec%a0%9c%eb%aa%a9-%ec%97%86%ec%9d%8c-193/" rel="attachment wp-att-1631"><img class="aligncenter  wp-image-1631" src="/images/2018/12/no-name-38.jpg" alt="" width="365" height="361" srcset="/images/2018/12/no-name-38.jpg 655w, /images/2018/12/no-name-38-300x297.jpg 300w" sizes="(max-width: 365px) 100vw, 365px" /></a>

&nbsp;

대표적으로 Integer 클래스로 Wrapper클래스의 사용법을 파헤쳐 본다. 다른 Wrapper클래스의 사용법도 거의 같으며 궁금한 것은 JavaDoc을 찾아보면 된다.

# 

# 1. Wrapper 클래스 사용법

* * *

~~~ java
package wrapperclass;

public class WrapperTest {

   public static void main(String[] args) {
      Integer intValue1 = new Integer(100);
      Integer intValue2 = new Integer("200"); 
      
      System.out.println(intValue1);
      System.out.println(intValue2);
   }

}
~~~

실행결과

100


200


 

Integer 클래스의 객체를 만들 때 생성자의 매개변수로는 정수형이나 문자열이 올 수 있다. **JavaDoc**에서 Integer.class 파일에서 클래스의 생성자를 찾아보면 어떤 식으로 객체를 생성해야할 지 알 수 있다.

~~~ java
private final int value;
.
.
public Integer(int value) {
   this.value = value;
}
~~~

Integer 클래스에는 value가 final int 형으로 선언되어 있어서 한 번 선언되면 그 값을 바꿀 수 없다. 우리가 생성자의 매개변수로 정수를 넣어주면 value에 저 값이 저장된다. Integer클래스의 intValue() 메소드는 value 값을 반환해주는 메소드이다. 지금부터 Integer 클래스의 대표적인 메소드들에 대해 살펴본다.

&nbsp;

&nbsp;

# 2. Integer클래스의 주요 메소드

* * *

## 1) int intValue()

&nbsp;

~~~ java
package wrapperclass;

public class WrapperTest {

   public static void main(String[] args) {
      Integer iValue = new Integer(100);
      int theValue = iValue.intValue();
      
      System.out.println(theValue);
   }
}
~~~

실행결과

100


 intValue 메소드로 클래스의 상수인 value 값을 불러왔다.

&nbsp;

참고로 intValue() 메소드의 반환형은 기본 자료형 int이다.

&nbsp;

## 2) Integer valueOf()

valueOf() 메소드는 매개변수로 정수나 문자열이 들어오면 정수형으로 다시 되돌려주는 메소드이다. 이 메소드는 static으로 정의되어 있어 클래스 메소드이다. 따라서 객체를 생성하지 않고도 사용 가능하다. 그리고 메소드의 반환형은 Integer클래스 형이다.

~~~ java
package wrapperclass;

public class WrapperTest {

   public static void main(String[] args) {
      int theValue1 = Integer.valueOf(100);
      int theValue2 = Integer.valueOf("200");
      
      System.out.println(theValue1);
      System.out.println(theValue2);
   }

}
~~~

실행결과

100


200

정수를 문자열 형태로 넣어줘도 정수형으로 변환하여 돌려준다. 사실 이 메소드 안에서는 정수를 return해줄 때 parseInt라는 메소드를 사용한다. parseInt 메소드는 문자열을 정수로 돌려주는 메소드이다.

&nbsp;

## 3) int parseInt()

~~~ java
package wrapperclass;

public class WrapperTest {

   public static void main(String[] args) {
      int theValue = Integer.parseInt("100");
      
      System.out.println(theValue);
   }

}
~~~

실행결과

100


 

&nbsp;

# 3. 언박싱 / 오토박싱

* * *

Integer와 int 자료형 사이의 연산이 일어날 때는 언박싱, 오토박싱이 일어난다.

~~~ java
package wrapperclass;

public class WrapperTest {

   public static void main(String[] args) {
      Integer num1 = new Integer(100);
      int num2 = 200;

      int sum1 = num1 + num2; // int에 저장되어야 하므로 num1이 num1.intValue()메소드를 통하여 Integer -> int (<strong>언박싱</strong>)
      Integer sum2 = num1 + num2; // Integer에 저장되어야 하므로 num2가 Integer.valueOf(num2)를 통하여 int -> Integer (<strong>오토박싱</strong>)
   }

}
~~~

Integer 자료형이 int 자료형으로 변환되는 것을 언박싱, 그 반대를 오토박싱이라고 한다. 이 과정은 컴파일러가 해주므로 사실 프로그래머는 Integer자료형과 int자료형을 구분 없이 편하게 사용할 수 있다.