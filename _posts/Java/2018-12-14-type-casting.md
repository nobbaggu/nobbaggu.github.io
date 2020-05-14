---
title: (Java) 5. 자료형(3) - 형 변환
date: 2018-12-14T16:56:59+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - casting
  - type
  - 강제
  - 자동
  - 형 변환
---
# 1. 자동 형 변환

* * *

~~~ java
package hello;

public class HelloJava{
    public static void main(String[] args){
        byte bNum = 10;
        int iNum = bNum; //bNum이 int형으로 자동 형 변한되어 iNum에 저장

        System.out.println(iNum); //10 출력
    }
}
~~~

**크기가 작은 자료형에서 큰 자료형으로는 형 변환이 자동으로 일어납니다.**

&nbsp;

~~~ java
package hello;

public class HelloJava{
    public static void main(String[] args){
        byte bNum = 10;
        float fNum = bNum; //bNum이 float형으로 자동 형 변한되어 iNum에 저장

        System.out.println(fNum); //10.0 출력
    }
}
~~~

**더욱 정밀한 자료형으로의 형 변환은 자동으로 일어납니다.**

&nbsp;

~~~ java
package hello;

public class HelloJava{
    public static void main(String[] args){
        float fNum = 10; // int -> float
        int iNum = 20;

        double dNum = fNum + iNum; //iNum이 float으로 변환되어 fNum과 연산되고 그 결과가 double로 변환되어 dNum에 저장

        System.out.println(dNum);
    }
}
~~~

&nbsp;

&nbsp;

# 2. 강제 형 변환

* * *

강제 형 변환은 명시적으로 형 변환을 시키는 경우입니다.

~~~ java
package hello;

public class HelloJava{
    public static void main(String[] args){
        int iNum = 10;
        byte bNum = (byte)iNum;
        System.out.println(bNum); //10 출력

        int iNum2 = 1000;
        byte bNum2 = (byte)iNum2;
        System.out.println(bNum2);// -24출력
    }
}
~~~

2번째에서 왜 -24가 출력되는지 알아봅시다.

int자료형에 1000을 저장하면 총 4byte=32bit에 0000 0000 0000 0000 0000 0011 1110 1000이 저장됩니다.

이것을 byte로 형 변환을 하면 1byte=8bit만 남기고 상위의 비트들이 다 날아가고 1110 1000만 남습니다. 이는 10진수로 -24입니다.

결론 : **크기가 큰 자료형이 작은 자료형으로 형 변환을 할 때에는 상위 바이트를 날려버린다.**

&nbsp;

~~~ java
package hello;

public class HelloJava{
    public static void main(String[] args){
        double dNum = 10.123456;
        int iNum = (int)dNum;

        System.out.println(iNum); //10 출력
    }
}
~~~

**더 정밀한 자료형을 덜 정밀한 자료형으로 바꿀 경우 소수점 부분이 날아간다.**

~~~ java
package hello;

public class HelloJava{
    public static void main(String[] args){
        double dNum = 1.2;
        float fNum = 0.9f;
        
        int iNum = (int)dNum + (int)fNum;
        int iNum2 = (int)(dNum + fNum);
        
        System.out.println(iNum); // 1 출력
        System.out.println(iNum2); // 2 출력
    }
}
~~~

iNum이 연산 될 때는 dNum과 fNum이 각각 int형인 1, 0으로 형 변환되어 더해져서 iNum에 1로 저장됩니다.

iNum2에는 dNum과 fNum 더해져 2.1이 됩니다. 이 때 fNum이 double로 형 변환되어 더해집니다. 아무튼 이 2.1이 int형으로 형 변환되어 2로 변하고 iNum2에 저장되므로 2가 출력되는 것입니다.