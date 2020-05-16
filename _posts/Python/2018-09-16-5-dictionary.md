---
title: (Python) 5 - 자료형(5) -딕셔너리(Dictionary)
date: 2018-09-16T18:20:36+09:00
author: nobbaggu
layout: post
categories: Python
image: /images/2018/09/파이썬-썸네일.jpg

tags:
  - python dictionary
  - 딕셔너리
  - 딕셔너리 자료형
  - 딕셔너리 함수
  - 파이썬 딕셔너리
---
&nbsp;

  * 딕셔너리는 대응관계를 나타내는 자료형이다.
  * 딕셔너리는 리스트, 튜플처럼 순차적인 요소가 아닌 key를 통해 value를 얻는 key : value 의 대응요소를 가지고 있다.

&nbsp;

# 1. 딕셔너리 표현방식
* * *


~~~ python
person = {'name':'John', 'age':20, 'phone':'010-0000-1234'}
~~~

- 딕셔너리 자료형 person의 key는 'name', 'age', 'phone' 이다.  
- 딕셔너리 자료형 person의 value는 'John', 20, '010-0000-1234' 이다.



~~~ python
a = {1:['a','b']} # value로 리스트 자료형도 가능하다.
b = {1:('a','b')} # value로 튜플 자료형도 가능하다.
~~~

&nbsp;


# 2. 딕셔너리 쌍 추가
* * *

~~~ python
a = {1:'a'}
print(a) # {1: 'a'}

a[2] = 'b' # 2: 'b' 쌍 추가
print(a) # {1: 'a', 2: 'b'}

a['age'] = 20 # 'age': 20 쌍 추가
print(a) # {1: 'a', 2: 'b', 'age': 20}

a[3] = [10,20] # 3: [10,20] 쌍 추가
print(a) # {1: 'a', 2: 'b', 'age': 20, 3:[10,20]}
~~~

&nbsp;

참고로 딕셔너리에서는 순차적으로 요소를 구하지 않고 key로 value를 얻으므로 순서는 신경쓸 필요 없다.

&nbsp;

# 3. 딕셔너리 쌍 삭제

* * *



~~~ python
print(a) # 현재 a = {1: 'a', 2: 'b', 'age': 20, 3: [10,20]}

del a[2] #key가 2인 key:value 쌍 삭제  << del a[key] 는 a의 key:value 쌍 삭제
~~~

a의 index 2 항목이 아니라는 것을 명심해야한다.



~~~ python
print(a) # {1: 'a', 'age': 20, 3: [10,20]}
~~~

&nbsp;

&nbsp;

&nbsp;

# 4. 딕셔너리 쌍 사용

* * *

  * 딕셔너리 자료형은 보통 공통의 분류 항목에 대한 다른 값을 저장할 때 쓴다.
  * 이는 리스트나 튜플 자료형으로 표현하기는 어렵다.

각 휴대폰 브랜드마다 제조사의 데이터를 담고 싶은 경우를 예로 들어보자.  


~~~ python
phone = {'galaxy':'Samsung', 'V20':'LG', 'G8':'LG', 'vega':'PanTech', 'iPhone':'Apple'}
print(phone['vega']) # 'PanTech'
print(phone['galaxy']) # 'Samsung'<code></code>
~~~

  * 리스트나 튜플에서는 인덱싱, 슬라이싱을 활용해 요소를 얻었다.
  * 딕셔너리에서는 key를 이용하는 방법 하나 뿐이다.
  * 딕셔너리 변수[key] 를 이용해서 key에 해당하는 value를 얻는다.

&nbsp;

&nbsp;

# 5. 주의사항

* * *

  * key 중복 금지
  * 딕셔너리에서 key는 고유한 값이어야 한다.
  * 같은 key값이 2개 이상 존재할 경우 1나를 제외한 나머지는 모두 무시된다.
  * 어떤 key값이 무시될 지는 예측할 수 없다. 따라서 중복되지 않도록 주의하자.



~~~ python
a = {'name':'John', 'age':20, 'name':'Kelly', 'name':'Jack'}
print(a['name']) # 'John' 혹은 'Kelly' 혹은 'Jack'
~~~

&nbsp;

  * key로 리스트 자료형 사용 금지
  * 리스트 자료형은 그 값이 변할 수 있기 때문에 key로 사용 불가능
  * key로 튜플 자료형은 사용 가능하다.
  * 튜플 자료형은 그 값이 변하지 않기 때문에 key로 사용 가능하다.
  * 딕셔너리의 key로 사용할 수 있냐 없냐는 값이 변할 수 있느냐 없느냐에 달려있다.
  * 딕셔너리의 value로는 변하는 값이든 변하지 않는 값이든 사용 가능하다.

&nbsp;

&nbsp;

&nbsp;

# 6. 딕셔너리 관련 함수

* * *



~~~ python
phone = {'galaxy':'Samsung', 'V20':'LG', 'G8':'LG', 'vega':'PanTech', 'iPhone':'Apple'}
~~~

&nbsp;

**1) key로 이루어진 list 만들기**  


~~~ python
phone.keys() # 딕셔너리 phone의 key들로만 이루어진 dict_keys 객체를 리턴
print(phone.keys()) # dict_keys(['galaxy', 'V20', 'G8', 'vega', 'iPhone'])
print(list(phone.keys())) # ['galaxy', 'V20', 'G8', 'vega', 'iPhone']
~~~

&nbsp;

  * list를 만들고 싶으면 list() 함수를 사용하면 된다.
  * Python 3.0 버전 이전에는 애초에 list를 리턴하였으나 3.0 이후 메모리 낭비의 문제로 dict_keys로 리턴



~~~ python
for k in phone.keys():
        print(k+" is made by "+phone[k])
~~~

&nbsp;

list() 함수를 써서 리스트로 변환하지 않더라도 dict_keys의 요소에 관한 반복문을 실행할 수 있다.

**2) value로 이루어진 list 만들기**  


~~~ python
phone.values() # 딕셔너리 phone의 value들로만 이루어진 dict_values 객체 리턴
print(phone.values()) # dict_values(['Samsung', 'LG', 'LG', 'PanTech', 'Apple'])
print(list(phone.values())) # ['Samsung', 'LG', 'LG', 'PanTech', 'Apple']
~~~

&nbsp;

**3) key,value 쌍 구하기**  


~~~ python
phone.items() # dict_items([('galaxy','Samsung'), ('V20','LG'),('G8','LG'),('vega','PanTech'),('iPhone','Apple')])
~~~

dict\_keys, dict\_values, dict_items 등의 객체는 정수, 문자열, 리스트 등과는 다르게 하나의 사용자 정의 자료형이다.



~~~ python
phone.items() # dict_items([('galaxy','Samsung'), ('V20','LG'),('G8','LG'),('vega','PanTech'),('iPhone','Apple')])
### dict_keys, dict_values, dict_items 등의 객체는 정수, 문자열, 리스트 등과는 다르게 하나의 사용자 정의 자료형이다.
type(phone.items()) # <class 'dict_items'>
a = list(phone.items()) # [('galaxy','Samsung'), ('V20','LG'),('G8','LG'),('vega','PanTech'),('iPhone','Apple')]
type(a) # <class 'list'>
type(a[0]) # <class 'tuple'>
~~~

&nbsp;

list(phone.items())는 리스트 안에 튜플이 있는 구조인 것이었다.

**4) key,value 쌍 지우기**  


~~~ python
phone.clear() # 딕셔너리 phone 요소들 모두 지우기
print(phone) # {}
~~~

&nbsp;

**5) key로 value를 얻는 또다른 방법 .get() 함수**  


~~~ python
phone = {'galaxy':'Samsung', 'V20':'LG', 'G8':'LG', 'vega':'PanTech', 'iPhone':'Apple'}
phone.get('vega') # 'PanTech'
~~~

phone['vega']와 phone.get('vega')는 같은 결과를 return 한다.



~~~ python
phone['G4'] # phone에 'G4'라는 key가 없어서 Key Error를 일으킨다.
phone.get('G4') # phone에 'G4'라는 key가 없어서 None을 반환한다.
print(phone.get('G4')) # None
~~~

&nbsp;

**6) 해당 key가 딕셔너리에 존재하는지 조사하기**  


~~~ python
'galaxy' in phone # True 리턴
'G4' in phone # False 리턴
~~~

