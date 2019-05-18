---
title: (머신러닝) 4. Nonlinear Model with Linear Regression
date: 2018-08-09T03:17:32+09:00
author: SWnomad
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - AI
  - feature normalization
  - feature scailing
  - hypothesis function
  - linear regression
  - machine learning
  - 가설함수
  - 기계학습
  - 머신러닝
  - 선형회귀
  - 인공지능
---
Linear Regression 이라고 해서 선형적인 데이터로 알고리즘을 학습 시킬 때에만 사용 할 것이라는 착각을 할 수도 있다. 오늘은 비선형적인 데이터로 선형회귀 알고리즘을 학습시킬 수 있다는 것을 공부해 볼 것이다.

<img class="aligncenter wp-image-255" src="/images/2018/08/1-2.jpg" alt="" width="283" height="253" srcset="/images/2018/08/1-2.jpg 517w, /images/2018/08/1-2-300x268.jpg 300w" sizes="(max-width: 283px) 100vw, 283px" /> 

집의 가격을 예측하는 프로그램을 만들려고 한다. feature는 집의 가로폭(length)과 세로폭(depth) 이다. 이 때 hypothesis function은 다음과 같이 세운다.

<img src="https://latex.codecogs.com/gif.latex?{&space;h&space;}_{&space;\theta&space;}(x)&space;=&space;{&space;\theta&space;}_{&space;0&space;}&space;+&space;{&space;\theta&space;}_{&space;1&space;}{&space;x&space;}_{&space;1&space;}&space;+&space;{&space;\theta&space;}_{&space;2&space;}{&space;x&space;}_{&space;2&space;}&space;(&space;{&space;x&space;}_{&space;1&space;}&space;:&space;length,&space;\quad{&space;x&space;}_{&space;2&space;}&space;:&space;depth)" alt="{ h }_{ \theta }(x) = { \theta }_{ 0 } + { \theta }_{ 1 }{ x }_{ 1 } + { \theta }_{ 2 }{ x }_{ 2 } ( { x }_{ 1 } : length, \quad{ x }_{ 2 } : depth)" align="absmiddle" /> 

&nbsp;

그런데 사실 집값은 길이보다는 면적으로 결정되는 게 일반적이다. 그럼 집의 평수(size)를 feature로 가지는 모델을 찾기 전에 먼저 집의 평수에 따른 가격의 분포를 보자.

<img class="aligncenter wp-image-256" src="/images/2018/08/2-2.jpg" alt="" width="345" height="222" srcset="/images/2018/08/2-2.jpg 526w, /images/2018/08/2-2-300x193.jpg 300w" sizes="(max-width: 345px) 100vw, 345px" /> 

위의 training data set이 보이는 분포는 선형 모델로 나타내기에는 무리가 있어보인다. 2차식으로 나타내려하니 뒤집힌 포물선 모양처럼 집값이 크기가 커지는데 가격이 내려갈 리는 없을 것 같다. 따라서 항을 하나 더 추가해서 3차식으로 표현해보려한다.

<img src="https://latex.codecogs.com/gif.latex?{&space;h&space;}_{&space;\theta&space;}(x)&space;=&space;{&space;\theta&space;}_{&space;0&space;}+{&space;\theta&space;}_{&space;1&space;}x+{&space;\theta&space;}_{&space;2&space;}{&space;x&space;}^{&space;2&space;}+{&space;\theta&space;}_{&space;3&space;}{&space;x&space;}^{&space;3&space;}\quad&space;(x:size)" alt="{ h }_{ \theta }(x) = { \theta }_{ 0 }+{ \theta }_{ 1 }x+{ \theta }_{ 2 }{ x }^{ 2 }+{ \theta }_{ 3 }{ x }^{ 3 }\quad (x:size)" align="absmiddle" /> 

꽤나 괜찮은 식이지만 linear regression 으로는 모델의 parameter를 찾을 수 없다. linear regression 을 사용하려면 어떻게 해야할까?  <img src="https://latex.codecogs.com/gif.latex?x=x_{1},\quad&space;x^2=x_{2},\quad&space;x^3=x_{3}" alt="x=x_{1},\quad x^2=x_{2},\quad x^3=x_{3}" align="absmiddle" /> 처럼 서로 다른 feature로 놓으면 된다.

<img src="https://latex.codecogs.com/gif.latex?{&space;h&space;}_{&space;\theta&space;}(x)={&space;\theta&space;}_{&space;0&space;}+{&space;\theta&space;}_{&space;1&space;}{&space;x&space;}_{&space;1&space;}+{&space;\theta&space;}_{&space;2&space;}{&space;x&space;}_{&space;2&space;}+{&space;\theta&space;}_{&space;3&space;}{&space;x&space;}_{&space;3&space;}\quad&space;({&space;x&space;}_{&space;1&space;}:size,\quad&space;{&space;x&space;}_{&space;2&space;}:{&space;size&space;}^{&space;2&space;},\quad&space;{&space;x&space;}_{&space;3&space;}:{&space;size&space;}^{&space;3&space;})" alt="{ h }_{ \theta }(x)={ \theta }_{ 0 }+{ \theta }_{ 1 }{ x }_{ 1 }+{ \theta }_{ 2 }{ x }_{ 2 }+{ \theta }_{ 3 }{ x }_{ 3 }\quad ({ x }_{ 1 }:size,\quad { x }_{ 2 }:{ size }^{ 2 },\quad { x }_{ 3 }:{ size }^{ 3 })" align="absmiddle" /> 

이제 linear regression 을 사용할 수 있게 되었다. 그리고 feature scailing을 잊지 말아야한다.

위와 같은 case에서는 하나의 feature가 다른 feature의 제곱, 세제곱으로 나타나니 feature scailing이 필수다.

가령 집의 size가 [1, 1000]의 feet^2 단위를 가진다고 해보자. 이 때 feature scailing은 다음과 같이 할 수 있다.

<img src="https://latex.codecogs.com/gif.latex?{&space;x&space;}_{&space;1&space;}=\frac&space;{&space;size&space;}{&space;1000&space;}&space;,\quad&space;{&space;x&space;}_{&space;2&space;}=\frac&space;{&space;{&space;size&space;}^{&space;2&space;}&space;}{&space;{&space;1000&space;}^{&space;2&space;}&space;}&space;,\quad&space;{&space;x&space;}_{&space;3&space;}=\frac&space;{&space;{&space;size&space;}^{&space;3&space;}&space;}{&space;{&space;1000&space;}^{&space;3&space;}&space;}" alt="{ x }_{ 1 }=\frac { size }{ 1000 } ,\quad { x }_{ 2 }=\frac { { size }^{ 2 } }{ { 1000 }^{ 2 } } ,\quad { x }_{ 3 }=\frac { { size }^{ 3 } }{ { 1000 }^{ 3 } }" align="absmiddle" /> 

집값의 분포는 square root 식도 잘 들어맞을 것 같다. 따라서 model의 feature를 다음과 같이 만들수도 있다.

<img src="https://latex.codecogs.com/gif.latex?{&space;h&space;}_{&space;\theta&space;}(x)={&space;\theta&space;}_{&space;0&space;}+{&space;\theta&space;}_{&space;1&space;}{&space;x&space;}_{&space;1&space;}+{&space;\theta&space;}_{&space;2&space;}{&space;x&space;}_{&space;2&space;}\quad&space;({&space;x&space;}_{&space;1&space;}:size,\quad&space;{&space;x&space;}_{&space;2&space;}:\sqrt&space;{&space;size&space;}&space;)\\&space;\\&space;{&space;x&space;}_{&space;1&space;}=\frac&space;{&space;size&space;}{&space;1000&space;}&space;,\quad&space;{&space;x&space;}_{&space;2&space;}=\frac&space;{&space;\sqrt&space;{&space;size&space;}&space;}{&space;\sqrt&space;{&space;1000&space;}&space;}" alt="{ h }_{ \theta }(x)={ \theta }_{ 0 }+{ \theta }_{ 1 }{ x }_{ 1 }+{ \theta }_{ 2 }{ x }_{ 2 }\quad ({ x }_{ 1 }:size,\quad { x }_{ 2 }:\sqrt { size } )\\ \\ { x }_{ 1 }=\frac { size }{ 1000 } ,\quad { x }_{ 2 }=\frac { \sqrt { size } }{ \sqrt { 1000 } }" align="absmiddle" /> 

&nbsp;