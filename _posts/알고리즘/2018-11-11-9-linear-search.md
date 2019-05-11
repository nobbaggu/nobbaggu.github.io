---
title: 9. 순차 탐색
date: 2018-11-11T17:29:45+09:00
author: SWnomad
layout: post
categories: 알고리즘
image: /images/2018/11/algorithm-thumbnail.jpg
tags:
  - 순차 탐색
  - 알고리즘
  - 파이썬
---
# 1. 문제 정의

* * *

순차 탐색 은 리스트 자료에서 특정 값을 찾을 때, 처음 요소부터 마지막 요소까지 하나 하나 순서대로 탐색하며 찾는 알고리즘이다.

찾는 값이 있으면 리스트 내에서의 index를 반환해준다.

<img class="aligncenter  wp-image-1326" src="/images/2018/11/no-name-7.jpg" alt="" width="590" height="150" srcset="/images/2018/11/no-name-7.jpg 771w, /images/2018/11/no-name-7-300x76.jpg 300w, /images/2018/11/no-name-7-768x195.jpg 768w" sizes="(max-width: 590px) 100vw, 590px" /> 

&nbsp;

&nbsp;

# 2. 알고리즘

* * *

순차 탐색 알고리즘은 다음과 같이 매우 단순한 과정으로 이루어져있다.

1. 리스트의 첫 번째 요소와 비교하여 같으면 index 반환, 없으면 다음

2. 리스트의 두 번째 요소와 비교하여 같으면 index 반환, 없으면 다음

3. 위의 과정을 마지막 요소까지 반복하고 끝까지 찾지 못하면 -1 반환

이 과정을 파이썬 코드로 만들면 다음과 같다.

~~~ python
def search(a, x):
    result = []  # index를 담을 리스트
    for i in range(0, len(a)): # i는 a의 첫 번째부터 마지막 index
        if a[i]==x:
            result.append(i)
        else:
            pass
        
    return result

list1 = [1,2,3,4,5,6,7]
print(search(list1,3)) # [2]
print(search(list1,10)) # []
~~~

&nbsp;

&nbsp;

&nbsp;

# 3. 알고리즘 효율

* * *

위 알고리즘은 리스트의 처음부터 마지막 요소까지 비교연산을 한다. 따라서 리스트의 요소의 갯수 n이 커질수록 그에 비례하여 비교연산의 횟수도 증가하므로 시간 복잡도는 O(n)이다.

그런데, 만약 위 알고리즘이 우리가 찾는 값을 발견하면 index를 반환하고 바로 함수를 빠져나오도록 고쳤다 해보자.

이 때에는 찾는 값이 요소의 첫 번째에 있으면 비교연산 한 번, 마지막에 있으면 비교연산 여러 번으로 경우에 따라 달라진다.

보통 이럴 경우에는 평균적인경우 혹은 최대 비교 횟수 등으로 계산복잡도를 따진다.

가령 리스트의 요소의 수가 100개라면, 최악의 경우 100번 비교를 해야한다. 최악의 경우의 비교연산의 수는 리스트의 요소의 수에 따라 달라지므로 이 경우에도 시간 복잡도는 O(n)이다.