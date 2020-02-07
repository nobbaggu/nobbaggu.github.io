---
title: (머신러닝) 13. 신경망 학습
date: 2018-08-14T20:09:22+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - back propagation
  - forward propagation
  - gradient check
  - hidden layer
  - input layer
  - neural network
  - output layer
  - 신경망
  - 은닉층
  - 인공
  - 입력층
  - 출력층
---
신경망 모델의 전반적인 이론과 파라미터를 학습시키기 위한 역전파의 방법에 대해 알아보았다. 이제 지금까지 배운 내용을 토대로 실제로 신경망 모델을 세우고 parameter를 학습시켜 최적화 시키는 절차에 대해 step by step으로 설명하겠다.

&nbsp;

# **1. 신경망 구조 결정하기**

* * *

  * input layer unit의 수 = feature의 수
  * output layer unit의 수 = class의 수
  * hidden layer : 층의 수 = default 1 layer, 많으면 많을수록 더 정확한 예측 가능. hidden layer가 2층 이상일 때에는 모든 층이 같은 갯수의 unit을 가지도록 하는것이 default

&nbsp;

# 

# **2. 초기 parameter값 설정**

* * *

regression 문제처럼 모든 parameter를 0으로 초기화시키면 학습이 이루어지지 않는다. 모든 parameter가 0이든 어떤 수가 되었든 같은 값으로 학습시킨다 생각해보자. 그럼 l-th layer에서 (l+1)-th layer로 forward propagation을 시키면 (l+1)층의 모든 unit이 같은 값을 갖는다. 역시 역방향으로 back propagation을 시킨다 해도 모든 parameter가 같은 값으로 update 되어버린다. 의미없는 model이 만들어져 버리는 것이다. 결론부터 말하자면, 모든 parameter는 어떠한 작은 값<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;[-\epsilon,\epsilon]" alt="\dpi{120} [-\epsilon,\epsilon]" align="absmiddle" /> 범위 내에서 랜덤하게 초기화된다. 방법은 다음과 같다.

먼저, <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Theta^{(l)}" alt="\dpi{120} \Theta^{(l)}" align="absmiddle" /> 행렬은 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;s^{(l+1)}\times&space;(s^{(l)}+1)" alt="\dpi{120} s^{(l+1)}\times (s^{(l)}+1)" align="absmiddle" /> 구조란 것을 기억해야한다. (l+1층의 unit 수)×(l층의 unit 수 +1)

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;\epsilon=\sqrt{\frac{6}{s^{(input-layer)}+s^{(output-layer)}}}\\&space;\\&space;\Theta^{(l)}=2\epsilon&space;\cdot&space;rand(s^{(l+1)},s^{(l)}+1)-\epsilon" alt="\dpi{120} \\ \epsilon=\sqrt{\frac{6}{s^{(input-layer)}+s^{(output-layer)}}}\\ \\ \Theta^{(l)}=2\epsilon \cdot rand(s^{(l+1)},s^{(l)}+1)-\epsilon" align="absmiddle" /> 

<img class="aligncenter size-medium wp-image-435" src="/images/2018/08/no-name-44-300x285.png" alt="" width="300" height="285" srcset="/images/2018/08/no-name-44-300x285.png 300w, /images/2018/08/no-name-44.png 570w" sizes="(max-width: 300px) 100vw, 300px" /> 

위와 같은 신경망의 경우에는

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;\epsilon=\frac{6}{s^{(1)}+s^{(3)}}=\frac{6}{5}=1.2\\&space;\\&space;\Theta^{(1)}=2\cdot&space;1.2&space;\cdot&space;rand(2,4)-1.2=2.4rand(2,4)-1.2\\&space;\\&space;\Theta^{(2)}=2\cdot&space;1.2&space;\cdot&space;rand(2,3)-1.2=2.4rand(2,3)-1.2\\&space;\\" alt="\dpi{120} \\ \epsilon=\frac{6}{s^{(1)}+s^{(3)}}=\frac{6}{5}=1.2\\ \\ \Theta^{(1)}=2\cdot 1.2 \cdot rand(2,4)-1.2=2.4rand(2,4)-1.2\\ \\ \Theta^{(2)}=2\cdot 1.2 \cdot rand(2,3)-1.2=2.4rand(2,3)-1.2\\ \\" align="absmiddle" /> 

rand함수는 octave나 matlab에서 제공한다. python이나 R 등의 다른 프로그래밍 툴 역시 위와 비슷한 함수를 제공할 것이니 manual을 참고하길 바란다.

<img class="aligncenter size-full wp-image-441" src="/images/2018/08/no-name-46.png" alt="" width="423" height="237" srcset="/images/2018/08/no-name-46.png 423w, /images/2018/08/no-name-46-300x168.png 300w" sizes="(max-width: 423px) 100vw, 423px" /> 

위와 같이 초기 Theta값을 계산할 수 있다.

&nbsp;

&nbsp;

&nbsp;

# **3. forward propagation**

* * *

위에서 계산한 초기 Theta값을 가지고 forward propagation을 하여 output layer의 unit 값들과 cost function J를 계산한다.

&nbsp;

&nbsp;

&nbsp;

# **4. back propagation**

* * *

output layer의 결과와 실제 data y의 차이를 error로 두고 back propagation하여 모든 layer의 error를 구한다. 그리고 역전파시켜 최종적으로 cost function J의 gradient  <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{\partial&space;J}{\partial&space;\Theta}" alt="\dpi{120} \frac{\partial J}{\partial \Theta}" align="absmiddle" />를 계산한다.  역전파에 대한 자세한 내용은 이 전의 back propagation을 주제로한 포스팅에 나와있다.

<span style="text-decoration: underline;"><a href="https://SWnomad.com/12-back-propagation역전파-알고리즘/">https://SWnomad.com/12-back-propagation역전파-알고리즘/</a></span>

&nbsp;

&nbsp;

# **5. gradient check**

* * *

역전파로 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{\partial&space;J}{\partial&space;\Theta}" alt="\dpi{120} \frac{\partial J}{\partial \Theta}" align="absmiddle" />를 계산했다고 해도 우리가 제대로 알고리즘을 구현했는지, 혹은 코드를 잘못 짜지는 않았는지 확인할 필요가 있다. 이 때에 쓰는 방법이 gradient check이다. 다음 그림을 보자.

<img class="aligncenter size-full wp-image-447" src="/images/2018/08/no-name-47.png" alt="" width="512" height="256" srcset="/images/2018/08/no-name-47.png 512w, /images/2018/08/no-name-47-300x150.png 300w" sizes="(max-width: 512px) 100vw, 512px" /> 

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta-\epsilon" alt="\dpi{120} \theta-\epsilon" align="absmiddle" /> 에서<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta+\epsilon" alt="\dpi{120} \theta+\epsilon" align="absmiddle" />  사이의 함수 f의 평균 변화량은  <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{f(\theta+\epsilon)-f(\theta-\epsilon)}{2\epsilon}" alt="\dpi{120} \frac{f(\theta+\epsilon)-f(\theta-\epsilon)}{2\epsilon}" align="absmiddle" />가 된다. 만약 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\epsilon" alt="\dpi{120} \epsilon" align="absmiddle" />이 충분히 작다면 이 값은 θ에서의 접선의 기울기와 거의 같을 것이다. 위와 같은 idea를 이용하여 우리가 gradient를 제대로 구했는지 체크할 수 있다. 참고로 여기서의 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\epsilon" alt="\dpi{120} \epsilon" align="absmiddle" />은 parameter를 초기화 할 때 사용한 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\epsilon" alt="\dpi{120} \epsilon" align="absmiddle" />과 아무 상관 없는 수이다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{\partial&space;J}{\partial&space;\Theta_{j}}\approx&space;\frac{J(\Theta_{1},\Theta_{2},...,\Theta_{j}+\epsilon,...,\Theta_{n})-J(\Theta_{1},\Theta_{2},...,\Theta_{j}-\epsilon,...,\Theta_{n})}{2\epsilon}" alt="\dpi{120} \frac{\partial J}{\partial \Theta_{j}}\approx \frac{J(\Theta_{1},\Theta_{2},...,\Theta_{j}+\epsilon,...,\Theta_{n})-J(\Theta_{1},\Theta_{2},...,\Theta_{j}-\epsilon,...,\Theta_{n})}{2\epsilon}" align="absmiddle" /> 

back propagation을 통해 numeric하게 계산한 왼쪽식이 analytical하게 구한 오른쪽 식과 거의 차이가 없어야한다. 보통 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\epsilon" alt="\dpi{120} \epsilon" align="absmiddle" />은 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;10^{-4}" alt="\dpi{120} 10^{-4}" align="absmiddle" />정도를 쓰면 적당하다.

아무튼 gradient check를 통해서 우리가 정말로 gradient를 제대로 구했는지 확인이 되면 걱정말고 gradient descent나 optimization function을 써서 학습만 시키면 된다.

&nbsp;

&nbsp;

# **6. parameter update하기**

* * *

back propagation으로 cost function J의 gradient, 즉 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{\partial&space;J}{\partial&space;\Theta}" alt="\dpi{120} \frac{\partial J}{\partial \Theta}" align="absmiddle" />를 구했으면 gradient descent나 다른 advanced optimization function을 이용하면 된다. 개인적으로 gradient descent보다는 numeric computation의 전문가들이 잘 만들어놓은 라이브러리에서 함수를 가져다 쓰는걸 추천한다. 예를들어 octave에는 fminunc나 fmincg함수등이 있다. cost function J와 위에서 계산한 gradient만 입력해주면 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Theta" alt="\dpi{120} \Theta" align="absmiddle" />에 대해 cost function J를 최소화 시켜주면서 가장 최적화된 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Theta" alt="\dpi{120} \Theta" align="absmiddle" />를 계산해준다.

&nbsp;

이상 위의 과정을 거치면 신경망 알고리즘으로 model이 만들어지는 것이다.

&nbsp;