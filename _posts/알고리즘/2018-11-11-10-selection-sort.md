---
title: 10. 선택 정렬
date: 2018-11-11T19:37:35+09:00
author: SWnomad
layout: post
categories: 알고리즘
image: /images/2018/11/algorithm-thumbnail.jpg
tags:
  - selection sort
  - 선택
  - 선택정렬
  - 알고리즘
  - 정렬
  - 파이썬
---
# 1. 문제 정의

* * *

선택 정렬 알고리즘은 리스트 안의 숫자를 작은 순서로, 혹은 큰 순서로 정렬하는 알고리즘이다.

<img class="aligncenter  wp-image-1331" src="/images/2018/11/no-name-8.jpg" alt="" width="531" height="135" srcset="/images/2018/11/no-name-8.jpg 771w, /images/2018/11/no-name-8-300x76.jpg 300w, /images/2018/11/no-name-8-768x195.jpg 768w" sizes="(max-width: 531px) 100vw, 531px" /> 

&nbsp;

&nbsp;

&nbsp;

# 2. 알고리즘

* * *

선택정렬 알고리즘은 다음의 과정을 거친다.

&nbsp;

1. 첫 번째 요소부터 마지막 요소까지 훑으며 최소값이 저장된 index를 찾는다. 최소값을 첫 번째 위치로 이동한다.

2. 두 번째 요소부터 마지막 요소까지 훑으며 최소값이 저장된 index를 찾는다. 최소값을 두 번째 위치로 이동한다.

3. 위의 과정을 마지막 요소만 남을 때 까지 반복한다.

<img class="aligncenter  wp-image-1332" src="/images/2018/11/4-1.jpg" alt="" width="464" height="319" srcset="/images/2018/11/4-1.jpg 612w, /images/2018/11/4-1-300x206.jpg 300w" sizes="(max-width: 464px) 100vw, 464px" /> 

이 과정을 파이썬 코드로 다음과 같이 작성할 수 있다.

~~~ python
def selection_sort(a):
    start_idx=0
    while(start_idx< len(a)-1):
        min_idx = start_idx # 비교 시작 인덱스로 초기화
        for i in range(start_idx+1, len(a)): # 비교 시작 바로 다음 요소부터 마지막 요소까지
            if a[i]<a[min_idx]: # 더 작은 값이 나타나면
                min_idx = i # 인덱스를 min_idx에 저장
            else:
                pass
        a[start_idx], a[min_idx] = a[min_idx], a[start_idx] # 최소값을 비교시작 위치로 이동
        start_idx += 1 # 시작 위치 +1 증가

    return a

list1 = [11,9,23,20,4,1,14]
list2 = [5,4,3,2,1]

print(selection_sort(list1)) # [1,4,9,11,14,20,23]
print(selection_sort(list2)) # [1,2,3,4,5]
~~~

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# 3. 알고리즘 효율

* * *

선택정렬 알고리즘에서 가장 중요한 연산은 비교 연산이다.

리스트의 갯수가 n개일 때, 비교 연산은 n-1번 + n-2번 + . . . + 1번 = n(n-1)/2 = n^2/2-n/2 번을 거친다.

따라서 선택 정렬 알고리즘의 계산 복잡도는 O(n^2)이다.