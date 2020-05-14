---
title: (C언어) 30. 2차원 배열과 포인터
date: 2018-10-25T23:28:42+09:00
author: nobbaggu
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - 2차원
  - array
  - array pointer
  - pointer
  - pointer array
  - 메모리
  - 배열
  - 배열 포인터
  - 자료형
  - 포인터
  - 포인터 배열
---
1차원 배열의 이름은 포인터고, 포인터형은 배열의 원소의 자료형과 같습니다. 그리고 포인터 배열의 이름은 더블포인터이며 포인터형은 배열의 원소가 가르키는 변수의 자료형과 같습니다. 포인터형에 집착하는 이유는 포인터형을 알아야 메모리 공간을 얼마나 참조하는지 알 수 있기 때문입니다.

2차원 배열의 이름 역시 1차원 배열의 이름처럼 포인터이며 배열의 첫 번째 원소를 가르킵니다. 사실 2차원 배열의 포인터형은 1차원 배열의 포인터형 처럼 명확한 이름이 없습니다. 하지만 단순합니다.

&nbsp;

&nbsp;

&nbsp;

# 1. 2차원 배열의 포인터형

* * *

**2차원 배열의 이름은 index \[0\]\[0\] 원소를 가르킵니다.**

![image](https://nobbaggu.github.io/images/2018/09/111-1.jpg){: width="50%" height="50%"}

&nbsp;

포인터 형은 결국 포인터가 참조하는 메모리 공간의 byte 수와 직결됩니다.

~~~ c
#include <stdio.h>

int main(void)
{
    int arr1[3][4];
    int arr2[3][7];
    
    printf("%#x,  %d\n", arr1, arr1);
    printf("%#x,  %d\n", arr1+1, arr1+1);
    printf("%#x,  %d\n", arr1+2, arr1+2);
    printf("%#x,  %d\n", arr1+3, arr1+3);
    
    printf("\n");
    
    printf("%#x,  %d\n", arr2, arr2);
    printf("%#x,  %d\n", arr2+1, arr2+1);
    printf("%#x,  %d\n", arr2+2, arr2+2);
    printf("%#x,  %d\n", arr2+3, arr2+3);
    
    return 0;
    }
~~~

&nbsp;

실행결과

0xaffb30,  11533104


0xaffb40,  11533120


0xaffb50,  11533136


0xaffb60,  11533152


0xaffad4,  11533012


0xaffaf0,  11533040


0xaffb0c,  11533068


0xaffb28,  11533096


계속하려면 아무 키나 누르십시오 . . .

&nbsp;

arr1은 +1 연산에서 16씩 증가하는 것을 보면 16byte 만큼씩의 메모리공간을 참조합니다. arr2는 +1연산에서 28씩 증가하며 이는 28byte만큼의 메모리공간을 참조한다는 겁니다. arr1과 arr2 모두 4byte의 int형 데이터를 원소로 가집니다. arr1은 한 행에 원소를 4개 가지고있고, arr2는 한 행에 원소를 7개 가지고 있습니다. arr1은 4byte x 4 = 16byte 만큼의 메모리공간을 참조하고, arr2는 4byte x 7 = 28byte 만큼의 메모리공간을 참조합니다.

**2차원배열의 이름은 한 번에 한 행씩 참조합니다. 따라서 +1연산을 하면 바로 다음 행을 가르키게 됩니다. 즉, 2차원 배열의 이름은 포인터형이 sizeof(원소의 자료형) × 한 행의 원소의 갯수(배열의 열의 갯수)가 됩니다. int형, double형 처럼 깔끔한 이름을 가지지는 않습니다.**

![image](https://nobbaggu.github.io/images/2018/09/222-1.jpg){: width="50%" height="50%"}

&nbsp;

&nbsp;

&nbsp;

# 2. 배열 포인터

* * *

예제를 하나 보겠습니다.

~~~ c
 #include <stdio.h>

void Increase(int (*ptr)[4])
{
    (ptr[0][0])++;
    (ptr[1][1])++;
    (ptr[2][2])++;
}

int main(void)
{
    int arr[3][4] = { {1,2,3,4},{5,6,7,8},{9,10,11,12}};
    int i,j;
    
    for(i=0; i<3; i++)
    {
        for(j=0; j<4; j++)
        {
            printf("%3d", arr[i][j]);
        }
        printf("\n");
    }
    printf("\n");
    
    Increase(arr);
    
    for(i=0; i<3; i++)
    {
        for(j=0; j<4; j++)
        {
            printf("%3d", arr[i][j]);
        }
        printf("\n");
    }
    return 0;
}
~~~

실행결과

1  2  3  4


5  6  7  8


9 10 11 12


2  2  3  4


5  7  7  8


9 10 12 12


계속하려면 아무 키나 누르십시오 . . .

arr는 3×4 배열이므로 포인터형은 4×sizeof(int)  입니다. 이 배열을 함수의 전달인자로 주려면 int형 데이터를 가르키면서 한 번에 4 칸씩 가르키는 곳의 위치를 변경하는 포인터가 필요합니다. int (*ptr) [4]; 와 같이 선언하면 됩니다. 2차원 배열을 위한 포인터는 일반적으로 다음과 같은 선언 형태를 가집니다.

![image](https://nobbaggu.github.io/images/2018/09/333-1.jpg){: width="50%" height="50%"}

이같은 포인터를 '**배열포인터' **라고 합니다.

**'포인터 배열'과 '배열 포인터'는 엄연히 다릅니다.**

int * ptr[4]; // 배열의 원소로 포인터를 가지는 **포인터 배열**

int (*ptr) [4]; // 배열 이름의 포인터형을 결정지을 때 선언하는 **배열 포인터**

&nbsp;

&nbsp;

&nbsp;

# 3. 2차원 배열의 메모리 참조

* * *

2차원 배열은 index를 하나만 사용하여 하나의 행을 표현할 수 있습니다.

arr[0]은 첫 번째 행, arr[1]은 두 번째 행, . . . 을 의미합니다.

~~~ c
#include <stdio.h>

int main(void)
{
    int arr[3][5];
    
    printf("arr가 가르키는 곳 :    %#x = %d, arr의 크기    : %d\n", arr, arr, sizeof(arr));
    printf("arr[0]이 가르키는 곳 : %#x = %d, arr[0]의 크기 : %d\n", arr[0], arr[0], sizeof(arr[0]));
    printf("arr[1]이 가르키는 곳 : %#x = %d, arr[1]의 크기 : %d\n", arr[1], arr[1], sizeof(arr[1]));
    printf("arr[2]가 가르키는 곳 : %#x = %d, arr[2]의 크기 : %d\n", arr[2], arr[2], sizeof(arr[2]));
    
    return 0;
}
~~~

실행결과

arr가 가르키는 곳 :    0x59f8ac = 5896364, arr의 크기    : 60


arr[0]이 가르키는 곳 : 0x59f8ac = 5896364, arr[0]의 크기 : 20


arr[1]이 가르키는 곳 : 0x59f8c0 = 5896384, arr[1]의 크기 : 20


arr[2]가 가르키는 곳 : 0x59f8d4 = 5896404, arr[2]의 크기 : 20


계속하려면 아무 키나 누르십시오 . . .

arr와 arr[0]은 같은 곳을 가르킵니다. arr는 배열 전체의 첫 번째 원소이자 첫 번째 행의 첫 번째 원소를 가르키고 arr[0]은 첫 번째 행의 첫 번째 원소를 가르킵니다. 두 포인터가 같은 곳을 가르키고 있지만 참조하는 메모리의 크기는 다릅니다. 다음 코드를 실행해보시면 알 수 있습니다.

~~~ c
#include <stdio.h>

int main(void)
{
    int arr[3][5];

    printf("arr가 가르키는 곳 :    %#x = %d\n", arr, arr);
    printf("arr[0]이 가르키는 곳 : %#x = %d\n", arr[0], arr[0]);
    printf("arr[1]이 가르키는 곳 : %#x = %d\n", arr[1], arr[1]);
    printf("arr[2]가 가르키는 곳 : %#x = %d\n", arr[2], arr[2]);
    printf("\n");
    printf("arr[0]     = %d\n", arr[0]);
    printf("arr[0] + 1 = %d\n", arr[0]+1);
    printf("arr[0] + 2 = %d\n", arr[0]+2);
    printf("\n");
    printf("arr[1]     = %d\n", arr[1]);
    printf("arr[1] + 1 = %d\n", arr[1]+1);
    printf("arr[1] + 2 = %d\n", arr[1]+2);
    printf("\n");
    printf("arr[2]     = %d\n", arr[2]);
    printf("arr[2] + 1 = %d\n", arr[2]+1);
    printf("arr[2] + 2 = %d\n", arr[2]+2);

    return 0;
}
~~~

실행결과

arr가 가르키는 곳 :    0xcff8ac = 13629612


arr[0]이 가르키는 곳 : 0xcff8ac = 13629612


arr[1]이 가르키는 곳 : 0xcff8c0 = 13629632


arr[2]가 가르키는 곳 : 0xcff8d4 = 13629652




arr[0]     = 13629612


arr[0] + 1 = 13629616


arr[0] + 2 = 13629620


 

arr[1]     = 13629632


arr[1] + 1 = 13629636


arr[1] + 2 = 13629640


 

arr[2]     = 13629652


arr[2] + 1 = 13629656


arr[2] + 2 = 13629660


계속하려면 아무 키나 누르십시오 . . .

arr는 한 번에 한 행씩 참조합니다. 하지만 arr[0], arr[1], arr[2]는 +1 연산의 결과로 4byte 만큼씩 커지며 이는 한 번에 원소 하나씩을 참조하기 때문입니다.

<table style="width: 100%; border-collapse: collapse; border-color: #000000; background-color: #f5f2f2;" border="1">
  <tr>
    <td style="width: 100%;">
      2차원 배열의 이름 arr는 한 번에 한 행씩 메모리공간을 참조하고, arr[N]은 N+1 번째 행의 한 원소의 자료형의 크기만큼 메모리공간을 참조한다.
    </td>
  </tr>
</table>

1차원 배열과 포인터의 관계를 완벽히 나타내는 식은 다음과 같았습니다.

**arr[i]=*(arr+i)**

배열의 index로 접근하던지, 포인터 연산을 통해 접근하던지 같습니다.

&nbsp;

2차원 배열에서 데이터에 접근하기 위해 포인터 연산, 배열의 index 중 아무거나 사용할 수 있습니다.

**1. arr\[i\]\[j\] : [i]를 이용하여 i+1번째 행에 접근한 이후 [j]를 이용하여 j+1번째 원소에 접근**

**2. (\*(arr+i))[j] : +i와 \*연산을 하여 i+1번째 행에 접근한 이후 [j]를 이용하여 j+1번째 원소에 접근**

**3. \*((arr[i])+j) : [i]를 이용하여 i+1번째 행에 접근한 이후 +j와 \* 연산을 이용하여 j+1번째 원소에 접근**

**4. \*((\*(arr+i))+j) : +i와 \*연산을 이용하여 i+1번째 행에 접근한 이후 +j와 \*를 이용하여 j+1번째 원소에 접근**

위의 식들은 표현만 다를 뿐 원리는 같습니다.

~~~ c
#include <stdio.h>

int main(void)
{
char arr[3][3] = { {'A','B','C'},{'D','E','F'},{'G','H','I'}};
int i,j;

for(i=0; i<3; i++)
{
    for(j=0; j<3; j++)
    {
        printf("%2c",arr[i][j]);
    }
    printf("\n");
}
    
printf("\n");

for(i=0; i<3; i++)
{
    for(j=0; j<3; j++)
    {
        printf("%2c",*(arr[i]+j));
    }
    printf("\n");
}
printf("\n");

for(i=0; i<3; i++)
{
    for(j=0; j<3; j++)
    {
        printf("%2c",(*(arr+i))[j]);
    }
    printf("\n");
}

printf("\n");

for(i=0; i<3; i++)
{
    for(j=0; j<3; j++)
    {
        printf("%2c",*(*(arr+i)+j));
    }
    printf("\n");
}

printf("\n");

}
~~~

실행결과

A B C


D E F


G H I


A B C


D E F


G H I


A B C


D E F


G H I


A B C


D E F


G H I


계속하려면 아무 키나 누르십시오 . . .