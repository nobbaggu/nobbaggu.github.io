---
title: 2. 가장 빠른 정렬 알고리즘
date: 2019-04-20T12:00:00+09:00
author: SWnomad
layout: post
categories: SW직무-면접-기출질문
tags:
  - quick sort
  - sort
  - 정렬
  - 알고리즘
  - algorithm
---

정렬 알고리즘 중 가장 빠른 알고리즘은 **퀵 정렬(quick sort)** 알고리즘이다.

## 1. 알고리즘
------------------------

먼저 원소중 하나를 기준(pivot)으로 잡는다. 리스트의 왼쪽 끝, 오른 쪽 끝 두 곳을 시작으로 탐색해 나가며 크기 비교를 하여 위치를 바꾸는 식으로 리스트를 정렬한다. 기준은 보통 3개 정도를 임의로 추출해 그 중 중간값을 가지는 원소로 정하는 방식이다.

여기서는 편의상 리스트의 마지막 원소를 기준(pivot)으로 잡는다.

1. 첫 번째 원소의 index를 low, 마지막 두 번째 원소 index를 high라고 한다.

2. low가 pivot보다 큰 값을 가르킬 때 까지 low를 오른쪽으로 +1 씩 이동

3. high가 pivot보다 작은 값을 가르킬 때 까지 high를 왼쪽으로 -1씩 이동

4. low<high면 두 곳의 위치 교환

5. 위 과정을 low와 high가 만날 때 까지 반복

6. low, high가 만나면 그 곳의 숫자와 pivot의 값 교환

7. pivot의 값이 옮겨진 위치 즉, 현재의 low를 기준으로 리스트를 나누어 각각에 대해 퀵 정렬 재실행(재퀴호출)

<img class="wp-image-1348 aligncenter" src="/images/2018/11/4123.jpg" alt="" width="546" height="356" srcset="/images/2018/11/4123.jpg 752w, /images/2018/11/4123-300x195.jpg 300w" sizes="(max-width: 546px) 100vw, 546px" /> 

<img class="wp-image-1350 aligncenter" src="/images/2018/11/4123-2.jpg" alt="" width="517" height="305" srcset="/images/2018/11/4123-2.jpg 731w, /images/2018/11/4123-2-300x177.jpg 300w" sizes="(max-width: 517px) 100vw, 517px" /> 

<img class="wp-image-1351 aligncenter" src="/images/2018/11/4123-3.jpg" alt="" width="344" height="180" srcset="/images/2018/11/4123-3.jpg 500w, /images/2018/11/4123-3-300x157.jpg 300w" sizes="(max-width: 344px) 100vw, 344px" /> 

&nbsp;

위 과정을 파이썬 코드로 표현하면 다음과 같다.

~~~ python
def quick_sort(a, start, end):
    if end-start<1: # 원소가 한 개면
        return # 종료

    low = start # 왼쪽 탐색 막대
    high = end # 오른쪽 탐색 막대
    pivot = a[end] # 마지막 원소를 기준으로

    while low<high: # 왼쪽 막대가 오른쪽 막대보다 작은 동안 실행
        while low<high and a[low]<=pivot:
            low+=1
        while low<high and a[high]>=pivot:
            high-=1
        if low<high: # 왼쪽 막대가 오른쪽 막대보다 작으면
            a[low], a[high] = a[high], a[low] # 위치 교환
        else: # 왼쪽 막대가 오른쪽으로 넘어가면
            a[end], a[low] = a[low], a[end] # 기준과 a[low] 교환하고
            break # 반복문 탈출

    quick_sort(a, start, low-1) # 새로운 기준을 중심으로 왼쪽 리스트 부분 정렬
    quick_sort(a, low+1, end) # 새로운 기준을 중심으로 오른쪽 리스트 부분 정렬

arr = [5,8,2,9,6,3,1,10,7,4]
arr2 = [4,4,2,2,1,1,3,3]
quick_sort(arr, 0, len(arr)-1)
quick_sort(arr2, 0, len(arr2)-1)
print(arr) # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
print(arr2) # [1, 1, 2, 2, 3, 3, 4, 4]
~~~

&nbsp;
## 2. O(n) 복잡도
-------------------

퀵 정렬의 계산 복잡도는 최악의 경우 O(n^2), 평균적으로 O(nlog(n))을 따른다. 즉, 기준을 얼마나 잘 잡느냐에 따라 계산 복잡도가 달라진다.

따라서 퀵 정렬을 효율적으로 사용하려면 기준을 잡는 방법에 대해 따로 공부할 필요가 있다. 퀵 정렬의 기준을 잡는 방법에 대한 연구된 좋은 방법들은 많으므로 궁금하면 자료를 찾아보자.

퀵 정렬은 배열이 클 경우에 사용하기 좋다. 그러나 진행하면서 블록이 점점 작아지면 잦은 재귀호출로 인해 불필요한 로드가 생길 수 있다. 따라서 기준을 정한 이후 어느정도 이후부터는 다른 정렬 알고리즘으로 대체하는 것이 좋다.