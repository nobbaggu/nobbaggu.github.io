---
title: (C언어) 19 - 함수(2) - 지역변수
date: 2018-10-06T19:53:02+09:00
author: nobbaggu
layout: post
categories: C
tags:
  - C언어
  - local variable
  - 로컬변수
  - 메모리
  - 소멸
  - 수명
  - 지역변수
  - 할당
---

![1-7](https://nobbaggu.github.io/images/2018/09/1-7.jpg){: width="80%" height="80%"}

위 사진에 보이는대로 `GetNum()` 함수를 정의했습니다. `main()`함수에서 `GetNum()`을 호출하여 실행하고 `printf()`함수를 통해 num을 출력해보려 했습니다. 11이 나올거라 예상했는데, 변수 num이 식별이 되지 않는 일이 일어납니다. 이는 **지역변수의 수명**과 관련된 문제입니다.


<br>
## 1. 지역변수의 수명
* * *

**중괄호 { } 안에서 선언된 변수를 지역변수라고 합니다. 지역변수의 수명은 중괄호 { }를 벗어나는 순간 끝이납니다.** 위 코드에서는 GetNum함수가 호출되면서 메모리공간에 변수 num이 할당됩니다. 그리고 GetNum함수의 호출이 끝이 나면서 메모리에서 사라집니다. 굳이 num을 사용하려면 GetNum함수를 num을 return하는 형태로 정의한 이후 main함수에서 return값을 다른 변수에 받아서 사용해야 합니다. for나 if 같은 반복문과 조건문의 중괄호 내에서 선언된 변수도 당연히 지역변수입니다.

<br>

![2-1](https://nobbaggu.github.io/images/2018/09/2-1.jpg){: width="80%" height="80%"}

<br>

위의 코드를 보시면 지역변수는 { } 안에서만 존재하기 때문에 서로 다른 지역에서는 이름이 같아도 상관이 없습니다.

정확하게 정리를 하자면, **지역변수는 선언이 되는 동시에 메모리공간에 할당이 되었다가 자신이 속한 중괄호를 빠져나가면서 메모리에서 소멸됩니다.**

<br>
## 2. 지역변수의 메모리 할당과 소멸
* * *

~~~ c
#include <stdio.h>

int Add(int a, int b)
{
    int c = a + b;
    
    return c;
}

int Mul(int a, int b)
{
    int c = a * b;
    
    return c;
}

int main(void)
{
    int num1= 10, num2 = 20; 
    int add_result= 0, mul_result= 0;
    
    add_result = Add(num1, num2);
    mul_result = Mul(num1, num2);
    
    printf("The results of two functions are %d and %d\n", add_result, mul_result);
    
    return 0;
}
~~~

프로그램이 실행이 되면 main 함수가 실행됩니다. main함수의 1행과 2행을 거치며 메모리에 네 개의 변수가 다음 그림과 같이 할당됩니다.

![3-1](https://nobbaggu.github.io/images/2018/09/3-1.jpg){: width="80%" height="80%"}

<br>

그리고 `Add()` 함수가 호출되면서 a,b,c가 메모리에 생성됩니다.

![4-1](https://nobbaggu.github.io/images/2018/09/4-1.jpg){: width="80%" height="80%"}

Add 함수는 num1의 값인 10과, num2의 값인 20을 넘겨받으며 a에는 10이, b에는 20이 저장됩니다. 그리고 `Add()` 함수가 종료되면서 메모리에 할당되었던 a,b,c는 소멸 됩니다. `Mul()` 함수가 호출되면서 같은 과정이 한 번 더 일어납니다. 물론 이 때의 a, b, c는 `Add()` 함수의 a,b,c와는 상관없는 변수입니다.

마지막으로, main함수가 끝이 나면서 나머지 4개의 변수도 소멸됩니다.

함수의 전달인자는 지역변수일까요? 함수의 전달인자는 해당 함수에서만 사용이 되고 해당 함수를 벗어나면 소멸되기 때문에 전달인자 역시 지역변수 입니다.