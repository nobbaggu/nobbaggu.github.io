---
title: (Python) 13. 사용자 입출력
date: 2018-09-20T20:37:26+09:00
author: nobbaggu
layout: post
categories: Python
image: /images/2018/09/파이썬-썸네일.jpg
tags:
  - input 함수
  - input()
  - python
  - python input
  - python output
  - 사용자 입출력
  - 파이썬
  - 파이썬 input
  - 파이썬 output
  - 파이썬 입력
  - 파이썬 입출력
  - 파이썬 출력
---
지금까지는 프로그램 내부의 미리 정해진 변수만을 가지고 놀았다. 하지만 사용자 인터페이스를 가지고 있는 대부분의 프로그램은 사용자로부터 어떠한 입력을 받는다. 가장 대표적인 입력의 예가 키보드 입력과 마우스 클릭이다. 지금은 어떻게 사용자로부터 입력을 받는지 알아볼 것이다.

&nbsp;

&nbsp;

&nbsp;

# 1. input() 함수

* * *

예제 코드를 보면 쉽게 이해가 갈 것이다.

1) input() 함수

~~~ python
a = input() # 사용자 입력을 변수 a에 대입
print(a)
~~~

input() 함수는 사용자로부터 입력을 받게 해주는 함수이다. 위의 예제에서는 사용자 입력을 변수 a에 저장하고 있다. 실제로 a에 저장이 되었는지 a를 출력해본다.

중요한 사실 한 가지가 있다. **input() 함수는 입력되는 모든 것을 문자열로 취급한다.**

input 함수의 괄호를 비워놓으면 아무것도 뜨지 않아 사용자가 뭘 입력하라는건지, 아니면 입력을 할 타이밍인지 조차 알 수 없을수도 있다. 따라서 사용자에게 보여줄 문구를 input() 함수의 괄호 안에 넣으면 된다.

&nbsp;

~~~ python
a = input("Type anything! : ")
print(a)
~~~

"아무 거나 치세요!" 라는 문장이 나오면 입력을 하면 된다. 그럼 입력이 변수 a에 저장이 되고 print 함수가 그것을 출력해준다.

&nbsp;

&nbsp;

&nbsp;

2. print() 함수

* * *

지금까지 자연스레 모니터 출력을 하기 위해서 print 함수를 사용했다. 이번에는 조금 더 자세히 print함수에 대해 알아볼 것이다.

~~~ python
print("Life is "+"too short "+"we need "+"python") # 문자열 더하기
print("Life is " "too short " "we need " "python") # 문자열의 연속은 + 연산이다
print("Life is","too short","we need","python") # 콤마(,)는 문자열 사이 띄어쓰기
~~~

위의 세 문장은 완전 동일한 것이다. 우리는 앞에서 **문자열 사이에 + 기호를 사용해 연산하면 문자열을 이어붙이는 결과가 나온다**는 것을 알고 있었다. 그런데 문자열 사이 +기호를 생략해도 된다. **문자열의 연속된 나열은 자동으로 +연산**이 되기 때문이다. 또한 **문자열 사이사이 콤마를 넣어주면 띄어쓰기를 해준다.**

&nbsp;

print 함수로 출력을 할 때, print 함수가 끝나면 자동으로 enter가 쳐져 그 다음 출력은 다음 줄에 출력된다. 다음 줄로 넘어가지 않고 한 줄에 출력하고 싶을 때 쓰는 방법이 있다.

&nbsp;

~~~ python
for i in range(5):
    print(i, end=' ')
~~~

end = ' ' 를 사용해 끝 문자를 지정해줄 수 있다. 위처럼 space bar 한 칸을 넣어주면 enter 대신 print문 다음에 space bar를 한 칸 띄우고 끝이 난다.

&nbsp;