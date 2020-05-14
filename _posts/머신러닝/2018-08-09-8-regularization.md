---
title: (머신러닝) 8. Regularization
date: 2018-08-09T20:00:14+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - cost function
  - gradient descent
  - hypothesis function
  - machine learning
  - overfitting
  - regularization
  - underfitting
  - 가설함수
  - 경사하강법
  - 머신러닝
  - 비용함수
---
# 1. Model의 Bias

* * *

결론부터 말하자면 Regularization 은 model의 high variance 를 방지하기 위한 목적으로 쓰인다.

hypothesis function을 너무 단순한 형태로 만들거나 feature의 갯수가 너무 적을 때에는 model이 training dataset에서 너무 벗어나있는 underfitting문제가 일어난다. 이를 다른말로 high bias 되었다고 한다.

이와는 반대로 hypothesis function을 모든 training data 하나하나에 억지스럽게 맞추려고 과하게 많은 feature를 사용한다면 overfit되어서 training data에는 엄청나게 잘 맞아떨어지지만 오히려 새로운 data의 output을 잘 예측하지 못하게 된다. 즉, 일반화(generalization)되지 못하여 예측 알고리즘의 질이 엄청나게 떨어지게 된다. 이런 경우를 high variance(과하게 변동적임) 되었다고 한다.

![image](https://nobbaggu.github.io/images/2018/08/no-name-11.png){: width="50%" height="50%"}

이와 같은 상황은 예측, 분류 등의 데이터 마이닝에서 자주 발생하는 중요한 문제다. underfit문제는 data의 trend를 파악하여 적절한 형태의 모델함수를 사용하고 고려해야할 feature를 충분히 수식에 집어넣어서 해결할 수 있고 이는 이미 우리가 알고있는 내용에 포함되어있다.

&nbsp;

오늘은 overfit문제를 해결하는 대표적인 방법인 regularization에 대해 설명하려 한다.

&nbsp;

먼저 model의 hypothesis function이 다음과 같다고 해보자.

$${ h }_{ \theta }(x)={ \theta }_{ 0 }+{ \theta }_{ 1 }x+{ \theta }_{ 2 }{ x }^{ 2 }+{ \theta }_{ 3 }{ x }^{ 3 }+{ \theta }_{ 4 }{ x }^{ 4 }$$ 

위의 식을 우리는 좀 더 quadratic하게(2차식 스럽게)만들어 굴곡이 더 심한 3차, 4차식보다는 완만하게 만들고싶다. 이러한 경우 (theta\_3)(x^3)과 (theta\_4)(x^4)의 두 term의 영향력을 인위적으로 작게 해 주는 방법이 있다.

cost function을 다음과 같이 세우는 것이다.

$$J(\theta )=\frac { 1 }{ 2m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) } })^{ 2 } } +1000{ \cdot { { \theta }_{ 3 } }^{ 2 } }+1000{ { \cdot \theta }_{ 4 } }^{ 2 }$$ 

이렇게 되면 cost function을 최대한 줄이며 theta를 설정하는 과정에서 theta\_3와 theta\_4의 크기가 자연스레 작아질 것이다. 그리고 이는 곧 hypothesis function에서 (theta\_3)(x^3)과 (theta\_4)(x^4) 두 항의 크기, 즉 영향력이 작아지는 것을 의미한다. 이것이 regularization의 기본 idea이다. 함수를 최소화 시키는 과정에서 그 함수의 parameter은 작아진다. 이와 같은 원리를 이용하는 것이 regularization이다.

&nbsp;

&nbsp;

# <span style="font-size: 18pt;"><b> </b></span>

# 2. Cost Function

* * *

regularization이 포함되어있는 일반적인 cost function의 형태는 다음과 같다.

$$linear\quad regression \\ \\ J(\theta )=\frac { 1 }{ 2m } [\sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) } })^{ 2 } } +\lambda \sum _{ j=1 }^{ n }{ { { \theta }_{ j } }^{ 2 } } ]$$ 

$$classification \\ \\ J(\theta )=-\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { y }^{ (i) }\log { ({ h }_{ \theta }({ x }^{ (i) }) } ) } +(1-{ y }^{ (i) })\log { (1-{ h }_{ \theta }({ x }^{ (i) })) } +\frac { \lambda }{ 2m } \sum _{ j=1 }^{ n }{ { { \theta }_{ j } }^{ 2 } }$$ 

λ를 regularization parameter라고 한다. 위의 식에서는 θ_0을 제외한 모든 θ(parameter)들이 λ만큼의 가중치를 받으며 식에 추가되었다. 목적은 cost를 최소화 시키는 것이다. 이 과정에서 parameter들 자체의 값이 작아지게 된다. 그리고 parameter값들이 작으면 model의 곡선은 더 단순한 형태를 가지게 되어있다. 즉, overfitting이 일어나는 것을 방지할 수 있게된다.

![image](https://nobbaggu.github.io/images/2018/08/no-name-12.png){: width="50%" height="50%"}

위 그림의 초록색 곡선은 regularization parameter가 추가된 model을 나타낸다. 이는 기존의 overfit이 일어나는 model보다 theta값들이 작아져 더욱 완만한 곡선을 나타내게 되어 overfit을 막아준다. 하지만 regularization λ를 너무 크게 잡으면 반대로  underfit이 일어난다.

&nbsp;

&nbsp;

&nbsp;

# 3. gradient  descent

* * *

gradient descent는 크게 달라지지 않는다. 단순히 regularization term의 미분만 추가될 뿐이다.

&nbsp;

$$linear\quad regression>> \\ \\ { \theta }_{ j }:={ \theta }_{ j }-\alpha (\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }_{ 0 }^{ (i) })-{ y }^{ (i) }) } } { x }_{ j }^{ (i) }+\frac { \lambda }{ m } { \theta }_{ j })={ \theta }_{ j }(1-\alpha \frac { \lambda }{ m } )-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }_{ 0 }^{ (i) })-{ y }^{ (i) }) } } { x }_{ j }^{ (i) }$$ 

$$classfication>> \\ \\ { \theta }_{ j }:={ \theta }_{ j }-\alpha (\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { [h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }]{ x }_{ j }^{ (i) } } +\frac { \lambda }{ m } { \theta }_{ j })={ \theta }_{ j }(1-\alpha \frac { \lambda }{ m } )-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { (h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }){ x }_{ j }^{ (i) } }$$ 

&nbsp;

위의 식에서 linear regression과 classification의 식은 완전히 동일하지만 h_theta(x)가 다른 형태의 함수라는 것을 잊으면 안된다.

vectorized form은 다음과 같다.

&nbsp;

$$linear\quad regression>> \\ \\ \theta :=\theta (1-\alpha \frac { \lambda }{ m } )-\frac { \alpha }{ m } { X }^{ T }(X\theta -y)$$ 

&nbsp;

$$classfication>> \\ \\ \theta \quad :=\theta (1-\alpha \frac { \lambda }{ m } )-\frac { \alpha }{ m } \cdot { X }^{ T }\cdot (g(X\cdot \theta )-\overrightarrow { y } )$$ 

&nbsp;

위의 update 식을 보면 θ에 1-α(λ/m)이라는, 1에서 얼마만큼을 뺀 1보다 작은 값을 가진 term이 곱해진다. 그리고 뒤의 나머지 부분은 원래의 update식과 완전히 동일하다. 이로써 regularization이 포함된 update식은 결과적으로 θ가 shrink(줄어들다)하는 효과를 주는것을 다시 한 번 확인할 수 있다.

아무튼간에 적절한 λ값만 설정한다면 위의 update식을 사용하여 model이 더욱 완만한 곡선을 가지도록 유도하여 overfit이 일어나는 것을 방지할 수 있다.

&nbsp;