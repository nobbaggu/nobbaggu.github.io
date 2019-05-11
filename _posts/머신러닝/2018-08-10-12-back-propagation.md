---
title: 12. 역전파 (back propagation)
date: 2018-08-10T11:37:14+09:00
author: SWnomad
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

<img class="aligncenter size-medium wp-image-435" src="/images/2018/08/no-name-44-300x285.png" alt="" width="300" height="285" srcset="/images/2018/08/no-name-44-300x285.png 300w, /images/2018/08/no-name-44.png 570w" sizes="(max-width: 300px) 100vw, 300px" /> 

<img src="https://latex.codecogs.com/gif.latex?\\&space;{&space;z&space;}_{&space;1&space;}^{&space;(2)&space;}={&space;\Theta&space;}_{&space;1,0&space;}^{&space;(1)&space;}{&space;a&space;}_{&space;0&space;}^{&space;(1)&space;}+{&space;\Theta&space;}_{&space;1,1&space;}^{&space;(1)&space;}{&space;a&space;}_{&space;1&space;}^{&space;(1)&space;}+{&space;\Theta&space;}_{&space;1,2&space;}^{&space;(1)&space;}{&space;a&space;}_{&space;2&space;}^{&space;(1)&space;}+{&space;\Theta&space;}_{&space;1,3&space;}^{&space;(1)&space;}{&space;a&space;}_{&space;3&space;}^{&space;(1)&space;}\\&space;{&space;z&space;}_{&space;2&space;}^{&space;(2)&space;}={&space;\Theta&space;}_{&space;2,0&space;}^{&space;(1)&space;}{&space;a&space;}_{&space;0&space;}^{&space;(1)&space;}+{&space;\Theta&space;}_{&space;2,1&space;}^{&space;(1)&space;}{&space;a&space;}_{&space;1&space;}^{&space;(1)&space;}+{&space;\Theta&space;}_{&space;2,2&space;}^{&space;(1)&space;}{&space;a&space;}_{&space;2&space;}^{&space;(1)&space;}+{&space;\Theta&space;}_{&space;2,3&space;}^{&space;(1)&space;}{&space;a&space;}_{&space;3&space;}^{&space;(1)&space;}" alt="\\ { z }_{ 1 }^{ (2) }={ \Theta }_{ 1,0 }^{ (1) }{ a }_{ 0 }^{ (1) }+{ \Theta }_{ 1,1 }^{ (1) }{ a }_{ 1 }^{ (1) }+{ \Theta }_{ 1,2 }^{ (1) }{ a }_{ 2 }^{ (1) }+{ \Theta }_{ 1,3 }^{ (1) }{ a }_{ 3 }^{ (1) }\\ { z }_{ 2 }^{ (2) }={ \Theta }_{ 2,0 }^{ (1) }{ a }_{ 0 }^{ (1) }+{ \Theta }_{ 2,1 }^{ (1) }{ a }_{ 1 }^{ (1) }+{ \Theta }_{ 2,2 }^{ (1) }{ a }_{ 2 }^{ (1) }+{ \Theta }_{ 2,3 }^{ (1) }{ a }_{ 3 }^{ (1) }" align="absmiddle" /> 

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;{&space;a&space;}_{&space;1&space;}^{&space;(2)&space;}=g({&space;z&space;}_{&space;1&space;}^{&space;(2)&space;})\\&space;{&space;a&space;}_{&space;2&space;}^{&space;(2)&space;}=g({&space;z&space;}_{&space;2&space;}^{&space;(2)&space;})\\&space;{&space;z&space;}_{&space;1&space;}^{&space;(3)&space;}={&space;\Theta&space;}_{&space;1,0&space;}^{&space;(2)&space;}{&space;a&space;}_{&space;0&space;}^{&space;(2)&space;}+{&space;\Theta&space;}_{&space;1,1&space;}^{&space;(2)&space;}{&space;a&space;}_{&space;1&space;}^{&space;(2)&space;}+{&space;\theta&space;}_{&space;1,2&space;}^{&space;(2)&space;}{&space;a&space;}_{&space;2&space;}^{&space;(2)&space;}\\&space;{&space;z&space;}_{&space;2&space;}^{&space;(3)&space;}={&space;\Theta&space;}_{&space;2,0&space;}^{&space;(2)&space;}{&space;a&space;}_{&space;0&space;}^{&space;(2)&space;}+{&space;\Theta&space;}_{&space;2,1&space;}^{&space;(2)&space;}{&space;a&space;}_{&space;1&space;}^{&space;(2)&space;}+{&space;\Theta&space;}_{&space;2,2&space;}^{&space;(2)&space;}{&space;a&space;}_{&space;2&space;}^{&space;(2)&space;}\\&space;{&space;a&space;}_{&space;1&space;}^{&space;(3)&space;}=g({&space;z&space;}_{&space;1&space;}^{&space;(3)&space;})\\&space;{&space;a&space;}_{&space;2&space;}^{&space;(3)&space;}=g({&space;z&space;}_{&space;2&space;}^{&space;(3)&space;})\\&space;{&space;J&space;}_{&space;1&space;}=-{&space;y&space;}_{&space;1&space;}log({&space;a&space;}_{&space;1&space;}^{&space;(3)&space;})-(1-{&space;y&space;}_{&space;1&space;})log(1-{&space;a&space;}_{&space;1&space;}^{&space;(3)&space;})\\&space;{&space;J&space;}_{&space;2&space;}=-{&space;y&space;}_{&space;2&space;}log({&space;a&space;}_{&space;2&space;}^{&space;(3)&space;})-(1-{&space;y&space;}_{&space;2&space;})log(1-{&space;a&space;}_{&space;2&space;}^{&space;(3)&space;})" alt="\dpi{120} \\ { a }_{ 1 }^{ (2) }=g({ z }_{ 1 }^{ (2) })\\ { a }_{ 2 }^{ (2) }=g({ z }_{ 2 }^{ (2) })\\ { z }_{ 1 }^{ (3) }={ \Theta }_{ 1,0 }^{ (2) }{ a }_{ 0 }^{ (2) }+{ \Theta }_{ 1,1 }^{ (2) }{ a }_{ 1 }^{ (2) }+{ \theta }_{ 1,2 }^{ (2) }{ a }_{ 2 }^{ (2) }\\ { z }_{ 2 }^{ (3) }={ \Theta }_{ 2,0 }^{ (2) }{ a }_{ 0 }^{ (2) }+{ \Theta }_{ 2,1 }^{ (2) }{ a }_{ 1 }^{ (2) }+{ \Theta }_{ 2,2 }^{ (2) }{ a }_{ 2 }^{ (2) }\\ { a }_{ 1 }^{ (3) }=g({ z }_{ 1 }^{ (3) })\\ { a }_{ 2 }^{ (3) }=g({ z }_{ 2 }^{ (3) })\\ { J }_{ 1 }=-{ y }_{ 1 }log({ a }_{ 1 }^{ (3) })-(1-{ y }_{ 1 })log(1-{ a }_{ 1 }^{ (3) })\\ { J }_{ 2 }=-{ y }_{ 2 }log({ a }_{ 2 }^{ (3) })-(1-{ y }_{ 2 })log(1-{ a }_{ 2 }^{ (3) })" align="absmiddle" /> 

위와 같은 3-2-2층의 신경망이 있다. 각 node는 activation function으로 sigmoid 함수를 사용하므로 우리는 error를 정량화할 목적으로 classification에서 썼던 형태의 cost function을 사용할 수 있다.

&nbsp;

<img src="https://latex.codecogs.com/gif.latex?J(\Theta&space;)=-\frac&space;{&space;1&space;}{&space;m&space;}&space;\sum&space;_{&space;i=1&space;}^{&space;m&space;}{&space;\sum&space;_{&space;k=1&space;}^{&space;K&space;}{&space;[{&space;y&space;}_{&space;k&space;}^{&space;(i)&space;}log({&space;({&space;h&space;}_{&space;\Theta&space;}({&space;x&space;}^{&space;(i)&space;}))&space;}_{&space;k&space;})+({&space;1-y&space;}_{&space;k&space;}^{&space;(i)&space;})log({&space;1-({&space;h&space;}_{&space;\Theta&space;}({&space;x&space;}^{&space;(i)&space;}))&space;}_{&space;k&space;})]&space;}&space;}&space;+" alt="J(\Theta )=-\frac { 1 }{ m } \sum _{ i=1 }^{ m }{ \sum _{ k=1 }^{ K }{ [{ y }_{ k }^{ (i) }log({ ({ h }_{ \Theta }({ x }^{ (i) })) }_{ k })+({ 1-y }_{ k }^{ (i) })log({ 1-({ h }_{ \Theta }({ x }^{ (i) })) }_{ k })] } } +" align="absmiddle" />  <img src="https://latex.codecogs.com/gif.latex?\frac&space;{&space;\lambda&space;}{&space;2m&space;}&space;\sum&space;_{&space;l=1&space;}^{&space;L-1&space;}{&space;\sum&space;_{&space;i=1&space;}^{&space;{&space;s&space;}_{&space;l&space;}&space;}{&space;\sum&space;_{&space;j=1&space;}^{&space;{&space;s&space;}_{&space;l+1&space;}&space;}{&space;{&space;({&space;\Theta&space;}_{&space;j,i&space;}^{&space;(l)&space;})&space;}^{&space;2&space;}&space;}&space;}&space;}" alt="\frac { \lambda }{ 2m } \sum _{ l=1 }^{ L-1 }{ \sum _{ i=1 }^{ { s }_{ l } }{ \sum _{ j=1 }^{ { s }_{ l+1 } }{ { ({ \Theta }_{ j,i }^{ (l) }) }^{ 2 } } } }" align="absmiddle" />

복잡해 보인다. 하지만 이 식에 너무 매몰되기보단 의미를 생각하면서 천천히 이해하면 된다.

단순히 m개의 훈련 데이터, K개의 클래스에 대하여 cost function J의 합을 계산할 뿐이다. 뒤에 있는 regularization term은 전체 신경망의 parameter를 집어넣었을 뿐이다.

각각의 parameter가 cost function에 얼마나 영향을 미치는지(얼마만큼의 error를 만들어 내는지) 알기 위해서는 일단 모든 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Theta" alt="\dpi{120} \Theta" align="absmiddle" />에 대해 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{\partial&space;J}{\partial&space;\Theta}" alt="\dpi{120} \frac{\partial J}{\partial \Theta}" align="absmiddle" />를 구해야한다. 이것을 구하면 gradient descent에서 했던 것처럼 적절한 learning rate를 곱하여 parameter를 update 할 수 있다. 이것이 역전파이다.

&nbsp;

&nbsp;

&nbsp;

# 2. 역전파를 위한 cost function의 미분 계산

* * *

목표는<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{\partial&space;J}{\partial&space;\Theta}" alt="\dpi{120} \frac{\partial J}{\partial \Theta}" align="absmiddle" /> 를 계산하여 parameter를 update하는 것이다. 쉽게 계산하는 법은 다음과 같다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\bigtriangleup^{(l)}&space;=&space;0\quad(l=1,2,...,L-1)" alt="\dpi{120} \bigtriangleup^{(l)} = 0\quad(l=1,2,...,L-1)" align="absmiddle" />      <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\quad&space;\bigtriangleup^{(l)}" alt="\dpi{120} \quad \bigtriangleup^{(l)}" align="absmiddle" />은 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Theta^{(l)}" alt="\dpi{120} \Theta^{(l)}" align="absmiddle" />과 같은 크기의 행렬이다. 아무튼 다음과 같은 행렬을 만들어놓고 다 0으로 초기화 시켜놓는다.

**for i = 1:m(m은 training dataset의 갯수),**

**   1. forward-propagation을 해서 output layer의 결과를 구한다. vector <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;a^{&space;(L)&space;}" alt="\dpi{120} a^{ (L) }" align="absmiddle" />** 

**   2. 계산결과값과 실제결과값의 차이 error vector를 다음과 같이 놓는다. <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\delta^{(L)}&space;=&space;a^{(L)}-y" alt="\dpi{120} \delta^{(L)} = a^{(L)}-y" align="absmiddle" />** 

**   3. 다음 공식에 따라 input layer를 제외한 층까지 역전파한다.  <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;{&space;\delta&space;}^{&space;(l)&space;}=({&space;({&space;\Theta&space;}^{&space;(l)&space;})&space;}^{&space;T&space;}{&space;\delta&space;}^{&space;(l+1)&space;}).*{&space;a&space;}^{&space;(l)&space;}.*(1-{&space;a&space;}^{&space;(l)&space;})" alt="\dpi{120} { \delta }^{ (l) }=({ ({ \Theta }^{ (l) }) }^{ T }{ \delta }^{ (l+1) }).*{ a }^{ (l) }.*(1-{ a }^{ (l) })" align="absmiddle" />**

**   4. <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\bigtriangleup^{(l)}=\bigtriangleup^{(l)}+\delta^{(l+1)}(a^{(l)})^{T}" alt="\dpi{120} \bigtriangleup^{(l)}=\bigtriangleup^{(l)}+\delta^{(l+1)}(a^{(l)})^{T}" align="absmiddle" />  ←!!주의 : <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\delta^{(l+1)}" alt="\dpi{120} \delta^{(l+1)}" align="absmiddle" />을 대입할 때 bias unit을 뺀 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\delta" alt="\dpi{120} \delta" align="absmiddle" />를 적용한다. 왜냐하면 forward propagation을 할 때에 고정된 값(1)인 bias unit에는 영향을 주지 않기 때문이다. 다시 한 번 말하자면 bias unit은 전파를 한 다음 추가하는 node이다. output layer에는 bias node를 애초에 추가하지 않았기 때문에 상관없다. 아무튼 만약 실수로 bias unit까지 넣는다면 행렬곱셈을 할 때에 dimension이 맞지 않아 컴퓨터가 계산 오류를 일으킬 것이다.**

**end**

이상으로 다음과 같은 멋진 결론이 나온다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{\partial&space;J}{\partial\Theta^{(l)}}=\frac{1}{m}\bigtriangleup&space;^{(l)}" alt="\dpi{120} \frac{\partial J}{\partial\Theta^{(l)}}=\frac{1}{m}\bigtriangleup ^{(l)}" align="absmiddle" /> 

&nbsp;

위의 수식의 증명은 길고 재미없고 심지어 더럽기까지한 미분의 연속이다. 알 필요 없다. 단지 실제 데이터와 우리가 순전파 시켜 나온 예상값과의 에러를 이전 층으로 다시 전파시키는 짓을 반복해가며 cost-function J를 줄인다는 것만 알고 가도 충분하다.

처음의 그림에 나와있는 3-2-2 신경망에 대해서 한 번 연습해보자.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;\Theta^{(1)}=2\times&space;4&space;matrix\\&space;\Theta^{(2)}=2\times&space;3&space;matrix" alt="\dpi{120} \\ \Theta^{(1)}=2\times 4 matrix\\ \Theta^{(2)}=2\times 3 matrix" align="absmiddle" />     따라서,    <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\bigtriangleup^{(1)}&space;=&space;\begin{bmatrix}&space;0&space;&0&space;&&space;0&space;&&space;0\\&space;0&space;&&space;0&space;&&space;0&&space;0&space;\end{bmatrix},\quad&space;\bigtriangleup^{(2)}&space;=&space;\begin{bmatrix}&space;0&space;&0&space;&&space;0&space;\\&space;0&space;&&space;0&&space;0&space;\end{bmatrix}" alt="\dpi{120} \bigtriangleup^{(1)} = \begin{bmatrix} 0 &0 & 0 & 0\\ 0 & 0 & 0& 0 \end{bmatrix},\quad \bigtriangleup^{(2)} = \begin{bmatrix} 0 &0 & 0 \\ 0 & 0& 0 \end{bmatrix}" align="absmiddle" />

for i=1:m

  * forward propagation, output layer error 계산

<img src="https://latex.codecogs.com/gif.latex?{&space;\delta&space;}_{&space;1&space;}^{&space;(3)&space;}={&space;a&space;}_{&space;1&space;}^{&space;(3)&space;}-{&space;y&space;}_{&space;1&space;}" alt="{ \delta }_{ 1 }^{ (3) }={ a }_{ 1 }^{ (3) }-{ y }_{ 1 }" align="absmiddle" /> 

<img src="https://latex.codecogs.com/gif.latex?{&space;\delta&space;}_{&space;2&space;}^{&space;(3)&space;}={&space;a&space;}_{&space;2&space;}^{&space;(3)&space;}-{&space;y&space;}_{&space;2&space;}" alt="{ \delta }_{ 2 }^{ (3) }={ a }_{ 2 }^{ (3) }-{ y }_{ 2 }" align="absmiddle" /> 

&nbsp;

  * back propagation, hidden layer error 계산

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\error\quad&space;back-propagation>>&space;\\&space;\\&space;\begin{bmatrix}&space;\delta_{1}^{(2)}&space;\\&space;\delta_{2}^{(2)}\end{bmatrix}=&space;\begin{bmatrix}&space;\Theta_{1,1}^{(2)}&space;&&space;\Theta_{2,1}^{(2)}&space;\\&space;\Theta_{1,2}^{(2)}&space;&&space;\Theta_{2,2}^{(2)}&space;\end{bmatrix}&space;\begin{bmatrix}&space;\delta_{1}^{(3)}&space;\\&space;\delta_{2}^{(3)}&space;\end{bmatrix}.*&space;\begin{bmatrix}&space;a_{1}^{(2)}&space;\\&space;a_{2}^{(2)}\end{bmatrix}.*&space;\begin{bmatrix}&space;1-a_{1}^{(2)}&space;\\&space;1-a_{2}^{(2)}\end{bmatrix}" alt="\dpi{120} \\error\quad back-propagation>> \\ \\ \begin{bmatrix} \delta_{1}^{(2)} \\ \delta_{2}^{(2)}\end{bmatrix}= \begin{bmatrix} \Theta_{1,1}^{(2)} & \Theta_{2,1}^{(2)} \\ \Theta_{1,2}^{(2)} & \Theta_{2,2}^{(2)} \end{bmatrix} \begin{bmatrix} \delta_{1}^{(3)} \\ \delta_{2}^{(3)} \end{bmatrix}.* \begin{bmatrix} a_{1}^{(2)} \\ a_{2}^{(2)}\end{bmatrix}.* \begin{bmatrix} 1-a_{1}^{(2)} \\ 1-a_{2}^{(2)}\end{bmatrix}" align="absmiddle" /> 

  * error 쌓아주기

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;\bigtriangleup&space;^{(1)}=\bigtriangleup&space;^{(1)}+\delta^{(2)}(a^{(1)})^{T}\\&space;\bigtriangleup&space;^{(2)}=\bigtriangleup&space;^{(2)}+\delta^{(3)}(a^{(2)})^{T}" alt="\dpi{120} \\ \bigtriangleup ^{(1)}=\bigtriangleup ^{(1)}+\delta^{(2)}(a^{(1)})^{T}\\ \bigtriangleup ^{(2)}=\bigtriangleup ^{(2)}+\delta^{(3)}(a^{(2)})^{T}" align="absmiddle" />        <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\delta^{(2)}" alt="\dpi{120} \delta^{(2)}" align="absmiddle" />에서 bias node 빼주기.

end

  * 쌓인 error 평균내서 cost function의 gradient 구하기

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;\frac{\partial&space;J}{\partial\Theta^{(1)}}=\frac{1}{m}\bigtriangleup&space;^{(1)}\\&space;\frac{\partial&space;J}{\partial\Theta^{(2)}}=\frac{1}{m}\bigtriangleup&space;^{(2)}" alt="\dpi{120} \\ \frac{\partial J}{\partial\Theta^{(1)}}=\frac{1}{m}\bigtriangleup ^{(1)}\\ \frac{\partial J}{\partial\Theta^{(2)}}=\frac{1}{m}\bigtriangleup ^{(2)}" align="absmiddle" /> 

&nbsp;

이제 gradient를 구했으니 적절한 learning rate를 이용해서 cost function J가 수렴할 때 까지 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Theta" alt="\dpi{120} \Theta" align="absmiddle" />를 update하거나 python, matlab, octave 등의 라이브러리가 지원하는 optimization 함수를 사용하면 된다.

한 가지 덧붙이자면 input layer는 parameter Θ와 상관없이 항상 고정된 값(feature)를 가지므로 error가 없다. 따라서 지금 예시로 다루고 있는 신경망에서는 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\delta^{&space;(2)&space;},&space;\delta^{&space;(3)&space;}" alt="\dpi{120} \delta^{ (2) }, \delta^{ (3) }" align="absmiddle" /> 두 층의 에러만 구하면 된다.

지금까지 계산한 것을 그림으로 간단히 나타내면 다음과 같다.

<img class="aligncenter size-medium wp-image-436" src="/images/2018/08/no-name-45-295x300.png" alt="" width="295" height="300" srcset="/images/2018/08/no-name-45-295x300.png 295w, /images/2018/08/no-name-45.png 548w" sizes="(max-width: 295px) 100vw, 295px" /> 

&nbsp;

&nbsp;