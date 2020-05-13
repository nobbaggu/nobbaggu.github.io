---
title: (머신러닝) 21. Stochastic Gradient Descent
date: 2018-09-11T22:43:27+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - batch
  - mini batch
  - mini batch gradient
  - stochastic
  - stochastic gradient descent
  - 확률적 경사하강법
---
Stochastic Gradient Descent 는 large dataset을 학습시킬 때 쓸 수 있는 방법이다.

학습 데이터가 100,000,000개 있다고 해보자.

$$\dpi{120} \\ J_{train}(\theta) = \frac{1}{2m}\sum_{i=1}^{m}[h_{\theta}(x^{(i)})-y^{(i)}]^{2}$$ 

cost function이 위와 같을 때 $$\dpi{120} \theta$$의 update식은 다음과 같다.

$$\\for\quad j=1:n \\ \left \{ { \theta }_{ j }:={ \theta }_{ j }-\alpha \frac { \partial J }{ \partial { \theta }_{ j } } ={ \theta }_{ j }-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ j }^{ (i) } \right \}$$ 

m = 100,000,000에 대한 sum을 계산해야 한다. 심지어 이것이 한 번의 step이다. 이 작업을 cost function이 수렴할 때 까지 하려면 메모리의 부담이 너무 크고 작업속도도 엄청 느릴 것이다.

이 때 우리가 시도할 수 있는 방법 중 하나인 stochastic gradient descent이다.

&nbsp;

# 1. Stochastic Gradient Descent

* * *

우리가 지금까지 알던 Gradient Descent 는 다른 말로 Batch Gradient Descent 라고도 부른다. Batch는 여기서 데이터전체 다발을 의미한다. 이 Batch Gradient Descent 에서의 cost function과 update식을 다시 한 번 짚고 넘어가보자.

$$\dpi{120} \\ J_{train}(\theta) = \frac{1}{2m}\sum_{i=1}^{m}[h_{\theta}(x^{(i)})-y^{(i)}]^{2}$$ 

$$\\for\quad j=1:n \\ \left \{ { \theta }_{ j }:={ \theta }_{ j }-\alpha \frac { \partial J }{ \partial { \theta }_{ j } } ={ \theta }_{ j }-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ j }^{ (i) } \right \}$$ 

&nbsp;

Stochastic Gradient Descent 는$$\dpi{120} \theta$$  update 식에서 전체 data batch의 cost function의 미분의 평균이 아닌 data 하나의 cost의 미분만 사용하는 것이다.

$$\\for\quad i=1:m \\ \left \{ for\quad j=1:n \left \{ { \theta }_{ j }: ={ \theta }_{ j }-\frac { \alpha }{ m } { { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ j }^{ (i) } \right \} \right \}$$ 

n개의 $$\dpi{120} \theta$$를 data 하나를 이용해서 update한다. 이 과정을 m개의 data에 대해서 update 하는 것이다.

Batch Gradient Descent 는 m개의 dataset을 사용하여 평균을 내어 한 번 update를 한다.

Stochastic Gradient Descent는 각각의 data마다 cost를 미분하여 update를 m번 한다.

직관적으로 global minimum으로의 방향의 정확도는 BGD가, speed는 SGD가 더 높을 것이라고 생각된다.

![image](/images/2018/09/no-name-5.png){: width="50%" height="50%"}

Stochastic Gradient Descent의 특징은 Global minimum으로 수렴하는 것이 아니라 주변을 맴돈다. 아무래도 하나의 데이터만 이용해서 update하다보니 움직임이 가볍고 정확한 global minimum으로 향하지는 못한다. 하지만 learning rate $$\dpi{120} \alpha$$를 작게 할 수록 수렴하는 것에 가깝게는 만들 수 있다.

Stochastic Gradient Descent 알고리즘은 m개의 data에 대해 parameter를 update하는 이 행위를 보통 1번이면 위의 그림과 같이 수렴한다. Batch Gradient Descent 알고리즘은 m개의 data에 대한 cost function의 미분의 평균을 한 번 update 한 것이 위의 화살표 한 번의 step이다. Stochastic Gradient Descent 는 Batch Gradient Descent 보다 학습 속도가 빠르고 우리가 large dataset에 대해 Stochastic Gradient Descent 알고리즘을 사용하는 이유이다.

&nbsp;

&nbsp;

# 2. Mini-Batch Gradient Descent

* * *

Mini-Batch Gradient Descent도 Stochastic Gradient Descent 와 마찬가지로 컴퓨터 메모리의 계산부담을 줄이고 학습속도를 향상시키기 위한 알고리즘으로, 위에서 말한 Stochastic Gradient Descent와 원리가 같다. 다만 parameter를 update 하기위해 data를 1개는 아니고 가령 10개, 혹은 b(<m)개를 사용하는 것이다. b는 batch size이다. 가령 10개의 data를 사용한다고 하면 알고리즘은 다음과 같다.

&nbsp;

$$\\for \quad k=1,11,21,...\\ \left \{ for\quad j=1:n \\ \left \{ { \theta }_{ j }:={ \theta }_{ j }-\frac { \alpha }{ m } \sum _{ i=k}^{ k+9 }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ j }^{ (i) } \right \} \right \}$$ 

이 알고리즘은 stochastic gradient descent보다도 학습속도에서 더 좋은 성능을 낼 수도 있다.