---
title: (Java) 11 - 조건문(2) - 조건 연산자
date: 2018-12-14T19:38:17+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - Java
  - 자바
  - 조건 연산자
---
**조건 연산자 **는 코드를 더 간결하고 직관적으로 작성하는 데 도움을 줍니다.

두개의 정수형 변수 num1과 num2가 있습니다. 그리고 larger 라는 변수에 num1과 num2 중 더 큰 값을 가지는 변수의 값을 담고 싶다고 해봅시다. 먼저 if ~ else 조건문으로 작성해봅시다.

~~~ java
package hello;

public class HelloJava{
    public static void main(String[] args){
        int num1 = 10;
        int num2 = 20;
        int larger;

        if (num1 >= num2)
        {
            larger = num1;
        }
        else
        {
            larger = num2;
        }
        System.out.println("The larger number is "+larger);
    }
}
~~~

실행결과

The larger number is 20


계속하려면 아무 키나 누르십시오 . . . 

위의 코드는 조건 연산자로 훨씬 간결하게 작성할 수 있습니다.

조건 연산자는 **'어떠한 조건을 판단하여 '참'이면 앞의 것, '거짓'이면 뒤의 것을 선택하는 연산자'** 입니다. 구조는 다음과 같습니다.

~~~ java
(조건) ? 변수1 : 변수2
~~~

(조건) 을 판단하여 그것이 '참'이면 변수1을 반환하고, '거짓'이면 변수2를 반환합니다.

~~~ java
package hello;

public class HelloJava{
    public static void main(String[] args){
        int num1=10;
        int num2=20;
        int larger = (num1>num2) ? num1 : num2;

        System.out.println(larger);
    }
}
~~~

실행결과

The larger number is 20


계속하려면 아무 키나 누르십시오 . . . 

if / else 문으로 작성했던 코드가 단 한 줄로 표현이 됩니다. 가독성 측면에서도 더 뛰어납니다.

조건 연산자 는 **중첩**이 됩니다.

~~~ java
package hello;

public class HelloJava{
    public static void main(String[] args){
        int num1 = 10;
        int num2 = 20;
        int num3 = 30;
        int max;

        max = ((num1 > num2 ) ? num1: num2) > num3 ? ((num1 > num2) ? num1: num2): num3;
        System.out.println("The maximum number is "+max);
    }
}
~~~

~~~ java

~~~

실행결과

The maximum number is 30


계속하려면 아무 키나 누르십시오 . . . 

먼저 num1과 num2를 비교해서 반환되는 더 큰 값과 num3를 또 한 번 비교하여 더 큰 값을 반환시켜 max에 담습니다.

중첩이 한 번이 아니라 여러번도 가능합니다. 사람에 따라서 if / else 문이 더 잘 읽히시는 분도 있을 것이기 때문에 상황에 따라 어떤것을 사용할지를 판단하시면 됩니다.