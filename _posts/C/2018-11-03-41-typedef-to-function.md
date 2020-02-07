---
title: (C언어) 41. 구조체의 함수 전달
date: 2018-11-03T23:35:40+09:00
author: nobbaggu
layout: post
categories:
  - C
---

우리가 함수를 호출할 때 구조체변수 역시 함수의 인자로 전달할 수 있으며 이는 기본 자료형 변수를 전달하는것과 완전히 똑같습니다. 그래도 전달하는 과정을 한 번 보고 넘어가죠.

~~~ c
#include <stdio.h>
typedef struct
{
	char name[10];
	int age;
	char bltp;
}Person;

void ShowPersonInfo(Person per)
{
	printf(“%s  %d  %c\n”, per.name, per.age, per.bltp);
}

int main(void)
{
	Person per1 = {“John”, 20, ‘A’};
	Person per2 = {“Kelly”, 41, ‘B’};

	ShowPersonInfo(per1);
	ShowPersonInfo(per2);
	
	return 0;
}
~~~

실행결과

John  20  A

Kelly  41  B

계속하려면 아무 키나 누르십시오 . . .

우리는 Person타입의 구조체변수 per1과 per2를 ShowPersonInfo로 전달해주고 출력시켰습니다. int 자료형이든 double 자료형이든 Person 자료형이든 전달하는 방법은 완전히 일치합니다.

물론 구조체 변수를 대상으로 Call-by-reference 형태의 호출을 할 수도 있어요.

~~~ c
#include <stdio.h>

typedef struct
{
	char name[10];
	int age;
	char bltp;
}Person;

void Clear(void)
{
	while(getchar()!=’\n’);
}

void ChangePersonInfo(Person * ptr)
{
	printf(“rename : “); gets(ptr->name);
	printf(“re-age : “); scanf(“%d”, &(ptr->age));

	Clear();
	
	printf(“re-bltp : “); scanf(“%c”, &(ptr->bltp));
}

int main(void)
{

	Person per1 = {“John”, 20, ‘A’};
	printf(“%s  %d  %c\n”, per1.name, per1.age, per1.bltp);
	
	ChangePersonInfo(&per1);
	printf(“%s  %d  %c\n”, per1.name, per1.age, per1.bltp);

	return 0;

}
~~~

실행결과

John  20  A

rename : Jack

re-age : 43

re-bltp : B

Jack  43  B

계속하려면 아무 키나 누르십시오 . . .

구조체의 주소값을 ChangePersonInfo함수로 전달하여 직접 메모리공간에 접근해서 데이터를 바꾸어 놓았습니다. 실행결과를 보시면 제대로 데이터가 바뀐 것을 확인하실 수 있습니다.

int num1 = 10; int num2; num2 = num1;을 하면 num2에는 num1의 값이 복사가되어 저장됩니다. 구조체에서는 어떨지 한 번 같이 봅시다.

~~~ c
#include <stdio.h>

typedef struct
{
	char name[10];
	int age;
	char bltp;
}Person;

int main(void)
{
	Person per1 = {“John”, 20, ‘A’};
	Person per2 = per1;

	printf(“%s  %d  %c\n”, per1.name, per1.age, per1.bltp);
	printf(“%s  %d  %c\n”, per2.name, per2.age, per2.bltp);

	return 0;
}
~~~

실행결과

John  20  A

John  20  A

계속하려면 아무 키나 누르십시오 . . .

그대로 똑같이 복사가 됩니다. 구조체 변수의 대입은 멤버간의 복사가 이루어집니다.

지금까지 구조체를 공부했는데 구조체는 정말 많이 사용하는 내용입니다. 데이터를 관리해야 하는 모든 코드에는 구조체가 들어간다해도 과언이 아닙니다. 구조체를 정의하면 연관있는 데이터를 하나로 묶어서 관리할 수 있고 코드의 작성도 간결해지는 만큼 정말 유용한 도구입니다.

지금까지 구조체에 대해서 공부해보았는데 구조체를 정의하는 방법과 구조체변수의 멤버에 접근하는 방법만 안다면 나머지는 기본자료형 변수들과 똑같기 때문에 별로 어렵지 않았으리라 생각합니다. 다음에는 구조체와 비슷한 공용체라는 것에 대해 설명드리겠습니다.