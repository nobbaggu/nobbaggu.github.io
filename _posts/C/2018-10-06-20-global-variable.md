---
title: (C언어) 20. 함수(3) - 전역변수
date: 2018-10-06T21:23:07+09:00
author: nobbaggu
layout: post
categories: C
tags:
  - C언어
  - global
  - variable
  - local
  - static
  - 글로벌
  - 로컬
  - 변수
  - 전역
  - 전역
  - 지역
---

<br>
## 1. 전역변수
* * *

전역변수(global variable)는 지역변수(local variable)와 반대(?)되는 개념입니다. 지역변수의 선언과 초기화는 중괄호 { } 안에서만 유효했다면, **전역변수는 프로그램이 실행 되자마자 메모리에 할당되어 프로그램이 종료될때까지 존재합니다. 따라서 언제 어떤 함수에서든 접근이 가능합니다.**

~~~ c
#include <stdio.h>

int num = 0;

void Add(int val)
{
	num += val; //num에 전달인자로 받은 val값을 더함
}

void Mul(int val)
{
    num *= val; //num에 전달인자로 받은 val값을 곱함
}

int main(void)
{
    printf("num is %d\n", num);
    
    Add(7);
    printf("num is %d\n", num);
    
    Mul(4);
    printf("num is %d\n", num);
    
    return 0;
}
~~~

실행결과

num is 0

num is 7

num is 28

계속하려면 아무 키나 누르십시오 . . . 

<br>

변수 num은 어떤 중괄호에도 둘러쌓여있지 않습니다. 따라서 `Add,()`, `Mul()` 두 함수에서 접근이 가능합니다.

한 가지 중요한 사실이 있습니다. **전역변수와 같은 이름을 가진 지역변수가 있다면 해당 지역 내에서 변수를 사용하면 지역변수로 접근이 됩니다.** 아래 코드를 통해 확인해보겠습니다.

~~~ c
#include <stdio.h>

int num = 0;

int Add(int val)
{
    int num = 10;
    num += val;
    
    return num;
}

int Mul(int val)
{
    int num = 3;
    num *= val;
    
    return num;
}

int main(void)
{
    int add_result, mul_result;
    
    add_result = Add(2); //num에 2를 더하여 반환
    mul_result = Mul(2); //num에 2를 곱하여 반환
    
    printf("Add : %d\n", add_result);
    printf("Mul : %d\n", mul_result);
    
    return 0;
}
~~~

실행결과

Add num : 12

Mul num : 6

계속하려면 아무 키나 누르십시오 . . . 

<br>

Add(2)의 결과는 Add함수 내의 num이 10이기 때문에 10+2 = 12가 될 것이고, Mul(2)의 결과는 Mul함수 내의 num이 3이기 때문에 3\*2 = 6이 됩니다.

사실 전역변수와 같은 이름을 가진 지역변수를 사용하지 않는것이 좋습니다. 굳이 그럴 필요가 없기 때문입니다.

<br>
## 2. 전역변수 사용에 관한 주의사항
* * *

프로그램 시작부터 끝까지 소멸되지 않고 유지되는 전역변수는 대충 생각하면 참 편리해 보입니다. 하지만 전역변수를 남용하는 것은 올바른 습관이 아닙니다.

전역변수를 남용하는 경우에는 다음의 위험이 있습니다.

+ 1) 전역변수는 더 이상 필요가 없어져도 프로그램 종료 때 까지 메모리공간을 차지하여 낭비를 초래한다.

+ 2) 전역변수는 모든 지역(모든 함수)에서 접근이 가능하기 때문에 실수로 접근하여 의도치 않은 결과를 나게 하기 쉽다.

+ 3) 전역변수가 수정되면 이에 관련된 모든 함수의 코드를 바꿔야 한다. 즉, 디버깅이 어려워진다.

전역변수는 여러 함수에서 접근해야하는 경우와 같이 필요할 때만 사용하는 것이 좋습니다.

<br>
# 3. static 변수
* * *

static 변수는 지역변수처럼 변수가 선언된 함수 내에서만 접근이 가능하지만, 일단 이 함수 내에서 선언이 되어 메모리공간을 차지하고 나면 프로그램이 종료될때까지 소멸되지 않고 존재합니다.

~~~ c
#include <stdio.h>

void Add(void)
{
    int var = 0;
    static int stvar = 0;
    
    var++, stvar++; //var, stvar 1씩 증가
    
    printf("var: %d, stvar: %d\n", var, stvar);
}

int main(void)
{
    Add();
    Add();
    Add();
    Add();
    Add();
    
    return 0;
}
~~~

실행결과

var: 1, stvar: 1

var: 1, stvar: 2

var: 1, stvar: 3

var: 1, stvar: 4

var: 1, stvar: 5

계속하려면 아무 키나 누르십시오 . . . 

<br>

var는 지역변수이기 때문에 `Add()`함수를 호출할 때 마다 메모리에 할당되고 1이 증가하여 출력된 다음 함수 호출이 끝나면서 다시 메모리에서 소멸됩니다. 반면에 stvar는 실행결과를 보면 알 수 있듯이 값을 유지하며 계속 1씩 커지고 있습니다. 이처럼 변수를 선언할 때 자료형 앞에 static 을 붙이면 변수가 프로그램이 종료될 때 까지 메모리에 존재합니다.