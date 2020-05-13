---
title: (C언어) 36. <string.h> 헤더파일이 제공하는 문자열 관련 함수들
date: 2018-10-31T02:01:44+09:00
author: nobbaggu
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - C언어
  - strcat
  - strcmp
  - strcpy
  - string.h
  - strlen
  - strncat
  - strncmp
  - strncpy
  - 문자열
  - 함수
---
C언어가 기본적으로 제공하는 라이브러리 중 'string.h' 헤더파일에는 문자열 관련 유용한 함수들이 정의되어 있습니다.

&nbsp;

# 1. 문자열 길이 반환 : **strlen**

* * *

**strlen 함수는 전달인자로 받은 문자열의 시작부터 null문자 이전까지의 길이를 반환합니다. 즉, null문자는 길이정보에 포함시키지 않습니다.**

~~~ c
#include <stdio.h>
#include <string.h>

int main(void)
{
    int n;
    char * ptr = "Hello";
    n = strlen("Hello"); //strlen이 반환하는 문자열의 길이를 변수 n에 저장
    
    printf("%d\n", n); //n 출력
    printf("%d\n", strlen("Hello")); //strlen함수가 반환하는 문자열길이 출력
    printf("%d\n", strlen(ptr)); //strlen함수가 반환하는 ptr이 가르키는 문자열 길이 출력
    
    return 0;
}
~~~

실행결과

5


5


5


계속하려면 아무 키나 누르십시오 . . .

우리가 예상한것처럼 각각 5,6이 나왔습니다. 이는 null문자가 제외된 문자열의 길이입니다.

&nbsp;

&nbsp;

&nbsp;

# 2. 문자열 복사 : **strcpy / strncpy**

* * *

~~~ c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char str1[15] = "ABCDEFGHIJ";
    char str2[15];
    
    strcpy(str2, str1);
    
    printf("%s\n", str2);
    
    return 0;
}
~~~

실행결과

`ABCDEFGHIJ


계속하려면 아무 키나 누르십시오 . . .

strcpy는 전달인자 2개 중에서 앞에오는 곳에 뒤에오는 곳의 문자열을 그대로 복사해서 가져다줍니다. 물론 NULL문자까지 포함하여 복사됩니다.

strncpy도 strcpy와 비슷한데, 전달인자가 2개가 아닌 3개입니다. 마지막 하나는 복사할 배열의 길이를 지정해주는 전달인자입니다.

~~~ c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char str1[15] = "ABCDEFGHIJ";
    char str2[15];
    
    strncpy(str2, str1,sizeof(str2));
    
    printf("%s\n", str2);
    
    return 0;
}
~~~

실행결과

`ABCDEFGHIJ


계속하려면 아무 키나 누르십시오 . . .

하지만 다음과 같은 경우를 주의해야합니다.

~~~ c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char str1[15] = "ABCDEFGHIJKLM"; //13 + NULL문자 = 14개의 문자
    char str2[10];
    
    strncpy(str2, str1,sizeof(str2));
    
    printf("%s\n", str2);
    
    return 0;
}
~~~

&nbsp;

위의 코드를 실행하였을 때 str2에 str1의 문자 중 10개만 전달되는 건 분명합니다. 그럼 실행결과를 먼저 보시죠.

실행결과

`ABCDEFGHIJ儆儆儆儆儆ABCDEFGHIJKLM


계속하려면 아무 키나 누르십시오 . . .

이해할 수 없는 문자열이 출력됩니다. 그 이유는 str2에는 NULL문자가 포함되어있지 않기 때문입니다.

![image](/images/2018/09/zz.jpg){: width="50%" height="50%"}

그래서 항상 우리는 복사된 문자열이 전달이 될 포인터나 배열에 항상 null문자까지 고려하여 전달할 문자열의 길이를 설정해야 합니다. 위의 경우에는 다음과 같이 코드를 수정하면 되겠습니다.

~~~ c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char str1[15] = "ABCDEFGHIJKLM";
    char str2[10];
    
    strncpy(str2, str1,sizeof(str2)-1); //str2의 길이보다 1만큼 작은 문자열의 길이만 복사
    str2[sizeof(str2)-1] = NULL; //str2의 마지막 index 칸에 NULL문자 추가
    
    printf("%s\n", str2);
    
    return 0;
}
~~~

실행결과

`ABCDEFGHI


계속하려면 아무 키나 누르십시오 . . .

&nbsp;

&nbsp;

&nbsp;

# 3. 문자열 덧붙이기 : **strcat / strncat**

* * *

strcat 함수와 strncat 함수는 아래와 같이 사용됩니다.

~~~ c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char str1[20] = "ABCD";
    char str2[20] = "EFGH";
    char str3[20] = "ABCD";
    char str4[20] = "EFGH";
    
    strcat(str1, str2);//str1 뒤에 str2를 이어붙임
    puts(str1); //str1 출력
    strncat(str3, str4, 2); //str3뒤에 str4를 2개만 이어붙임
    puts(str3);
    
    return 0;
}
~~~

strncat함수는 strcat함수와 달리 이어붙일 문자열의 갯수를 지정하는 전달인자가 추가됩니다.

&nbsp;

실행결과

`ABCDEFGH


ABCDEF


계속하려면 아무 키나 누르십시오 . . .

이 때 NULL 문자의 위치는 자동으로 조정됩니다.

![image](/images/2018/09/ghg.jpg){: width="50%" height="50%"}

&nbsp;

&nbsp;

# 4. 문자열 비교 : **strcmp, strncmp**

* * *

이 두 함수는 두 개의 문자열의 내용이 같은지 다른지 비교해줍니다. 하나씩 비교하다가 다른 문자를 만났을 때 str1의 문자 ASCII코드 값이 크면 양수를, str2의 문자 ASCII코드값이 크면 음수를 반환합니다. 두 문자열이 같으면 0을 반환합니다.

strncmp가 strcmp와 다른점은 비교 할 문자열의 갯수를 지정할 수 있다는 것 뿐입니다.

~~~ c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char * str1 = "ABCE";
    char * str2 = "ABCDE";
    char * str3 = "ABCDE";
    
    printf("%d\n", strcmp(str1, str2)); // C까진 동일하나 str1의 E가 str2의 D보다 ASCII코드값 크므로 양수 반환
    printf("%d\n", strcmp(str2, str1)); // C까진 동일하나 str2의 D가 str1의 E보다 ASCII코드값 작으므로 음수 반환
    printf("%d\n", strcmp(str2, str3)); // 동일한 문자열이므로 0 반환
    printf("%d\n", strncmp(str1, str2, 3)); // 3번째 문자까지는 같으므로 0 반환
    
    return 0;
}
~~~

실행결과

`1


-1


0


0


계속하려면 아무 키나 누르십시오 . . .