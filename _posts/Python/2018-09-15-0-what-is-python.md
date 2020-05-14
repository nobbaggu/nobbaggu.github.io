---
title: (Python) 0. 파이썬(Python) 개요
date: 2018-09-15T01:29:54+09:00
author: nobbaggu
layout: post
categories: Python
image: /images/2018/09/파이썬-썸네일.jpg
---
파이썬 (Python)은 1990년 귀도 반 로섬이라는 프로그래머가 개발한 인터프리터 언어다.

파이썬은 우리나라에서 자바, C++ 등의 프로그래밍 언어보다는 아직 덜 대중적이지만 현재 그 가치를 인정받고 특히 데이터과학, 머신러닝 분야에서 사용이 점점 더 늘어나는 추세이다.

실제로 구글에서 만든 SW의 50% 이상이 파이썬으로 만들어졌다는 이야기도 있다.

&nbsp;

# 1. 파이썬의 특징

* * *

<span style="font-size: 14pt;"><strong>1) 인간 친화적 언어</strong></span>

파이썬은 사람이 생각하는 사고의 순서를 그대로 따라가며 표현할 수 있는 언어이다.



~~~ python
if 'a' in ['a','b','c','d','e']:
        print("a는 알파벳이다")

~~~

&nbsp;

a라는 문자가 위 리스트의 항목 중 하나이면 a는 알파벳이다 라는 것을 표현하고 싶다. 소스코드를 보면 알 수 있듯이 매우 직관적이다.

&nbsp;

<span style="font-size: 14pt;"><strong>2) 다른 언어에 접착성이 좋다</strong></span>

파이썬으로 만든 프로그램은 C/C++ 에서 사용 가능하고 C/C++에서 만든 프로그램도 파이썬에서 사용 가능하다.

파이썬은 수치연산같이 빠른 실행속도를 필요로 하는 부분은 C로 만들어서 파이썬 라이브러리에 포함시킬 수 있다.

즉, 전체적인 프레임은 파이썬으로, 특정 기능흔 C나 C++로 만든 라이브러리를 가져다 쓸 수 있다.

&nbsp;

<span style="font-size: 14pt;"><strong>3) 개발 속도가 빠르다</strong></span>

파이썬에 관한 유명한 말이 있다. "Life is too short, You need Python."(인생은 너무 짧아서 파이썬이 필요하다.)

&nbsp;

파이썬은 다른 언어들이 할 수 있는 대부분의 일을 쉽게 해낸다. GUI(Graphic User Interface) 구현, 웹 프로그래밍, 수치 연산, DB 프로그래밍, 데이터 사이언스, 인공지능 등을 할 수 있다.

하지만 파이썬이 하기에 적합하지 않은 일 역시 존재한다. 하드웨어를 다루는 임베디드 같은 경우 Linux나 엄청난 연산이 필요하다. 하드웨어를 제어하거나 엄청난 연산을 요구하는 경우는 파이썬이 적합하지 않다.

또한 Android , IOS 등의 플랫폼에서 실행되는 앱은 개발은 할 수 있지만 아직까지 파이썬이 지원하는 좋은 API가 부족하다.

&nbsp;

&nbsp;

# 2. 파이썬 설치

* * *

<http://www.python.org/downloads>

위 url은 파이썬 공식 홈페이지이다. 위에서 최신 버전의 python을 다운받을 수 있다.

이후 launcher를 실행하면 설치를 할 수 있는 화면이 나오는데, **Add Python 3.6 to PATH **부분을 꼭 체크를 해주어야 파이썬이 어느 곳에서든 실행될 수 있고 이해할 수 없는 오류를 방지할 수 있다.

![image](https://nobbaggu.github.io/images/2018/09/1.jpg){: width="50%" height="50%"}

이후 Install Now를 누르면 설치가 끝난다.

이후 파이썬을 실행하면 대화형 인터프리터가 뜬다.

![image](https://nobbaggu.github.io/images/2018/09/1-1.jpg){: width="50%" height="50%"}

위의 대화형 인터프리터를 파이썬 셸(Python shell) 이라고 부른다.

&nbsp;

&nbsp;

# 3. 파이썬 문법 맛보기

* * *

일단 파이썬 셸은 ctrl+Z + enter 를 눌러 종료할 수 있다. 혹은 sys 모듈을 불러와 exit()함수를 실행할 수도 있다.

import sys


sys.exit()

&nbsp;

  * 사칙연산

\> 1+3  
\> 4  
\> 2*5  
\> 10  
\> 3/7  
\> 0.42857142857142855  
\> 4-3  
\> 1  
\> 5%2  
\> 1

&nbsp;

  * 변수에 문자 대입하고 출력

> a="Python"  
> print(a)  
> Python

&nbsp;

  * 복소수

> a = 4+3j  
> b = 2-6j  
> a+b  
> (6-3j)

&nbsp;

  * if문

> if a==10:  
> ... print("a is 10")  
> ...  
> a is 10

![image](https://nobbaggu.github.io/images/2018/09/1-2.jpg){: width="50%" height="50%"}

&nbsp;

&nbsp;

&nbsp;

&nbsp;

조건문 다음줄에서 spacebar 4칸 혹은 tab 1칸을 띄우고 명령어를 써야한다. 그리고 엔터를 치고 두 번째 ...에서 엔터를 치면 조건문이 끝이난다.

&nbsp;

  * for문

> >> for a in [1,2,3]:  
> ... print(a)  
> ...  
> 1  
> 2  
> 3

&nbsp;

  * while문

> >> a=0  
> >>> while a<3:  
> ... print(a)  
> ... a=a+1  
> ...  
>  
> 1  
> 2

&nbsp;

  * 함수

> >> def mul(a,b):  
> ... return a*b  
> ...  
> >>> mul(2,5)  
> 10  
> >>> mul(3,4)  
> 12

&nbsp;

&nbsp;

4. 파이썬 에디터

* * *

파이썬 에디터에는 editplus, pycharm, notepad++ 등이 있다.

이 중 PyCharm(파이참)은 코드 자동완성, 문법 디버깅 등 편리한 기능을 제공하는 하나의 IDE 이다.

&nbsp;

<http://www.jetbrains.com/pycharm/download/>

위 url은 PyCharm 공식 다운로드 사이트이다.

Windows, Linux, MacOS 버전이 있는데 자신에게 맞는 것을 받으면 된다. Professional은 유료버전이므로 무료Community 버전을 받으면 된다. 실행을 하여 Next를 몇 번 누르면 다음과 같은 화면이 뜬다.

&nbsp;

![image](https://nobbaggu.github.io/images/2018/09/1-3.jpg){: width="50%" height="50%"}

32-bit launcher만 클릭해제하고 Next를 누르고 Install을 하면 된다.

![image](https://nobbaggu.github.io/images/2018/09/no-name-6.png){: width="50%" height="50%"}

그리고 우리는 처음 설치한 것이므로 'Do not import settings'를 클릭하고 OK를 한다.

![image](https://nobbaggu.github.io/images/2018/09/no-name-7.png){: width="50%" height="50%"}

그럼 위와 같이 첫 화면에서는 UI를 선택할 수 있는데, 자기가 마음에 드는 테마를 고르면 된다.

![image](https://nobbaggu.github.io/images/2018/09/no-name-9.png){: width="50%" height="50%"}

이후 넘기다가 Create new project를 누르면 위와 같이 디렉토리파일에 프로젝트를 만들 수 있는데 적당한 프로젝트 이름을 설정하고 Create를 누른다. 그리고 project interpreter를 선택하는 화살표를 누른다. 이미 Python 3.7이 설치되어 있으므로 Existing interpreter를 선택하고 Python3.7을 선택한 후 Create를 누르면 프로젝트 생성이 된다.

![image](https://nobbaggu.github.io/images/2018/09/no-name-10.png){: width="50%" height="50%"}

그리고 프로젝트 이름에서 마우스 오른쪽 버튼을 누르고 New > Python File을 선택하면 드디어 .py 확장자의 python 파일이 만들어진다.

![image](https://nobbaggu.github.io/images/2018/09/no-name-11.png){: width="50%" height="50%"}

위와 같이 스크립트를 작성해보자.

참고로 큰 따움표 세 개 """ 사이에 쓰는 글은 주석이다. 컴파일 되지 않지만 다른 사람이 볼 때, 그리고 자신이 나중에 볼 때 알아보기 위한 기록을 할 때 사용한다. #은 한 라인이 주석이 되게 할 때 사용한다.

위와 같이 작성한 후 저장을 하고 cmd 창에서 파일이 있는 디렉토리로 가서 실행을 해보자.

![image](https://nobbaggu.github.io/images/2018/09/no-name-12.png){: width="50%" height="50%"}

cmd 창은 window + R 키로 실행창을 띄운 다음 cmd를 입력하고 enter를 치면 된다.

C:\Users\jchg1>cd C:\Users\jchg1\PycharmProjects\MyProject

C:\Users\jchg1\PycharmProjects\MyProject>python PyChang.py  
Welcome to Python!

cd(change directory) 명령어로 프로젝트 파일이 있는 폴더로 이동하여 python + 파일명.py 를 입력하면 해당 파일이 실행이 된다.

지금은 한 문장을 print 하는 코드지만, 보통 프로그램은 아주 많은 양의 코드로 작성이 되므로 파이썬 쉘 보다는 에디터에서 작성한다. 파이썬 쉘에서 만든 프로그램은 쉘을 종료하면 사라지지만 에디터로 만든 프로그램은 계속 파일로 저장이 되어있어 계속해서 재사용 할 수 있다. 이것이 에디터를 사용하는 이유이다.

&nbsp;