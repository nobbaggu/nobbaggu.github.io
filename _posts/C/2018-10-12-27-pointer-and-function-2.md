---
title: (C언어) 27. 포인터와 함수(2)
date: 2018-10-12T02:13:52+09:00
author: nobbaggu
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - call by reference
  - call by value
  - const
  - C언어
  - pointer
  - 상수
  - 상수포인터
  - 포인터
  - 포인터상수
  - 함수
---
# 1. Call-by-Value    vs.    Call-by-Reference

* * *

함수의 전달인자로서 일반 변수와 포인터 변수를 넘겨받은 경우에 우리가 데이터를 다루게 되는 방식에는 차이가 있다고 했습니다.

![image](/images/2018/09/111.jpg){: width="50%" height="50%"}

위처럼 일반 변수를 함수의 전달인자를 통해 넘길때는 변수 자체가 아닌 변수의 값이 전달된다 했습니다. func함수의 num과 main함수의 x는 애초에 할당된 메모리공간이 다릅니다. func함수의 num변수의 메모리공간에 x의 값인 10이 복사되어 저장되고 ++연산이 수행되면서 num의 값이 10에서 11로 증가합니다. 그리고 func함수의 호출이 끝나며 변수 num에 할당된 메모리공간의 데이터는 소멸되어요.

![image](/images/2018/09/222.jpg){: width="50%" height="50%"}

반면에 전달인자를 포인터 형으로 선언하고 main함수의 변수 x가 있는 메모리의 주소값을 전달해주면 *연산자를 써서 직접 그 주소의 메모리공간에 접근해서 ++연산을 시켜줄 수 있습니다. 이 때는 함수의 호출이 종료되어도 변수 x의 값은 11인 상태로 유지가 됩니다. 직접 메모리공간에 접근하여 데이터를 변경시켜주었기 때문입니다.

첫 번째 경우처럼 변수의 값을 복사하여 전달인자로 주며 함수를 호출하는 경우를 값에 의한 호출이라 하여 특별히 **call by value**라고 부릅니다. 그리고 두 번째 경우처럼 주소값을 전달인자로 넘겨주며 메모리를 직접 참조하며 함수를 호출하는 경우를 참조에 의한 호출이라 하여 **call by reference**라고 부릅니다.

call by value와 call by reference는 직접 메모리공간을 참조하느냐 아니냐의 차이가 있습니다.

~~~ c
#include <stdio.h>

void PosChange_val(int val1, int val2)
{
    int moment = val1; //val1의 값을 잠시 저장
    val1 = val2; //val1에 val2의 값을 저장
    val2 = moment; //val2에 moment변수에 담겨있던 val1의 값 저장
}

void PosChange_ref(int * ptr1, int * ptr2)
{
    int moment = *ptr1;
    *ptr1 = *ptr2;
    *ptr2 = moment;
}

int main(void)
{
    int num1 = 10, num2 = 20;
    printf("num1 : %d, num2 : %d\n\n", num1, num2);
    
    PosChange_val(num1, num2);
    printf("num1 : %d, num2 : %d\n\n", num1, num2);
    
    PosChange_ref(&num1, &num2);
    printf("num1 : %d, num2 : %d\n", num1, num2);

    return 0;
}
~~~

실행결과

num1 : 10, num2 : 20


num1 : 10, num2 : 20


num1 : 20, num2 : 10


계속하려면 아무 키나 누르십시오 . . .

PosChange\_val 함수는 call by value의 호출방법을 사용하고 PosChange\_ref 함수는 call by reference의 호출방법을 사용합니다. PosChange\_val 함수는 num1과 num2의 복사값을 val1과 val2에 저장하고 두 변수의 값을 서로 교환시킵니다. 하지만 이 때에 바뀌는 값은 val1과 val2이며 num1과 num2의 값이 교환되는 것은 아닙니다. PosChange\_val 함수는 val1과 val2에 할당된 메모리에 num1과 num2의 값을 복사하여 교환을 실행했기 때문입니다.

하지만 PosChange\_ref 함수는 num1과 num2의 주소값을 넘겨받아 *연산자를 이용하여 직접 num1과 num2의 메모리공간에 접근하여 값을 뒤바꿔 놓습니다. 그리고 PosChange\_ref 함수의 호출이 종료되면서 ptr1과 ptr2 두 개의 포인터변수가 소멸되더라도 num1과 num2의 데이터 값은 유효합니다. 이처럼 call by value는 메모리상에 직접적으로 접근이 불가능하지만 call by reference는 메모리상에 직접 접근을 할 수 있기때문에 위의 일이 가능한 것입니다.

&nbsp;

&nbsp;

&nbsp;

# 2. 상수 포인터와 포인터 상수

* * *

포인터 변수도 일반 변수와 마찬가지로 const 선언이 가능합니다. 일반 변수에서의 const 선언은 변수의 값이 변경되는것을 방지하기 위한 목적으로 사용이 되었습니다.

포인터변수는 2 가지의 const 선언 방법이 있습니다. 이 둘은 const의 위치를 달리 하는 것인데 이 둘은 상수화시키는 대상이 다릅니다.

**1. const int * ptr; : 포인터가 가르키는 데이터가 상수**

**2. int * const ptr; : 포인터가 가르키는 위치가 상수**

![image](/images/2018/09/333.jpg){: width="50%" height="50%"}

위의 코드에서 ptr1과 ptr2 모두 변수  num을 참고하고 있습니다. 하지만 둘은 분명 차이를 보입니다. ptr1에 의한 num값의 변경은 되지만 ptr2에 의한 num값의 변경은 컴파일 에러를 일으킵니다. 이는 ptr2가 const 선언에 의해 **상수포인터**가 되어서입니다. **상수 포인터란 말 그대로 상수를 가르키는 포인터입니다. 포인터에 의해서는 상수로 취급되어 값을 바꿀 수 없습니다.**

&nbsp;

![image](/images/2018/09/444.jpg){: width="50%" height="50%"}

ptr1과 ptr2는 모두 num1을 가르키도록 초기화 시켰습니다. 하지만 ptr1은 가르키는 위치를 num2로 변경이 가능한데 ptr2가 가르키는 위치를 num2로 변경하려 했더니 컴파일 에러가 일어났습니다. 위와 같이 const 선언을 하면 포인터를 상수화시켜 포인터상수로 만들어버립니다. **포인터상수란 포인터가 상수입니다. 위치의 변경이 불가능합니다. **포인터상수의 대표적인 경우가 배열의 이름입니다. 배열의 이름은 항상 자신의 첫 번째 원소만을 가르키도록 상수화 되어있습니다.

포인터를 const 선언하는 이유는 실수로 포인터로 위치만 참조하고 건들지는 말아야 할 데이터를 변경하거나 포인터가 가르키는 위치를 변경시켜 엉뚱한 데이터를 참조하여 예상치 못한 결과가 나옴을 방지하기 위해서입니다.