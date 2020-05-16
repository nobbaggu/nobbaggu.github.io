---
title: (머신러닝) 7 - Logistic Regression
date: 2018-08-09T17:11:03+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - classification
  - cost function
  - gradient descent
  - hypothesis function
  - logistic regression
  - machine learning
  - sigmoid function
  - sigmoid함수
  - 경사하강법
  - 머신러닝
  - 분류
  - 비용함수
  - 시그모이드 함수
---
# 1. Logistic Regression

* * *

Logistic regression 은 discrete한 여러개의 값으로 분류를 하는 것이 목적일 때 사용한다.

지금 이야기할 classification(분류) 문제는 데이터를 분류하는 것이 주 목적이다. 예를들어 사진이 입력되었을 때 사람이냐 아니냐의 문제, 혹은 이메일 스팸 필터링이 classfication 문제에 해당한다.

사진을 사람 or Not으로 분류하는 문제를 생각해보자. 이 문제를 linear regression을 통해서 사람인 사진은 0보다 큰 값을 내어놓도록 학습시키고 아닌 사진은 0보다 작은 값을 내어놓도록 학습시켜서 model을 사용할 수 있지 않을까 생각해볼 수도 있지다. 하지만 이런 경우에는 일반적으로 output이 feature의 값과 같이 linear하게 움직이지 않는다. 분류문제에 linear regression을 적용하는 예시를 한 번 보자.

![image](https://nobbaggu.github.io/images/2018/08/no-name-5.png){: width="50%" height="50%"}

위 프로그램은 종양의 크기에 따라 0(양성종양) 혹은 1(악성종양)으로 분류하는 알고리즘이다. 빨간색으로 표시된 데이터들로부터 만들어진 모델이 검은색 직선이다. 이 모델로 파란색 동그라미 데이터의 결과를 예측해보자. 저 크기라면 악성(1)로 분류되어야 할 것 같지만 모델에 따르면 양성에 더 가깝다. 그리고 더욱 큰 종양의 데이터들이 많이 들어갈수록 이러한 잘못된 분류는 더 많이 일어날 것이다. 따라서 위와같은 분류문제에 linear regression 알고리즘을 적용하는것은 적절치 않다.

일단 이러한 classification 문제를 학습시키는 방법에 대해서 가장 간단하게 &#8216;두 가지&#8217;로 분류하는 binary classification 문제에 대해 설명할것이다. 두 가지보다 많은 경우로 확장하는 것은 매우 간단하기 때문이다.

먼저 binary classification 문제에서 training data의 output이나 model의 output은 &#8216;0&#8217;과 &#8216;1&#8217; 두 가지의 이산적인(discrete) 값 만을 가질 수 있다.

$$y\in \left \{ 0,1 \right \}$$ 

여기서 두 개의 클래스를 각각 &#8216;class 0&#8217;과 &#8216;class 1&#8217;이라 부르겠다.

&nbsp;

&nbsp;

&nbsp;

# 2. hypothesis function

* * *

0과 1의 두 개의 값만을 가지는 모델을 수식적으로 어떻게 나타내야할지 생각해보아야한다. Logistic Regression 을 이용한  분류 문제에서는 모든 output을 (0,1)의 범위로 제한하기위해 sigmoid function을 사용한다.

![image](https://nobbaggu.github.io/images/2018/08/no-name-6.png){: width="50%" height="50%"}

sigmoid function은 0보다 작은 구간에서 (0, 0.5)의 범위를, 0보다 큰 구간에서는 (0.5, 1)의 범위를 가진다. 그렇다면 sigmoid function을 이용한 모델의 형태는 다음과 같다.

$${ h }_{ \theta }(x)=g(\theta^{T} x)=\frac { 1 }{ 1+{ e }^{ -\theta x } }$$ 

이 model은 분명 모든 output을 (0, 1)의 범위로 제한한다. 우리는 이 model의 output이 0.5보다 작으면 class 0으로 분류하고 0.5보다 크거나 같으면 class 1로 분류할 것이다. 그렇다는것은 θ&#8217;x 의 값이 0보다 크거나 같으면 class1, 0보다 작으면 class0으로 분류한다는 말과 같다.

$${ (h }_{ \theta }(x)\ge 0.5\Leftrightarrow \theta x\ge 0)\quad \rightarrow \quad y=1$$ 

$${ (h }_{ \theta }(x)<0.5\Leftrightarrow \theta x<0)\quad \rightarrow \quad y=0$$ 

&nbsp;

사실 sigmoid function이 &#8216;0&#8217; 혹은 &#8216;1&#8217;의 두 가지 값을 딱 내어놓는것은 아니다. 단지 출력의 범위를 (0,1)로 제한할 뿐이다. 그렇다면 모델의 output이 가지는 (0,1)사이의 값을 어떻게 해석해야 할까? 여기서 h_theta(x)의 값은 모델의 예측값이 1을 가질 &#8216;확률&#8217;이다. 만약 0.9를 내놓는다면 &#8216;class 1&#8217;일 확률이 90%이고 &#8216;class 0&#8217;일 확률은 1-0.9 = 10%이다.

$${ h }_{ \theta }(x)=P(y=1|x,\theta )=1-P(y=0|x,\theta )$$ 

&nbsp;

<span style="font-size: 18pt;"><strong> </strong></span>

&nbsp;

# 3. cost function

* * *

cost function의 역할은 linear regression에 있어서나 classification에 있어서나 동일하다. 모델이 training dataset을 기준으로 얼마만큼의 error를 가지는지 나타내주는 지표이다. 하지만 linear regression의 training dataset들은 output이 연속적인 함수였다. 하지만 classification 문제에서는 model은 (0, 1)의 범위에 존재하는 값을, training dataset의 output은 0 혹은 1의 값만을 가진다. lineare regression과 같이 모든 dataset과 model사이의 squared error를 구하고 더한다면 이것은 theta에 대해서 매우 불규칙적이고 울룩불룩한 모양을 나타낼 가능성이 크다. 매우 많은 수의 local minimum을 가지고 있다는 뜻이다. 따라서 우리가 cost function의 크기를 줄이기 위해 사용했던 강력한 수학적 도구인 gradient descent도 무용지물이 되어버린다. 따라서 Logistic  Regression 의 cost function은 다음의 형태를 가진다.

$$J(\theta )=\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ Cost({ h }_{ \theta }({ x }^{ (i) }), } { y }^{ (i) })\\ Cost({ h }_{ \theta }({ x }^{ (i) }),{ y }^{ (i) })=-\log { ({ h }_{ \theta }({ x }^{ (i) }) } )\quad if\quad y=1\\ Cost({ h }_{ \theta }({ x }^{ (i) }),{ y }^{ (i) })=-\log { (1-{ h }_{ \theta }({ x }^{ (i) }) } )\quad if\quad y=0$$ 

&nbsp;

위의 그래프에서 x 대신에 h_theta(x)가 들어갈 뿐이다. 그럼 위와 같은 형태의 cost function이 정말로 training dataset과 model의 error를 충분히 잘 나타내주는지 보자.

![image](https://nobbaggu.github.io/images/2018/08/no-name-7.png){: width="50%" height="50%"}

training data의 output이 1인 경우는 model의 예측값이 1로 갈수록 error는 0으로 수렴하고 예측값이 0에 가까워질수록 error는 무한히 커진다.

training data의 output이 0인 경우는 model의 예측값이 1로 갈수록 error는 무한히 커지고 예측값이 1에 가까워질수록 error는 0으로 수렴한다.

정리하자면, 위의 cost function에 따르면 예측값과 training data의 output의 차이가 작을수록 cost function이 작아지고 차이가 클수록 cost function이 커진다. cost function 원래의 역할을 충실히 잘 따르는 함수라는 결론이 나온다.

y=0인 경우와 y=1인 경우를 구분하기 위해서 위와 같이 cost function을 나누었지만 두 식을 하나로 합칠 수 있다.

$$J(\theta )=-\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { y }^{ (i) }\log { ({ h }_{ \theta }({ x }^{ (i) }) } ) } +(1-{ y }^{ (i) })\log { (1-{ h }_{ \theta }({ x }^{ (i) })) }$$ 

이 경우 y=1인 경우는 두 번째 항이 삭제되고 y=0인 경우는 첫 번째 항이 삭제되어 결국은 이전에 세웠던 cost function식과 동일하게 된다.

그리고 이번 경우 역시 linear regression과 마찬가지로 선형대수학으로 더 간단한 연산이 가능하다.

$$h=g(X\theta )$$ 

$$J(\theta )=-\frac { 1 }{ m } { (y }^{ T }log(h)+{ (1-y) }^{ T }log(1-h))$$ 

&nbsp;

&nbsp;

<span style="font-size: 18pt;"><strong> </strong></span>

# 4. gradient descent

* * *

이 경우도 역시 cost function의 gradient vector를 구해서 cost function이 감소하는 방향으로 learning rate만큼의 step씩 theta를 변화시키는 gradient descent의 방법을 사용해서 training dataset에 가장 fit하는 model함수를 구할 수 있다.

먼저 cost function의 미분을 구해야 한다. cost function을 미분하는 과정은 지루하고 긴 풀이과정인데 반해 결과는 의외로 아주 간단하게 나온다. j-th parameter에 대한 cost function의 미분은 다음과 같다.

$$\frac { \partial }{ \partial { \theta }_{ j } } J(\theta )=\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { [h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }]{ x }_{ j }^{ (i) } }$$ 

이 역시 선형대수학을 사용하여 연산하기 위해 수식을 matrix-vector형태로 간결하게 나타내보겠다.

$$\nabla J(\theta )=\frac { 1 }{ m } \cdot { X }^{ T }\cdot (g(X\cdot \theta )-\overrightarrow { y } )$$ 

직접 행렬에 원소를 채워넣고 계산해보면 위의 두 식이 같다는 것을 알 수 있다. 이제 얼마만큼의 learning rate로 학습을 시킨지만 결정하면 gradient descent를 사용해서 theta값들을 update해가며 최적의 theta를 찾아 모델을 설정할 수 있다.

gradient descent의 일반적인 형식은 linear regression이나 classification에서나 동일하다.

$${ \theta }_{ j }:={ \theta }_{ j }-\alpha \frac { \partial }{ \partial { \theta }_{ j } } J(\theta )={ \theta }_{ j }-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { [h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }]{ x }_{ j }^{ (i) } }$$ 

vectorized form으로 나타내면 다음과 같다.

$$\theta :=\theta -\alpha \nabla J(\theta )=\theta -\frac { \alpha }{ m } \cdot { X }^{ T }\cdot (g(X\cdot \theta )-\overrightarrow { y } )$$ 

결국 classification은 문제의 특성상 linear regression과 다른형태의 hypothesis function을 사용한다는 것, 그리고 training data와 model간의 오차가 얼마나 큰지 정량적으로 나타내기 위해 다른 형태의 cost function을 사용한다는 사실 이외에 linear regression과 다를것이 없다.

마지막으로 이러한 classfication의 model에 대해 좀 더 직관적으로 파악하기 위해 그림을 그려서 어떻게 알고리즘이 동작하는지를 보아야겠다.

gradient descent로 얻은 어떠한 binary classification 알고리즘이 다음과 같은 모델을 가진다고 해보자.

$$\theta=\begin{bmatrix} \theta_{0} \\ \theta_{1} \\ \theta_{2} \end{bmatrix} = \begin{bmatrix} -12 \\ 4 \\ 3 \end{bmatrix} \Rightarrow \quad { h }_{ \theta }(x)=g(-12+4{ x }_{ 1 }+3{ x }_{ 2 })\quad (\because g(z)=\frac { 1 }{ 1+{ e }^{ -z } } )$$ 

위 모델의 output이 0.5보다크거나 같으면 class 1, 0.5보다 작으면 class 0으로 분류된다.

$${ h }_{ \theta }(x)=g(-12+4{ x }_{ 1 }+3{ x }_{ 2 })\ge 0.5\Leftrightarrow -12+4{ x }_{ 1 }+3{ x }_{ 2 }\ge 0:\quad class1$$ 

$${ h }_{ \theta }(x)=g(-12+4{ x }_{ 1 }+3{ x }_{ 2 })<0.5\Leftrightarrow -12+4{ x }_{ 1 }+3{ x }_{ 2 }<0:\quad class0$$ 

각 식의 마지막 부분에있는 부등식을 풀어보면 x1, x2평면에서 어떠한 직선을 기준으로 class0과 class1로 나뉜다는것을 알 수 있다. 이를 그림으로 나타내면 다음과 같다.

![image](https://nobbaggu.github.io/images/2018/08/no-name-8.png){: width="50%" height="50%"}

또 한 가지 예를 보자. 아래 그림과 같은 training dataset이 있다.

![image](https://nobbaggu.github.io/images/2018/08/no-name-9.png){: width="50%" height="50%"}

이러한 data들에 fit하는 model을 구하기위해 hypothesis function의 형태를 세울려고 하는데 단순한 선형식으로는 되지 않을 것 같다. 이 때에는 더 높은 차원의 polynomial term을 추가하는 것이 좋겠다.

$${ h }_{ \theta }(x)=g({ \theta }_{ 0 }+{ \theta }_{ 1 }{ x }_{ 1 }+{ \theta }_{ 2 }{ x }_{ 2 }+{ \theta }_{ 3 }{ { x }_{ 1 } }^{ 2 }+{ \theta }_{ 4 }{ { x }_{ 2 } }^{ 2 })\quad (\because { x }_{ 3 }={ { x }_{ 1 } }^{ 2 },\quad { x }_{ 4 }={ { x }_{ 2 } }^{ 2 })$$ 

gradient descent를 통해서 다음과 같은 parameter를 구했다.

$$\theta = \begin{bmatrix}-4 \\ 0 \\ 0 \\ 1 \\ 1 \end{bmatrix}$$ 

$$g(-4+{ { x }_{ 1 } }^{ 2 }+{ { x }_{ 2 } }^{ 2 })\ge 0.5\quad \Leftrightarrow \quad -4+{ { x }_{ 1 } }^{ 2 }+{ { x }_{ 2 } }^{ 2 }\ge 0\quad :\quad class1$$ 

$$g(-4+{ { x }_{ 1 } }^{ 2 }+{ { x }_{ 2 } }^{ 2 })<0.5\quad \Leftrightarrow \quad -4+{ { x }_{ 1 } }^{ 2 }+{ { x }_{ 2 } }^{ 2 }<0\quad :\quad class0$$ 

위의 parameter를 hypothesis function에 적용시키고 결과가 0.5보다 크거나 같으면 class1, 0.5보다 작으면 class0이 된다. 이 때 class0과 class1의 경계는 위의 부등식을 풀어서 구할 수 있다. 위의 부등식을 보았을 때 경계선은 중심이 0이고 반지름이 2인 원이 된다는 것을 고등학교 수학시간에 배운 원의 방정식만 알고있다면 쉽게 알 수 있다. 이를 그림으로 표현하면 다음과 같다.

![image](https://nobbaggu.github.io/images/2018/08/no-name-10.png){: width="50%" height="50%"}