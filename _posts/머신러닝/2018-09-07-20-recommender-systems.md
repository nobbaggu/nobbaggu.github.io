---
title: 20. Recommender Systems
date: 2018-09-07T20:29:58+09:00
author: SWnomad
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - collaborative filtering
  - low rank matrix
  - recommender system
  - 영화 추천 알고리즘
  - 추천 시스템
  - 추천 알고리즘
---
Recommender System은 머신러닝 알고리즘의 적용이 가장 활발한 분야들 중 하나이다. 어떠한 사용자의 시청기록, 평점부여 기록을 토대로 그 사람에게 관련영화를 추천해 주는 시스템이 있다.

<img class="aligncenter wp-image-706" src="/images/2018/09/no-name-4.png" alt="" width="488" height="206" srcset="/images/2018/09/no-name-4.png 663w, /images/2018/09/no-name-4-300x127.png 300w" sizes="(max-width: 488px) 100vw, 488px" /> 

방법 하나는 각 영화마다 로맨스정도, 액션정도, 코믹 정도등을 feature로 정량화 시키는 것이다.

각 user마다 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(\theta_{1},\theta_{2})" alt="\dpi{120} (\theta_{1},\theta_{2})" align="absmiddle" />의 parameter가 있다고 하면 아직 그 user가 rating하지 않은 영화에 대해서도 feature vector를 곱하여 추측할 수 있다. user2의 parameter vector가 (4,10)이라고 하면 이 유저의 movie2에 대한 예상되는 평점은 3.8점이다.

이를 좀 일반적으로 정리해보겠다.

&nbsp;

# <span style="font-size: 18pt;"><strong>1. Problem formulation</strong></span>

* * *

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;n_{u}=" alt="\dpi{120} n_{u}=" align="absmiddle" /> user(사용자) 수

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;n_{m}=" alt="\dpi{120} n_{m}=" align="absmiddle" /> movie(영화) 수

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;r(i,j)=" alt="\dpi{120} r(i,j)=" align="absmiddle" /> user<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;j" alt="\dpi{120} j" align="absmiddle" /> 가 movie<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;i" alt="\dpi{120} i" align="absmiddle" />  에 rate(평가)를 했으면 1 아니면 0

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;y(i,j)=" alt="\dpi{120} y(i,j)=" align="absmiddle" /> user<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;j" alt="\dpi{120} j" align="absmiddle" /> 가 movie<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;i" alt="\dpi{120} i" align="absmiddle" />  에 매긴 rating(평점)       <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;r(i,j)=1" alt="\dpi{120} r(i,j)=1" align="absmiddle" />인 경우만 정의

&nbsp;

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta&{(j)}=" alt="\dpi{120} \theta&{(j)}=" align="absmiddle" />  parameter vector for user <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;j" alt="\dpi{120} j" align="absmiddle" />

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}=" alt="\dpi{120} x^{(i)}=" align="absmiddle" />  feature vector for user <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;i" alt="\dpi{120} i" align="absmiddle" />

prediction =<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(\theta^{(j)})^T&space;x^{(i)}" alt="\dpi{120} (\theta^{(j)})^T x^{(i)}" align="absmiddle" /> 

&nbsp;

user <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;j" alt="\dpi{120} j" align="absmiddle" />의 parameter를 optimize 한다고 할 때 결국 우리의 목표는 다음의 cost function을 최소화 시키는 것이다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;J(\theta^{(j)})=\frac{1}{2}\sum_{i:r(i,j)=1}((\theta^{(j)})^{(T)}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{k=1}^{n}(\theta^{(j)}_{k})^{2}" alt="\dpi{120} J(\theta^{(j)})=\frac{1}{2}\sum_{i:r(i,j)=1}((\theta^{(j)})^{(T)}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{k=1}^{n}(\theta^{(j)}_{k})^{2}" align="absmiddle" /> 

앞서 linear regression에서 배운 gradient descent나 matlab, octave, python 등에서 제공하는 라이브러리의 optimization function을 사용해도 된다.

좀 더 범위를 넓혀 모든 사용자들의 cost function을 계산하는 것이 일반적이다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;J(\theta)=\frac{1}{2}\sum_{j=1}^{n_{u}}\sum_{i:r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{j=1}^{n_{u}}\sum_{k=1}^{n}(\theta^{(j)}_{k})^{2}" alt="\dpi{120} J(\theta)=\frac{1}{2}\sum_{j=1}^{n_{u}}\sum_{i:r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{j=1}^{n_{u}}\sum_{k=1}^{n}(\theta^{(j)}_{k})^{2}" align="absmiddle" /> 

우리가 익숙한 cost function과 다른 것은 training dataset(movie)의 갯수 m을 식에 넣지 않았다는 것이다. m이 있으나 없으나 최소화 되는 과정에서 parameter가 정해지는 것에는 아무런 영향이 없기 때문에 상관없다.

&nbsp;

&nbsp;

# <span style="font-size: 18pt;"><strong>2. Collaborative Filtering</strong></span>

* * *

하지만 영화의 로맨스 정도, 액션 정도 등의 feature는 주관적인 생각이 많이 개입되는 특성이 있어 이를 수치화 하는 것 또한 애매한 부분이 있어 쉽지가 않다. 만약 사용자의 parameter가 정해져 있다고 가정한다면 역으로 위의 parameter에 대한 cost function이 아닌 feature에 대한 cost function을 사용해서 feature를 찾을 수 있다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;J(x)=\frac{1}{2}\sum_{i=1}^{n_{m}}\sum_{j:r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{i=1}^{n_{m}}\sum_{k=1}^{n}(x^{(i)}_{k})^{2}" alt="\dpi{120} J(x)=\frac{1}{2}\sum_{i=1}^{n_{m}}\sum_{j:r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{i=1}^{n_{m}}\sum_{k=1}^{n}(x^{(i)}_{k})^{2}" align="absmiddle" /> 

이렇게 찾은 feature를 이용해서 parameter를 다시 update한다. 그리고 이 parameter를 사용해서 feature를 다시 update 한다. 이렇게 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta&space;\rightarrow&space;x&space;\rightarrow&space;\theta&space;\rightarrow&space;x&space;\rightarrow&space;\cdot\cdot\cdot" alt="\dpi{120} \theta \rightarrow x \rightarrow \theta \rightarrow x \rightarrow \cdot\cdot\cdot" align="absmiddle" />의 과정을 반복하여 수렴시키면 좋은 parameter와 feature를 구할 수 있다.

위 처럼 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta" alt="\dpi{120} \theta" align="absmiddle" />와 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x" alt="\dpi{120} x" align="absmiddle" />를 왔다갔다 하며 update 하는 것 보다 간단한 방법이 있다.

다음의 식을 minimize 함으로써 parameter와 feature를 **동시에** optimize 할 수 있다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;J(x,\theta)=\frac{1}{2}\sum_{(i,j):r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{i=1}^{n_{m}}\sum_{k=1}^{n}(x^{(i)}_{k})^{2}+\frac{\lambda}{2}\sum_{j=1}^{n_{u}}\sum_{k=1}^{n}(\theta^{(j)}_{k})^{2}" alt="\dpi{120} J(x,\theta)=\frac{1}{2}\sum_{(i,j):r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{i=1}^{n_{m}}\sum_{k=1}^{n}(x^{(i)}_{k})^{2}+\frac{\lambda}{2}\sum_{j=1}^{n_{u}}\sum_{k=1}^{n}(\theta^{(j)}_{k})^{2}" align="absmiddle" /> 

식이 복잡해 보이지만 우리가 익숙하게 알고있던 parameter의 cost function과 feature의 cost function을 합친 것일 뿐이다. 이것을 우리는 collaborative filtering이라고 부른다.

참고로 이 알고리즘은 스스로 학습을 하므로 bias unit은 없다. <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x\in&space;\mathbb{R}^{n},\quad&space;\theta&space;\in&space;\mathbb{R}^{n}" alt="\dpi{120} x\in \mathbb{R}^{n},\quad \theta \in \mathbb{R}^{n}" align="absmiddle" />

위의 알고리즘을 순서대로 정리해보자.

1.<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(x^{(1)},x^{(2)},\cdot\cdot\cdot,x^{(n_{m})}),&space;(\theta^{(1)},\theta^{(2)},\cdot\cdot\cdot,\theta^{(n_{u})})" alt="\dpi{120} (x^{(1)},x^{(2)},\cdot\cdot\cdot,x^{(n_{m})}), (\theta^{(1)},\theta^{(2)},\cdot\cdot\cdot,\theta^{(n_{u})})" align="absmiddle" /> 를 small random value로 initialize 한다.

2. <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;J(x,\theta)" alt="\dpi{120} J(x,\theta)" align="absmiddle" />를 gradient descent 혹은 라이브러리에서 제공하는 optimization 함수를 사용하여 minimize 한다.

3. 위에서 구한 parameter를 가지고 아직 어떤 유저가 아직 rating하지 않은 영화를 predict 한다.

&nbsp;

&nbsp;

# <span style="font-size: 18pt;"><strong>3. Vectorization : Low Rank Matrix Factorization</strong></span>

* * *

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;X=\begin{bmatrix}---x^{(1)}---&space;\\&space;---x^{(2)}---&space;\\&space;\cdot&space;\cdot&space;\cdot&space;\\&space;---x^{(n_{m})}---&space;\end{bmatrix}\quad&space;\qaud&space;\Theta=\begin{bmatrix}&space;---\theta^{(1)}---&space;\\&space;---\theta^{(2)}---&space;\\&space;\cdot&space;\cdot&space;\cdot&space;\\&space;---\theta^{(n_{u})}---&space;\end{bmatrix}" alt="\dpi{120} X=\begin{bmatrix}---x^{(1)}--- \\ ---x^{(2)}--- \\ \cdot \cdot \cdot \\ ---x^{(n_{m})}--- \end{bmatrix}\quad \qaud \Theta=\begin{bmatrix} ---\theta^{(1)}--- \\ ---\theta^{(2)}--- \\ \cdot \cdot \cdot \\ ---\theta^{(n_{u})}--- \end{bmatrix}" align="absmiddle" /> 

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;X" alt="\dpi{120} X" align="absmiddle" /> 의 각 row는 영화 하나의 feature vector이고, <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Theta" alt="\dpi{120} \Theta" align="absmiddle" />의 각 row는 유저 한 명의 parameter vector이다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;Y=X\Theta^{T}" alt="\dpi{120} Y=X\Theta^{T}" align="absmiddle" />  matrix의 각 row는 한 편의 영화, 각 column은 한 명의 유저를 나타낸다.

Y의 3행 4열 위치는 3번째 영화에 대한 4번째 유저의 평점을 나타낸다.

&nbsp;

새로운 유저를 추가한다고 했을 때, 초기 parameter는 regularization term에 의하여 모둔 parameter가 0으로 initialize 될 것이다. 따라서 어떤 영화에 대한 예측 평점도 0이 될 것이고, 이는 상식적으로 생각해도 부적절하다.

기존의 유저, 영화들로 계산한 matrix Y가 다음과 같다고 해보자.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;Y&space;=&space;\begin{bmatrix}&space;5&5&0&0&space;\\&space;4&?&?&0&space;\\&space;0&0&5&4&space;\\&space;0&0&5&0&space;\end{bmatrix}" alt="\dpi{120} Y = \begin{bmatrix} 5&5&0&0 \\ 4&?&?&0 \\ 0&0&5&4 \\ 0&0&5&0 \end{bmatrix}" align="absmiddle" /> 

영화 i에 대한 평균은 다음의 식으로 계산 가능하다. <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\mu_{i}=\frac{\sum_{j:r(i,j)=1}Y_{i,j}}{\sum_{j}{r(i,j)}}" alt="\dpi{120} \mu_{i}=\frac{\sum_{j:r(i,j)=1}Y_{i,j}}{\sum_{j}{r(i,j)}}" align="absmiddle" />

위의 matrix의 평균은 다음과 같이 된다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\begin{bmatrix}2.5\\2\\2.25\\1.25&space;\end{bmatrix}" alt="\dpi{120} \begin{bmatrix}2.5\\2\\2.25\\1.25 \end{bmatrix}" align="absmiddle" />  각 영화의 (총 평점의 합) / (평가한 사용자 수) 이다.

따라서 새로운 유저를 추가할 때에는 평점을 기존 데이터의 평균으로 초기화하고 parameter는 위에서 말한 그대로 random small value로 초기화하면 된다. 이후에 위의 collaborative filtering 알고리즘으로 parameter와 feature를 학습시키면 된다.

이 알고리즘의 목적을 다른 식으로 해석하자면 어떤 유저가 어떤 영화 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;i" alt="\dpi{120} i" align="absmiddle" />를 좋아했다면, 그 영화의 feature <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}" alt="\dpi{120} x^{(i)}" align="absmiddle" />와 비슷한 feature <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(j)}" alt="\dpi{120} x^{(j)}" align="absmiddle" />를 가진 영화를 추천해주는 것이다. 즉,<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\left&space;\|&space;x^{(i)}-x^{(j)}&space;\right&space;\|" alt="\dpi{120} \left \| x^{(i)}-x^{(j)} \right \|" align="absmiddle" /> 가 작은 영화를 찾는 것이다.