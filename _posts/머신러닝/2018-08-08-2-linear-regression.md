---
title: (머신러닝) 2. Linear Regression
date: 2018-08-08T22:00:49+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - AI
  - cost function
  - gradient descent
  - hypothesis function
  - linear regression
  - machine learning
  - 가설함수
  - 경사하강법
  - 머신러닝
  - 비용함수
  - 선형회귀
  - 인공지능
---
Linear Regression (선형회귀)은 데이터 패턴과 비슷한 선형함수를 구하는 일이다. 물론 모든 데이터가 linear하지는 않다. 이런 경우에는 몇 가지 테크닉을 사용해 linear regression 으로 문제를 풀 수 있다.

Linear Regression with One Variable(단변수 선형)회귀는 **1개의 input(x)이 들어왔을 때 1개의 output(y)을 예측하는 모델을 구하는 문제이다**. Linear Regression은 지도학습(supervised learning)에 해당하는 문제이다.

&nbsp;

# <span style="font-size: 18pt;"><strong>1. Hypothesis Function</strong></span>

* * *

**Hypothesis Function이란 주어진 데이터들의 관계식을 나타내는 함수이다.** input이 한 개 인 경우의 Linear Regression 의 hypothesis function의 형태는 다음과 같다.

<img src="https://latex.codecogs.com/gif.latex?\hat&space;{&space;y&space;}&space;=&space;{&space;h&space;}_{&space;\theta&space;}(x)&space;=&space;{&space;\theta&space;}_{&space;0&space;}+{&space;\theta&space;}_{&space;1&space;}x" alt="\hat { y } = { h }_{ \theta }(x) = { \theta }_{ 0 }+{ \theta }_{ 1 }x" align="absmiddle" /> 

헤헤헷 $$\hat { y } = { h }_{ \theta }(x) = { \theta }_{ 0 }+{ \theta }_{ 1 }x$$ 헤헤헷

위는 고등학교 때 배운 1차함수식이다. model이 주어진 훈련 dataset을 얼마나 작은 오차로 잘 표현하는지는<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" />(offset) 와<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" /> (기울기)의 값에 달려있다.<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" /> **와 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" />은 모델을 결정하는 인자로서 parameter라고 한다**.

![image](/images/2018/08/1.png){: width="50%" height="50%"}

&nbsp;

**regression은 결과를 피드백 해주어 함수의 parameter값을 최적화 하는 방법이다. 결국 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" />와<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" />  parameter들의 best 값을 찾아가는 일이다**. 위 그림에서 데이터를 가장 잘 따르는 parameter는<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}=1" alt="\dpi{120} \theta_{0}=1" align="absmiddle" /> ,<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}=\frac{1}{2}" alt="\dpi{120} \theta_{1}=\frac{1}{2}" align="absmiddle" />  이다. 어떤 parameter가 좋은 모델을 만드는지 평가할 수 있는 지표가 필요한데 그것이 바로 cost function이다.

&nbsp;

&nbsp;

# <span style="font-size: 18pt;"><strong>2. Cost Function</strong></span>

* * *

모델과 훈련 dataset 사이의 오차를 나타내주는 함수가 바로 Cost Function이다. Cost Function은 수식적으로 어떻게 표현되는지 보자.

<img src="https://latex.codecogs.com/gif.latex?J({&space;\theta&space;}_{&space;0&space;},{&space;\theta&space;}_{&space;1&space;})\quad&space;=\quad&space;\frac&space;{&space;1&space;}{&space;2m&space;}&space;\sum&space;_{&space;i=1&space;}^{&space;m&space;}{&space;{&space;({&space;\hat&space;{&space;y&space;}&space;}_{&space;i&space;}-{&space;y&space;}_{&space;i&space;})&space;}^{&space;2&space;}\quad&space;}&space;=\quad&space;\frac&space;{&space;1&space;}{&space;2m&space;}&space;\sum&space;_{&space;i=1&space;}^{&space;m&space;}{&space;{&space;({&space;h&space;}_{&space;\theta&space;}({&space;x&space;}_{&space;i&space;})-{&space;y&space;}_{&space;i&space;})&space;}^{&space;2&space;}&space;}&space;\quad&space;=\quad&space;\frac&space;{&space;1&space;}{&space;2m&space;}&space;\sum&space;_{&space;i=1&space;}^{&space;m&space;}{&space;{&space;({&space;\theta&space;}_{&space;0&space;}+{&space;\theta&space;}_{&space;1&space;}{&space;x&space;}_{&space;i&space;}-{&space;y&space;}_{&space;i&space;})&space;}^{&space;2&space;}&space;}" alt="J({ \theta }_{ 0 },{ \theta }_{ 1 })\quad =\quad \frac { 1 }{ 2m } \sum _{ i=1 }^{ m }{ { ({ \hat { y } }_{ i }-{ y }_{ i }) }^{ 2 }\quad } =\quad \frac { 1 }{ 2m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }_{ i })-{ y }_{ i }) }^{ 2 } } \quad =\quad \frac { 1 }{ 2m } \sum _{ i=1 }^{ m }{ { ({ \theta }_{ 0 }+{ \theta }_{ 1 }{ x }_{ i }-{ y }_{ i }) }^{ 2 } }" align="absmiddle" /> 

여기서 m은 훈련시킨 데이터의 총 갯수(size of the training data set)이다. **간단히 말해서 error = 예측값 &#8211; 데이터의 결과값 이라고 했을때 모든 training data sample에 대해서 error의 제곱을 더한 값 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\sum&space;error^{2}" alt="\dpi{120} \sum error^{2}" align="absmiddle" /> 을 총 훈련 데이터의 갯수 m으로 나눠서 평균값을 구한것이(사실 m이 아니라 2m으로 나누지만) cost function이다.** **cost function의 크기가 작을수록 실제와의 오차가 작은 모델이다.** m이 아닌 1/2m로 나누는 이유는 나중에 미분을 하였을 때 튀어나오는 2를 없애고 수식을 조금이나마 더 간단히 하기위함이다.

![image](/images/2018/08/2.jpg){: width="50%" height="50%"}

위 그림에는 5개의 데이터 set이 있다. 예측값과 실제 결과값 사이의 에러를 e1, e2, e3, e4, e5라 했을 때 각각의 차이의 제곱을 모두 더한 값이 cost값이다. 이 cost값은<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" /> 와<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" /> 에 의해 변한다. 아까 말한 linear regression 방법을 써 최적의 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" /> 와<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" /> 을 찾을 수 있다.

&nbsp;

&nbsp;

# <span style="font-size: 18pt;"><strong>3. Gradient Descent</strong></span>

* * *

Gradient Descent라는 것은 어떠한 함수가 최소값을 가지게하는 point를 찾아가는 일종의 수학 테크닉이다.이 gradient descent를 통해서 cost function이 최소가 되게하는<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(\theta_{0},\theta_{1})" alt="\dpi{120} (\theta_{0},\theta_{1})" align="absmiddle" />을 구할 수 있다.

graident descent의 기본적인 idea는 어떠한 함수의 gradient가 가지는 성질중 한 가지로부터 기인한다. cost function J는<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" /> 와<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" /> 의 함수이다. 이 함수의 gradient는 다음과 같다.

<img src="https://latex.codecogs.com/gif.latex?\triangledown&space;J({&space;\theta&space;}_{&space;0&space;},{&space;\theta&space;}_{&space;1&space;})\quad&space;=\quad&space;(\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;0&space;}&space;}&space;,\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;1&space;}&space;}&space;)" alt="\triangledown J({ \theta }_{ 0 },{ \theta }_{ 1 })\quad =\quad (\frac { \partial J }{ \partial { \theta }_{ 0 } } ,\frac { \partial J }{ \partial { \theta }_{ 1 } } )" align="absmiddle" /> 

어떠한 함수의 gradient는하나의 vector이다. **gradient 벡터의 특징은 그 좌표에서 함수가 가장 빠르게 증가하는 방향을 가르킨다.**

cost function J의 gradient를 이용하여 J가 줄어드는 방향으로<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(\theta_{0},\theta_{1})" alt="\dpi{120} (\theta_{0},\theta_{1})" align="absmiddle" /> 을 움직여 새로운<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(\theta_{0},\theta_{1})" alt="\dpi{120} (\theta_{0},\theta_{1})" align="absmiddle" /> 을 얻으려 한다. 이 때 새로운<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(\theta_{0},\theta_{1})" alt="\dpi{120} (\theta_{0},\theta_{1})" align="absmiddle" /> 은 다음과 같이 결정된다.

<img src="https://latex.codecogs.com/gif.latex?({&space;\theta&space;}_{&space;0&space;},{&space;\theta&space;}_{&space;1&space;})&space;:=&space;({&space;\theta&space;}_{&space;0&space;},{&space;\theta&space;}_{&space;1&space;})-&space;\alpha&space;\triangledown&space;J({&space;\theta&space;}_{&space;0&space;},{&space;\theta&space;}_{&space;1&space;})&space;=&space;({&space;\theta&space;}_{&space;0&space;}-\alpha&space;\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;0&space;}&space;}&space;,&space;{&space;\theta&space;}_{&space;1&space;}-\alpha&space;\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;1&space;}&space;}&space;)" alt="({ \theta }_{ 0 },{ \theta }_{ 1 }) := ({ \theta }_{ 0 },{ \theta }_{ 1 })- \alpha \triangledown J({ \theta }_{ 0 },{ \theta }_{ 1 }) = ({ \theta }_{ 0 }-\alpha \frac { \partial J }{ \partial { \theta }_{ 0 } } , { \theta }_{ 1 }-\alpha \frac { \partial J }{ \partial { \theta }_{ 1 } } )" align="absmiddle" /> 

이 식에 의해 계산된 새로운<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(\theta_{0},\theta_{1})" alt="\dpi{120} (\theta_{0},\theta_{1})" align="absmiddle" /> 는 gradient 벡터의 성질때문에 cost function이 작아지는 방향으로 이동한다. 이해를 돕기위해 다음 그림을 보자.

![image](/images/2018/08/3.png){: width="50%" height="50%"}

적당한 alpha라는 factor를 곱하여 기울기의 반대방향으로 이동시키면 점의 위치는 local minimum 방향으로 이동하게 된다. 어떤<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(\theta_{0},\theta_{1})" alt="\dpi{120} (\theta_{0},\theta_{1})" align="absmiddle" /> 을 잡아도 위의 공식에 따르면 local minimum의 방향으로 움직이게 된다. **이 작업을 cost function의 값이 수렴할 때 까지 충분한 횟수로 반복해주면 되는것이다.**

이 때 alpha가 너무 작으면 반복횟수가 커져야하고 학습속도가 느려진다. alpha가 너무 크면 진동하는 경향이 커져 수렴하지 못할 수가 있다. **alpha는** **학습속도를 결정하므로 learning rate라고 부른다.**

다시 위에서 나온 식을 마저 정리해보자. J의<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" /> 와<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" /> 에 대한 편미분은 위의 hypothesis function과 cost function의 식을 적용해서 구하면 다음과 같이 정리된다.

&nbsp;

<img src="https://latex.codecogs.com/gif.latex?\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;0&space;}&space;}&space;\quad&space;=\quad&space;\frac&space;{&space;1&space;}{&space;m&space;}&space;\sum&space;_{&space;i=1&space;}^{&space;m&space;}{&space;{&space;({&space;\theta&space;}_{&space;0&space;}+{&space;\theta&space;}_{&space;1&space;}{&space;x&space;}_{&space;i&space;}-{&space;y&space;}_{&space;i&space;})&space;}&space;}&space;\quad&space;=\quad&space;\frac&space;{&space;1&space;}{&space;m&space;}&space;\sum&space;_{&space;i=1&space;}^{&space;m&space;}{&space;{&space;({&space;h&space;}_{&space;\theta&space;}({&space;x&space;}_{&space;i&space;})-{&space;y&space;}_{&space;i&space;})&space;}&space;}" alt="\frac { \partial J }{ \partial { \theta }_{ 0 } } \quad =\quad \frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ \theta }_{ 0 }+{ \theta }_{ 1 }{ x }_{ i }-{ y }_{ i }) } } \quad =\quad \frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }_{ i })-{ y }_{ i }) } }" align="absmiddle" /> 

<img src="https://latex.codecogs.com/gif.latex?\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;1&space;}&space;}&space;\quad&space;=\quad&space;\frac&space;{&space;1&space;}{&space;m&space;}&space;\sum&space;_{&space;i=1&space;}^{&space;m&space;}{&space;{&space;({&space;\theta&space;}_{&space;0&space;}+{&space;\theta&space;}_{&space;1&space;}{&space;x&space;}_{&space;i&space;}-{&space;y&space;}_{&space;i&space;})\cdot&space;}{&space;x&space;}_{&space;i&space;}&space;}&space;\quad&space;=\quad&space;\frac&space;{&space;1&space;}{&space;m&space;}&space;\sum&space;_{&space;i=1&space;}^{&space;m&space;}{&space;{&space;({&space;h&space;}_{&space;\theta&space;}({&space;x&space;}_{&space;i&space;})-{&space;y&space;}_{&space;i&space;})\cdot&space;}&space;}&space;{&space;x&space;}_{&space;i&space;}" alt="\frac { \partial J }{ \partial { \theta }_{ 1 } } \quad =\quad \frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ \theta }_{ 0 }+{ \theta }_{ 1 }{ x }_{ i }-{ y }_{ i })\cdot }{ x }_{ i } } \quad =\quad \frac { 1 }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }_{ i })-{ y }_{ i })\cdot } } { x }_{ i }" align="absmiddle" /> 

이 식을 적용하면 다음과 같은 최종식이 나온다.

<img src="https://latex.codecogs.com/gif.latex?\\({&space;\theta&space;}_{&space;0&space;},{&space;\theta&space;}_{&space;1&space;})&space;:=&space;({&space;\theta&space;}_{&space;0&space;}-\alpha&space;\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;0&space;}&space;}&space;,&space;{&space;\theta&space;}_{&space;1&space;}-\alpha&space;\frac&space;{&space;\partial&space;J&space;}{&space;\partial&space;{&space;\theta&space;}_{&space;1&space;}&space;}&space;)&space;\\=&space;\&space;({&space;\theta&space;}_{&space;0&space;}-\frac&space;{&space;\alpha&space;}{&space;m&space;}&space;\sum&space;_{&space;i=1&space;}^{&space;m&space;}{&space;{&space;({&space;h&space;}_{&space;\theta&space;}({&space;x&space;}_{&space;i&space;})-{&space;y&space;}_{&space;i&space;})&space;}&space;}&space;,{&space;\theta&space;}_{&space;1&space;}-\frac&space;{&space;\alpha&space;}{&space;m&space;}&space;\sum&space;_{&space;i=1&space;}^{&space;m&space;}{&space;{&space;(({&space;h&space;}_{&space;\theta&space;}({&space;x&space;}_{&space;i&space;})-{&space;y&space;}_{&space;i&space;})&space;}&space;}&space;{&space;\cdot&space;x&space;}_{&space;i&space;})" alt="\\({ \theta }_{ 0 },{ \theta }_{ 1 }) := ({ \theta }_{ 0 }-\alpha \frac { \partial J }{ \partial { \theta }_{ 0 } } , { \theta }_{ 1 }-\alpha \frac { \partial J }{ \partial { \theta }_{ 1 } } ) \\= \ ({ \theta }_{ 0 }-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { ({ h }_{ \theta }({ x }_{ i })-{ y }_{ i }) } } ,{ \theta }_{ 1 }-\frac { \alpha }{ m } \sum _{ i=1 }^{ m }{ { (({ h }_{ \theta }({ x }_{ i })-{ y }_{ i }) } } { \cdot x }_{ i })" align="absmiddle" /> 

이 최종식을 적용하여<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" /> 와<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" /> 이 수렴할 때 까지 update를 해주면 데이터들에 가장 fit하는 모델의<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" /> 와<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" /> 이 나온다. alpha값이 고정되어있어도 local minimum에 접근할수록 기울기가 작아진다. 따라서 자동적으로 더 작은 step씩 이동하게되고 수렴할 수 있게 되는것이다.

&nbsp;

&nbsp;

# **4. Summary**

* * *

Linear Regression withe One Variable의 내용을 정리해보자.

<span style="font-size: 12pt;"><strong>1) 1차함수 형태의 hypothesis function을 세우고 무작위로 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" />과 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" />을 set한다.</strong></span>

<span style="font-size: 12pt;"><strong>2) learning rate alpha를 정하고 위의 update식에 따라 model parameter(<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" /> <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" />)을 반복 학습시킨다. 이를 gradient descent(경사하강법)이라고 한다.</strong></span>

<span style="font-size: 12pt;"><strong>3) <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;J(\theta)" alt="\dpi{120} J(\theta)" align="absmiddle" />가 수렴했으면 지금의 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{0}" alt="\dpi{120} \theta_{0}" align="absmiddle" />와 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{1}" alt="\dpi{120} \theta_{1}" align="absmiddle" />을</strong> 이<strong> model parameter로 사용한다</strong></span>