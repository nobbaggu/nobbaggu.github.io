---
title: (Java) 자료형(1) - 정수와 실수
date: 2018-12-14T15:41:01+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - byte
  - char
  - int
  - long
  - short
  - 문자
  - 자료형
  - 정수
---
변수를 선언할 때 자료형 을 명시해주는 이유는 다음과 같습니다.

1. **‘정수’와 ‘정수가 아닌 실수’를 표현하는 방식 자체가 다르기 때문에 컴파일러에게 미리 알려줘야한다.**

2. **데이터를 저장할 메모리공간의 크기를 컴파일러에게 알려주어야한다**.

&nbsp;

&nbsp;

# 1. 정수 자료형 : 숫자 표현

* * *

Java에는 아래와 같은 자료형이 있습니다.

<table class="table table-bordered" border="1">
  <tr>
    <td style="width: 88px;" colspan="2">
      <strong>자료형</strong>
    </td>
    
    <td style="width: 49px;">
      <strong>메모리</strong>
    </td>
    
    <td style="width: 235px;">
      <strong>표현할 수 있는 숫자의 범위</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 33px;" rowspan="4">
      <strong>정수</strong>
    </td>
    
    <td style="width: 49px;">
      byte
    </td>
    
    <td style="width: 49px;">
      1byte
    </td>
    
    <td style="width: 235px;">
      -128 ~ +127
    </td>
  </tr>
  
  <tr>
    <td style="width: 49px;">
      short
    </td>
    
    <td style="width: 49px;">
      2byte
    </td>
    
    <td style="width: 235px;">
      -32,768 ~ +32,767
    </td>
  </tr>
  
  <tr>
    <td style="width: 49px;">
      int
    </td>
    
    <td style="width: 49px;">
      4byte
    </td>
    
    <td style="width: 235px;">
      -2,147,483,648 ~ +2,147,483,647
    </td>
  </tr>
  
  <tr>
    <td style="width: 49px;">
      long
    </td>
    
    <td style="width: 49px;">
      8byte
    </td>
    
    <td style="width: 235px;">
      -9,223,372,036,854,775,808 ~ +9,223,372,036,854,775,807
    </td>
  </tr>
  
  <tr>
    <td style="width: 33px;">
      <strong>문자</strong>
    </td>
    
    <td style="width: 49px;">
      char
    </td>
    
    <td style="width: 49px;">
      2byte
    </td>
    
    <td style="width: 235px;">
      문자
    </td>
  </tr>
  
  <tr>
    <td style="width: 33px;" rowspan="2">
      <strong>실수</strong>
    </td>
    
    <td style="width: 49px;">
      float
    </td>
    
    <td style="width: 49px;">
      4byte
    </td>
    
    <td style="width: 235px;">
      ±3.4×10^-37 ~ ±3.4×10^+38
    </td>
  </tr>
  
  <tr>
    <td style="width: 49px;">
      double
    </td>
    
    <td style="width: 49px;">
      8byte
    </td>
    
    <td style="width: 235px;">
      ±1.7×10^-307 ~ ±1.7×10^+308
    </td>
  </tr>
</table>

사실 우리는 **정수형 변수를 선언할 때 보통 int보다 작은 자료형은 잘 쓰지 않습니다. byte나 short, int 중 무엇을 쓰던지 사실 CPU의 처리속도는 차이가 거의 나지 않기 때문입니다.** 그렇다면 굳이 int보다 작은 자료형을 쓸 필요가 없죠. **메모리 공간까지 신경써야 할 상황이 아니라면 말이죠.**

정수 자료형을 사용하여 표현할 수 있는 숫자의 범위는 그 자료형의 크기가 나타낼 수 있는 경우의 수와 같습니다. 가령 short는 2byte = 16bit이기 때문에 2^16 = 65,536의 경우의 수를 나타낼 수 있어요. 단, 가장 앞에 오는 최상위 비트(MSB)는 부호를 표현하는데 사용이 됩니다. 따라서 실제로 데이터의 크기를 표현하는데에는 15bit만 사용이 되어 2^15 = 32,768 가지의 상태를 나타낼 수 있어요. 가장 앞의 bit가 0이면 양수, 1이면 음수를 나타냅니다.

자바에서는 정수를 기본적으로 int로 처리하므로 int의 범위를 넘어서는 숫자는 long으로 쓰더라도 long num = 12345678900L;와 같이 뒤에 L이나 l을 써주어야 합니다.

또한 실수는 기본적으로 double로 처리되므로 float을 쓸 때도 float num = 3.14f;와 같이 뒤에 F나 f를 붙여주어야 합니다.

&nbsp;

&nbsp;

&nbsp;

# 2. 문자 자료형

* * *

char는 문자(character)를 나타내는 데 사용됩니다. 문자가 표현되는 원리를 알려면 ASCII(American Standard Code for Information Interchange)에 에 대해 알아야합니다. 아스키 코드는 미국표준협회(ANSI)에서 만든 표준 코드체계입니다. 아스키 코드에서는 문자를 표현하기 위해서 우리는 문자 하나 하나를 어떠한 정수에 대입시키기로 약속을 하였습니다.

<img class="aligncenter size-full wp-image-1213 img-responsive" src="/images/2018/09/ASCII.jpg" sizes="(max-width: 1306px) 100vw, 1306px" srcset="/images/2018/09/ASCII.jpg 1306w, /images/2018/09/ASCII-300x167.jpg 300w, /images/2018/09/ASCII-768x429.jpg 768w, /images/2018/09/ASCII-1024x572.jpg 1024w" alt="" width="1306" height="729" /> 

~~~ java
package hello;

public class HelloJava{
    public static void main(String[] args){
        char char1 = 'A';
        char char2 = 'Z';
        
        System.out.println(char1);
        System.out.println(char2);
    }
}
~~~

&nbsp;

실행결과

A

Z