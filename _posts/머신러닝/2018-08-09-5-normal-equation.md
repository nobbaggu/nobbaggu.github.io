---
title: (머신러닝) 5. Normal Equation
date: 2018-08-09T06:00:19+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - AI
  - cost function
  - gradient descent
  - hypothesis function
  - machine learning
  - normal equation
  - pinv
  - 가설함수
  - 경사하강법
  - 기계학습
  - 머신러닝
  - 비용함수
  - 인공지능
---
오늘은 Normal Equation 이라는 tiral and error 보다는 analytic한 방법을 통해서 hypothesis function을 구하는 법을 알아보겠다.

먼저, 기본적인 idea는 고등 미적분의 local minimum에서의 미분계수가 0이라는 사실에 근거한다.

<img class="aligncenter  wp-image-259" src="/images/2018/08/1-3.jpg" alt="" width="613" height="250" srcset="/images/2018/08/1-3.jpg 900w, /images/2018/08/1-3-300x122.jpg 300w, /images/2018/08/1-3-768x313.jpg 768w" sizes="(max-width: 613px) 100vw, 613px" /> 

&nbsp;

포물선의 최소값에서는 함수 f의 theta에 대한 미분계수가 0이다. 이 사실을 cost function에 적용하여 cost function의 최소값에서의 <img src="https://latex.codecogs.com/gif.latex?\theta" alt="\theta" align="absmiddle" />를 구하는 것이다. 하지만 보통 cost function은 2개 이상의 feature와 parameter가 있기때문에 vector와 matrix 형태의 선형대수학으로 풀어야한다.

<img src="https://latex.codecogs.com/gif.latex?\triangledown&space;J({&space;\theta&space;})=(\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;0&space;}&space;}&space;,&space;\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;1&space;}&space;}&space;,&space;\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;2&space;}&space;}&space;,&space;\cdot&space;\cdot&space;\cdot&space;,&space;\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;n&space;}&space;}&space;)=(0,&space;0,&space;0,&space;\cdot&space;\cdot&space;\cdot&space;,&space;0)" alt="\triangledown J({ \theta })=(\frac { \partial J }{ \partial { \theta }_{ 0 } } , \frac { \partial J }{ \partial { \theta }_{ 1 } } , \frac { \partial J }{ \partial { \theta }_{ 2 } } , \cdot \cdot \cdot , \frac { \partial J }{ \partial { \theta }_{ n } } )=(0, 0, 0, \cdot \cdot \cdot , 0)" align="absmiddle" /> 

<img src="https://latex.codecogs.com/gif.latex?J(\theta&space;)=\frac&space;{&space;1&space;}{&space;2m&space;}&space;{&space;(X\theta&space;-y)&space;}^{&space;T&space;}(X\theta&space;-y)" alt="J(\theta )=\frac { 1 }{ 2m } { (X\theta -y) }^{ T }(X\theta -y)" align="absmiddle" />  공식을 위의 방정식에 넣고 정리하면 다음과 같은 결과가 나온다.

<img src="https://latex.codecogs.com/gif.latex?\theta&space;={&space;{&space;(X&space;}^{&space;T&space;}X)&space;}^{&space;-1&space;}{&space;X&space;}^{&space;T&space;}y" alt="\theta ={ { (X }^{ T }X) }^{ -1 }{ X }^{ T }y" align="absmiddle" /> 

이 θ는 정확히 기울기가 0이 되게하는, 즉 J(θ)가 최소일 때의 θ이다.

&nbsp;

normal equation을 사용하면 귀찮게 learning rate를 찾아가며 결정해야 할 필요도 없고 gradient descent의 반복적인 iteration도 할 필요가 없다. 하지만 normal equation은 단점도 가지고 있다. X는 m×n(m = #training data set, n = #feature) 행렬이기때문에 위의 식에서 (XTX)는 n × n 행렬이 된다. 따라서 <span style="color: #ff0000;"><strong>n이 매우 크다면 이 행렬을 계산하기에는 컴퓨터에도 벅차기에 느린 작업이 될 것이다.</strong></span> 요즘 컴퓨터로는 10,000 × 10,000 행렬 정도는 충분히 수월하게 계산이 된다. 하지만 n이 100,000 혹은 1,000,000이나 그 이상이라면 이야기는 달라진다.

얼핏 들으면 gradient descent는 잘 쓸 일이 없어보이지만 그렇지 않다. feature나 parameter가 매우 많은경우 Normal Equation은 매우 높은 계산비용이 들기 때문에 잘 맞지 않는다.

&nbsp;

**※<img src="https://latex.codecogs.com/gif.latex?\mathbf{(X^{T}X)^{-1}}" alt="\mathbf{(X^{T}X)^{-1}}" align="absmiddle" /> 가 존재하지 않는다면?**

만약 X'(X transpose이다.)와 X의 곱인 X&#8217;X가 역행렬을 가지지 않는다면? 선형대수학에서 배웠다면 알겠지만 역행렬을 가지지 않는(non-invertible) 행렬이 있고 이를 degenerate 혹은 singular 하다고 한다. 모든 행렬에 역행렬이 있는 것은 아니다. 가장 대표적인 case로 <span style="color: #ff0000;"><strong>두 개 이상의 row 나 column이 linearly dependent하다면 이 행렬은 역행렬이 존재하지 않는다.</strong></span> 가령 집값을 예측해주는 프로그램에서 x\_1 = feet단위로 표현된 집의 크기이고 x\_2는 m단위의 집의 크기라면 모든 training set 행렬 X에서 x\_2 column은 x\_1 column의 값에 단순히 3.28을 곱한 값이기 때문이다.(1m = 3.28feet)

&nbsp;

X&#8217;X가 non-invertible하여 feature의 수를 줄이고 싶은 경우에는 여러가지 dimensionality reduction 방법이 있다. 대표적으로 PCA와 LDA 등이 있는데, 이후에 다룰 것이다.

&nbsp;

사실 cost function의 minimum을 구하는 상황에서 X&#8217;X의 역행렬이 존재하지 않는 경우는 거의 없다. 그리고 있다고 하더라도 거의 오차가 0에 가깝게 근사적으로 계산할 수 있다. octave의 pinv 함수를 이용한다면 말이다. octave에는 inv 함수와 pinv 함수가 있는데 inv는 기술적으로 일반적인 역행렬(inverse) 계산이고 pinv는 pseudo inverse를 계산해준다.