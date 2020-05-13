---
title: (머신러닝) 3. multivariate linear regression
date: 2018-08-09T01:00:40+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - AI
  - cost function
  - feature normalization
  - feature scailing
  - gradient descent
  - hypothesis function
  - learning rate
  - linear regression
  - machine learning
  - 가설함수
  - 경사하강법
  - 머신러닝
  - 비용함수
  - 선형회귀
  - 인공지능
  - 학습속도
  - 학습율
---
<span style="font-family: arial, helvetica, sans-serif;">오늘 설명한 내용은 실제로 input 변수가 2개 이상인 경우에서 Linear Regression 문제의 hypothesis function을 세우고 cost function이 가장 작아지는 방향으로 hypothesis function의 parameter를 조정할것이다. 이 때 쓰는 가장 대표적인 방법이 gradient descent (경사 하강법)이다.</span>

&nbsp;

# <span style="font-size: 18pt; font-family: arial, helvetica, sans-serif;"><strong><hypothesis function></strong></span>

* * *

<span style="font-family: arial, helvetica, sans-serif;">$$h_{ { \theta } }(x)= { \theta }_{ 0 } + { \theta }_{ 1 }x_{ 1 } + { \theta }_{ 2 }x_{ 2 }+ { \theta }_{ 3 }x_{ 3 } + \cdot \cdot \cdot + { \theta }_{ n }x_{ n }$$</span>

<span style="font-family: arial, helvetica, sans-serif;">위는 output을 결정짓는 input이 n개의 변수로 이루어진 데이터들을 학습시켜 만든 model 이다.</span>

<span style="font-family: arial, helvetica, sans-serif;">Multiple Variables가 있는 function은 matrix나 vector를 사용하여 선형대수로 취급하는 것이 훨씬 간편하다. 따라서 몇가지 정의를 하겠다.</span>

$$\theta \quad = \begin{bmatrix} { \theta }_{ 0 }\\ { \theta }_{ 1 }\\ { \theta }_{ 2 }\\ \cdot \\ \cdot \\ \cdot \\ { \theta }_{ n } \end{bmatrix} \quad x\quad = { \begin{bmatrix} { x }_{ 0 }\\ { x }_{ 1 }\\ { x }_{ 2 }\\ \cdot \\ \cdot \\ \cdot \\ { x }_{ n } \end{bmatrix} }\quad ({ x }_{ 0 } = 1)\\$$ 

<span style="font-family: arial, helvetica, sans-serif;">$${ h }_{ \theta }(x)\quad =\quad { \theta }^{ T }\cdot x\quad =\quad \begin{bmatrix} { \theta }_{ 0 }\quad { \theta }_{ 1 }\quad { \theta }_{ 2 }\quad \cdot \quad \cdot \quad \cdot \quad { \theta }_{ n } \end{bmatrix} \begin{bmatrix} { x }_{ 0 }\\ { x }_{ 1 }\\ { x }_{ 2 }\\ .\\ \cdot \\ { x }_{ n } \end{bmatrix}$$</span>

<span style="font-family: arial, helvetica, sans-serif;">θ를 hypothesis function을 결정짓는 <strong>&#8216;parameter&#8217;</strong>라 부르고 데이터의 input인 x를  <strong>&#8216;feature&#8217;</strong>라 부른다. 그리고 feature는 사실 n개인데 편의성을 위해 parameter의 bias term $$\theta_{0}$$와 matching되는 $$x_{0}$$를 추가하여 parameter vector와 feature vector의 element 갯수를 맞추었다. 단 $$x_{0}$$은 항상 값이 1로 고정이다.</span>

&nbsp;

&nbsp;

# <span style="font-size: 18pt; font-family: arial, helvetica, sans-serif;"><strong><cost function></strong></span>

* * *

<span style="font-family: arial, helvetica, sans-serif;">$$J({ \theta }) = \frac { 1 }{ 2m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) }^{ 2 } } = \frac { 1 }{ 2m } \sum _{ i=1 }^{ m }{ { ({ \theta }_{ 0 } + { \theta }_{ 1 }{ x }_{ 1 }^{ (i) } + { \theta }_{ 2 }{ x }_{ 2 }^{ (i) } +{ \theta }_{ 3 }{ x }_{ 3 }^{ (i) } + \cdot \cdot \cdot + { \theta }_{ n }{ x }_{ n }^{ (i) } - { y }^{ (i) }) }^{ 2 } }$$</span>

<span style="font-family: arial, helvetica, sans-serif;">supscript (i)는 i번째 training data를 뜻한다.</span>

<span style="font-family: arial, helvetica, sans-serif;"><strong>cost function은 (hypothesis function의 결과) &#8211; (실제 데이터의 값)인 error를 제곱하여 더한 값이다.</strong> 이는 model이 실제 training data를 얼마나 잘 예측하는지 나타낸다. <strong>cost function을 최소화 시키는 parameter(θ)를 결정하는 것이 여기서 할 일이다.</strong></span>

<span style="font-family: arial, helvetica, sans-serif;">일단 그 전에 cost function의 식을 matrix notation으로 간결하게 표현해보자. 그 이전에 먼저 training data set을 하나의 matrix에 표현하여 X(대문자)라고 표현할 것이다.</span>

<span style="font-family: arial, helvetica, sans-serif;">    $$X\quad = \begin{bmatrix} { x }_{ 0 }^{ (1) } & { x }_{ 1 }^{ (1) }& { x }_{ 2 }^{ (1) }& { x }_{ 3 }^{ (1) }& \cdot & \cdot & \cdot & { x }_{ n }^{ (1) } \\ { x }_{ 0 }^{ (2) }& { x }_{ 1 }^{ (2) }& { x }_{ 2 }^{ (2) }& { x }_{ 3 }^{ (2) }& \cdot & \cdot & \cdot & { x }_{ n }^{ (2) } \\ { x }_{ 0 }^{ (3) }& { x }_{ 1 }^{ (3) }& { x }_{ 2 }^{ (3) }& { x }_{ 3 }^{ (3) }& \cdot & \cdot & \cdot & { x }_{ n }^{ (3) } \\ \cdot \\ \cdot \\ { x }_{ 0 }^{ (m) } & { x }_{ 1 }^{ (m) } & { x }_{ 2 }^{ (m) } & { x }_{ 3 }^{ (m) } & \cdot & \cdot & \cdot & { x }_{ n }^{ (m) } \end{bmatrix}$$</span>

<span style="font-family: arial, helvetica, sans-serif;">$$X{\theta}-y=\begin{bmatrix} { x }_{ 0 }^{ (1) }{ \theta }_{ 0 } + { x }_{ 1 }^{ (1) }{ \theta }_{ 1 } + { x }_{ 2 }^{ (1) }{ \theta }_{ 2 } + \cdot \cdot \cdot + { x }_{ n }^{ (1) }{ \theta }_{ n } - { y }^{ (1) } \\ { x }_{ 0 }^{ (2) }{ \theta }_{ 0 }+{ x }_{ 1 }^{ (2) }{ \theta }_{ 1 }+{ x }_{ 2 }^{ (2) }{ \theta }_{ 2 }+\cdot \cdot \cdot +{ x }_{ n }^{ (2) }{ \theta }_{ n }-{ y }^{ (2) } \\ { x }_{ 0 }^{ (3) }{ \theta }_{ 0 }+{ x }_{ 1 }^{ (3) }{ \theta }_{ 1 }+{ x }_{ 2 }^{ (3) }{ \theta }_{ 2 }+\cdot \cdot \cdot +{ x }_{ n }^{ (3) }{ \theta }_{ n }-{ y }^{ (3) }\\ \cdot \\ \cdot \\ \cdot \\ { x }_{ 0 }^{ (m) }{ \theta }_{ 0 }+{ x }_{ 1 }^{ (m) }{ \theta }_{ 1 }+{ x }_{ 2 }^{ (m) }{ \theta }_{ 2 }+\cdot \cdot \cdot +{ x }_{ n }^{ (m) }{ \theta }_{ n }-{ y }^{ (m) } \end{bmatrix}$$ = $$\begin{bmatrix} { h }_{ \theta }({ x }^{ (1) })-{ y }^{ (1) }\\ { h }_{ \theta }({ x }^{ (2) })-{ y }^{ (2) }\\ { h }_{ \theta }({ x }^{ (3) })-{ y }^{ (3) }\\ \cdot \\ \cdot \\ \cdot \\ { h }_{ \theta }({ x }^{ (m) })-{ y }^{ (m) } \end{bmatrix}$$</span>

&nbsp;

<span style="font-family: arial, helvetica, sans-serif;">이 식을 제곱하면 다음과 같다.</span>

<span style="font-family: arial, helvetica, sans-serif;">$${ (X\theta -y) }^{ T }(X\theta - y) = \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) }^{ 2 } }$$</span>

&nbsp;

<span style="font-family: arial, helvetica, sans-serif;">최종적으로 cost function은 다음과 같이 표현된다.</span>

<span style="font-family: arial, helvetica, sans-serif;">$$J(\theta ) = \frac { 1 }{ 2m } { (X\theta - y) }^{ T }(X\theta - y)$$</span>

&nbsp;

&nbsp;

# <span style="font-size: 18pt; font-family: arial, helvetica, sans-serif;"><strong><Gradient Descent></strong></span>

* * *

<span style="font-family: arial, helvetica, sans-serif;">gradient descent 의 기본적인 idea는 Linear Regression with One Variable과 같다. 그래도 다시 한 번 설명하는것이 좋을것 같다.</span>

<span style="font-family: arial, helvetica, sans-serif;"><strong>Gradient Descent 라는 것은 어떠한 함수가 최소값을 가지게하는 point를 찾아가는 일종의 수학 테크닉</strong>이다. gradient descent 를 통해서 모델이 training data set에 가장 일치하는 최적의 $$[\theta_{0}\quad\theta_{1}\quad\theta_{2}\quad \cdot\cdot\cdot \quad \theta_{n}]$$을 구할 수 있다.</span>

<span style="font-family: arial, helvetica, sans-serif;">우리는 일단 무작위로 parameter 값들(θ&#8217;s)을 설정하고 조금씩 변화시켜보면서 cost function 최소값을 찾을 수 있지 않을까 하는 생각을 해본다. 이것이 기본적인 idea다. 하지만 값을 얼마만큼 어떻게 변화시킬것인가? 증가시키며? 감소시키며? 여기에 한 가지 idea가 추가되는데, 이는 벡터 미적분에서 배우는 함수의 gradient가 가지는 기본적인 성질중 한가지로부터 기인한다. cost function( J )는 θ의 함수이다. 그리고 이 함수의 gradient는 다음과 같다.</span>

<span style="font-family: arial, helvetica, sans-serif;">$$\triangledown J({ \theta })\quad =\quad (\frac { \partial J }{ \partial { \theta }_{ 0 } } ,\quad \frac { \partial J }{ \partial { \theta }_{ 1 } } ,\quad \frac { \partial J }{ \partial { \theta }_{ 2 } } ,\quad \cdot \quad \cdot \quad \cdot ,\quad \frac { \partial J }{ \partial { \theta }_{ n } } )$$</span>

&nbsp;

**<span style="font-family: arial, helvetica, sans-serif;">어떠한 함수의 gradient는 그 위치에서 함수가 가장 빠르게 증가하는 방향을 가르킨다.</span>**

<span style="font-family: arial, helvetica, sans-serif;">우리는 hypothesis function에서 일단 θ값을 무작위로 정한다. 그리고 cost function( J )의 gradient를 이용하여 J가 줄어드는 방향으로 θ를 변화시키면서 이동하여 새로운 θ를 얻으려 한다. 이 때 새로운 θ은 다음과 같은 형태가 된다.</span>

<span style="font-family: arial, helvetica, sans-serif;">$$({ \theta }_{ 0 },{ \theta }_{ 1 },{ \theta }_{ 2 },\cdot \cdot \cdot ,{ \theta }_{ n }):=({ \theta }_{ 0 },{ \theta }_{ 1 },{ \theta }_{ 2 },\cdot \cdot \cdot ,{ \theta }_{ n })-\alpha \triangledown J({ \theta }_{ 0 },{ \theta }_{ 1 },{ \theta }_{ 2 },\cdot \cdot \cdot ,{ \theta }_{ n }) \\ \\ = ({ \theta }_{ 0 }-\alpha \frac { \partial J }{ \partial { \theta }_{ 0 } } ,{ \theta }_{ 1 }-\alpha \frac { \partial J }{ \partial { \theta }_{ 1 } } ,{ \theta }_{ 2 }-\alpha \frac { \partial J }{ \partial { \theta }_{ 2 } } ,\cdot \cdot \cdot ,{ \theta }_{ n }-\alpha \frac { \partial J }{ \partial { \theta }_{ n } } )\\ \\matrix\quad notation\quad \Rightarrow \quad \theta := \theta - \alpha \triangledown J$$</span>

&nbsp;

<span style="font-family: arial, helvetica, sans-serif;">이 식에 의하면 새로운 θ는 gradient라는 벡터의 성질때문에 cost function이 이전보다 작아지게 한다. 이해를 돕기위해 다음 그림을 보자. 설명을 위해서 그림은 1개의 θ에 대해서만 설명하겠다.</span>

<span style="font-family: arial, helvetica, sans-serif;">![image](/images/2018/08/1-1.jpg){: width="50%" height="50%"}

<span style="font-family: arial, helvetica, sans-serif;">어느 point에서든지 적당한 α라는 factor를 곱하여 기울기의 반대방향으로 θ를 이동시키면 J값은 local minimum 쪽으로 움직인다. 이 작업을 <strong>cost function이  최소값으로 수렴할 때 까지 충분한 횟수로 반복</strong>해주면 되는것이다. 이 때 <strong>α가 너무 작으면 이 반복 횟수가 너무 크고 α가 너무 크면 수렴하지 못할 수 있다.</strong> α는 반복횟수, 즉 배우는 속도에를 결정하기 때문에<strong> learning rate</strong>라고 부른다. 열전달 함수에서의 열전달상수의 개념과 비슷한 개념이다.</span>

<span style="font-family: arial, helvetica, sans-serif;">다시 위에서 나온 식을 마저 정리해보자. J의 $$\theta_{0}$$과 $$\theta_{1}$$에 대한 편미분은 위의 hypothesis function과 cost function의 식을 적용해서 구하면 다음과 같이 정리된다.</span>

<span style="font-family: arial, helvetica, sans-serif;">$$\frac { \partial J }{ \partial { \theta }_{ 0 } } =\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ \theta }_{ 0 }+{ \theta }_{ 1 }{ x }_{ 1 }^{ (i) }+{ \theta }_{ 2 }{ x }_{ 2 }^{ (i) }+{ \theta }_{ 3 }{ x }_{ 3 }^{ (i) }+\cdot \cdot \cdot +{ \theta }_{ n }{ x }_{ n }^{ (i) }-{ y }^{ (i) }) } } =\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }_{ 0 }^{ (i) })-{ y }^{ (i) }) } }$$</span>

<span style="font-family: arial, helvetica, sans-serif;">$$\frac { \partial J }{ \partial { \theta }_{ 1 } } =\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ \theta }_{ 0 }+{ \theta }_{ 1 }{ x }_{ 1 }^{ (i) }+{ \theta }_{ 2 }{ x }_{ 2 }^{ (i) }+{ \theta }_{ 3 }{ x }_{ 3 }^{ (i) }+\cdot \cdot \cdot +{ \theta }_{ n }{ x }_{ n }^{ (i) }-{ y }^{ (i) }) } } { x }_{ 1 }^{ (i) }=\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }_{ 0 }^{ (i) })-{ y }^{ (i) }) } } { x }_{ 1 }^{ (i) }$$</span>

<span style="font-family: arial, helvetica, sans-serif;">$$\frac { \partial J }{ \partial { \theta }_{ 1 } } =\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ \theta }_{ 0 }+{ \theta }_{ 1 }{ x }_{ 1 }^{ (i) }+{ \theta }_{ 2 }{ x }_{ 2 }^{ (i) }+{ \theta }_{ 3 }{ x }_{ 3 }^{ (i) }+\cdot \cdot \cdot +{ \theta }_{ n }{ x }_{ n }^{ (i) }-{ y }^{ (i) }) } } { x }_{ 2 }^{ (i) }=\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }_{ 0 }^{ (i) })-{ y }^{ (i) }) } } { x }_{ 2 }^{ (i) }$$</span>

<p style="text-align: center;">
  <span style="font-family: arial, helvetica, sans-serif;"><strong>·</strong></span>
</p>

<p style="text-align: center;">
  <span style="font-family: arial, helvetica, sans-serif;"><strong>·</strong></span>
</p>

<span style="font-family: arial, helvetica, sans-serif;">$$\frac { \partial J }{ \partial { \theta }_{ n } } =\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ \theta }_{ 0 }+{ \theta }_{ 1 }{ x }_{ 1 }^{ (i) }+{ \theta }_{ 2 }{ x }_{ 2 }^{ (i) }+{ \theta }_{ 3 }{ x }_{ 3 }^{ (i) }+\cdot \cdot \cdot +{ \theta }_{ n }{ x }_{ n }^{ (i) }-{ y }^{ (i) }) } } { x }_{ n }^{ (i) }=\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }_{ 0 }^{ (i) })-{ y }^{ (i) }) } } { x }_{ n }^{ (i) }$$</span>

&nbsp;

<span style="font-family: arial, helvetica, sans-serif;">이 식을 위에서 얻은 새로운 θ의 식에 집어넣으면 다음과 같다.</span>

&nbsp;

$${ \theta }_{ 0 }:={ \theta }_{ 0 }-\alpha \frac { \partial J }{ \partial { \theta }_{ 0 } } ={ \theta }_{ 0 }-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } }$$ 

$${ \theta }_{ 1 }:={ \theta }_{ 1 }-\alpha \frac { \partial J }{ \partial { \theta }_{ 1 } } ={ \theta }_{ 1 }-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ 1 }^{ (i) }$$ 

$${ \theta }_{ 2 }:={ \theta }_{ 2 }-\alpha \frac { \partial J }{ \partial { \theta }_{ 1 } } ={ \theta }_{ 2 }-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ 2 }^{ (i) }$$ 

<span style="font-family: arial, helvetica, sans-serif;"><strong>                                         .</strong></span>

<span style="font-family: arial, helvetica, sans-serif;"><strong>                                         .</strong></span>

$${ \theta }_{ n }:={ \theta }_{ n }-\alpha \frac { \partial J }{ \partial { \theta }_{ n } } ={ \theta }_{ n }-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ n }^{ (i) }$$ 

<span style="font-family: arial, helvetica, sans-serif;">이 최종식을 적용하여 θ가 수렴할 때 까지 update를 해주면 cost function이 가장 작아지는최적의 θ가 나온다.</span>

<span style="font-family: arial, helvetica, sans-serif;">위의 update식을 matrix notation으로 간결하게 정리하면 다음과 같이 된다.</span>

<span style="font-family: arial, helvetica, sans-serif;">$${ X }^{ T }(X\theta -y)=\begin{bmatrix} { x }_{ 0 }^{ (1) }&{ x }_{ 0 }^{ (2) }&{ x }_{ 0 }^{ (3) }& \cdot & \cdot & \cdot & { x }_{ 0 }^{ (m) }\\ { x }_{ 1 }^{ (1) }&{ x }_{ 1 }^{ (2) }&{ x }_{ 1 }^{ (3) } & \cdot & \cdot& \cdot & { x }_{ 1 }^{ (m) }\\ { x }_{ 2 }^{ (1) }&{ x }_{ 2 }^{ (2) }&{ x }_{ 2 }^{ (3) } & \cdot & \cdot & \cdot & { x }_{ 2 }^{ (m) }\\ \cdot \\ \cdot \\ { x }_{ n }^{ (1) } & { x }_{ n }^{ (2) } & { x }_{ n }^{ (3) } & \cdot & \cdot & \cdot & { x }_{ n }^{ (m) } \end{bmatrix}$$$$\begin{bmatrix} { h }_{ \theta }({ x }^{ (1) })-{ y }^{ (1) }\\ { h }_{ \theta }({ x }^{ (2) })-{ y }^{ (2) }\\ { h }_{ \theta }({ x }^{ (3) })-{ y }^{ (3) }\\ \cdot \\ \cdot \\ \cdot \\ { h }_{ \theta }({ x }^{ (m) })-{ y }^{ (m) } \end{bmatrix}$$ = $$\begin{bmatrix} \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ 0 }^{ (i) }\\ \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ 1 }^{ (i) }\\ \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ 2 }^{ (i) }\\ \quad \quad \quad \quad \quad \quad \cdot \\ \quad \quad \quad \quad \quad \quad \cdot \\ \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }^{ (i) })-{ y }^{ (i) }) } } { x }_{ n }^{ (i) } \end{bmatrix}$$</span>

&nbsp;

<span style="font-family: arial, helvetica, sans-serif;">따라서 update equation은 다음과 같이 정리된다.</span>

&nbsp;

<span style="font-family: arial, helvetica, sans-serif;">$$\theta := \theta - \frac { \alpha }{ m } { X }^{ T }(X\theta-y)$$</span>

&nbsp;

&nbsp;

# <span style="font-size: 14pt; font-family: arial, helvetica, sans-serif;"><strong><Feature Scailing & Mean Normalization></strong></span>

* * *

<span style="font-family: arial, helvetica, sans-serif;">기말고사 성적을 예측하는 인공지능을 구현하려고 할 때 training data set의 feature가 &#8216;\원&#8217; 단위의 책을 구매하는데 쓴 비용과 공부에 투자한 시간, 그리고 IQ로 이루어져있다고 해보자. 이 때 책의 가격은 절대크기가 [10,000 ~ 1,000,000]의 범위에 분포해있고 IQ는 [80 ~ 150]의 범위, 그리고 공부에 투자한 시간은 [0 ~ 300]으로 이루어져있다고 하자. 이처럼 feature의 scale 차이가 많이 나는 경우에는 cost function의 모양이 다음과 같이 매우 skewed한 모양이 된다.</span>

<span style="font-family: arial, helvetica, sans-serif;">![image](/images/2018/08/2-1.jpg){: width="50%" height="50%"}

<span style="font-family: arial, helvetica, sans-serif;">이런 경우 <strong>절대크기가 큰 feature에 의해 나머지 feature가 휘둘리게 된다</strong>. 이런 경우에는 수렴하는 속도가 느려지고 학습속도가 저하된다. 따라서 feature normalization을 해주어야 한다.</span>

<span style="font-family: arial, helvetica, sans-serif;">feature scailing은 각 feature를 max와 min값의 차이로 각 feature를 나누는 것이다. 그럼 feature의 scale이 비슷해질 것이다. 또 평균을 0으로 옮기기 위해서 feature에서 mean값을 빼는 것을 mean normailization이라고 한다. feature scailing과 mean normalization을 한 feature는 다음과 같이 치환이 된다.</span>

<span style="font-family: arial, helvetica, sans-serif;">$${ x }_{ i } := \frac { { x }_{ i }-{ \mu }_{ i } }{ { s }_{ i } } \left({ \mu }_{ i } :\quad average, \quad { s }_{ i } :\quad max-min \right)$$</span>

<span style="font-family: arial, helvetica, sans-serif;">(max-min) 대신에 standard deviation(표준편차)를 써도 된다.</span>

&nbsp;

<span style="font-family: arial, helvetica, sans-serif;"><strong>※ Learning Rate를 결정하는 Tip</strong></span>

<span style="font-family: arial, helvetica, sans-serif;">Learning Rate는 Gradient Descent 에서 한 step의 이동거리를 결정한다. α는 수렴속도, 즉 학습속도를 결정하기때문에 Learning Rate라 부른다.</span>

<span style="font-family: arial, helvetica, sans-serif;">만약 Learning Rate가 너무 크다면 어떻게 될까?</span>

<span style="font-family: arial, helvetica, sans-serif;">![image](/images/2018/08/3.jpg){: width="50%" height="50%"}

<span style="font-family: arial, helvetica, sans-serif;">위와 같이 <strong>α가 너무 크면 overshoot이 나면서 J가 수렴하지 못한다</strong>.</span>

<span style="font-family: arial, helvetica, sans-serif;">α가 충분히 작기만 하다면 cost function은 항상 최소로 가고 θ는 수렴할 수 있게 된다. 결국 우리가 원하는 그림은 다음과 같다.</span>

<span style="font-family: arial, helvetica, sans-serif;">![image](/images/2018/08/4.jpg){: width="50%" height="50%"}

<span style="font-family: arial, helvetica, sans-serif;">하지만 <strong>α가 너무 작으면 수렴은 너무 천천히 일어날 것이고 그것은 학습속도가 너무 느리다는 말이 된다</strong>.</span>

<span style="font-family: arial, helvetica, sans-serif;">Prof.Ng은 대략 α의 값을 3배 정도씩 늘려가며 cost function이 작아지는지 관찰한다고 말한다. 0.001, 0.003, 0.006, . . . 같은 식으로 말이다. 이렇게 하며 cost function을 관찰하고 θ가 가장 빠르게 수렴하는 α를 선택하면 된다.</span>