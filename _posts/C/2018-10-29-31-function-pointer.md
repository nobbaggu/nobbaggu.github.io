---
title: 31. 함수 포인터
date: 2018-10-29T22:21:38+09:00
author: SWnomad
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - function
  - function pointer
  - pointer
  - void 포인터
  - 포인터
  - 함수
  - 함수 포인터
---
# 1. 함수 포인터

* * *

함수 역시 정수, 실수, 배열 등과 마찬가지로 메모리공간에 저장이 되어있습니다. 그리고 함수를 호출할 때에는 메모리공간을 찾아가서 불러옵니다. 사실 **함수의 이름은 함수가 저장된 메모리공간을 가르키는 포인터입니다!** 그렇다면 이번에도 함수 이름의 포인터형에 관한 이야기가 빠질 수 없겠습니다.

**함수의 포인터형은 함수의 '반환형'과 '전달인자'로 결정됩니다.**

아래와 같은 함수들이 있습니다.

_1. int func1(char a, char b)_ :반환형이 int이고 전달인자가 int형 변수 2개인 함수

_2. double func2(int a)_ :반환형이 double이고 전달인자가 int형 변수 1개인 함수

_3. char func3(void)_ : 반환형이 char이고 전달인자가 void형인(전달인자가 없는) 함수

**함수를 저장하기 위한 포인터는 함수의 반환형과 전달인자를 기준으로 선언합니다.**

위에서 예시로 든 함수들의 이름에 해당하는 포인터변수는 다음과 같이 선언합니다.

_<span style="font-family: 'arial black', sans-serif;">1. int func1(char a, char b) → int (*ptr1) (char, char);</span>_

_<span style="font-family: 'arial black', sans-serif;">2. double func2(int a) → double (*ptr2) (int);</span>_

_<span style="font-family: 'arial black', sans-serif;">3. char func3(void) → char (*ptr4) (void);</span>_

위의 포인터형들은 int형, char형 포인터처럼 딱 나누어 떨어지는 이름이 없습니다. 예를들면 1번 함수같은 경우에는 '반환형이 int이고 전달인자가 char형 2개인 포인터형' 이라고 풀어서 해석하면 됩니다.

예제 코드를 봅시다.

~~~ c
#include <stdio.h>

int Add(int n1, int n2)
{
    return n1+n2;
}

int Mul(int n1, int n2)
{
    return n1*n2;
}

int main(void)
{
    int num1 = 10, num2 = 20;
    int (*ptr) (int, int); //Add함수와 Mul함수의 포인터형에 해당하는 포인터 변수 선언
    
    ptr = Add; // ptr에 Add함수 저장
    printf("%d\n", ptr(num1, num2));
    
    ptr = Mul; // ptr에 Mul함수 저장
    printf("%d\n", ptr(num1, num2));
    
    return 0;
}
~~~

실행결과

30


200


계속하려면 아무 키나 누르십시오 . . .

함수의 전달인자와 반환형을 기준으로 선언한 포인터변수는 함수를 저장할 수 있는 것이 확인되었습니다.

&nbsp;

그렇다면 이제 함수를 다른 함수에 전달인자로 줄 수 있는 방법이 생겼습니다.

~~~ c
#include <stdio.h>

int Add(int n1, int n2) //반환형이 int이고 전달인자로 int형 변수가 2개 선언된 포인터형의 함수
{
    return n1+n2;
}

int Mul(int n1, int n2) //반환형이 int이고 전달인자로 int형 변수가 2개 선언된 포인터형의 함수
{
    return n1*n2;
}

void ShowResult(int n1, int n2, int (*func) (int, int)) //전달인자로 Add와 Mul함수를 전달하기 위한 포인터변수 func 선언
{
    int result = func(n1,n2); //포인터변수 func를 이용하여 전달받은 함수를 호출
    printf("The result is %d\n", result);
}

int main(void)
{
    int num1 = 10, num2 = 20;
    
    ShowResult(num1, num2, Add); // ShowResult 함수에 두 변수 num1, num2와 함수 Add를 전달
    ShowResult(num1, num2, Mul); // ShowResult 함수에 두 변수 num1, num2와 함수 Mul을 전달
    
    return 0;
}
~~~

실행결과

The result is 30


The result is 200


계속하려면 아무 키나 누르십시오 . . .

&nbsp;

&nbsp;

&nbsp;

# 2. void 포인터

* * *

한 가지 더 소개해드릴 내용은 void 포인터라는 것입니다. void라는 단어가 가지고 있는 '비어있다'는 의미와 어울리게 void 포인터는 포인터형이 없는 포인터변수 입니다. 즉, void 포인터로 선언된 포인터는 어떤 포인터형도 가지지 않는다는 것을 의미합니다.

&nbsp;

_void * ptr;_

위와 같이 선언된 void 포인터는 정수, 실수, 배열, 함수 어떠한 것의 주소값도 저장할 수 있습니다.

~~~ c
#include <stdio.h>

int Func(int n1, int n2)
{
    return n1+n2;
}

int main(void)
{
    int num = 10;
    int arr[] = {1,2,3};
    void * ptr;
    
    ptr = &num; //num의 주소값 저장
    printf("%#x\n", ptr);
    
    ptr = arr; // 배열 arr의 이름(=첫 번째 원소의 주소값) 저장
    printf("%#x\n", ptr);
    
    ptr = Func; // 함수 Func의 이름(=함수의 주소값) 저장
    printf("%#x\n", ptr);
    
    return 0;
}
~~~

실행결과

0x113fe84


0x113fe70


0x8111fe


계속하려면 아무 키나 누르십시오 . . .

위의 코드에서 void 포인터는 어떠한 것들의 주소값도 담을 수 있다는게 확인이 되었습니다. 포인터형은 포인터 연산을 할 때에 건너뛰는 메모리공간의 byte단위 크기를 결정합니다. 그렇다면 void 포인터의 연산결과는 어떻게 될까요?

<img class="aligncenter wp-image-1028" src="/images/2018/09/rr.jpg" alt="" width="682" height="457" srcset="/images/2018/09/rr.jpg 795w, /images/2018/09/rr-300x201.jpg 300w, /images/2018/09/rr-768x515.jpg 768w" sizes="(max-width: 682px) 100vw, 682px" /> 

일단 포인터형이 없으므로 얼마만큼이 메모리공간을 참조해야할지 알 수 없으므로 포인터로 변수에 접근을하여 값을 변경하는것도 불가능합니다. 또한 포인터 증가연산을 하였을 때에도 얼마만큼의 메모리공간을 뛰어넘어서 가르키는 곳을 변경해야 할 지의 기준도 없어서 포인터 연산도 불가능합니다. 도대체 이러한 포인터를 왜 쓰는지 이해가 가지않더라도, 지금은 void포인터의 존재와 성질에 관해서만 알아두어도 충분합니다.