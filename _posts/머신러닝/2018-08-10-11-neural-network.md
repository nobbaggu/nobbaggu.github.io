---
title: (머신러닝) 11. 신경망 (Neural Network)
date: 2018-08-10T08:36:24+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - back propagation
  - deep learning
  - hidden layer
  - input layer
  - neural network
  - output layer
  - perceptron
  - 뉴럴 네트워크
  - 딥러닝
  - 신경망
  - 역전파
  - 은닉층
  - 입력층
  - 출력층
  - 퍼셉트론
  - 히든층
---
신경망 알고리즘은 비선형적인 문제를 다루기에 매우 적합하다. Regression 모델은 feature의 갯수가 늘어나면서 복잡도가 높아질수록 힘을 잃는다. 가령 3차항을 포함하는 수식을 만들려고하는데 feature가 100개라면 모델의 항은 대략 17만개정도가 나온다. 특히 컴퓨터 비전처리같은 경우 10000개의 pixel로 된 이미지를 분류한다하면 몇백만, 몇천만개의 term까지 늘어나버린다. 더 이상 regression으로 해결할 수 있는 문제가 아니다.

요약하자면 <span style="color: #ff0000;"><b>feature의 갯수가 많고 nonlinear한 경우 regression으로 문제를 해결하는데에는 한계가 있다.</b></span> 그리고 많은 머신러닝 문제들에서 feature의 갯수는 아주 많다.

지금부터 이야기할 <span style="color: #ff0000; font-size: 14pt;"><b>인공 신경망</b></span>은 인공 신경망은 복잡하고 nonlinear한 문제를 다루기에 적합하다.

&nbsp;

&nbsp;

# 1. 신경세포의 모방

* * *

신경망 알고리즘은 동물의 뇌가 학습하는 과정의 아주 제한적인 부분을 그대로 모방하려 한 것이 그 시작이다.

일단 동물의 신경세포 하나가 신호를 처리하는 과정을 간단히 보자. 수상돌기로 전기적인 신호(입력)이 들어오면 뉴런이 학습한 알고리즘에 따라 신호를 처리하여 축색돌기로 전기적인신호(출력)을 낸다. 이런 뉴런들이 아주 많이 모여서 집합을 이루는 것이 신경망이다.

![image](/images/2018/08/KakaoTalk_20180722_194509991.png){: width="50%" height="50%"}

![image](/images/2018/08/KakaoTalk_20180722_194509991-1.png){: width="50%" height="50%"}

우리가 구현할 인공 신경망에서는 <span style="color: #ff0000;"><b>feature들이 입력</b></span>으로 들어와 중간단계를 거치고 마지막으로 계산되어서 나오는 <span style="color: #ff0000;"><b>hypothesis function의 결과값이 출력</b></span>이 된다. 그리고 인공신경망의 중간에 추가적인 몇겹의 층(layer)이 더 있고 몇 층을 더 사용할지는 우리가 결정할 일이다.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# 2. 퍼셉트론(perceptron)

* * *

신경망의 시작은 <span style="color: #ff0000;"><b>perceptron(퍼셉트론)</b></span>이다. perceptron은 하나의 신경 세포라고 보면 된다. 아주 많은 신경 세포가 군집을 이룬 것이 신경망 이다. perceptron이 어떻게 동작하는지 알면 인공신경망의 동작도 이해할 수 있다. 수식적인 복잡성만 더해질 뿐이다. perceptron의 동작은 하나의 간단한 알고리즘을 따른다.

![image](/images/2018/08/no-name-27.png){: width="50%" height="50%"}

각각의 입력이 가중치에 의해 곱해져서 나온 값을 다 더한다. 그리고 거기에 어떠한 bias term(DC term)이 더해진다. 그리고 이 sum값이 perceptron에 들어가면 activation function에 의해 활성화된 최종값이 나오게 된다. 이 activation function은 sigmoid function이다. 이러한 activation function은 결과값의 범위를 제한하여 수식적 편의성을 제공한다. 하지만 하나의 간단한 perceptron 알고리즘으로 해결할 수 있는 문제는 너무 제한적이다. 따라서 더 복잡한 알고리즘을 구현하기 위해 perceptron들을 여러 층 쌓은 Multi Layer Perceptron Model이 나왔고 이것이 현재의 신경망 모델이다.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# 3. 신경망 모델

* * *

<img src="https://latex.codecogs.com/gif.latex?x\rightarrow&space;[\quad&space;]\rightarrow&space;{&space;h&space;}_{&space;\theta&space;}(x)" alt="x\rightarrow [\quad ]\rightarrow { h }_{ \theta }(x)" align="absmiddle" /> 

위 수식이 무엇을 의미하는가? 머신러닝 알고리즘은 결국에는 input x에 대한 출력 <img src="https://latex.codecogs.com/gif.latex?h_{\theta}(x)" alt="h_{\theta}(x)" align="absmiddle" /> 를 얻어내는 것이다. linear regression이든, classification이든 각자만의 알고리즘에 의해 input x에 대한<img src="https://latex.codecogs.com/gif.latex?h_{\theta}(x)" alt="h_{\theta}(x)" align="absmiddle" /> 를 구해냈다. neural network도 마찬가지다. 먼저 modeling을 하기 전 neural network를 직관적으로 표현하는 그림을 보자.

![image](/images/2018/08/no-name-28.png){: width="50%" height="50%"}

퍼셉트론이 여러층을 쌓은 것일 뿐이다. 이 때에는 하나의 layer를 하나의 vector로 보고 node와 node사이의 계산과정에 필요한 parameter들은 하나의 matrix(행렬)로 묶어서 다루면 된다.

한 layer에서 다음 layer의 node가 가지는 값들이 계산되는 과정은 다음과 같다.

&nbsp;

**⇒**  <span style="color: #ff0000; font-size: 14pt;"><strong>1. </strong><strong>p</strong><strong>arameter matrix를 곱해준다.</strong></span>

<span style="color: #ff0000; font-size: 14pt;"><strong>      2. sigmoid함수를 씌운다.</strong></span>

&nbsp;

무슨말이냐? 아래의 예시를 보면서 설명하겠다.

&nbsp;

<img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;{&space;x&space;}_{&space;0&space;}\\&space;{&space;x&space;}_{&space;1&space;}\\&space;{&space;x&space;}_{&space;2&space;}\\&space;{&space;x&space;}_{&space;3&space;}&space;\end{bmatrix}&space;\rightarrow&space;\begin{bmatrix}&space;{&space;a&space;}_{&space;0&space;}\\&space;{&space;a&space;}_{&space;1&space;}\\&space;{&space;a&space;}_{&space;2&space;}&space;\end{bmatrix}&space;\rightarrow&space;{&space;h&space;}_{&space;\theta&space;}(x)" alt="\begin{bmatrix} { x }_{ 0 }\\ { x }_{ 1 }\\ { x }_{ 2 }\\ { x }_{ 3 } \end{bmatrix} \rightarrow \begin{bmatrix} { a }_{ 0 }\\ { a }_{ 1 }\\ { a }_{ 2 } \end{bmatrix} \rightarrow { h }_{ \theta }(x)" align="absmiddle" /> 

&nbsp;

먼저 x로 구성된 입력 부분을<span style="color: #ff0000;"> <b>input layer</b></span>라고 하고 마지막의 결과 부분을 **<span style="color: #ff0000;">output</span> <span style="color: #ff0000;">layer</span>**라고 한다. 그리고 중간에 있는 층을 **<span style="color: #ff0000;">hidden</span> <span style="color: #ff0000;">layer</span>**라고 한다. hidden layer라고 부르는 이유는 간단하다. 우리가 결국 보는것은 어떤 알고리즘에 집어넣는 &#8216;입력&#8217;과 나오는 &#8216;출력&#8217; 뿐이기 때문이다. 따라서 중간에 있는 층은 숨어있다고 해서 hidden layer라고 부르는 것이다. 이 hidden layer는 2개이상이 될 수도 있다. 이 예시에서는 hidden layer가 한 층이다. 또한 각 layer의 요소 하나하나를 <span style="color: #ff0000;"><b>unit</b></span>혹은 <span style="color: #ff0000;"><b>node</b></span>라고 부른다.

**<span style="color: #ff0000;">모든 layer는 다음 layer로 전파되기 이전에 첫 번째 node로 항상 1을 추가한다</span>**. 이는 가중치 matrix가 w0를 가지기 때문에 행렬의 곱셈을 위한 dimension을 맞추기 위함이다.

<img src="https://latex.codecogs.com/gif.latex?g(\begin{bmatrix}&space;\Theta_{10}^{(1)}&space;&\Theta_{11}^{(1)}&space;&\Theta_{12}^{(1)}&space;&&space;\Theta_{13}^{(1)}\\&space;\Theta_{20}^{(1)}&space;&\Theta_{21}^{(1)}&space;&\Theta_{22}^{(1)}&space;&&space;\Theta_{23}^{(1)}&space;\end{bmatrix}&space;\begin{bmatrix}&space;x_{0}&space;\\&space;x_{1}&space;\\&space;x_{2}&space;\\&space;x_{3}&space;\end{bmatrix})&space;=&space;\begin{bmatrix}&space;a_{1}&space;\\&space;a_{2}&space;\end{bmatrix}\rightarrow&space;\begin{bmatrix}&space;a_{0}&space;\\&space;a_{1}&space;\\&space;a_{2}&space;\end{bmatrix}&space;\\&space;sigmoid&space;function&space;:&space;g(z)&space;=&space;\frac{1}{1+e^{-z}}" alt="g(\begin{bmatrix} \Theta_{10}^{(1)} &\Theta_{11}^{(1)} &\Theta_{12}^{(1)} & \Theta_{13}^{(1)}\\ \Theta_{20}^{(1)} &\Theta_{21}^{(1)} &\Theta_{22}^{(1)} & \Theta_{23}^{(1)} \end{bmatrix} \begin{bmatrix} x_{0} \\ x_{1} \\ x_{2} \\ x_{3} \end{bmatrix}) = \begin{bmatrix} a_{1} \\ a_{2} \end{bmatrix}\rightarrow \begin{bmatrix} a_{0} \\ a_{1} \\ a_{2} \end{bmatrix} \\ sigmoid function : g(z) = \frac{1}{1+e^{-z}}" align="absmiddle" /> 

&nbsp;

<span style="color: #ff0000;"><b>1. 이전 layer에 parameter matrix를 곱한다.</b></span>

<span style="color: #ff0000;"><b>   </b></span>

<span style="color: #ff0000;"><b>2. activation function을 씌운다.</b></span>

&nbsp;

<span style="color: #ff0000;"><b>3. output layer가 아니면 a0 ==1 을 추가한 이후 다음 layer로 전파</b></span>

&nbsp;

다음 layer로 전파되기에 앞서 추가되는 1의 값을 가지는 node를 <span style="color: #ff0000;"><strong>bias unit</strong></span> 이라고 부른다고 했다. 이것이 θ0와 곱해져 일종의 DC성분을 만들어낸다. 가령 집값 예측 모델의 경우 feature에 추가된 1은 θ0와 곱해져 DC성분을 만들어내며 집값에 항상 일정한 최소값을 부여한다. 일단 집이라고 하면 기본적으로 θ0만큼의 가격에서 시작하여 각 feature들에 따라 변한다고 해석할 수 있는것이다.

위의 과정을 간단히 나타내면 다음과 같이 된다.

<img src="https://latex.codecogs.com/gif.latex?g({&space;\Theta&space;}^{&space;(j)&space;}\cdot&space;{&space;a&space;}^{&space;(j)&space;})={&space;a&space;}^{&space;(j+1)&space;}" alt="g({ \Theta }^{ (j) }\cdot { a }^{ (j) })={ a }^{ (j+1) }" align="absmiddle" /> 

&nbsp;

※ parameter matrix의 행/열의 수는 다음의 규칙을 기억하면 알기 쉽다.

지금의 layer의 unit수가 j이고 다음 layer의 unit수가 k라면(j, k 모두 DC성분인 index 0항을 빼고 계산한 것이다.) **k × (j+1)** 행렬이 된다. j에 +1이 더해지는 것은 bias unit이 추가되었기 때문이다.

&nbsp;

output layer로 전파되는 경우에도

<img src="https://latex.codecogs.com/gif.latex?g(\begin{bmatrix}&space;\Theta_{10}^{(2)}&space;&\Theta_{11}^{(2)}&space;&\Theta_{12}^{(2)}&space;\end{bmatrix}&space;\begin{bmatrix}&space;a_{0}&space;\\&space;a_{1}&space;\\&space;a_{2}&space;\end{bmatrix})=&space;h_{\Theta}(x)" alt="g(\begin{bmatrix} \Theta_{10}^{(2)} &\Theta_{11}^{(2)} &\Theta_{12}^{(2)} \end{bmatrix} \begin{bmatrix} a_{0} \\ a_{1} \\ a_{2} \end{bmatrix})= h_{\Theta}(x)" align="absmiddle" /> 

여기에서는 output의 갯수(k)가 1이고 곱해지는 hidden layer의 성분의 수가 3(j)+1개 이기 때문에 1×4 행렬이 된다. 이렇게 해서 최종적으로 output이 나오는 것이다.

위의 수식들을 그림으로 나타내면 다음과 같다.

![image](/images/2018/08/no-name-29.png){: width="50%" height="50%"}

hidden layer의 갯수가 2층 이상인 경우는 이 과정의 반복일 뿐이다.

이는 동물 뇌신경이 정보를 전다하는 과정과 흡사하다. node와 node를 잊는 wire는 neuron과 neuron 사이의 신경섬유에 해당한다.

사실 항상 hypothesis output의 갯수가 1개이지는 않다. multiclass classification의 경우에는 마지막 output이 class의 갯수만큼 될 것이다.

<img src="https://latex.codecogs.com/gif.latex?\begin{bmatrix}&space;x_{0}&space;\\&space;x_{1}&space;\\&space;x_{2}&space;\\&space;x_{3}&space;\end{bmatrix}&space;\rightarrow&space;\begin{bmatrix}&space;a_{0}^{(2)}&space;\\&space;a_{1}^{(2)}&space;\\a_{2}^{(2)}&space;\\a_{3}^{(2)}&space;\\a_{4}^{(2)}&space;\end{bmatrix}&space;\rightarrow&space;\begin{bmatrix}&space;a_{0}^{(3)}&space;\\&space;a_{1}^{(3)}&space;\\a_{2}^{(3)}&space;\\a_{3}^{(3)}&space;\end{bmatrix}&space;\rightarrow&space;\cdot&space;\cdot&space;\cdot&space;\rightarrow&space;\begin{bmatrix}&space;h_{\Theta}(x)_{1}&space;\\&space;h_{\Theta}(x)_{2}&space;\\&space;h_{\Theta}(x)_{3}&space;\\&space;h_{\Theta}(x)_{4}\end{bmatrix}" alt="\begin{bmatrix} x_{0} \\ x_{1} \\ x_{2} \\ x_{3} \end{bmatrix} \rightarrow \begin{bmatrix} a_{0}^{(2)} \\ a_{1}^{(2)} \\a_{2}^{(2)} \\a_{3}^{(2)} \\a_{4}^{(2)} \end{bmatrix} \rightarrow \begin{bmatrix} a_{0}^{(3)} \\ a_{1}^{(3)} \\a_{2}^{(3)} \\a_{3}^{(3)} \end{bmatrix} \rightarrow \cdot \cdot \cdot \rightarrow \begin{bmatrix} h_{\Theta}(x)_{1} \\ h_{\Theta}(x)_{2} \\ h_{\Theta}(x)_{3} \\ h_{\Theta}(x)_{4}\end{bmatrix}" align="absmiddle" /> 

위의 식처럼 말이다. 위의 hidden layer의 unit들 위에 붙어있는 (2) (3)같은 위첨자는 hidden layer가 전체 인공신경망에서 몇 번째 layer인지 구분하기 위해 넣은 것이다.

또한 위의 식처럼 class가 총 4개인 경우 실제 output vector y는 다음과 같은 값들을 가질 수 있다.

<img src="https://latex.codecogs.com/gif.latex?y=\begin{bmatrix}&space;1\\&space;0\\&space;0\\&space;0&space;\end{bmatrix},&space;\begin{bmatrix}&space;0\\&space;1\\&space;0\\&space;0&space;\end{bmatrix},&space;\begin{bmatrix}&space;0\\&space;0\\&space;1\\&space;0&space;\end{bmatrix},&space;\begin{bmatrix}&space;0\\&space;0\\&space;0\\&space;1&space;\end{bmatrix}" alt="y=\begin{bmatrix} 1\\ 0\\ 0\\ 0 \end{bmatrix}, \begin{bmatrix} 0\\ 1\\ 0\\ 0 \end{bmatrix}, \begin{bmatrix} 0\\ 0\\ 1\\ 0 \end{bmatrix}, \begin{bmatrix} 0\\ 0\\ 0\\ 1 \end{bmatrix}" align="absmiddle" /> 

&nbsp;

&nbsp;

&nbsp;

&nbsp;

# 4. 신경망 예제

* * *

간단한 예제를 하나 보자. 전자회로, 디지털공학에서 가장 먼저 배우는 것은 논리 gate이다. 그 중 몇개의 gate를 신경망 알고리즘으로 구현해보겠다.

![image](/images/2018/08/no-name-30.png){: width="50%" height="50%"}

&nbsp;

두 개의 input이 모두 1일 때에만 output이 1인 AND gate와 하나라도 1이면 output이 1인 OR gate이다. (NOT x1) AND (NOT x2) gate 는 라는 것은 x1과 x2 모두 0이어야  output이 1이 된다.

![image](/images/2018/08/no-name-31.png){: width="50%" height="50%"}

XNOR gate는 어떻게 만들까? 참고로 XNOR gate는 input 두 개가 서로 같을 때에만 output이 1인 논리소자이다. 답은 이미 나와있다. 위의 세 개를 합치면 된다.

![image](/images/2018/08/no-name-33.png){: width="50%" height="50%"}

단지 hidden layer만 하나 추가하였을 뿐인데 XNOR gate를 아주 간단히 모델링하였다. XNOR gate를 classification으로 치면 아래 왼쪽 그림과 같다.

![image](/images/2018/08/no-name-34.png){: width="50%" height="50%"}

XNOR 문제를 modeling 할 수 있다는 것은 오른쪽의 분류문제도 modeling 할 수 있다는 것이다. hidden layer를 추가하거나 node수를 늘려 nonlinearity를 키워주면 된다. <span style="color: #ff0000;"><b>신경망의 hidden layer의 수를 추가할수록 훨씬 더 복잡하고 nonlinear한 학습 알고리즘을 표현할 수 있다.</b></span>

지금은 인공 신경망 model의 수식적인 형태가 어떻게 생겼는지와 어떤 방식으로 동작을 하는지 설명하였을 뿐이다. 신경망의 parameter 학습의 대표적인 방법은 <span style="color: #ff0000;"><b>back propagation</b></span>이라는 일종의 feedback을 이용한 방법인데 이는 다음에 설명하도록 하겠다.