---
id: 649
title: (머신러닝) 17. Unsupervised Learning - K-Means Algorithm
date: 2018-08-29T21:59:44+09:00
author: SWnomad
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - cluster
  - clustering
  - k-means
  - k-means algorithm
  - k-means 알고리즘
  - unsupervised learning
  - 비지도학습
  - 클러스터
  - 클러스터링
---
이번에는 비지도학습(unsupervised learning)에 대해서 알아보자.

<img class="aligncenter wp-image-650" src="/images/2018/08/no-name-101.png" alt="" width="608" height="291" srcset="/images/2018/08/no-name-101.png 863w, /images/2018/08/no-name-101-300x144.png 300w, /images/2018/08/no-name-101-768x368.png 768w" sizes="(max-width: 608px) 100vw, 608px" /> 

지도학습에서의 classification 문제에서는 각각의 데이터가 어떤 class에 해당 하는지 미리 답을 가지고 있는 training dataset으로 학습을 시켰다. training dataset의 feature x와 label y의 관계를 잘 따라가는 model을 얻어서 지금까지 보지 못한 data의 feature x를 가지고 label y를 예측하는 것이었다.

하지만 비지도학습에서는 이러한 정해진 label이 없다. feature만 가지고있다. 그럼 위의 두 번째 그림과 같이 data가 퍼져있으면 어떠한 특징들에 따라 알아서 분류를 하는 것이다. 가령 장문의 글이 수천 개 있으면 글자 수에 따라 나누던지, 많이 쓰인 단어의 수에 따라 시, 문학, 뉴스기사 등으로 나누던지, 다른 페이지에서 인용된 수 등의 feature를 가지고 유용한 글 level 1,2,3 등으로 나누던지한다. 그러니까 우리가 분류하는 목적에 따라 알맞게 feature를 정해주고 그룹을 나누는 것이다. 이러한 grouping 작업을 clustering(클러스터링)이라고 한다. 클러스터링은 비지도 학습의 가장 대표적인 예이다.

&nbsp;

<span style="font-size: 18pt;"><strong>1. K-Means Algorithm</strong></span>

* * *

<img class="aligncenter wp-image-653" src="/images/2018/08/no-name-102.png" alt="" width="454" height="195" srcset="/images/2018/08/no-name-102.png 729w, /images/2018/08/no-name-102-300x129.png 300w" sizes="(max-width: 454px) 100vw, 454px" /> 

feature가 2개인 dataset이 있다고 해보자. 이 dataset을 두 그룹으로 분류한다고 해보자. 일단 위와 같이 랜덤하게 두 개의 centroid를 정한다. 그리고 각각의 데이터가 두 centroid 중 어떤 곳에 가까운지에 따라 grouping을 한다. 그럼 일단 위와 같이 두 개의 그룹으로 나뉜다. 이렇게 나뉘고 나면 각각의 그룹에 있는 데이터들의 평균을 구해 그 곳으로 centroid를 옮긴다.

<img class="aligncenter wp-image-662" src="/images/2018/08/no-name-106.png" alt="" width="451" height="194" srcset="/images/2018/08/no-name-106.png 729w, /images/2018/08/no-name-106-300x129.png 300w" sizes="(max-width: 451px) 100vw, 451px" /> 

위의 새로운 각각의 데이터를 새로운 centroid에 가까운 그룹으로 다시 배정한다.

<img class="aligncenter size-medium wp-image-655" src="/images/2018/08/no-name-103-300x266.png" alt="" width="300" height="266" srcset="/images/2018/08/no-name-103-300x266.png 300w, /images/2018/08/no-name-103.png 348w" sizes="(max-width: 300px) 100vw, 300px" /> 

다시 그룹핑을 하고 위와 같이 각 그룹의 데이터의 평균을 다시 구해 또다시 centroid를 구한다. 이렇게 되면 위와 같이 그룹이 나눠지고 각 그룹이 얼추 적당한 centroid를 가지게 된다. 위와 같은 작업을 계속 반복해서 하다보면 알맞은 centorid에 대해 알맞게 그룹핑이 된다. 각 group을 cluster(클러스터)라 부르고 이러한 clustering 알고리즘이 바로 K-means 알고리즘이다.

이 과정을 수식을 사용해서 정량적으로 표현해보자. 그 전에 몇 가지 정의를 하겠다.

  * k  :  cluster의 수
  *<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(1)},x^{(2)},\cdot\cdot\cdot,x^{(m)}" alt="\dpi{120} x^{(1)},x^{(2)},\cdot\cdot\cdot,x^{(m)}" align="absmiddle" />   :  training dataset
  *<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;c^{(i)}" alt="\dpi{120} c^{(i)}" align="absmiddle" />   :  데이터 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}" alt="\dpi{120} x^{(i)}" align="absmiddle" /> 가 속하는 cluster의 index <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(1,2,3,\cdot\cdot\cdot,k)" alt="\dpi{120} (1,2,3,\cdot\cdot\cdot,k)" align="absmiddle" />

&nbsp;

**1. 랜덤하게 그룹의 수 K만큼 centroid를 초기화한다.**

→  <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\mu_{1},&space;\mu_{2},\cdot\cdot\cdot,\mu_{k}" alt="\dpi{120} \mu_{1}, \mu_{2},\cdot\cdot\cdot,\mu_{k}" align="absmiddle" />

**2. 각각의 데이터 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}" alt="\dpi{120} x^{(i)}" align="absmiddle" />마다 가장 가까운 centroid의 index <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;c^{(i)}" alt="\dpi{120} c^{(i)}" align="absmiddle" /> 를 찾아 그룹핑한다.**

→ <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;c^{(1)},c^{(2)},\cdot\cdot\cdot,c^{(m)}\quad\quad(c^{(i)}=argmin_{k}\left&space;\|&space;x^{(i)}-\mu_{k}&space;\right&space;\|^{2})" alt="\dpi{120} c^{(1)},c^{(2)},\cdot\cdot\cdot,c^{(m)}\quad\quad(c^{(i)}=argmin_{k}\left \| x^{(i)}-\mu_{k} \right \|^{2})" align="absmiddle" /> 

**3. 위에서 정해진 그룹마다 포함되는 데이터들 location의 평균을 구해 centroid를 갱신한다.**

→  update <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\mu_{1},&space;\mu_{2},\cdot\cdot\cdot,\mu_{k}" alt="\dpi{120} \mu_{1}, \mu_{2},\cdot\cdot\cdot,\mu_{k}" align="absmiddle" />

**4. (2), (3)의 과정을 수렴할 때 까지, 즉 더 이상 data의 group이 변하지 않을 때 까지 <span style="color: #ff0000;">반복</span>한다.**

위의 과정을 머릿속으로 그림을 그려가며 따라가보면 정말 간단하고 직관적인 알고리즘이라는 것을 알 수 있다.

과정 (2)를 &#8216;cluster assignment&#8217; step이라고 한다.

과정 (3)을 &#8216;move centroid&#8217; step이라고 한다.

&nbsp;

&nbsp;

<span style="font-size: 18pt;"><strong>2</strong><strong>. Optimization</strong></span>

* * *

우리가 optimization(최적화) 해야 하는 것은 결국 cost function이다. clustering에서 cost function은 다음과 같이 정의된다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;J(c^{(1)},\cdot\cdot\cdot,c^{(m)},\mu_{1},\cdot\cdot\cdot,\mu_{k})=\frac{1}{m}\sum_{i=1}^{m}\left&space;\|&space;x^{(i)}-\mu_{c^{(i)}}&space;\right&space;\|^{2}" alt="\dpi{120} J(c^{(1)},\cdot\cdot\cdot,c^{(m)},\mu_{1},\cdot\cdot\cdot,\mu_{k})=\frac{1}{m}\sum_{i=1}^{m}\left \| x^{(i)}-\mu_{c^{(i)}} \right \|^{2}" align="absmiddle" /> 

각각의 [데이터 ~ 데이터가 속한 cluster의 centroid 사이 거리]의 제곱을 모두 더하여 평균을 낸 값이다.

위에서 말한 (2)의 cluster assignment 과정에서는 각 데이터를 가장 가까운 centroid에 할당하는 과정이었다. 이 과정에서는 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\mu_{1},&space;\mu_{2},\cdot\cdot\cdot,\mu_{k}" alt="\dpi{120} \mu_{1}, \mu_{2},\cdot\cdot\cdot,\mu_{k}" align="absmiddle" />는 고정한 채<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;c^{(1)},c^{(2)},\cdot\cdot\cdot,c^{(m)}" alt="\dpi{120} c^{(1)},c^{(2)},\cdot\cdot\cdot,c^{(m)}" align="absmiddle" /> 를 정하면서 cost function J를 최소화 시켰다.

그리고 (3)의 move centroid 과정은 cost function J가 가장 작아지는 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\mu_{1},&space;\mu_{2},\cdot\cdot\cdot,\mu_{k}" alt="\dpi{120} \mu_{1}, \mu_{2},\cdot\cdot\cdot,\mu_{k}" align="absmiddle" />를 찾는 과정이었다.

우리가 실행한 알고리즘 자체가 cost function J를 최소화 하는 과정이었던 것이다.

&nbsp;

<span style="font-size: 18pt;"><strong>3. Tips</strong></span>

* * *

몇 가지 주의사항이 있다.

**1. centroid 초기화**

위에서는 centroid를 random하게 initialization하라고 했는데 한 가지 추천 방법으로는 dataset 중에서 임의로 k개를 골라서 centroid로 삼는다. 당연한 소리지만 다른 cluster에 centroid를 할당하지 않는다.

&nbsp;

**2. local optima**

K-Means 알고리즘을 쓰다보면 참 운이 없게도 좋지 않은 centroid initialization으로 local optima에 걸려 버릴 수가 있다.

<img class="wp-image-681 alignleft" src="/images/2018/08/no-name-110.png" alt="" width="482" height="199" srcset="/images/2018/08/no-name-110.png 797w, /images/2018/08/no-name-110-300x124.png 300w, /images/2018/08/no-name-110-768x317.png 768w" sizes="(max-width: 482px) 100vw, 482px" /><img class="aligncenter  wp-image-680" src="/images/2018/08/no-name-109.png" alt="" width="215" height="185" /> 

&nbsp;

이를 예방하는 방법 중 하나는 K-Means 알고리즘을 서로 다른 centroid 초기화를 하여 여러 번 시행하는 것이다.

가령 각각 다른 100개의 초기화 된 centroid를 가지고 각각에 대해 K-means 알고리즘을 적용하여 가장 작은 cost function J가 나오는 것을 고르면 된다.

&nbsp;

<span style="font-size: 18pt;"><strong>4. Cluster의 수</strong></span>

* * *

클러스터의 갯수 k를 몇 으로 해야할지 애매할 수 있다. 클러스터의 수를 정하는 확실한 통용되는 방법이 있지는 않다. 한 가지 방법이 있기는한데, cluster를 2개부터 점점 늘리며 cost function J를 계산해 본다.

<img class="aligncenter size-medium wp-image-659" src="/images/2018/08/no-name-105-300x204.png" alt="" width="300" height="204" srcset="/images/2018/08/no-name-105-300x204.png 300w, /images/2018/08/no-name-105.png 454w" sizes="(max-width: 300px) 100vw, 300px" /> 

cluster의 수를 늘리다 보면 더 이상 cost function이 거의 줄어들지 않는 시점이 올 것이다. 이 때의 k를 클러스터의 수로 정하는 것이 하나의 방법이다. 위의 iteration 횟수에 대한 cost function J의 그래프를 보면 사람의 팔을 닮았다. 그리고 위에서 빨간색 화살표로 표시한 곳이 팔꿈치이다. 따라서 이런식으로 cluster의 수를 정하는 방법을 elbow method라고 한다.

하지만 elbow method가 통하지 않을때도 있다. 이 때에는 K-means 알고리즘의 분류 목적에 잘 맞도록 cluster의 수를 정할 수 있다.