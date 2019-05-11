---
title: 26. 포인터와 함수
date: 2018-10-11T22:14:12+09:00
author: SWnomad
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - array
  - C언어
  - pointer
  - 배열
  - 복사값
  - 전달인자
  - 포인터
  - 함수
---
# 1. 함수의 전달인자

* * *

오늘은 배열과 포인터가 함수의 인자로 전달되는 과정에 대해 이야기 할겁니다. 이전에 일반적인 변수가 전달인자로서 함수로 전달되는 과정을 다시 한 번 되짚어보겠습니다.

<img class="aligncenter wp-image-995" src="/images/2018/09/11.jpg" alt="" width="320" height="226" srcset="/images/2018/09/11.jpg 594w, /images/2018/09/11-300x212.jpg 300w" sizes="(max-width: 320px) 100vw, 320px" /> 

&nbsp;

<table style="height: 134px; width: 99.8788%; border-collapse: collapse; border-color: #000000; background-color: #ede8e8;" border="1">
  <tr style="height: 134px;">
    <td style="width: 99.8788%; height: 134px;">
      <b>1. 전달인자로 어떠한 변수를 전달시켰을 때, 실제로 전달되는 것은 그 변수의 값의 복사본이다. 즉, 함수가 어떠한 변수를 전달받아도 실제로 그 변수의 메모리공간상 데이터에는 접근할 수 없다.</b></p> 
      
      <p>
        <b>2. 전달인자로 선언된 이외의 자료형은 전달받을 수 없다. 즉, 함수를 선언할 때 전달받고자 하는 데이터의 자료형을 변수, 배열, 포인터 등에 따라 적절히 명시해 주어야한다. </b></td> </tr> </tbody> </table> 
        
        <p>
          &nbsp;
        </p>
        
        <p>
          다음 코드를 봅시다.
        </p>
        
        ~~~ c
#include <stdio.h>

double Div(int, int);

int main(void)
{
    int money = 10;
    int people = 20;
    
    double moneyperpeople = Div(money,people);
    
    printf("The money per 1 person : %f\n", moneyperpeople);
}

double Div(int num1, int num2)
{
    double result = (double)num1/(double)num2;
    
    return result;
}
~~~
        
        <p>
          실행결과
        </p>
        
        <p>
          <code>The money per 1 person : 0.500000<br />
계속하려면 아무 키나 누르십시오 . . . </code>
        </p>
        
        <p>
          위에서는 int형 변수 2개를 입력받아 두 번째 인자로 첫 번째 인자를 나눈 값을 반환시키는 Div 함수를 정의했습니다. 전달되는 인자는 함수에서 정의된 전달인자의 자료형과 일치하고 반환형도 일치합니다.
        </p>
        
        <p>
          &nbsp;
        </p>
        
        <p>
          &nbsp;
        </p>
        
        <h1>
          2. 포인터와 배열의 함수 전달
        </h1>
        
        <hr />
        
        <p>
          포인터나 배열도 함수의 인자로 전달할 수 있습니다.
        </p>
        
        ~~~ c
#include <stdio.h>

void Increase(int *);

int main(void)
{
    int age = 10;
    printf("The age is %d\n", age);
    
    Increase(&age);
    printf("The age is %d\n", age);
    
    Increase(&age);
    printf("The age is %d\n", age);
    
    return 0;
}

void Increase(int * num)
{
    (*num)++;
}
~~~
        
        <p>
          &nbsp;
        </p>
        
        <p>
          실행결과
        </p>
        
        <p>
          <code>The age is 10<br />
The age is 11<br />
The age is 12<br />
계속하려면 아무 키나 누르십시오 . . . </code>
        </p>
        
        <p>
          Increase 함수는 포인터를 전달인자로 받아 그 포인터가 가르키는 메모리공간의 주소값에 *연산자를 이용해 접근해서 데이터를 +1 증가시키는 연산을 합니다. 반환형은 없어 void를 썼습니다. main 함수에서 age 변수를 선언하면서 10으로 초기화 했습니다. 그리고 Increase함수에 age변수의 주소값을 전달해주면서 함수를 호출했습니다. 실행결과를 보시면 age값이 증가된 것이 보입니다. 실제로 age라는 이름의 메모리공간에 접근해서 데이터를 바꾼것입니다. 이런 일은 일반 변수로는 불가능합니다.
        </p>
        
        ~~~ c
#include <stdio.h>

void Increase(int);

int main(void)
{
    int age = 10;
    printf("The age is %d\n", age);
    
    Increase(age);
    printf("The age is %d\n", age);
    
    Increase(age);
    printf("The age is %d\n", age);
    
    return 0;
}

void Increase(int num)
{
    num++;
}
~~~
        
        <p>
          실행결과
        </p>
        
        <p>
          <code>The age is 10<br />
The age is 10<br />
The age is 10<br />
계속하려면 아무 키나 누르십시오 . . . </code>
        </p>
        
        <p>
          여기서 Increase 함수는 전달인자로 받은 값을 ++연산 시켜줍니다. 하지만 출력을 해보면 age의 값이 변하지 않은 것을 볼 수 있습니다. Increase 함수는 age라는 변수의 값인 10의 복사값을 전달받아 ++연산을 진행합니다. 그리고 함수의 호출이 끝이나는 동시에 메모리공간에서 사라집니다. 애초에 함수의 실행결과를 사용하려면 반환형을 가지는 형태로 함수의 반환형을 바꾼다음 함수를 호출하고 함수가 반환하는 값을 변수에 저장해두고 써먹던가 해야합니다. 다음과 같이 말입니다.
        </p>
        
        ~~~ c
#include <stdio.h>

int Increase(int);

int main(void)
{
    int age = 10;
    printf("The age is %d\n", age);
    
    age = Increase(age);
    printf("The age is %d\n", age);
    
    age = Increase(age);
    printf("The age is %d\n", age);
    
    return 0;
}

int Increase(int num)
{
    num++;
    
    return num;
}
~~~
        
        <p>
          실행결과
        </p>
        
        <p>
          <code>The age is 10<br />
The age is 11<br />
The age is 12<br />
계속하려면 아무 키나 누르십시오 . . . </code>
        </p>
        
        <p>
          반환형을 int로 바꾸고 age에 계속해서 반환되는 값을 저장해가며 age를 증가시킵니다. 이것이 일반 변수와 포인터의 차입니다. 일반변수는 그 복사값을 전달받기 때문에 직접적으로 데이터에 접근할 수 없습니다. 함수의 호출이 끝이나는 동시에 그 함수가 가지는 모든 변수는 할당되었던 메모리 공간에서 사라집니다. 물론 포인터형태로 전달받는 값도 복사된 값은 맞습니다. 하지만 주소값의 복사값입니다. 복사값이던 뭐던 주소를 알고있기때문에 그 주소를 참조하여 메모리공간에 접근해서 데이터를 바꾸는 것이 가능합니다.
        </p>
        
        <p>
          &nbsp;
        </p>
        
        <p>
          <img class="aligncenter wp-image-996" src="/images/2018/09/22.jpg" alt="" width="522" height="237" srcset="/images/2018/09/22.jpg 895w, /images/2018/09/22-300x136.jpg 300w, /images/2018/09/22-768x349.jpg 768w" sizes="(max-width: 522px) 100vw, 522px" />
        </p>
        
        <p>
          &nbsp;
        </p>
        
        <p>
          <img class="aligncenter wp-image-997" src="/images/2018/09/33.jpg" alt="" width="463" height="221" srcset="/images/2018/09/33.jpg 900w, /images/2018/09/33-300x143.jpg 300w, /images/2018/09/33-768x367.jpg 768w" sizes="(max-width: 463px) 100vw, 463px" />
        </p>
        
        <p>
          배열의 이름은 포인터라는 것을 우리는 알고 있습니다. 따라서 포인터를 전달인자로 받을 수 있다는 것은 배열도 함수에 전달이 가능하다는 것입니다.
        </p>
        
        ~~~ c
#include <stdio.h>

void Aging(int[], int);

int main(void)
{
    int people[5] = {10,31,44,9,12};
    int i;
    
    for(i = 0; i < sizeof(people)/sizeof(int); i++)
    {
        printf("%d\n", people[i]);
    }
    
    printf("\n");
    
    Aging(people, sizeof(people)/sizeof(int));
    
    for(i = 0; i < sizeof(people)/sizeof(int); i++)
    {
        printf("%d\n", people[i]);
    }
    
    return 0;
}

void Aging(int arr[], int length) //배열의 길이정보를 받을 length도 선언
{
    int i;
    
    for(i = 0; i<length; i++)
    {
        (arr[i])++;
    }
}
~~~
        
        <p>
          실행결과
        </p>
        
        <p>
          <code>10<br />
31<br />
44<br />
9<br />
12</code>
        </p>
        
        <p>
          <code><br />
11<br />
32<br />
45<br />
10<br />
13<br />
계속하려면 아무 키나 누르십시오 . . .</code>
        </p>
        
        <p>
          위 코드에서 함수를 정의할 때 전달인자부분에서 int arr[] 대신 int * arr를 사용해도 됩니다. 어차피 <b>배열이 인자로 전달 될 때 실제로 전달되는 정보는 배열의 가장 첫 번째 원소의 주소값</b>이기 때문입니다. 하지만 배열 자체의 전달목적이 클 때는 배열의 형태로 전달인자 부분을 선언해주는것이 코드의 가독성을 높이기때문에 상황에 따라 적절한 선언이 필요합니다.
        </p>
        
        <p>
          &nbsp;
        </p>
        
        <p>
          &nbsp;
        </p>
        
        <p>
          &nbsp;
        </p>
        
        <h1>
          3. 배열을 전달하면 배열의 주소값이 전달된다
        </h1>
        
        <hr />
        
        ~~~ c
#include <stdio.h>

void arrsize1(int * arr)
{
    printf("%d\n", sizeof(arr));
}

void arrsize2(int arr[])
{
    printf("%d\n", sizeof(arr));
}

int main(void)
{
    int arr[] = {1,2,3,4,5,6,7};
    
    arrsize1(arr);
    arrsize2(arr);
    
    return 0;
}
~~~
        
        <p>
          실행결과
        </p>
        
        <p>
          <code>4<br />
4<br />
계속하려면 아무 키나 누르십시오 . . . </code>
        </p>
        
        <p>
          전달인자로 받은 배열의 사이즈를 계산하는 함수 2 개를 정의하였습니다. 하나는 전달인자로 배열을 받고 다른 하나는 포인터변수를 받습니다. 하지만 둘 다 sizeof 연산을 통해 전달인자로 받은 것들의 byte 크기를 계산하여보면 4가 나옵니다. 배열은 int형 변수 7개를 가지고 있기때문에 총 메모리 크기가 28byte라서 28이 나오길 기대했는데 말입니다. 4가 나온 이유는 제 컴파일러는 메모리공간의 주소값을 표현하는데 32bit = 4byte를 사용하기 때문입니다. 이로부터 알 수 있는것은 <b>함수 내에서는 인자로 전달받은 배열의 길이를 계산할 수 없다는 겁니다. 왜냐하면 전달인자로 받는것은 배열이나 포인터나 동일하게 주소값을 전달받기 때문입니다. 전달되는 것은 배열 자체가 아닌 배열 이름이 가지는 주소값이었던 것입니다. 따라서 함수에 전달인자로 배열을 전달할 때에는 필요한 경우 배열의 길이도 같이 전달해주어야합니다.</b>
        </p>
        
        <p>
          오늘 말씀드린 내용을 정리해봅시다.
        </p>
        
        <table style="width: 100%; border-collapse: collapse; border-color: #000000; background-color: #ebebeb;" border="1">
          <tr>
            <td style="width: 100%;">
              1. 함수가 전달인자로 받는 것은 변수가 아닌 변수의 값의 복사본이다. 따라서 실제로 전달받은 변수의 메모리공간에는 접근할 수 없다.</p> 
              
              <p>
                2. 포인터의 형태로 전달인자를 선언하고 변수의 주소값을 넘겨받는 형태로 변수의 메모리에 직접적인 접근이 가능하다.
              </p>
              
              <p>
                3. 포인터나 배열도 함수의 전달인자를 통해 전달될 수 있다.
              </p>
              
              <p>
                4. 배열을 전달인자로 받을 때 실제로 받는 것은 배열의 이름, 즉 배열의 첫 번째 원소의 주소값이다. 이와 관련되어 배열이나 포인터변수를 전달인자로 넘겨받고 싶을 때 함수의 전달인자의 선언은 포인터나 배열 무엇으로 해도 된다.
              </p>
              
              <p>
                5. 함수 내에서는 배열의 길이를 계산할 수 없기때문에 필요하다면 배열의 길이를 넘겨받을 변수를 같이 선언해주어야한다.</td> </tr> </tbody> </table> 
                
                <p>
                  &nbsp;
                </p>