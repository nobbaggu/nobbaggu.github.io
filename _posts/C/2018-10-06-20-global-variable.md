---
title: 20. 함수(3) - 전역변수
date: 2018-10-06T21:23:07+09:00
author: SWnomad
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - C언어
  - global
  - global variable
  - local
  - static
  - 글로벌
  - 글로벌 변수
  - 로컬
  - 로컬 변수
  - 변수
  - 전역
  - 전역변수
  - 지역변수
---
# 1. 전역변수

* * *

전역변수 (global variable)는 지역변수(local variable)과 반대(?)되는 개념입니다. 지역변수의 선언과 초기화는 중괄호 { } 안에서만 유효했다면, **전역변수는 프로그램이 실행 되자마자 메모리에 할당되어 프로그램이 종료될때까지 존재합니다.** **따라서 언제 어떤 함수에서든 접근이 가능합니다.**

~~~ c
#include <stdio.h>

int num = 0;

void Add(int val)
{
    num+=val; //num에 전달인자로 받은 val값을 더함
}

void Mul(int val)
{
    num*=val; //num에 전달인자로 받은 val값을 곱함
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

&nbsp;

실행결과

num is 0


num is 7


num is 28


계속하려면 아무 키나 누르십시오 . . . 

변수 num은 어떤 중괄호에도 둘러쌓여있지 않습니다. 따라서 Add, Mul 두 함수에서 접근이 가능합니다.

전역변수 num이 있고 func라는 함수도 num이라는 이름의 변수를 가지고 있다고 해봅시다. 이 경우에 func함수를 실행시켜 num 변수에 접근하면 전역변수 num과 지역변수 num 중 어떤 변수에 접근이 될까요? 정답은 func함수 내에 있는 num이 조작이 됩니다. **전역변수가 있고, 같은 이름의 지역변수가 있을 때 지역변수가 포함된 지역내서는 지역변수가 같은 이름의 전역변수를 덮어씁니다.**

~~~ c
#include <stdio.h>

int num = 0;

int Add(int val)
{
    int num = 10;
    num+=val;
    
    return num;
}

int Mul(int val) //num값에 전달인자 val값을 곱하여 반환
{
    int num = 3;
    num*=val;
    
    return num;
}

int main(void)
{
    int add_result, mul_result;
    
    add_result = Add(2); //num에 2를 더하여 반환
    mul_result = Mul(2); //num에 2를 곱하여 반환
    
    printf("Add function num : %d\n", add_result);
    printf("Mul function num : %d\n", mul_result);
    
    return 0;
}
~~~

&nbsp;

Add(2)의 결과는 Add함수 내의 num이 10이기 때문에 10+2 = 12가 될 것이고, Mul(2)의 결과는 Mul함수 내의 num이 3이기 때문에 3*2 = 6이 될겁니다.

실행결과

Add function num : 12


Mul function num : 6


계속하려면 아무 키나 누르십시오 . . . 

사실 지역변수와 전역변수의 이름을 같지 않게 하는것이 관례입니다. 굳이 그럴 필요 없기 때문입니다.

&nbsp;

# 2. 전역변수 사용에 관한 주의사항

* * *

프로그램 시작부터 끝까지 소멸되지 않고 유지되는 전역변수는 대충 생각하면 참 편리해 보입니다. 하지만 전역변수를 남용하는 것은 올바른 습관이 아닙니다.

전역변수를 남용하는 경우에는 다음의 위험이 있습니다.

**1) 전역변수는 더 이상 필요가 없어져도 프로그램 종료 때 까지 메모리공간을 차지하여 낭비를 초래한다.**

**2) 전역변수는 모든 지역(모든 함수)에서 접근이 가능하기 때문에 실수로 접근하여 의도치 않은 결과를 나게 하기 쉽다.**

**3) 전역변수가 수정되면 이에 관련된 모든 함수의 코드를 바꿔야 한다. 즉, 디버깅이 어려워진다.**

전역변수는 꼭 필요한 경우에만 사용해야 합니다.

&nbsp;

&nbsp;

# 3. static 변수

* * *

static변수라는 것이 있습니다. static 변수는 지역변수처럼 변수가 선언된 함수에서만 접근이 가능하지만, 일단 이 함수 내에서 선언이 되어 메모리공간을 차지하고 나면 프로그램이 종료될때까지 소멸되지 않고 존재합니다. 어떻게 사용하는건지 한 번 보시죠.

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

&nbsp;

실행결과

var: 1, stvar: 1


var: 1, stvar: 2


var: 1, stvar: 3


var: 1, stvar: 4


var: 1, stvar: 5


계속하려면 아무 키나 누르십시오 . . . 

Add()함수를 실행할 때 마다 var는 새로 선언되고 0으로 초기화 됩니다. 한 번 실행하고 끝날 때 마다 소멸되기 때문입니다. 반면에 stvar는 그 이전 실행의 증가된 값을 유지하며 계속 1씩 커지고 있습니다. 이처럼 변수를 선언할 때 자료형 앞에 static 을 붙여 프로그램 종료까지 소멸되지않고 값을 저장할 수 있는 static 변수를 선언할 수 있습니다.

**static변수는 지역변수처럼 해당 함수 내에서만 접근이 가능하지만, 전역변수처럼 프로그램이 종료될 때 까지 메모리공간에 할당이 됩니다.**