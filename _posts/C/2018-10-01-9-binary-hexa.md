---
title: (C언어) 9. 2진수와 16진수
date: 2018-10-01T06:00:21+09:00
author: SWnomad
layout: post
categories: C
image: /images/2018/09/C-썸네일.jpg
tags:
  - 16진수
  - 2진수
  - 8진수
  - binary
  - hex
  - hexadecimal
  - oct
  - octal
---
# 1. 2진수(binary)

* * *

## 1) 2진수(binary) 읽는 법

10진수는 다음과 같이 읽습니다.

<table class="txc-table" style="border: currentColor; width: 304px; font-family: '맑은 고딕',sans-serif; font-size: 13px; border-collapse: collapse; background-color: #f7f7f7; border-color: #000000;" border="1" width="304" cellspacing="0" cellpadding="0">
  <tr>
    <td>
       4
    </td>
    
    <td>
       9
    </td>
    
    <td>
       3
    </td>
    
    <td>
       1
    </td>
    
    <td>
       0
    </td>
    
    <td>
       5
    </td>
    
    <td>
       1
    </td>
    
    <td>
       2
    </td>
  </tr>
  
  <tr>
    <td>
      10^7
    </td>
    
    <td>
      10^6
    </td>
    
    <td>
      10^5
    </td>
    
    <td>
      10^4
    </td>
    
    <td>
      10^3
    </td>
    
    <td>
      10^2
    </td>
    
    <td>
      10^1
    </td>
    
    <td>
      10^0
    </td>
  </tr>
</table>

먼저 위의 십진수 49310512 = 4×10^7 + 9×10^6 + 3×10^5 + 1×10^4 + 0×10^3 + 5×10^2 + 1×10^1 + 2×10^0 입니다. 가장 낮은 자리수부터 10^0, 10^1, 10^2, 10^3, ... 의 가중치를 가지면서 각 자리수는 0~9의 10개의 수 중에서 하나를 가질 수 있어요. **0~9의 10개의 숫자를 사용하기 때문에 이것을 10진수라고 부릅니다.**

&nbsp;

<table class="txc-table" style="height: 38px; border: currentcolor; width: 304px; font-family: '맑은 고딕', sans-serif; font-size: 13px; border-collapse: collapse; border-color: #050505; background-color: #f0f0f0;" border="1" width="304" cellspacing="0" cellpadding="0">
  <tr style="height: 19px;">
    <td style="height: 19px; width: 36px;">
      1
    </td>
    
    <td style="height: 19px; width: 37px;">
    </td>
    
    <td style="height: 19px; width: 37px;">
    </td>
    
    <td style="height: 19px; width: 37px;">
      1
    </td>
    
    <td style="height: 19px; width: 37px;">
      1
    </td>
    
    <td style="height: 19px; width: 37px;">
    </td>
    
    <td style="height: 19px; width: 37px;">
      1
    </td>
    
    <td style="height: 19px; width: 37px;">
    </td>
  </tr>
  
  <tr style="height: 19px;">
    <td style="height: 19px; width: 36px;">
      2^7
    </td>
    
    <td style="height: 19px; width: 37px;">
      2^6
    </td>
    
    <td style="height: 19px; width: 37px;">
      2^5
    </td>
    
    <td style="height: 19px; width: 37px;">
      2^4
    </td>
    
    <td style="height: 19px; width: 37px;">
      2^3
    </td>
    
    <td style="height: 19px; width: 37px;">
      2^2
    </td>
    
    <td style="height: 19px; width: 37px;">
      2^1
    </td>
    
    <td style="height: 19px; width: 37px;">
      2^0
    </td>
  </tr>
</table>

2진수도 같은 원리입니다. 단, **2진수이기 때문에 각 자리수는 0, 1 두 개의 숫자만 사용합니다.** 그리고 가장 낮은 수부터 2^0, 2^1, 2^2, 2^3, ...의 가중치를 가져요. 따라서, 위의 2진수 10011010 = 1×2^7 + 0×2^6 + 0×2^5 + 1×2^4 + 1×2^3 + 0×2^2 + 1×2^1 + 0×2^0 입니다. 단, **컴퓨터에서는 하나의 예외가 있습니다.** <span style="text-decoration: underline;"><strong>가장 최상위 비트는 -값을 가진다는 겁니다.</strong></span> 위의 10011010 에서는 2^7 자릿수는 음수로 계산해야 해요. 따라서, 컴퓨터에서는 10011010 = -1×2^7 + 0×2^6 + 0×2^5 + 1×2^4 + 1×2^3 + 0×2^2 + 1×2^1 + 0×2^0 이고, 이는 10진수로 -102가 됩니다.

&nbsp;

## 2) 컴퓨터에서의 2진수 양수, 음수 표현

1byte = 8bit를 이용해 숫자 10을 표현한다고 하면, 00001010 입니다. 그렇다면, -10은 어떻게 표현할까요? 가장 최상위 비트가 0이면 양수, 1이면 음수이기 때문에 단순히 최상위비트를 1로 바꾸면 된다고 생각할 수 있습니다. 하지만 <span style="text-decoration: underline;"><strong>최상위비트가 단순히 부호'만' 표시한다고 생각하시면 안됩니다.</strong></span> 그 자릿수의 수를 가지면서 부호만 음이 되는 비트입니다. 예를들어, 8비트에서 가장 최상위비트의 가중치는 2^7 = 128이므로 가장 최상위비트는 -128을 표현하는거에요. 따라서 -10을 표현하려면 어떤 숫자들의 조합으로 표현할지 생각해보면 됩니다. -10 = -128+64+32+16+4+2 입니다. 따라서 -10을 2진수로 표현하면 11110110 입니다. 그런데 이렇게 -10을 만들기 위해서 각 자릿수의 조합을 만드는건 귀찮은 일입니다. 쉬운 방법이 있습니다.

### a)  보수를 취한다. 보수란, 각 비트의 반전이다. 0은 1로, 1은 0으로

10의 2진수 표현은 00001010 입니다. 따라서, 그 보수는 11110101이죠?

### b) 그 보수에 +1을 더한다.

그럼, 11110101+00000001 = 11110110 이 나옵니다. 위에서 조합을 이용해서 구한 결과랑 같습니다.

&nbsp;

정리를 해보죠.

<table style="width: 78.9263%; height: 60px; border-collapse: collapse;" border="1">
  <tr>
    <td style="width: 100%;">
      어떠한 양의 2진수를 같은 크기를 가지는 음의 2진수로 표현하는 방법은 다음과 같다.

 1) 2진수의 보수를 구한다.(0 → 1 , 1 → 0)

 2) 구해진 보수에 1을 더한다.
    </td>
  </tr>
</table>

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# 2. 16진수(hex)

* * *

2진수는 0~1의 2개 숫자, 10진수는 0~9의 10개숫자를 사용합니다. 16진수는 당연히 16개의 숫자 체계를 가집니다.

숫자는 그 수가 0~9까지 10개밖에 없으므로 알파벳을 사용하여 나머지 6개(10~15)를 표현합니다. 다음과 같이 말입니다.

0 1 2 3 4 5 6 7 8 9 A B C D E F

10진수에서는 9에서 10으로 넘어갈 때, 9가 0으로 다시 바뀌고 자릿수가 증가하여 10이 됩니다. 하지만, 16진수에서는 9 다음의 수(즉, 10)를 A로 표현하고 B, C, D, E, F(11, 12, 13, 14, 15)까지 하나의 자릿수로 표현 가능합니다. 그렇다면 16진수는 어떻게 읽을까요? 다음의 예시를 보세요.

<table class="txc-table" style="border-color: #000000; width: 154px; background-color: #ebe8e8; border-style: solid;" border="1" width="154" cellspacing="0" cellpadding="0">
  <tr>
    <td>
      A
    </td>
    
    <td>
      1
    </td>
    
    <td>
      7
    </td>
    
    <td>
      E
    </td>
  </tr>
  
  <tr>
    <td>
      16^3
    </td>
    
    <td>
      16^2
    </td>
    
    <td>
      16^1
    </td>
    
    <td>
      16^0
    </td>
  </tr>
</table>

16진수 A17E = 16^3×A + 16^2×1 + 16^1×7 + 16^0×E = 16^3×10 + 16^2×1 + 16^1×7 + 16^0×14 = 41,342 입니다.

16진수는 사실 마이크로프로세서 프로그래밍같은 임베디드 분야에서 자주 사용합니다. hex code로 된 ROM파일을 하드웨어에 심기도하고, 통신을 할 때 문자를 구분하는 이유도 있습니다. 그럼에도 16진수를 알려드리는 이유는 C언어에서도 가끔 사용하기 때문입니다. 16진수의  가장 큰 장점은 2진수를 훨씬 간결하게 표현 가능하기 때문이에요. 2진수 4자리, 즉 4bit가 표현할 수 있는 수의 개수는 2^4 = 16개(0000~1111 = 0~15)입니다. 이는 16진수 체계와 딱 들어맞습니다. 4bit를 1개의 16진수(hex)로 표현 가능합니다.

<table style="width: 49.9576%; height: 68px; border-collapse: collapse;" border="1">
  <tr>
    <td style="width: 100%;">
      <16진수(hex)>

 1) 0 1 2 3 4 5 6 7 8 9 A B C D E F 의 숫자체계를 가진다.

 2) 4bit를 하나의 hex로 표현 가능하다.
    </td>
  </tr>
</table>

그리고 8bit 숫자체계도 종종 사용하는데, 이는 여러분들이 직접 생각해보시기 바랍니다. 단순히 0~7의 7가지 숫자만 사용하는 숫자 체계에요. 그리고 8진수임을 표현하기 위해 앞에 0을 붙입니다. 모든 숫자체계는 사용하는 숫자의 갯수만 다를 뿐 원리는 같습니다.

그럼 마지막으로 2진수, 8진수, 10진수, 16진수를 코드로 출력하는 방법은 다음과 같습니다.

%d는 10진수 정수의 서식문자, %o는 8진수, %x는 16진수를 표현하는 서식문자입니다. 단, 8진수와 16진수라는 것을 표현하고 싶으면 %o 대신 %#o, %x 대신 %#x를 쓰시면 됩니다.

~~~ c
# include <stdio.h>

int main(void)
{
   char num = 13;
   
   printf("10 - dec : %d", num);
   printf("\n\n");
   printf("8 - oct : %o", num);
   printf("\n");
   printf("8 - odct : %#o", num);
   printf("\n\n");
   printf("16 - hex : %x", num);
   printf("\n");
   printf("16 - hex : %#x", num);
   printf("\n");
   
   return 0;
}
~~~

&nbsp;

출력결과

10 - dec : 13


8 - oct : 15


8 - oct : 015


16 - hex : d


16 - hex : 0xd


계속하려면 아무 키나 누르십시오 . . . 

&nbsp;