---
title: 15. 이분 탐색
date: 2018-11-15T23:06:56+09:00
author: SWnomad
layout: post
categories: 알고리즘
image: /images/2018/11/algorithm-thumbnail.jpg
tags:
  - binary search
  - 알고리즘
  - 이분 탐색
  - 파이썬
---
# 1. 문제 정의

* * *

이분 탐색 알고리즘은 &#8216;순서대로 정렬 된&#8217; 리스트 내에서 찾는 값의 index를 돌려주는 문제이다. 찾는 값이 없을 시에는 -1을 return 한다.

<img class="aligncenter  wp-image-1362" src="/images/2018/11/no-name-11.jpg" alt="" width="563" height="141" srcset="/images/2018/11/no-name-11.jpg 783w, /images/2018/11/no-name-11-300x75.jpg 300w, /images/2018/11/no-name-11-768x192.jpg 768w" sizes="(max-width: 563px) 100vw, 563px" /> 

&nbsp;

&nbsp;

&nbsp;

# 2. 알고리즘

* * *

이분 탐색은 이름이 의미하는 대로 탐색할 리스트의 범위를 반 씩 줄여가며 찾는다.

가령 도서관에서 &#8216;물리학&#8217;이라는 책을 찾으려 한다. 도서관은 일반적으로 초성을 기준으로 책을 분류해 놓는다. 내 앞에는 ㅈ으로 시작하는 책꽂이가 있다. 이 때 우리가 할 행동은 왼쪽으로 가는 것이다. 왜냐하면 ㅁ은 ㅈ보다 이전의 자음이기 때문이다. 덕분에 찾아야 할 범위가 반으로 좁혀진 것이다.

위에서 예로 든 상황은 이분 탐색에 해당한다. 이러한 행위를 여러 번 하여 결국 값을 찾는 것이 이분 탐색이다.

<img class="aligncenter  wp-image-1363" src="/images/2018/11/3123.jpg" alt="" width="684" height="343" srcset="/images/2018/11/3123.jpg 961w, /images/2018/11/3123-300x150.jpg 300w, /images/2018/11/3123-768x385.jpg 768w" sizes="(max-width: 684px) 100vw, 684px" /> 

위 알고리즘을 파이썬 코드로 작성하면 다음과 같다.

~~~ python
def binary_search(a, x): # 리스트 a에서 값이 x인 요소 찾기
    start = 0 # 첫 번째 index
    end = len(a)-1 # 마지막 index

    while start<=end:
        mid = (start+end)//2 # start와 mid사이 값 탐색
        if a[mid]==x: # 발견하면
            return mid # index 반환
        elif a[mid]< x: # 찾는 값이 더 크면
            start = mid+1 # 오른쪽 탐색
        else: # 찾는 값이 더 작으면
            end = mid-1 # 왼쪽 탐색

    return -1 # 결국 못 찾고 반복문 빠져나오면 -1 반환

arr = [1,2,3,4,5,6,7,8,9,10,11]
arr2 = [1,4,7,10,15,21,23,34,39,40,45,48,50]
print(binary_search(arr,5)) # 4
print(binary_search(arr,100)) # -1
print(binary_search(arr2,23)) # 6
~~~

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# 3. 알고리즘 효율

* * *

1000개의 자료에서 이분 탐색 알고리즘을 활용해서 자료를 찾는다고 해보자. 최악의 경우(마지막 1개만 남을 때 까지 비교 연산을 할 경우)에 10번을 하면 된다. 1000을 2로 10번 나누면 대략 1이 된다.

<img src="https://latex.codecogs.com/gif.latex?log_{2}1000\approx&space;10" alt="log_{2}1000\approx 10" align="absmiddle" /> 

이분 탐색 알고리즘의 계산 복잡도는 O(log(n)) 이며 이는 심지어 O(n)의 경우보다 계산 복잡도가 낮다. 이분 탐색 알고리즘이 엄청 효율적인 탐색 알고리즘이라는 것을 알 수 있다.