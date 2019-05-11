---
title: 19. 그래프 - 지인 찾기
date: 2018-11-17T17:05:48+09:00
author: SWnomad
layout: post
categories: 알고리즘
image: /images/2018/11/algorithm-thumbnail.jpg
tags:
  - 그래프
  - 알고리즘
  - 지인
  - 찾기
  - 추천
  - 친구
  - 파이썬
---
# 1. 문제 no-name

* * *

페이스북과 같은 SNS의 친구 추천 알고리즘은 나와 직접적으로나 간접적으로 연결된 사람들의 목록을 띄워준다.

이번에는 어떠한 사람의 직,간접적으로 연결된 모든 사람의 목록(자기자신 포함)을 출력해주는 문제에 대해 알아본다.

<img class="aligncenter  wp-image-1384" src="/images/2018/11/제목-없음-14.jpg" alt="" width="583" height="146" srcset="/images/2018/11/제목-없음-14.jpg 783w, /images/2018/11/제목-없음-14-300x75.jpg 300w, /images/2018/11/제목-없음-14-768x192.jpg 768w" sizes="(max-width: 583px) 100vw, 583px" /> 

&nbsp;

&nbsp;

&nbsp;

# 2. 그래프

* * *

그래프는 연결 관계를 시각적으로 나타내주는 수학 도구이다.

<img class="aligncenter  wp-image-1385" src="/images/2018/11/ddd.jpg" alt="" width="735" height="286" srcset="/images/2018/11/ddd.jpg 985w, /images/2018/11/ddd-300x117.jpg 300w, /images/2018/11/ddd-768x299.jpg 768w" sizes="(max-width: 735px) 100vw, 735px" /> 

위 그림은 사람들의 연결 관계를 나타내는 그래프이다.

&nbsp;

이번 알고리즘의 목적은 어떠한 한 사람의 이름을 입력하면 자기 자신을 시작으로 직, 간접적으로 연결 된 모든 사람들의 이름을 출력해주는 것이다.

&nbsp;

&nbsp;

# 3. 알고리즘

* * *

위 그림의 그래프의 사람 관계를 집합과 리스트 자료형을 사용해서 표현하면 다음과 같다.

~~~ python
friends={
    'Newton': ['Snell', 'Leibniz', 'Kepler'],
    'Leibniz': ['Newton', 'Fourier', 'Euler'],
    'Fourier': ['Laplace', 'Snell'],
    'Bohr': ['Lurtherford', 'Schrodinger', 'Einstein'],
    'Snell': ['Newton', 'Fourier'],
    'Kepler': ['Newton'],
    'Euler': ['Leibniz'],
    'Laplace': ['Fourier'],
    'Lurtherford': ['Bohr'],
    'Einstein': ['Schrodinger', 'Bohr'],
    'Schrodinger': ['Einstein', 'Bohr']
}
~~~

&nbsp;

이제 한 사람의 이름을 넣으면 모든 직,간접적인 인맥관계를 출력해주는 알고리즘의 과정을 알아보자.

1. 시작할 사람 이름 출력

2. 그 사람의 직접적인 지인을 큐, 집합에 추가

3. 큐에서 하나를 꺼내어 집합에 없으면 출력하고 모든 직접적 지인 큐에 추가. 집합에 있으면 건너뜀

4. 위의 과정을 큐가 빌 때 까지 반복

이 과정을 파이썬 코드로 다음과 같이 표현할 수 있다.

~~~ python
def find_friends(g, start):
     qu = [] # 출력할 사람들을 저장하기 위한 큐
     done = [] # 이미 출력한 사람들을 저장하기 위한 집합

     qu.append(start) # qu에 시작점 추가
     done.append(start) # done에 시작점 추가

     while qu: # qu가 빌 때 까지
         p = qu.pop(0) # qu에서 하나를 꺼내어
         print(p) # 출력
         for x in g[p]: # p의 지인들을 하나하나 탐색하여
             if x not in done: # 이미 처리된 사람들의 목록인 done에 없으면
                 qu.append(x) # qu에 추가
                 done.append(x) # done에도 추가
             else: # 이미 처리된 사람들의 목록인 done에 있으면
                 pass # 건너뜀
~~~

한 명을 골라 출력해서 결과가 그림과 같은지 확인해볼 수 있다.

~~~ python
find_friends(friends, 'Schrodinger')
print('-'*50)
find_friends(friends, 'Leibniz')
~~~

&nbsp;

**실행결과**

`Schrodinger


Einstein


Bohr


Lurtherford


--------------------------------------------------


Leibniz


Newton


Fourier


Euler


Snell


Kepler


Laplace`

&nbsp;

## Bonus) 친밀도 추가

몇 번의 연결을 거치는 지인인지 알아보려면 간단하게 하나만 추가하면 된다.

~~~ python
def find_friends(g, start):
     qu = [] # 출력할 사람들을 저장하기 위한 큐
     done = [] # 이미 출력한 사람들을 저장하기 위한 집합

     qu.append((start,<span style="color: #ff0000;"></span>)) # qu에 시작점 추가
     done.append(start) # done에 시작점 추가

     while qu: # qu가 빌 때 까지
         p, <span style="color: #ff0000;">d</span> = qu.pop(0) # qu에서 하나를 꺼내어
         print(p, <span style="color: #ff0000;">d</span>) # 출력
         for x in g[p]: # p의 지인들을 하나하나 탐색하여
             if x not in done: # 이미 처리된 사람들의 목록인 done에 없으면
                 qu.append((x, <span style="color: #ff0000;">d+1</span>)) # qu에 추가
                 done.append(x) # done에도 추가
             else: # 이미 처리된 사람들의 목록인 done에 있으면
                 pass # 건너뜀
~~~