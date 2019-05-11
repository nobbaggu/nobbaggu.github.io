---
title: 7. 재귀호출 (2) - 최대 공약수
date: 2018-11-10T19:01:36+09:00
author: SWnomad
layout: post
categories: 알고리즘
image: /images/2018/11/algorithm-thumbnail.jpg
tags:
  - 공약수
  - 알고리즘
  - 재귀호출
  - 최대
  - 파이썬
---
# 1. 문제 정의

* * *

<img class="aligncenter  wp-image-1316" src="/images/2018/11/no-name-5.jpg" alt="" width="559" height="142" srcset="/images/2018/11/no-name-5.jpg 771w, /images/2018/11/no-name-5-300x76.jpg 300w, /images/2018/11/no-name-5-768x195.jpg 768w" sizes="(max-width: 559px) 100vw, 559px" /> 

최대 공약수 는 공통적인 약수의 최대값이다.

가령 30, 12의 약수들의 집합은 {1, 2, 3, 6} 이다. 이 중 최대값은 6이므로 30, 12의 최대 공약수는 6이다.

&nbsp;

&nbsp;

# 2. 알고리즘

* * *

## 1) 재귀호출 사용하지 않은 알고리즘

입력을 두 개의 정수 a, b가 있고 a<b 라고 해보자.

&nbsp;

먼저, 1은 모든 수의 약수이므로 최대 공약수(great common divisor)를 gcd = 1로 초기화한다.

1. a/2와 b/2가 모두 정수인지 확인하여 맞다면 최대공약수 2로 업데이트

2. a/3과 b/3이 모두 정수인지 확인하여 맞다면 최대공약수 3으로 업데이트

3. a/4와 b/4가 모두 정수이지 확인하여 맞다면 최대공약수 4로 업데이트

4. 위의 과정을 a로 나누는 것까지 반복

&nbsp;

위의 알고리즘을 코드로 표현하면 다음과 같다.

~~~ python
def find_gcd(a,b):
    gcd=1 #최대 공약수 초기값 1
    k = a if a<b else b # a, b 중 작은 값 k

    for i in range(1, k+1): # i는 1~k
        if a/i==int(a/i) and b/i==int(b/i): # a/i, b/i가 각각 정수면 i는 a,b의 공약수이므로
            gcd=i # 최대 공약수 업데이트
        else:
            pass

    return gcd

print(find_gcd(12,18)) # 6
print(find_gcd(30,25)) # 5
print(find_gcd(100,33)) # 1
~~~

&nbsp;

참고로 a/i==int(a/i)는 i가 a의 약수인지 확인하는 과정이다.

예를 들어 a가 12이고 i가 5라면 12/5=2.4이고 int(12/5)=int(2.4)=2가 되어 조건이 False가 된다.

&nbsp;

## 2) 재귀호출을 사용한 알고리즘

먼저 재귀호출을 사용하기 전에 수학자 유클리드(Euclid)가 증명한 공약수의 성질 중 한 가지에 대해 상기시켜보자.

a, b(a>b) 일 때, a와 b의 최대 공약수는 b와 (a를 b로 나눈 나머지)의 최대 공약수와 같다.

예를 들어 12와 9의 최대 공약수는 9와 12%9==3의 최대공약수와 같고, 9와 3의 최대 공약수는 3과 9%3==0의 최대 공약수와 같다. 그런데 0은 어떠한 수로 나누어도 나머지가 0이므로 3과 0의 최대 공약수는 3이다. 따라서 12와 9의 최대 공약수는 3이다.

이 성질을 사용하여 최대공약수를 구하는 과정은 또 다른 두 수의 최대공약수를 구하는 과정을 포함한다. 따라서 최대 공약수를 구하는 알고리즘은 재귀호출로 풀 수 있다.

~~~ python
def find_gcd(a,b):
    if a==0 and b!=0: # 0과 b의 최대 공약수는 b
        return b
    elif a!=0 and b==0: # a와 0의 최대 공약수는 a
        return a
    elif a!=0 and b!=0: # 둘 다 0이 아니면
        if a>b:
            return find_gcd(b,a%b) # (작은 값)과 (큰 값을 작은 값으로 나눈 나머지)의 최대공약수
        elif a<b:
            return find_gcd(a,b%a) # (작은 값)과 (큰 값을 작은 값으로 나눈 나머지)의 최대공약수
        else: # a와 b가 같으면
            return a # a 리턴 혹은 b 리턴도 무관
    else: # 0과 0의 최대 공약수는 무한대로 발산.
        return 'both numbers are zero!'

print(find_gcd(10,25)) # 5
print(find_gcd(9,21)) # 3
print(find_gcd(28,49)) # 7
~~~

&nbsp;

&nbsp;

# 3. 알고리즘 효율

* * *

첫 번째 알고리즘에서 가장 중요한 연산은 나누기와 비교 연산자이다.

입력된 두 개의 수 중 작은 값이 n이라 해보자.

나누기 연산은 1~n까지 각각 4번씩이므로 4n 번이다.

비교 연산은 1~n까지 각각 2번이므로 2n 번이다.

총 연산의 수가 입력된 수 n에 비례하므로 첫 번째 알고리즘의 시간복잡도는 O(n)인 것을 알 수 있다.

&nbsp;

두 번째 알고리즘에서 가장 중요한 연산은 비교 연산이다. 하지만 비교 연산의 횟수는 입력 값과 연관되는 법칙이 딱히 존재하지 않는다. n이 아무리 커도 (1, 10000000000)의 최대 공약수는 1이므로 한 번에 값이 나와버린다.