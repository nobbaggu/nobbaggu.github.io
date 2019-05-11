---
title: 5. 중복 찾기
date: 2018-11-10T15:42:19+09:00
author: SWnomad
layout: post
categories: 알고리즘
image: /images/2018/11/algorithm-thumbnail.jpg
tags:
  - algorithm
  - 알고리즘
  - 중복
  - 찾기
  - 파이썬
---
# 1. 문제 정의

* * *

~~~ python
list1 = ['Newton', 'Bohr', 'Einstein', 'Dirac', 'Gauss', 'Laplace', 'Bohr']
~~~

위 리스트에서 중복 되는 이름을 따로 저장하고자 한다.

&nbsp;

<img class="aligncenter  wp-image-1305" src="/images/2018/11/no-name-3.jpg" alt="" width="570" height="145" srcset="/images/2018/11/no-name-3.jpg 771w, /images/2018/11/no-name-3-300x76.jpg 300w, /images/2018/11/no-name-3-768x195.jpg 768w" sizes="(max-width: 570px) 100vw, 570px" /> 

&nbsp;

&nbsp;

# 2. 알고리즘

* * *

1. 첫 번째 요소를 뒤의 요소들과 비교하여 중복이 있으면 index 저장

2. 두 번째 요소를 뒤의 요소들과 비교하여 중복이 있으면 index 저장

3. 세 번째 요소를 뒤의 요소들과 비교하여 중복이 있으면 index 저장

4. 위의 과정을 끝까지 반복

<img class="aligncenter  wp-image-1306" src="/images/2018/11/4.jpg" alt="" width="485" height="247" srcset="/images/2018/11/4.jpg 757w, /images/2018/11/4-300x153.jpg 300w" sizes="(max-width: 485px) 100vw, 485px" /> 

주의할 것은 자신 뒤의 것들과만 비교하면 된다는 것이다. 앞의 것들과는 이미 비교를 했기 때문이다.

이 과정을 코드로 표현하면 다음과 같다.

~~~ python
def find_dup(a):
    dup = set() # 중복된 자료 저장할 공백 집합
    for i in range(0, len(a)): # a의 요소를 하나씩 탐색
        for j in range(i+1, len(a)): # 비교할 요소 하나씩 탐색
                if a[i]==a[j]: # 중복된거 발견하면
                   dup.add(a[j]) # index를 dup에 저장
                else:
                    pass
    return dup

list1 = ['Newton', 'Bohr', 'Einstein', 'Dirac', 'Gauss', 'Laplace', 'Bohr']
list2 = ['physics', 'math', 'chemistry', 'biology', 'linear algebra', 'statistics', 'physics', 'biology']

print(find_dup(list1)) # {'Bohr'}
print(find_dup(list2)) # {'physics', 'biology'}


~~~

&nbsp;

# 3. 알고리즘 효율

* * *

중복 찾기 알고리즘의 가장 중요한 연산은 비교 연산이다. 비교 연산의 횟수가 시간 복잡도와 관련된다.

자료의 갯수가 n개인 경우 비교횟수는 다음과 같이 계산할 수 있다.

1번째 요소 : 요소2 ~ 요소n 까지 n-1번 비교

2번째 요소 : 요소3 ~ 요소n 까지 n-2번 비교

.

.

n-1번째 요소 : 요소 n과 1번 비교

n번째 요소 : 비교하지 않음

따라서 총 비교횟수는 다음과 같다.

<img src="https://latex.codecogs.com/gif.latex?\sum_{i=1}^{n-1}i=\frac{(n-1)n}{2}=\frac{n^{2}}{2}-\frac{n}{2}" alt="\sum_{i=1}^{n-1}i=\frac{(n-1)n}{2}=\frac{n^{2}}{2}-\frac{n}{2}" align="absmiddle" /> 

n이 증가할수록 비교 횟수는 n^2에 비례하여 증가한다.

따라서 중복 찾기 알고리즘의 시간 복잡도는 O(n^2) 이다.