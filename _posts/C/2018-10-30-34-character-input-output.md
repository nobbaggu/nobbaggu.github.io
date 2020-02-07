---
title: (C언어) 34. 문자 입출력 함수들
date: 2018-10-30T22:41:16+09:00
author: nobbaggu
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - character
  - C언어
  - EOF
  - fgetc
  - fputc
  - getchar
  - input()
  - output
  - putchar
  - string
  - 문자
  - 문자열
  - 입력
  - 입출력
  - 출력
---
# 1. scanf 함수의 한계

* * *

입출력되는 대부분의 데이터는 문자나 숫자 형태입니다. 변수나 배열을 이용하여 숫자와 문자, 그리고 문자열을 저장할 수 있고 printf함수와 scanf함수를 이용하여 문자와 문자열을 저장 및 표현할 수 있었습니다.

scanf함수가 문자열을 구분하는 기준은 '띄어쓰기'입니다. scanf함수는 띄어쓰기를 만나면 거기가 문자열의 끝이라 생각하고 저장을 거기서 끝내버립니다. 따라서 띄어쓰기가 있는 문자열을 저장할 수 없었습니다.

~~~ c
#include <stdio.h>

int main(void)
{
    char str[100];
    
    printf("Type any sentences : ");
    
    scanf("%s", str);
    
    printf("%s\n", str);
    
    return 0;
}
~~~

실행결과

Type any sentences : Hello there


Hello


계속하려면 아무 키나 누르십시오 . . .

위 예제에서 scanf함수를 사용하여 배열 str에 "Hello there"이라는 문자열을 저장하려했지만 정작 담긴 데이터는 Hello 까지입니다.

scanf 함수를 대체할 수 있고 공백도 포함할 수 있는 문자열 입출력 함수들이 있습니다. 문자열 입출력 함수들에 대해서는 다음 포스팅에 설명 드리고, 오늘은 이전에 문자 입출력 함수에 대해 알아보겠습니다.

&nbsp;

&nbsp;

# 2. 문자 출력 함수

* * *

putchar와 fputc는 문자 1개를 출력하는 함수입니다.

~~~ c
int putchar(int ch); // 문자전달, 표준 출력 스트림으로 전송
int fputc(int ch, FILE * stream);  //문자 전달, 원하는 스트림 지정하여 전송
~~~

두 함수 모두 반환형이 int입니다. char 자료형과 마찬가지로 int 자료형 역시 정수 자료형이므로 문자가 대입될 수 있습니다.

putchar는 전달인자로 원하는 문자를 넣어주면 됩니다. 그리고 그것을 '표준 출력 스트림'으로 보내 '모니터'를 통해 출력을 시킵니다.

fputc는 전달인자로 문자이외에도 전송 스트림도 넣어줘야 합니다. putchar와는 다르게 표준 출력 스트림 이외의 다른 스트림으로도 전송이 가능합니다.

&nbsp;

&nbsp;

# 3. 문자 출력 함수

* * *

getchar와 fgetc는 문자 하나를 입력으로 받는 함수입니다.

~~~ c
int getchar(void); // 문자를 사용자로부터 표준 입력 스트림을 통해 입력받아 int형으로 반환
int fgetc(FILE * stream);  // 문자를 사용자로부터 지정된 스트림을 통해 입력받아 int형으로 반환
~~~

getchar함수는 사용자로부터 문자를 하나 입력받아서 반환시켜주는 함수입니다. 따로 스트림을 지정하지 않아 표준 입력 스트림을 통해 데이터를 받습니다.

fgetc함수는 사용자로부터 문자를 하나 입력받고 반환시켜 줍니다. 하지만 getchar함수와는 다르게 지정받은 스트림을 통하여 데이터를 받을 수 있습니다.

&nbsp;

&nbsp;

# 4. 예제

* * *

~~~ c
#include <stdio.h>

int main(void)
{
    int ch1, ch2;
    
    printf("A or B? : ");
    ch1 = getchar(); //getchar함수를 사용하여 문자를 입력받아 변수 ch1에 저장
    putchar(ch1); // putchar함를 사용하여 ch1에 저장된 문자 출력
    putchar('\n');
    
    printf("C or D? : ");
    ch2 = fgetc(stdin); // fgetc함수를 사용하여 문자를 입력받아 ch1에 저장. 입력 스트림은 표준입력스트림으로 지정
    fputc(ch2, stdin); //fputc함수를 사용하여 ch2에 저장된 문자 출력. 출력 스트림은 표준출력스트림으로 지정
    fputc('\n', stdout);
    
    return 0;
}
~~~

출력결과

A or B? : A


A


C or D? :


계속하려면 아무 키나 누르십시오 . . .

&nbsp;

'C or D?' 문장이 출력이 되자마자 우리가 입력할 기회도 얻지 못하고 프로그램은 끝이 납니다. 앞선 질문에서 우리는 'A'를 입력했습니다. 이어서 엔터 '\n'도 입력했는데, getchar() 함수는 문자를 하나밖에 저장하지 못해서 버퍼에 남아있던 '\n'이 뒤의 fgetc()함수로 입력이 된 것입니다. 이 부분은 이후에 버퍼에 대해 배우면 이해가 되실겁니다.

반복문을 사용하면 문자 입출력 함수로 문자열도 입출력 시킬 수 있습니다.

~~~ c
#include <stdio.h>

int main(void)
{
    int ch=0;
    
    while(ch!=EOF) //EOF는 getchar함수가 더 이상 읽을 게 없을 때 반환하는 값으로 -1입니다.
    {
        ch = getchar();
        putchar(ch);
    }
    
    return 0;
}
~~~

실행결과

C is very easy!


C is very easy!


Yo man


Yo man


^Z


계속하려면 아무 키나 누르십시오 . . .

우리가 문장을 입력해주고 엔터를 치면 버퍼에 담겨있던 데이터들이 while문을 통해 하나씩하나씩 출력이 됩니다. while문의 조건은 ch에 저장된 값, 즉 getchar() 함수가 읽어들이는 문자가 EOF가 되면 반복문을 종료시켜달라는 겁니다. **EOF란 End Of File의 약자로 문자나 문자열 입력 함수들이 더 이상 읽어들일 값이 없거나 어떠한 에러로인해 호출에 실패하였을 때 반환하는 값**입니다. Window에서  **EOF는 Ztrl + Z 로 입력**합니다.