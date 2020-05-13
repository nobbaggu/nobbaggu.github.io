---
title: (머신러닝) 12. 역전파 (back propagation)
date: 2018-08-10T11:37:14+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - activation function
  - back
  - forward
  - gradient descent
  - hidden layer
  - input layer
  - machine learning
  - MLP
  - multi layer perceptron
  - neural network
  - output layer
  - perceptron
  - propagation
  - sigmoid
  - 머신러닝
  - 순전파
  - 시그모이드
  - 신경망
  - 에러
  - 역전파
  - 은닉층
  - 인공신경망
  - 퍼셉트론
  - 활성함수
---
# 1. 역전파 알고리즘

* * *

신경망의 개념이 나온지는 꽤 되었지만 신경망을 학습시키는 방법을 몰라 오랫동안 사장된 이론이었다. 하지만 1970년대 미국의 한 대학원생은 신경망을 학습시킬 방법을 발견했고, 이것이 지금부터 설명할 역전파 이다. 사실 역전파 알고리즘은 우리가 regression 문제에서 이미 보았던 <span style="color: #000000;"><strong>gradient-descent(경사하강법)과 </strong>본질적으로 같다. </span>아무튼 역전파를 아래와 같은 3-2-2 층을 가진 신경망을 예시로 들어 설명하겠다.

![image](/images/2018/08/no-name-44-300x285.png){: width="50%" height="50%"}

$$\\ { z }_{ 1 }^{ (2) }={ \Theta }_{ 1,0 }^{ (1) }{ a }_{ 0 }^{ (1) }+{ \Theta }_{ 1,1 }^{ (1) }{ a }_{ 1 }^{ (1) }+{ \Theta }_{ 1,2 }^{ (1) }{ a }_{ 2 }^{ (1) }+{ \Theta }_{ 1,3 }^{ (1) }{ a }_{ 3 }^{ (1) }\\ { z }_{ 2 }^{ (2) }={ \Theta }_{ 2,0 }^{ (1) }{ a }_{ 0 }^{ (1) }+{ \Theta }_{ 2,1 }^{ (1) }{ a }_{ 1 }^{ (1) }+{ \Theta }_{ 2,2 }^{ (1) }{ a }_{ 2 }^{ (1) }+{ \Theta }_{ 2,3 }^{ (1) }{ a }_{ 3 }^{ (1) }$$ 

$$\dpi{120} \\ { a }_{ 1 }^{ (2) }=g({ z }_{ 1 }^{ (2) })\\ { a }_{ 2 }^{ (2) }=g({ z }_{ 2 }^{ (2) })\\ { z }_{ 1 }^{ (3) }={ \Theta }_{ 1,0 }^{ (2) }{ a }_{ 0 }^{ (2) }+{ \Theta }_{ 1,1 }^{ (2) }{ a }_{ 1 }^{ (2) }+{ \theta }_{ 1,2 }^{ (2) }{ a }_{ 2 }^{ (2) }\\ { z }_{ 2 }^{ (3) }={ \Theta }_{ 2,0 }^{ (2) }{ a }_{ 0 }^{ (2) }+{ \Theta }_{ 2,1 }^{ (2) }{ a }_{ 1 }^{ (2) }+{ \Theta }_{ 2,2 }^{ (2) }{ a }_{ 2 }^{ (2) }\\ { a }_{ 1 }^{ (3) }=g({ z }_{ 1 }^{ (3) })\\ { a }_{ 2 }^{ (3) }=g({ z }_{ 2 }^{ (3) })\\ { J }_{ 1 }=-{ y }_{ 1 }log({ a }_{ 1 }^{ (3) })-(1-{ y }_{ 1 })log(1-{ a }_{ 1 }^{ (3) })\\ { J }_{ 2 }=-{ y }_{ 2 }log({ a }_{ 2 }^{ (3) })-(1-{ y }_{ 2 })log(1-{ a }_{ 2 }^{ (3) })$$ 

위와 같은 3-2-2층의 신경망이 있다. 각 node는 activation function으로 sigmoid 함수를 사용하므로 우리는 error를 정량화할 목적으로 classification에서 썼던 형태의 cost function을 사용할 수 있다.

&nbsp;

$$J(\Theta )=-\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ \sum _{ k=1 }^{ K }{ [{ y }_{ k }^{ (i) }log({ ({ h }_{ \Theta }({ x }^{ (i) })) }_{ k })+({ 1-y }_{ k }^{ (i) })log({ 1-({ h }_{ \Theta }({ x }^{ (i) })) }_{ k })] } } +$$  $$\frac { \lambda }{ 2m } \sum _{ l=1 }^{ L-1 }{ \sum _{ i=1 }^{ { s }_{ l } }{ \sum _{ j=1 }^{ { s }_{ l+1 } }{ { ({ \Theta }_{ j,i }^{ (l) }) }^{ 2 } } } }$$

복잡해 보인다. 하지만 이 식에 너무 매몰되기보단 의미를 생각하면서 천천히 이해하면 된다.

단순히 m개의 훈련 데이터, K개의 클래스에 대하여 cost function J의 합을 계산할 뿐이다. 뒤에 있는 regularization term은 전체 신경망의 parameter를 집어넣었을 뿐이다.

각각의 parameter가 cost function에 얼마나 영향을 미치는지(얼마만큼의 error를 만들어 내는지) 알기 위해서는 일단 모든 $$\dpi{120} \Theta$$에 대해 $$\dpi{120} \frac{\partial J}{\partial \Theta}$$를 구해야한다. 이것을 구하면 gradient descent에서 했던 것처럼 적절한 learning rate를 곱하여 parameter를 update 할 수 있다. 이것이 역전파이다.

&nbsp;

&nbsp;

&nbsp;

# 2. 역전파를 위한 cost function의 미분 계산

* * *

목표는$$\dpi{120} \frac{\partial J}{\partial \Theta}$$ 를 계산하여 parameter를 update하는 것이다. 쉽게 계산하는 법은 다음과 같다.

$$\dpi{120} \bigtriangleup^{(l)} = 0\quad(l=1,2,...,L-1)$$      $$\dpi{120} \quad \bigtriangleup^{(l)}$$은 $$\dpi{120} \Theta^{(l)}$$과 같은 크기의 행렬이다. 아무튼 다음과 같은 행렬을 만들어놓고 다 0으로 초기화 시켜놓는다.

**for i = 1:m(m은 training dataset의 갯수),**

**   1. forward-propagation을 해서 output layer의 결과를 구한다. vector $$\dpi{120} a^{ (L) }$$** 

**   2. 계산결과값과 실제결과값의 차이 error vector를 다음과 같이 놓는다. $$\dpi{120} \delta^{(L)} = a^{(L)}-y$$** 

**   3. 다음 공식에 따라 input layer를 제외한 층까지 역전파한다.  $$\dpi{120} { \delta }^{ (l) }=({ ({ \Theta }^{ (l) }) }^{ T }{ \delta }^{ (l+1) }).*{ a }^{ (l) }.*(1-{ a }^{ (l) })$$**

**   4. ![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;\delta){: width="50%" height="50%"}

**end**

이상으로 다음과 같은 멋진 결론이 나온다.

$$\dpi{120} \frac{\partial J}{\partial\Theta^{(l)}}=\frac{1}{m}\bigtriangleup ^{(l)}$$ 

&nbsp;

위의 수식의 증명은 길고 재미없고 심지어 더럽기까지한 미분의 연속이다. 알 필요 없다. 단지 실제 데이터와 우리가 순전파 시켜 나온 예상값과의 에러를 이전 층으로 다시 전파시키는 짓을 반복해가며 cost-function J를 줄인다는 것만 알고 가도 충분하다.

처음의 그림에 나와있는 3-2-2 신경망에 대해서 한 번 연습해보자.

$$\dpi{120} \\ \Theta^{(1)}=2\times 4 matrix\\ \Theta^{(2)}=2\times 3 matrix$$     따라서,    $$\dpi{120} \bigtriangleup^{(1)} = \begin{bmatrix} 0 &0 & 0 & 0\\ 0 & 0 & 0& 0 \end{bmatrix},\quad \bigtriangleup^{(2)} = \begin{bmatrix} 0 &0 & 0 \\ 0 & 0& 0 \end{bmatrix}$$

for i=1:m

  * forward propagation, output layer error 계산

$${ \delta }_{ 1 }^{ (3) }={ a }_{ 1 }^{ (3) }-{ y }_{ 1 }$$ 

$${ \delta }_{ 2 }^{ (3) }={ a }_{ 2 }^{ (3) }-{ y }_{ 2 }$$ 

&nbsp;

  * back propagation, hidden layer error 계산

$$\dpi{120} \\error\quad back-propagation>> \\ \\ \begin{bmatrix} \delta_{1}^{(2)} \\ \delta_{2}^{(2)}\end{bmatrix}= \begin{bmatrix} \Theta_{1,1}^{(2)} & \Theta_{2,1}^{(2)} \\ \Theta_{1,2}^{(2)} & \Theta_{2,2}^{(2)} \end{bmatrix} \begin{bmatrix} \delta_{1}^{(3)} \\ \delta_{2}^{(3)} \end{bmatrix}.* \begin{bmatrix} a_{1}^{(2)} \\ a_{2}^{(2)}\end{bmatrix}.* \begin{bmatrix} 1-a_{1}^{(2)} \\ 1-a_{2}^{(2)}\end{bmatrix}$$ 

  * error 쌓아주기

$$\dpi{120} \\ \bigtriangleup ^{(1)}=\bigtriangleup ^{(1)}+\delta^{(2)}(a^{(1)})^{T}\\ \bigtriangleup ^{(2)}=\bigtriangleup ^{(2)}+\delta^{(3)}(a^{(2)})^{T}$$        $$\dpi{120} \delta^{(2)}$$에서 bias node 빼주기.

end

  * 쌓인 error 평균내서 cost function의 gradient 구하기

$$\dpi{120} \\ \frac{\partial J}{\partial\Theta^{(1)}}=\frac{1}{m}\bigtriangleup ^{(1)}\\ \frac{\partial J}{\partial\Theta^{(2)}}=\frac{1}{m}\bigtriangleup ^{(2)}$$ 

&nbsp;

이제 gradient를 구했으니 적절한 learning rate를 이용해서 cost function J가 수렴할 때 까지 $$\dpi{120} \Theta$$를 update하거나 python, matlab, octave 등의 라이브러리가 지원하는 optimization 함수를 사용하면 된다.

한 가지 덧붙이자면 input layer는 parameter Θ와 상관없이 항상 고정된 값(feature)를 가지므로 error가 없다. 따라서 지금 예시로 다루고 있는 신경망에서는 $$\dpi{120} \delta^{ (2) }, \delta^{ (3) }$$ 두 층의 에러만 구하면 된다.

지금까지 계산한 것을 그림으로 간단히 나타내면 다음과 같다.

![image](/images/2018/08/no-name-45-295x300.png){: width="50%" height="50%"}

&nbsp;

&nbsp;