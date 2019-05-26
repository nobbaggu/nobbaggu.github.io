---
title: (C언어) 16. 조건문(2) - 조건 연산자(삼항 연산자)
date: 2018-10-06T11:33:29+09:00
author: SWnomad
layout: post
categories: C
tags:
  - C언어
  - 삼항
  - 조건
  - 연산자
---

**조건 연산자 **는 코드를 더 간결하고 직관적으로 작성하는 데 도움을 줍니다.

두개의 정수형 변수 num1과 num2가 있습니다. 그리고 larger 라는 변수에 num1과 num2 중 더 큰 값을 가지는 변수의 값을 담고 싶다고 해봅시다. 먼저 if ~ else 조건문으로 작성해봅시다.

~~~ c
#include <stdio.h>

int main(void)
{
    int num1 = 10;
    int num2 = 20;
    int larger;
    
    if(num1>=num2)
    {
        larger = num1;
    }
    else
    {
        larger = num2;
    }
    printf("The larger number is %d\n", larger);
    
    return 0;
}
~~~

실행결과

The larger number is 20

계속하려면 아무 키나 누르십시오 . . . 

<br>

위의 코드는 조건 연산자로 훨씬 간결하게 작성할 수 있습니다.

조건 연산자는 **어떠한 조건을 판단하여 '참'이면 앞의 것, '거짓'이면 뒤의 것을 선택하는 연산자** 입니다. 구조는 다음과 같습니다.

~~~ c
(조건) ? 변수1 : 변수2
~~~

(조건) 을 판단하여 그것이 '참'이면 변수1을 반환(return)하고, '거짓'이면 변수2를 반환합니다.

~~~ c
#include <stdio.h>

int main(void)
{
    int num1 = 10;
    int num2 = 20;
    int larger;
    
    larger = (num1>num2) ? num1 : num2;
    
    printf("The larger number is %d\n", larger);
    
    return 0;
}
~~~

실행결과

The larger number is 20

계속하려면 아무 키나 누르십시오 . . . 

<br>

if / else 문으로 작성했던 코드가 단 한 줄로 표현이 됩니다. 가독성 측면에서도 더 뛰어납니다.

조건 연산자 는 **중첩**이 됩니다.

~~~ c
int main(void)
{
    int num1 = 10;
    int num2 = 20;
    int num3 = 30;
    int max;

    max = ((num1 > num2 ) ? num1 : num2) > num3 ? ((num1 > num2 ) ? num1 : num2) : num3;

    printf("The maximum number is %d\n", max);

    return 0;
}
~~~

실행결과

The maximum number is 30

계속하려면 아무 키나 누르십시오 . . . 

<br>

num1과 num2를 비교해서 반환되는 더 큰 값과 num3를 또 한 번 비교하여 더 큰 값을 반환시켜 max에 대입합니다. 삼항 연산자의 중첩은 몇 번이고 가능합니다.