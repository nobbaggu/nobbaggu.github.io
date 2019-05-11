---
title: 11. 삽입 정렬
date: 2018-11-12T20:27:05+09:00
author: SWnomad
layout: post
categories: 알고리즘
image: /images/2018/11/algorithm-thumbnail.jpg
tags:
  - insert sort
  - 삽입
  - 삽입정렬
  - 알고리즘
  - 정렬
  - 파이썬
---
# 1. 문제 정의

* * *

삽입 정렬 알고리즘은 리스트 안의 숫자를 작은 순서로, 혹은 큰 순서로 정렬하는 알고리즘이다.

<img class="aligncenter size-full wp-image-1335" src="/images/2018/11/no-name-9.jpg" alt="" width="783" height="196" srcset="/images/2018/11/no-name-9.jpg 783w, /images/2018/11/no-name-9-300x75.jpg 300w, /images/2018/11/no-name-9-768x192.jpg 768w" sizes="(max-width: 783px) 100vw, 783px" /> 

&nbsp;

&nbsp;

# 2. 알고리즘

* * *

## 1) 하나씩 빼서 다른 리스트로 옮기기

a : 정렬해야 할 리스트

result : 정렬 된 리스트

1. a의 첫 번째 요소를 빼서 rseult에 추가

2. a의 첫 번째 요소를 빼서 result의 모든 요소와 비교하여 크기순에 맞게 배치

3. a의 첫 번째 요소를 빼서 result의 모든 요소와 비교하여 크기순에 맞게 배치

4. 위의 과정을 리스트 a가 빌 때 까지 반복

<img class="aligncenter  wp-image-1337" src="/images/2018/11/44.jpg" alt="" width="555" height="328" srcset="/images/2018/11/44.jpg 783w, /images/2018/11/44-300x177.jpg 300w, /images/2018/11/44-768x454.jpg 768w" sizes="(max-width: 555px) 100vw, 555px" /> 

이 과정을 코드로 표현하면 다음과 같다.

~~~ python
def insert_sort(a):
    result = []
    result.append(a.pop(0)) # 일단 a의 첫 번째 요소 추가
    while(a): # a가 다 나가고 비기 전까지
        for i in range(0,len(result)): # a[0]을 result의 각 요소와 크기 비교
            if a[0]<result[i]: # a[0]이 result[i]보다 작으면
                result.insert(i,a.pop(0)) # result index i자리에 a[0] 삽입
                break # for문 탈출
            else:
                if i==len(result)-1: # result의 마지막 요소까지 비교했는데 어떤 것 보다도 작지 않으면
                    result.append(a.pop(0)) # result의 마지막에 a[0] 삽입
                else: # 아니면
                    continue # 계속 비교
    return result

list1 = [11,9,23,20,4,1,14]
list2 = [5,4,3,2,1]

print(insert_sort(list1)) # [1,4,9,11,14,20,23]
print(insert_sort(list2)) # [1,2,3,4,5]
~~~

&nbsp;

&nbsp;

## 2) 일반적인 삽입정렬

1. a[1]을 왼쪽의 모든 요소들과 비교하여 적절한 위치를 찾아 삽입

2. a[2]를 왼쪽의 모든 요소들과 비교하여 적절한 위치를 찾아 삽입

3. 마지막 요소까지 위의 과정 반복

<img class="aligncenter size-full wp-image-1338" src="/images/2018/11/44-1.jpg" alt="" width="1227" height="356" srcset="/images/2018/11/44-1.jpg 1227w, /images/2018/11/44-1-300x87.jpg 300w, /images/2018/11/44-1-768x223.jpg 768w, /images/2018/11/44-1-1024x297.jpg 1024w" sizes="(max-width: 1227px) 100vw, 1227px" /> 

위의 과정을 파이썬 코드로 표현하면 다음과 같다.

~~~ python
def insert_sort(a):
    for i in range(1,len(a)):
        key = a[i]
        j=i-1
        while a[j]>key and j>=0:
            a[j+1] = a[j]
            j=j-1
        a[j+1] = key

    return a

list1 = [11,9,23,20,4,1,14]
list2 = [5,4,3,2,1]

print(insert_sort(list1)) # [1,4,9,11,14,20,23]
print(insert_sort(list2)) # [1,2,3,4,5]
~~~

&nbsp;

&nbsp;

# 3. 알고리즘 효율

* * *

삽입정렬 알고리즘은 계산 복잡도를 따지기에 애매한 구석이 있다.

가령 [5,4,3,2,1]과 [1,2,3,4,5]를 비교해보면 데이터의 크기는 같지만 연산의 횟수는 완전히 다르다.

단순히 완전히 크기의 역순으로 되어있는 경우, 즉 연산 횟수의 최대의 경우를 따지면 계산 복잡도는 O(n^2)이다.