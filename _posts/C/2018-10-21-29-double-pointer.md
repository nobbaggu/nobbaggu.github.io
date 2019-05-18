---
title: (C언어) 29. 더블 포인터
date: 2018-10-21T22:12:38+09:00
author: SWnomad
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - array
  - C언어
  - double
  - double pointer
  - pointer
  - pointer array
  - 더블
  - 더블 포인터
  - 포인터
  - 포인터 배열
---
# 1. 더블 포인터 선언

* * *

더블 포인터 라고 하는것은 포인터를 가르키는 포인터입니다. 포인터변수 ptr은 num의 주소값을 가지고 있다고 해봅시다. 그런데 num의 주소를 담기 위해 포인터변수 ptr을 정의하는 순간 ptr도 메모리공간에 할당이 됩니다.

~~~ c
#include <stdio.h>

int main(void)
{
    int num = 10;
    int * ptr = &num;
    int **pptr = &ptr;
    
    printf("address of num : %#x\n", &num);
    printf("value of ptr : %#x\n", ptr);
    printf("adress of ptr : %#x\n", &ptr);
    printf("value of pptr : %#x\n", pptr);
    
    return 0;
}
~~~

실행결과

address of num : 0x5af844


value of ptr : 0x5af844


adress of ptr : 0x5af838


value of pptr : 0x5af838


계속하려면 아무 키나 누르십시오 . . . 

위의 코드에서 변수 num을 가르키는 포인터변수 ptr을 선언했습니다. ptr은 이제 num을 가르킵니다. 그리고 pptr이라는 더블포인터변수에 ptr의 주소값을 저장했습니다. 이 더블포인터는 이제 포인터변수 ptr을 가르키게 된겁니다. 실행결과를 보시면 ptr의 값은 &num과 같고 pptr의 값은 &ptr과 같습니다. 여기서 하나 기억해두셔야할건 더블포인터 변수를 선언하는 방법입니다. **더블포인터변수는 \*연산자 2개(\**)를 사용하여 선언합니다. 더블포인터변수란 포인터변수를 가르키는 포인터 변수입니다.**

<img class="aligncenter wp-image-1019" src="/images/2018/09/11-1.jpg" alt="" width="606" height="252" srcset="/images/2018/09/11-1.jpg 900w, /images/2018/09/11-1-300x125.jpg 300w, /images/2018/09/11-1-768x319.jpg 768w" sizes="(max-width: 606px) 100vw, 606px" /> 

이제 변수 num에 접근하는 방법은 총 3가지 입니다. num으로 직접 접근, \*ptr을 써서 접근, \**pptr을 써서 접근하는 방법입니다.

~~~ c
#include <stdio.h>

int main(void)
{
    int num = 10;
    int * ptr = &num;
    int ** pptr = &ptr;
    
    printf("%d\n", num);
    printf("%d\n", *ptr);
    printf("%d\n", **pptr);
    
    return 0;
}
~~~

실행결과

10


10


10


계속하려면 아무 키나 누르십시오 . . .

&nbsp;

그리고 더블포인터 역시 함수의 인자로 전달가능합니다.

~~~ c
#include <stdio.h>

void Increase(int ** ptr)
{
    (**ptr)++;
}

int main(void)
{
    int num = 10;
    int * pnum = &num;
    int ** ppnum = &pnum;
    
    printf("%d\n", num);
    Increase(ppnum);
    printf("%d\n", num);
    
    return 0;
}
~~~

&nbsp;

실행결과

10


11


계속하려면 아무 키나 누르십시오 . . .

&nbsp;

더블포인터는 포인터와 똑같이 생각하시면 됩니다. 단지 가르키는 곳이 일반 변수냐, 포인터변수냐의 차이 입니다.

다음 문제를 생각해봅시다. 두 변수 num1과 num2가 있고 각 변수를 가르키는 pnum1과 pnum2를 선언합니다. 그 이후 pnum1과 pnum2가 가르키는 대상을 각각 num2와 num1으로 서로 바꾸려면 어떻게 해야할까요?

아래와 같이 작성하면 바뀌지 않습니다.

~~~ c
#include <stdio.h>

void Change(int * ptr1, int * ptr2)
{
    int * temp;
    temp = ptr1;
    ptr1 = ptr2;
    ptr2 = temp;
}

int main(void)
{
    int num1 = 1, num2 = 2;
    int * pnum1 = &num1, * pnum2 = &num2;
    
    printf("%d  %d\n", *pnum1, *pnum2);
    Change(pnum1, pnum2);
    printf("%d  %d\n", *pnum1, *pnum2);
    
    return 0;
}
~~~

&nbsp;

실행결과

1  2


1  2


계속하려면 아무 키나 누르십시오 . . .

위의 코드처럼 하면 두 포인터가 가르키는 방향이 바뀌지 않습니다. 함수의 전달인자가 받는 값이 무엇인지 생각해보시면 답이 나옵니다. 함수에서 전달인자가 받는 것은 변수 자체가 아닙니다. 변수의 값이 복사되어 전달될 뿐인것입니다.

<img class="aligncenter wp-image-1020" src="/images/2018/09/22-1.jpg" alt="" width="356" height="362" srcset="/images/2018/09/22-1.jpg 478w, /images/2018/09/22-1-294x300.jpg 294w" sizes="(max-width: 356px) 100vw, 356px" /> 

바뀌는 것은 Change 함수의 ptr1과 ptr2입니다. pnum1과 pnum2와 상관 없는겁니다. 그리고 이는 함수의 호출이 끝나면 소멸되어버릴 지역변수일 뿐입니다. 이 문제는 더블포인터를 써서 해결할 수 있습니다.

~~~ c
#include <stdio.h>

void Change(int ** pptr1, int ** pptr2)
{
    int *temp;
    temp = *pptr1;
    *pptr1 = *pptr2;
    *pptr2 = temp;
}

int main(void)
{
    int num1 = 1, num2 = 2;
    int * pnum1 = &num1, * pnum2 = &num2;
    int **ppnum1 = &pnum1, **ppnum2 = &pnum2;
    
    printf("%d  %d\n", *pnum1, *pnum2);
    Change(ppnum1, ppnum2);
    printf("%d  %d\n", *pnum1, *pnum2);
    
    return 0;
}
~~~

실행결과

1  2


2  1


계속하려면 아무 키나 누르십시오 . . .

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# 2. 포인터 배열의 이름은 더블 포인터였다

* * *

더블포인터를 배웠으니 우리는 포인터배열의 이름의 정체에 대해 다시 생각해볼 수 있게 되었습니다. 포인터 배열은 포인터를 원소로 가지고 있는 배열입니다. 배열의 이름은 첫 번째 원소를 가르키는 포인터라고 했습니다. 그런데 포인터 배열의 첫 번째 원소는 포인터입니다. 따라서 포인터배열의 이름은 포인터를 가르키는 포인터이기 때문에 더블포인터입니다. 확인해봅시다.

~~~ c
#include <stdio.h>

int main(void)
{
    int num1 = 1, num2 = 2, num3 = 3, num4 = 4;
    int * arr[]={&num1, &num2, &num3, &num4};
    int **pptr = arr;
    
    printf("%d\n", **arr);
    
    return 0;
}
~~~

실행결과

1


계속하려면 아무 키나 누르십시오 . . .

실제로 포인터배열의 이름 arr는 int형 더블포인터인 **pptr에 저장이 됩니다. 두 개의 포인터형이 일치한다는 뜻입니다.