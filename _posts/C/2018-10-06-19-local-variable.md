---
title: (C언어) 19. 함수(2) - 지역변수
date: 2018-10-06T19:53:02+09:00
author: SWnomad
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - '{}'
  - C언어
  - local variable
  - 로컬변수
  - 메모리
  - 소멸
  - 수명
  - 지역변수
  - 할당
---
<img class="aligncenter size-full wp-image-958" src="/images/2018/09/1-7.jpg" alt="" width="657" height="324" srcset="/images/2018/09/1-7.jpg 657w, /images/2018/09/1-7-300x148.jpg 300w" sizes="(max-width: 657px) 100vw, 657px" />

우리는 GetNum이라는 함수를 정의하고 그 안에서 num = 10;으로 num을 선언하고 10으로 초기화 했습니다. 그리고 num을 1 증가시켰어요. 그 이후에 main함수에서 GetNum을 호출하여 실행하고 printf함수를 통해 num을 출력해보려 했습니다. 11이 나오길 기대하면서 말입니다. 그런데, 위에서 보이듯이 변수 num이 식별이 되지 않는 일이 일어납니다.  이것을 이해하기 위해 오늘은 지역변수에 관해 말씀드리겠습니다.

&nbsp;

# 1. 지역변수 수명

* * *

**{ } 안에서 선언된 변수를 지역변수라고 합니다. 그리고 그 지역변수의 수명은 중괄호 { }를 벗어나는 순간 끝이납니다. 소멸이 되어버립니다.** 위 코드에서는 GetNum함수가 호출되면서 메모리공간에 변수 num이 할당됩니다. 그리고 GetNum함수의 호출이 끝이 나며 변수 num은 메모리 공간에서 소멸됩니다. 따라서 컴파일러는 num이 무엇인지 더 이상 모르게되고 오류가 납니다. 굳이 num을 사용하려면 GetNum함수를 return값을 가지는 형태의 함수로 만든 이후 최종 num값을 반환시키고 main함수에서 그 return값을 다른 변수에 저장해두는 방법이 있습니다. for나 if 같은 반복문과 조건문의 중괄호 내에서 선언된 변수도 당연히 지역변수입니다.

<img class="aligncenter size-full wp-image-962" src="/images/2018/09/2-1.jpg" alt="" width="535" height="313" srcset="/images/2018/09/2-1.jpg 535w, /images/2018/09/2-1-300x176.jpg 300w" sizes="(max-width: 535px) 100vw, 535px" /> 

위의 코드를 보시면 지역변수는 { } 안에서만 존재하기 때문에 서로 다른 지역에서는 이름이 같아도 상관이 없습니다.

정확하게 정리를 하자면, **지역변수는 선언이 되는 동시에 메모리공간에 할당이 되었다가 자신이 속한 중괄호를 빠져나가면서 메모리공간에서 소멸됩니다.**

&nbsp;

&nbsp;

# 2. 지역변수와 메모리공간

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

먼저 프로그램이 실행이 되면 main 함수가 실행이 됩니다. main함수의 1행과 2행을 거치며 메모리공간에 네 개의 변수가 다음 그림과 같이 할당됩니다.

<img class="aligncenter wp-image-963" src="/images/2018/09/3-1.jpg" alt="" width="589" height="268" srcset="/images/2018/09/3-1.jpg 900w, /images/2018/09/3-1-300x136.jpg 300w, /images/2018/09/3-1-768x349.jpg 768w" sizes="(max-width: 589px) 100vw, 589px" /> 

그리고 add_result = Add(num1, num2);가 실행이 되며 a,b,c가 메모리에 할당이 돼요.

<img class="aligncenter wp-image-964" src="/images/2018/09/4-1.jpg" alt="" width="580" height="325" srcset="/images/2018/09/4-1.jpg 900w, /images/2018/09/4-1-300x168.jpg 300w, /images/2018/09/4-1-768x430.jpg 768w, /images/2018/09/4-1-678x381.jpg 678w" sizes="(max-width: 580px) 100vw, 580px" /> 

Add 함수는 num1의 값인 10과, num2의 값인 20을 넘겨받으며 a에는 10이, b에는 20이 대입되는겁니다. 한가지 짚고 넘어가야할건, **어떤 변수를 인자로 전달받을 때 실제로 전달받는 것은 그 변수가 가지고 있는 값의 복사본입니다.**

그리고 add\_result = Add(num1, num2);의 코드가 끝이 나면서 메모리공간에 할당되었던 a,b,c는 소멸이 됩니다. 그리고 mul\_result = Mul(num1, num2);의 코드가 실행되면서 또다시 a,b,c라는 변수가 메모리공간에 할당이 됩니다. 물론 이 때의 **a, b, c는 이전에 가지고 있던 a,b,c가 아닌 전혀 새로운 변수**입니다. 그리고, mul_result = Mul(num1, num2); 코드가 끝이 나면 a, b, c는 다시 메모리공간에서 소멸이 됩니다.

마지막으로, main함수가 끝이 나면서 나머지 4개의 변수도 소멸됩니다.

한 가지 추가적으로, 함수의 전달인자는 지역변수일까요? 함수의 전달인자는 해당 함수에서만 사용이 되고 해당 함수를 벗어나면 소멸됩니다. 이러한 특징이 말해주듯, 함수의 전달인자 역시 지역변수 입니다.

&nbsp;

정리

<table class="txc-table" style="border-style: solid; width: 864px; border-color: #000000; background-color: #f2eded;" border="1" width="864" cellspacing="0" cellpadding="0">
  <tr>
    <td>
      1) 지역변수는 <strong>중괄호{ } 내에서 선언된 변수</strong>이다.</p> 
      
      <p>
        2) 지역변수는 해당 함수나 조건문, 반복문의 <strong>중괄호{ }를 빠져나가면 메모리 공간에서 소멸</strong>되어 더 이상 유효하지 않다.
      </p>
      
      <p>
        3) 지역 변수는 같은 이름을 가져도 된다. 메모리 공간에 동시에 할당이 되는 일이 없기 때문이다.</td> </tr> </tbody> </table>