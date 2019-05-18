---
title: (C언어) 22. 배열(2) - 배열과 문자열
date: 2018-10-07T15:21:48+09:00
author: SWnomad
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - '&amp;'
  - '%s'
  - array
  - C언어
  - pointer
  - scanf
  - string
  - 문자열
  - 배열
  - 서식문자
  - 연산자
  - 포인터
---
문자열 이전에 문자에 관해 상기 해보겠습니다.

~~~ c
#include <stdio.h>

int main(void)
{
    char ch1 = 'A';
    char ch2 = 65;
    
    printf("ch1 dec : %d, ch1 ASCII : %c\n", ch1, ch1);
    printf("ch2 dec : %d, ch2 ASCII : %c\n", ch2, ch2);
    
    return 0;
}
~~~

실행결과

ch1 dec : 65, ch1 ASCII : A


ch2 dec : 65, ch2 ASCII : A


계속하려면 아무 키나 누르십시오 . . .

문자 'A'의 10진수 ASCII code는 10입니다. 그리고 10진수 65의 ASCII 문자는 'A' 입니다. 'A'를 저장하던지 십진수 65를 저장 하던지 같은 겁니다. 물론 문자를 표현할 때 char형 말고 다른 정수 자료형을 써도 되지만 아스키 코드는 0~127의 정수를 다루기 때문에 char 자료형만으로도 충분히 표현가능하기 때문에 문자를 저장할 때 보통 char 자료형을 쓰는 것입니다.

&nbsp;

&nbsp;

# 1. 배열에 여러 개의 문자를 저장

* * *

아무튼 변수에는 문자 '한 개'만 저장이 가능합니다. 문자가 여러개인 문자열은 하나의 변수에 저장이 되지 않습니다. 배열을 이용하면 문자열을 저장할 수 있습니다.

~~~ c
#include <stdio.h>

int main(void)
{
    char str[] = {'C',' ','i','s',' ','v','e','r','y',' ','e','a','s','y','!'};
    int i;
    
    for(i = 0; i<sizeof(str)/sizeof(char); i++)
    {
        printf("%c",str[i]);
    }
    printf("\n");
    printf("The length of str : %d\n", sizeof(str)/sizeof(char));
    
    return 0;
}
~~~

실행결과

C is very easy!


The length of str : 15


계속하려면 아무 키나 누르십시오 . . . 

위와 같이 우리는 문자를 하나하나 배열에 저장할 수 있습니다. 하지만 하나하나 저장하기가 너무 귀찮습니다.

&nbsp;

&nbsp;

# 2. 큰 따움표를 사용한 문자열 저장

* * *

배열을 선언하지 않고도 선언하는 방법이 있는데, 큰 따움표 " " 를 사용하는 것입니다.

~~~ c
#include <stdio.h>

int main(void)
{
    char str[]="C is very easy!";
    
    printf("%s\n", str);
    printf("The length of str : %d\n", sizeof(str)/sizeof(char));
    
    return 0;
}
~~~

실행결과

C is very easy!


The length of str : 16


계속하려면 아무 키나 누르십시오 . . . 

**%s는 문자열을 출력해주는 서식문자입니다.**

그리고 저장한 문자열의 길이를 출력했는데,  16이 나왔습니다. 위에서 문자 하나하나를 저장했던 것과 다른 결과입니다. 비밀은 바로 큰 따움표 " " 입니다.

**큰 따움표 " "를 이용하여 문자열을 저장하면 항상 마지막에는 null문자 \0이 추가가 됩니다. 그리고 null문자 \0은 ASCII code 값 0에 해당합니다. 그리고 이 null문자를 %c를 이용하여 문자형태로 출력하면 아무런 출력이 발생하지 않습니다. 그리고 이 null 문자 '\0'은 공백문자 ' '와는 엄연히 다릅니다.**

<img class="aligncenter wp-image-974" src="/images/2018/09/1-9.jpg" alt="" width="673" height="86" srcset="/images/2018/09/1-9.jpg 900w, /images/2018/09/1-9-300x38.jpg 300w, /images/2018/09/1-9-768x98.jpg 768w" sizes="(max-width: 673px) 100vw, 673px" /> 

**C언어에서 무조건 모든 문자열의 끝에는 null문자가 삽입됩니다. 반대로 null문자가 존재하면 그것은 문자열로 인식이 가능합니다. 그리고 <span style="text-decoration: underline;">%s 서식문자는 배열의 시작부터 문자를 하나하나 출력해주다가 바로 null문자를 만나면 출력을 종료해합니다.</span> null 문자가 필요한 이유기도 합니다. 어디까지를 하나의 문자열로 인식하여 출력해주는지 null문자가 기준이 되어주는 것입니다.**

%s를 이용하여 문자열을 출력할 수 있는 것처럼, 사용자로부터 문자열을 입력받는것도 당연히 가능합니다.

~~~ c
#include <stdio.h>

int main(void)
{
    char str[100];
    
    printf("Type a sentence : ");
    scanf("%s", str);
    
    printf("%s\n", str);
    
    return 0;
}
~~~

&nbsp;

실행결과

Type a sentence : Hello


Hello


계속하려면 아무 키나 누르십시오 . . . 

Hello를 입력하면 마지막에 null문자가 자동으로 삽입이 되어 Hello\0으로 저장이 됩니다.

&nbsp;

&nbsp;

&nbsp;

# 3. scanf 함수와 문자열

* * *

그런데 scanf 함수를 이용하여 변수에 데이터를 저장할 때는 & 연산자를 붙여주었었습니다. 그런데 이번에는 없습니다. **&연산자는 주소값을 반환시켜주는 연산자입니다.** 가령 변수 num에 &연산자를 이용하여 &num으로 표현하면 num 변수의 주소값이 반환됩니다. scanf("%d", &num)은 사용자로부터 입력을 받아 10진수 정수 형태로 num이라는 변수의 주소가 가르키는 메모리공간에 저장을하라는 의미인데, **사실 배열의 이름은 그 자체가 메모리공간의 주소를 의미합니다. 더 정확히는 그 배열에 할당된 메모리공간의 첫 번째 byte의 주소를 의미합니다.** 따라서 &연산자를 붙여줄 필요가 없던겁니다. 사실 **서식문자 %s의 원리도 배열의 이름인 메모리공간의 첫 byte 주소값부터 시작하여 null문자를 만날 때 까지 출력을 해주는 것이었습니다.** 이 부분은 포인터를 배우면 확실히 이해가 갈 것이기 때문에 **%s 서식문자로 배열을 출력할 때에는 &연산자를 사용하지 않는다**는 것 정도만 아시면 됩니다.

우리가 처음에 출력한 "C is very easy!" 라는 문장을 출력할 때는 %s 서식문자를 사용할 수 없습니다. 왜냐하면 마지막에 null 문자를 넣어주지 않아 %s 서식문자가 어디까지를 문자열의 끝으로 인식할 지 알 수 없기 때문입니다.

~~~ c
#include <stdio.h>

int main(void)
{
    char str1[] = {'C',' ','i','s',' ','v','e','r','y',' ','e','a','s','y','!'};
    char str2[] = {'C',' ','i','s',' ','v','e','r','y',' ','e','a','s','y','!','\0'};
    
    printf("%s\n", str1);
    printf("%s\n", str2);
    
    return 0;
}
~~~

실행결과

C is very easy!儆儆?．???


C is very easy!


계속하려면 아무 키나 누르십시오 . . . 

첫 번째 배열은 null문자가 없어 끝에 알 수 없는 값을 출력합니다.

또 한 가지 팁을 드리자면 scanf 함수를 이용하여 문자열을 입력받을 때 띄어쓰기를 사용할 수 없습니다. 띄어쓰기가 없는 문자열만 넣을 수 있습니다.

~~~ c
#include <stdio.h>

int main(void)
{
    char str[100];
    
    printf("Type a sentence : ");
    scanf("%s", str);
    
    printf("%s\n", str);
    
    return 0;
}
~~~

실행결과

Type a sentence : C is very easy!


C


계속하려면 아무 키나 누르십시오 . . .

scanf 함수는 띄어쓰기나 탭 등의 공백을 기준으로 문자열을 구분짓습니다. 위와 같은 경우에는 C\0 , is\0 , very\0 , easy!\0 처럼 총 4개의 문자열로 인식을 합니다. scanf 함수는 문자열을 입력받기에 적절하지 않은 함수입니다. 나중에 문자열을 입력받기 위한 함수도 소개드릴 겁니다.