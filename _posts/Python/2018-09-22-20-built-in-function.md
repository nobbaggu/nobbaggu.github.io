---
title: (Python) 20 - 파이썬의 유용한 내장함수들
date: 2018-09-22T15:13:42+09:00
author: nobbaggu
layout: post
categories: Python
image: /images/2018/09/파이썬-썸네일.jpg
tags:
  - python
  - 내장함수
  - 라이브러리
  - 파이썬
---
라이브러리를 잘 활용하는 것은 프로그래밍에 있어 매우 중요하다. 직접 코딩하여 만들고 사용하기보다는 이미 수없이 최적화 검증이 된 라이브러리를 활용하면 시간, 메모리관리, 프로그램 작동 면에서 효율적이다. 그 중에서 파이썬을 설치하면 자동으로 따라오는 내장함수들은 이미 라이브러리 패쓰 리스트에 포함되어있어 import 없이 사용할 수 있다. 오늘은 자주 활용되고 유용한 몇 가지 내장함수에 대해 설명한다.

<span style="font-size: 14pt;"><strong>1) abs()</strong></span>

절대값을 리턴해준다.

~~~ python
a = abs(-10)
b = abs(7)
print(a) # 10
print(b) # 7
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>2) all()</strong></span>

반복 가능한 자료형(for문에서 값을 출력할 수 있는 자료형)을 입력 인수로 받아 모두 참이면 True, 거짓이 하나라도 있으면 False를 리턴

~~~ python
a = [1,'a',[3,2]]
b = [4, ''] # 공백 문자열
c = [3,4,()] # 공백 튜플
d = [0,1,2,3] # 0은 숫자형의 False

print(all(a)) # True
print(all(b)) # False
print(all(c)) # False
print(all(d)) # False
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>3) any()</strong></span>

하나라도 '참'이면 True를, 모두 '거짓'이면 False를 리턴한다.

~~~ python
a = [1,2,'']
b = [0,1,2]
c = [(),[1,2],'hello']
d = [0,'',()]

print(any(a)) # True
print(any(b)) # True
print(any(c)) # True
print(any(d)) # False
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>4) chr()</strong></span>

입력 인수로 ASCII CODE값을 받아 해당 하는 문자력 출력해준다.

~~~ python
print(chr(11)) # ''
print(chr(20)) # ''
print(chr(50)) # '2'
print(chr(70)) # 'F'
~~~

ASCII(American Standard Code for Information Interchange)통신에서 문자열 전송을 위해 국제적으로 약속된 정보교환 규격이다. 통신상에서 디지털 신호를 보내면 해당하는 문자열을 출력해준다. 참고로 우리 모니터에 숫자 2가 뜨게하려면 디지털 값 50(110010)을 전송해야 한다. 전송할 때는 2진수 형태로 전송을 한다.

&nbsp;

<span style="font-size: 14pt;"><strong>5) dir()</strong></span>

객체, 모듈이 자체지원하는 함수, 메소드를 보여준다.

~~~ python
print(dir(tuple)) # ['__add__', '__class__', '__contains__', ...]
print(dir(int)) # ['__abs__', '__add__', '__and__', '__bool__',...]
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>6) divmod()</strong></span>

2개의 숫자를 입력인수로 받아 몫과 나머지를 튜플 형태로 리턴

~~~ python
a = divmod(8,3)
print(a) # (2,2)
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>7) enumerate()</strong></span>

순서가 있는 자료형을 입력인수로 받아 index, 해당index 값 을 객체로 리턴

~~~ python
for i, name in enumerate(['a','b','c','d']):
    print(i,name)
~~~

0 a


1 b


2 c


3 d

주의할 건 enumerate가 리턴하는 것은 하나의 객체이기 때문에 객체 자체를 출력하면 객체가 저장된 메모리공간의 주소값이 출력된다. 위 처럼 for문 내에서 enumerate 함수를 사용하면 리스트, 튜플 등 자료형의 index와 내용을 헷갈리지 않고 쉽게 파악 가능하다.

&nbsp;

<span style="font-size: 14pt;"><strong>8) eval()</strong></span>

문자열을 실행 가능한 코드 형태로 변환해준다.

~~~ python
print('1+1') # '1+1'
print('len([1,2,3])') # len([1,2,3])
print(eval('1+1')) # '2'
print(eval('len([1,2,3])')) # '3'
~~~

외부에서 문자열 형태로 전송된 실행코드를 eval함수를 통해 바로 실행이 가능하다.

&nbsp;

<span style="font-size: 14pt;"><strong>9) filter()</strong></span>

~~~ python
def returnNum(x):
    if(type(x)==type(1)):
        return x
    else:
        return

numList = list(filter(returnNum,[1,'a',2,3,'hi',5,(10,11)]))
print(numList) # [1,2,3,5]
~~~

returnNum 이라는 함수는 변수를 판별해 숫자 자료형이면 리턴시켜주고 아니면 함수를 벗어난다.

filter 는 첫 번째 인수로 함수의 이름을 넣어주면 두 번째 인수로 들어온 반복 가능한 자료형들의 요소를 하나씩 해당함수에 집어넣어 결과를 돌려준다. filter함수를 쓰지 않으면 애초에 returnNum 함수의 입력인수가 리스트나 튜플 형태가 되게 정의해야 하지만 단순히 변수 하나만 입력인수로 받을 수 있게 되었다. 하지만 객체 형태로 돌려주기 때문에 list() 를 써서 list 형태로 형 변환을 시키고 출력했다.

<span style="font-size: 14pt;"><strong>10) hex()</strong></span>

~~~ python
print(hex(100))
print(hex(0b1101110))
~~~

정수를 입력받아 16진수 형태로 되돌려준다. 참고로 2진수를 표현하기 위해 앞에 0b를 붙이고 16진수를 표현하기위해 앞에 0x를 붙인다.

&nbsp;

<span style="font-size: 14pt;"><strong>11) id()</strong></span>

객체를 입력받아 고유주소값 리턴

~~~ python
a = 3

def sum(a,b):
    return a+b

class Person:
    name = "John"
    age = 20

print(id(3)) # 1754020128
print(id(sum)) # 2951256
print(id(Person)) # 5706720
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>12) input()</strong></span>

사용자로부터 입력을 받아 문자열 형태로 출력

~~~ python
a = input("Type anything!") # hi + enter
print(a) # 'hi'
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>13) int()</strong></span>

문자열 형태로 있는 숫자나 실수를 정수로 변환하여 리턴

~~~ python
print(int(3.141592)) # 3
print(int('10')) # 10
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>14) isinstance()</strong></span>

첫 번째 입력인수로 인스턴스, 두 번째 입력인수로 클래스를 입력받아 인스턴스가 해당 클래스의 인스턴스인지 판단하여 '참'이면 True '거짓'이면 False를 리턴

~~~ python
class Person:
    pass

class Dog:
    pass

a = Person()
b = Dog()
print(isinstance(a,Person)) # True
print(isinstance(a,Dog)) # False
print(isinstance(b,Person)) # False
print(isinstance(b,Dog)) # True
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>15) len()</strong></span>

문자열, 튜플, 리스트 등을 입력받아 길이(요소의 갯수) 반환

~~~ python
a = "hello"
b = [1,2,3,4,5]
print(len(a)) # 5
print(len(b)) # 5
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>16) list()</strong></span>

반복 가능한 형태의 자료형을 리스트 형태로 변환하여 반환

~~~ python
print(list((1,2,3,4,5))) # [1,2,3,4,5]
print(list("hello")) # ['h','e','l','l','o']
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>17) map()</strong></span>

첫 번째 입력인수로 받은 함수에 두 번째 입력인수로 받은 반복 가능한 자료형의 요소를 하나하나 대입해 결과를 묶어서 리턴시켜준다.

~~~ python
def double(x):
    return 2*x

print(list(map(double,[1,2,3,4,5]))) # [2,4,6,8,10]
~~~

map 함수의 리턴 결과는 객체 형태라 그냥 출력하면 메모리공간의 주소값이 나오기때문에 list() 함수로 형 변환을 해주어야한다.

&nbsp;

<span style="font-size: 14pt;"><strong>18) max()</strong></span>

반복가능한 자료형 요소들 중 최대값을 리턴

~~~ python
print(max([1,7,41,8,22])) # 41
print(max("python")) # 'y'
~~~

두 번째 결과가 'y' 인 이유는 y의 ASCII CODE 가 가장 크기 때문이다.

<span style="font-size: 14pt;"><strong>19) min()</strong></span>

반복가능한 자료형 요소들 중 최소값을 리턴

~~~ python
print(min([1,7,41,8,22])) # 1
print(min("python")) # 'h'
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>20) open()</strong></span>

첫 번째 입력인수로 파일명, 두 번째 입력인수로 읽기 모드를 받아 파일을 열어 객체를 변수에 저장

~~~ python
f = open("doitpython.txt",'r')
f = open("doitpython.txt") # 읽기 모드 생략되면 default로 'r' 읽기모드
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>21) ord()</strong></span>

입력받은 문자열의 아스키 코드 리턴

~~~ python
print(ord('a')) # 97
print(ord('@')) # 64
print(ord(';')) # 59
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>22) pow()</strong></span>

'첫 번째 입력인수' 의 '두 번째 입력인수' 승수한 값을 리턴

~~~ python
a = pow(2,3) # 2^3
b = pow(10,2) # 10^2
print(a) # 8
print(b) # 100
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>23) range()</strong></span>

숫자를 입력받아 범위를 반복가능한 객체형태로 리턴

  * 입력인수 1개

~~~ python
a = range(5)
print(list(a)) # [0,1,2,3,4]
~~~

&nbsp;

  * 입력인수 2개

~~~ python
b = range(3,7)
print(list(b)) # [3,4,5,6]
~~~

&nbsp;

  * 입력인수 3개

~~~ python
c = range(1,10,2) # 1이상, 10미만의 수를 2 간격으로
print(list(c)) # [1,3,5,7,9]
~~~

&nbsp;

&nbsp;

<span style="font-size: 14pt;"><strong>24) sorted()</strong></span>

반복 가능한 자료형을 크기순으로 재정렬하여 **'리스트'** 형태로 리턴

~~~ python
a = [4,1,3,2]
b = "python"
c = (8,1,3,10)

print(sorted(a)) # [1,2,3,4]
print(sorted(b)) # ['h','n','o','p','t','y']
print(sorted(c)) # [1,3,8,10]
~~~

list 자료형의 내장함수 sort() 도 있었다. 하지만 sort() 함수는 리스트를 재정렬 하고 끝이다. 그 결과를 리턴하지 않는다. 필요에 따라 sort()와 sorted()를 적절히 구별하여 사용하면 된다.

&nbsp;

<span style="font-size: 14pt;"><strong>25) str()</strong></span>

입력받은 객체를 문자열 형태로 변환하여 리턴

~~~ python
print(str(100)) # '100'
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>26) tuple()</strong></span>

반복가능한 자료형을 입력받아 튜플 자료형으로 변환하여 리턴

~~~ python
print(tuple([1,2,3])) # (1,2,3)
print(tuple({1,2,3})) # (1,2,3)
print(tuple("hello")) # ('h','e','l','l','o')
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>27) type()</strong></span>

입력인수로 받은 객체의 자료형을 리턴

~~~ python
print(type(10)) # <class 'int'>
print(type('a')) # <class 'str'>
print(type([1,2,3])) # <class 'list'>
~~~

&nbsp;

<span style="font-size: 14pt;"><strong>28) zip()</strong></span>

같은 갯수를 가진 여러개의 반복가능 자료형의 요소를 같은 index끼리 묶어 리턴

~~~ python
print(list(zip([1,2,3],[11,12,13],[21,22,23])))
print(list(zip(['a','b','c'],['A','B','C'])))
~~~

[(1, 11, 21), (2, 12, 22), (3, 13, 23)]


[('a', 'A'), ('b', 'B'), ('c', 'C')]


