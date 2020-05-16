---
title: (Python) 16 - 클래스 상속과 메소드 오버라이딩
date: 2018-09-21T19:48:43+09:00
author: nobbaggu
layout: post
categories: Python
image: /images/2018/09/파이썬-썸네일.jpg
tags:
  - __add__
  - __mul__
  - __sub__
  - __truediv__
  - class
  - inheritance
  - overloading
  - overriding
  - python
  - 메소드
  - 상속
  - 오버라이딩
  - 오버로딩
  - 클래스
  - 파이썬
---
클래스 상속에서 말하는 상속은 우리가 알고있는 유산 상속에서의 상속과 같은 뜻이다. '무엇인가를 물려준다'는 느낌으로 접근하면 된다.

&nbsp;

# 1. 클래스 복습

* * *

~~~ python
class Seoul:

    live = 'Seoul'

    # __init__ 메소드의 입력인수는 인스턴스 생성부터 초기화 필수
    def __init__(self, lastname, gender):
        self.lastname = lastname
        self.gender = gender


    def where(self):
        if self.gender == 'M':
            print("Mr.%s lives in %s"%(self.lastname, self.live))

        elif self.gender == 'W':
            print("Ms.%s lives in %s" %(self.lastname, self.live))

kim = Seoul('Kim', 'M')
kim.where()
lee = Seoul('Lee', 'W')
lee.where()
~~~

위 예제코드를 천천히 분석해보기 바란다. 서울 사는 사람의 성별과 이름을 알 수 있도록 서울에 산다는 내용을 출력해주는 서비스 코드이다.

\_\_init\_\_ 메소드의 입력인수는 인스턴스를 정의하는 동시에 넣어주어야한다. 그런 이유로 \_\_init\_\_ 메소드를 **생성자(constructor)** 라고도 부른다. \_\_init\_\_ 메소드에서 '성씨'와 '성별' 을 넣어주어야한다. 그리고 where 함수에서는 성별에 따라 출력의 앞부분에 Mr.를 넣을지 Ms. 를 넣을지 결정하게 되고, 앞서 정의되었던 '성씨'를 집어넣어 문장을 출력해준다.

Mr.Kim lives in Seoul


Ms.Lee lives in Seoul


 

&nbsp;

&nbsp;

&nbsp;

# 2. 클래스 상속

* * *

위의 코드와 같은 기능을 하는 코드를 작성하려는데, 이번에는 부산 사람들을 대상으로 하려한다. 위와 똑같은 클래스를 부산 버전으로 하나 더 만들어야 할 것 같지만, 상속이라는 기능을 사용하면 이를 편하게 할 수 있다.

~~~ python
class Busan(Seoul):
    live = 'Busan'
~~~

위처럼 선언을 하면 Seoul 클래스와 같은 구조의 Busan 클래스가 만들어진다. 그리고 Busan 클래스에서는 live 변수만 'Busan'으로 **재정의** 하였다. 나머지는 모두 같게 두었다.

~~~ python
kim = Seoul('Kim', 'M')
kim.where()
lee = Seoul('Lee', 'W')
lee.where()
park = Busan('Park', 'M')
park.where()
choi = Busan('Choi', 'W')
choi.where()
~~~

위의 출력결과를 확인하면 다음과 같다.

Mr.Kim lives in Seoul


Ms.Lee lives in Seoul


Mr.Park lives in Busan


Ms.Choi lives in Busan

&nbsp;

&nbsp;

&nbsp;

# 3. 메소드 오버라이딩

* * *

위의 코드에서 클래스를 상속할 때 변수만 재정의하고 메소드는 건드리지 않았다. 하지만 메소드 역시 다른 기능으로 동작하도록 할 수 있는데, 이렇게 메소드를 수정하는 일을 메소드 **오버라이딩(overriding)** 이라고 한다.

~~~ python
class Busan(Seoul):
    live = 'Busan'

    def where(self, year):
        if self.gender == 'M':
            print("Mr.%s has lived in %s for %d years"%(self.lastname, self.live, year))

        elif self.gender == 'W':
            print("Ms.%s has lived in %s for %d years"%(self.lastname, self.live, year))
~~~

위에서 Seoul 클래스를 상속받았던 Busan 클래스와는 다르게 where 함수를 바꾸었다. 일단 year 라는 입력 인수가 추가되었고, 그 인수를 사용해서 부산에 몇 년을 살았는지 출력하려한다.

~~~ python
park = Busan('Park', 'M')
park.where(5)
choi = Busan('Choi', 'W')
choi.where(10)
~~~

위 처럼 park 객체(인스턴스)의 where 함수를 호출하려면 입력인수 year의 값을 같이 주어야한다.

Mr.Park has lived in Busan for 5 years


Ms.Choi has lived in Busan for 10 years

&nbsp;

&nbsp;

&nbsp;

# 4. 연산자 오버로딩

* * *

연산자 오버로딩(overloading)은 +,-,*,/ 등의 연산이 객체끼리 가능하게 하는 기법이다. 가령, 클래스 내부의 메소드 이름을 \_\_add\_\_로 설정하면 객체 끼리 + 연산자를 사용 했을 때 이 메소드가 실행되게 하는 것이다. \_\_init\_\_은 인스턴스가 생성될 때 입력인수를 필수로 설정하도록 하는 파이썬 내장 메소드명이었다명 \_\_add\_\_는 인스턴스 끼리의 + 연산이 가능하도록 하는 파이썬 내장 메소드명이다.

~~~ python
class Seoul:

    live = 'Seoul'

    # __init__ 메소드의 입력인수는 인스턴스 생성부터 초기화 필수
    def __init__(self, lastname, gender):
        self.lastname = lastname
        self.gender = gender


    def where(self):
        if self.gender == 'M':
            print("Mr.%s lives in %s"%(self.lastname, self.live))

        elif self.gender == 'W':
            print("Ms.%s lives in %s" %(self.lastname, self.live))

    def __add__(self, other): # 연산자 오버로딩
        print("%s  vs.  %s" %(self.live, other.live))

class Busan(Seoul):
    live = 'Busan'

    def where(self, year):
        if self.gender == 'M':
            print("Mr.%s has lived in %s for %d years"%(self.lastname, self.live, year))

        elif self.gender == 'W':
            print("Ms.%s has lived in %s for %d years"%(self.lastname, self.live, year))
~~~

위와 같이 Seoul 클래스와 그를 상속받은 Busan 클래스가 있다. Seoul 클래스에 정의된 \_\_add\_\_ 메소드를 보자.

~~~ python
def __add__(self, other):
    print("%s  vs.  %s" %(self.live, other.live))
~~~

위에서 입력인수에 self와 other이 있다. self는 \_\_add\_\_를 직접 호출하는 인스턴스이고 other은 다른 인스턴스이다. 두 인스턴스는 'self.live   vs.   other.live' 라는 문장을 출력한다.

~~~ python
kim = Seoul('Kim', 'M')
lee = Busan('Lee', 'M')
kim+lee
lee+kim
~~~

Seoul 클래스의 인스턴스 kim과 Busan 클래스의 인스턴스 lee를 만들었다. 그리고 각각의 인스턴스 사이 +연산을 하였다. 여기서 + 연산은 \_\_add\_\_ 메소드에 해당한다. kim + lee 연산은 kim.\_\_add\_\_(lee)와 동등하다. self = kim 이고 other = lee 이다.

그렇다면 lee+kim 연산은 lee.\_\_add\_\_(kim) 과 동등하다. self = lee 이고 other = kim 이다.

Seoul  vs.  Busan


Busan  vs.  Seoul

위에서는 인스턴스 사이의 + 연산으로 \_\_add\_\_ 메소드를 호출했다. 이것이 연산자 오버로딩이다. 연산자 오버로딩에는 + 연산만 있는 것이 아니다. 대표적인 것 몇 개만 말하자면 다음과 같은 것들도 있다.

~~~ python
__add__ # +
__sub__ # -
__mul__ # *
__truediv__ # /
~~~