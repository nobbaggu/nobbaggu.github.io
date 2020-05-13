---
title: (머신러닝) 10. Multiclass Classfication
date: 2018-08-10T02:00:10+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - cost function
  - gradient descent
  - hypothesis function
  - machine learning
  - multiclass classification
  - 가설함수
  - 경사하강법
  - 머신러닝
  - 비용함수
---
# 1. MultiClass Classification

* * *

강의 #7에서는 binary classfication 에 대해서 다루었다. class0이냐, class1이냐 두 가지 case밖에 없는 경우에 대해서 다루었다. 그러나 실제로 모든 데이터가 두 가지 클래스로 분류되느냐 하면 그렇지 않다. 예를들면 날씨 예측같은 경우 &#8216;맑음 / 흐림 / 비 / 눈&#8217; 처럼 4가지 경우로 분류할 수 있다. 혹은 사진 패턴인식을 이용한 숫자 분류 같은경우 &#8216;0 / 1 / 2 / 3 / 4 / 5 / 6 / 7 / 8 / 9&#8217;와 같이 총 10가지 case로 분류할 수도 있다. 따라서 binary classification을 확장한 multiclass classification 알고리즘에 대해서도 생각하지 않을 수 없다.

먼저 말하고자 하는것은 multiclass classification을 하기 위해서는 우리가 binary classification에서 다룬 내용만 알고 있으면 된다. 간단한 트릭이 들어갈 뿐 전혀 새로운 내용이 없다.

binary classification에 대해 다시 한 번 짚고 넘어가자.

&nbsp;

  1. hypothesis function을 세운다. 형태는 다음과 같다.

$${ h }_{ \theta }(x)=\frac { 1 }{ 1+{ e }^{ -\theta^{T} x } }$$ 

2. cost function을 계산한다. classfication의 cost function은 다음과 같다.

$$J(\theta )=-\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { y }^{ (i) }\log { ({ h }_{ \theta }({ x }^{ (i) }) } ) } +(1-{ y }^{ (i) })\log { (1-{ h }_{ \theta }({ x }^{ (i) })) }$$ 

3. training dataset을 가지고 parameter(θ)를 학습시킨다. 이 때에는 gradient descent나 octave에서 지원하는 함수를 사용할 수 있다. gradient descent를 사용할 경우 update식은 다음과 같다.

$$\theta :=\theta -\alpha \nabla J(\theta )=\theta -\frac { \alpha }{ m } \cdot { X }^{ T }\cdot (g(X\cdot \theta )-\overrightarrow { y } )$$ 

&nbsp;

결국 위의 과정을 거치고 나면 우리가 가진 training dataset에 가장 최적화된 parameter(θ)가 구해진다. 이 model을 가지고 분류 작업을 할 수 있다.

multiclass classfication은 위의 과정을 여러번 반복하면 될 뿐이다.

가령 날씨가 맑음 / 흐림 / 비 / 눈 중 한가지를 예측하는 알고리즘을 만들고 싶다.

처음으로 맑음 = class1 , 흐림 / 비 / 눈 = class0으로 나눈다. 그리고 binary classification 학습 알고리즘을 적용하여 model(hypothesis function)을 구한다. 이 때 구한 hypothesis function을 h1\_θ(x)라 하자. 그리고 또다시 흐림 = class1, 맑음 / 비 / 눈 = class0으로 취급하여 동일한 학습을 진행한다. 이 때 구한 hypothesis function을 h2\_θ(x)라 하자. 이렇게 하면 총 4개의 model이 구해질 것이다. 결론은 이렇다.

h1_θ(x) : 날씨가 맑을 확률

h2_θ(x) : 날씨가 흐릴 확률

h3_θ(x) : 비가 올 확률

h4_θ(x) : 눈이 올 확률

이제 어떠한 data를 넣어서 예측을 하면 h1\_θ(x) ~ h4\_θ(x) 4가지 model의 output이 나오는데, 이 중 가장 높은 값으로 분류되는 것이다.

조금 더 일반적으로 정의해보겠다. 총 n가지 경우로 분류하는 알고리즘의 경우 regression을 총 n번 진행하여 n개의 hypothesis function을 구한다.

&nbsp;

$$y\in \{ 1, 2, ..., n\} \\ { h }_{ \theta }^{ (1) }(x)=P(y=1|\theta ;x)\\ { h }_{ \theta }^{ (2) }(x)=P(y=2|\theta ;x)\\ { h }_{ \theta }^{ (3) }(x)=P(y=3|\theta ;x)\\ \qquad \qquad \cdot \\ \qquad \qquad \cdot \\ \qquad \qquad \cdot \\ { h }_{ \theta }^{ (n) }(x)=P(y=n|\theta ;x)$$ 

각각의 hypothesis function의 output이 의미하는 것은 해당 class로 분류될 &#8216;확률&#8217;이다. 이 중 가장 높은 확률을 가지는 class로 분류된다.