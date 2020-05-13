---
title: (머신러닝) (ex) linear regression1
date: 2018-08-09T11:00:58+09:00
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
  - 기계학습
  - 머신러닝
  - 비용함수
  - 선형회귀
  - 인공지능
---
예측 문제를  지도학습의 linear regression 를 사용해 풀어보고자 한다.

&nbsp;

<http://people.sc.fsu.edu/~jburkardt/datasets/regression/regression.html>  <<  dataset은 이 링크를 따라가서 구할 수 있ㅆ다.

&nbsp;

x01.txt 파일은 포유동물의 뇌와 몸무게 데이터를 담고있다. dataset을 복사하여 .txt파일의 형태로 저장한다.

![image](/images/2018/08/1-1.png){: width="50%" height="50%"}

가장 앞의 column은 index일 뿐이다. 두 번째 줄은 뇌의 무게, 세 번째 줄은 몸무게인데 뇌의 무게에 따른 몸무게를 예측하는 프로그램이므로 뇌의 무게를 feature로, 몸무게를 output으로 설정하자. 그렇다면 feature 가 1개이므로 모델의 식의 형태는 다음과 같이 될 것이다.

$${ h }_{ \theta }(x)={ \theta }_{ 0 }+{ \theta }_{ 1 }x_{ 1 }={ \theta }_{ 0 }{ x }_{ 0 }+{ \theta }_{ 1 }x_{ 1 }\quad (x_{ 0 }=1\quad for\quad all\quad datasets)$$ 

물론 이는 linear regression one variable이지만 이는 일반적인 linear regression with multiple variables에서 variable이 1개인 경우일 뿐이다. 따라서 이 때에도 선형대수학을 사용해서 다룰 수 있다. 사실 octave같은 계산 툴에서는 선형대수학을 사용하는것이 훨씬 간결하게 식이 표현되기때문에 그렇게 할 것이다. 일단 cost function과 gradient descent를 통한 theta의 update식을 다시 한 번 상기해보자.

$$X = \begin{bmatrix} { x }_{ 0 }^{ (1) } & { x }_{ 1 }^{ (1) }& { x }_{ 2 }^{ (1) }& { x }_{ 3 }^{ (1) }& \cdot & \cdot & \cdot & { x }_{ n }^{ (1) } \\ { x }_{ 0 }^{ (2) }& { x }_{ 1 }^{ (2) }& { x }_{ 2 }^{ (2) }& { x }_{ 3 }^{ (2) }& \cdot & \cdot & \cdot & { x }_{ n }^{ (2) } \\ { x }_{ 0 }^{ (3) }& { x }_{ 1 }^{ (3) }& { x }_{ 2 }^{ (3) }& { x }_{ 3 }^{ (3) }& \cdot & \cdot & \cdot & { x }_{ n }^{ (3) } \\ \cdot \\ \cdot \\ { x }_{ 0 }^{ (m) } & { x }_{ 1 }^{ (m) } & { x }_{ 2 }^{ (m) } & { x }_{ 3 }^{ (m) } & \cdot & \cdot & \cdot & { x }_{ n }^{ (m) } \end{bmatrix}$$$$=\begin{bmatrix} 1 & { x }_{ 1 }^{ (1) }& { x }_{ 2 }^{ (1) }& { x }_{ 3 }^{ (1) }& \cdot & \cdot & \cdot & { x }_{ n }^{ (1) } \\ 1& { x }_{ 1 }^{ (2) }& { x }_{ 2 }^{ (2) }& { x }_{ 3 }^{ (2) }& \cdot & \cdot & \cdot & { x }_{ n }^{ (2) } \\ 1& { x }_{ 1 }^{ (3) }& { x }_{ 2 }^{ (3) }& { x }_{ 3 }^{ (3) }& \cdot & \cdot & \cdot & { x }_{ n }^{ (3) } \\ \cdot \\ \cdot \\ 1 & { x }_{ 1 }^{ (m) } & { x }_{ 2 }^{ (m) } & { x }_{ 3 }^{ (m) } & \cdot & \cdot & \cdot & { x }_{ n }^{ (m) } \end{bmatrix}$$ 

$$\theta = \begin{bmatrix} { \theta }_{ 0 }\\ { \theta }_{ 1 }\\ { \theta }_{ 2 }\\ \cdot \\ \cdot \\ \cdot \\ { \theta }_{ n } \end{bmatrix} \quad y= \begin{bmatrix} y^{(1)} \\ y^{(2)} \\ y^{(3)} \\ \cdot \\ \cdot \\ \cdot \\ y^{(m)} \end{bmatrix}$$ 

&nbsp;

$${ h }_{ \theta }(x)=X\theta \\ J(\theta ) = \frac { 1 }{ 2m } { (X\theta - y) }^{ T }(X\theta - y) \\ \theta := \theta - \frac { \alpha }{ m } { X }^{ T }(X\theta-y)$$ 

&nbsp;

![image](/images/2018/08/2.png){: width="50%" height="50%"}

2열이 feature고 3열이 output이다. m = length(y) 는 데이터의 갯수이다. theta_0에 match되는 1로 구성된 벡터를 한 줄 추가해서 feature matrix X를 만들었다. parameter 초기값은 (0,0)으로 설정했다. 그리고나서 공식에 따라 cost function을 구해보았는데 엄청 크다.

![image](/images/2018/08/s.png){: width="50%" height="50%"}

learning rate인 alpha를 0.00001로 set을하고 theta를 update시키면서 gradient descent를 두 번 해보았는데. 오히려 cost function이 커진다. alpha가 너무 큰것이다. 계속 이렇게 하려니 귀찮다. 아예 함수를 외부에서 따로 정의하고 계속 불러들이면서 사용해야겠다.

![image](/images/2018/08/3-2.png){: width="50%" height="50%"}

위의 두 함수를 불러서 실행해보자.

![image](/images/2018/08/4.png){: width="50%" height="50%"}

처음에 theta를 [0; 0]으로 set하고 cost를 계산해보니 엄청 크다. 그 다음 alpha를 0.000001로 설정하고 graD함수를 이용해서 gradient descent를 해보았다. 함수를 보면 gradient descent를 총 100번 하게 되어있다. 그리고 마지막에는 그 100번동안 어떻게 cost function J가 변하는지 그래프로 띄우도록 하였다. 수렴한다. 그것도 아주 빠르게 10번도 안되는 iteration동안 다 수렴해버린다. 그런데도 cost function이 아주 크다는 생각이 든다. 실제 training data들과 현재 수렴한 parameter로 세워진 model을 비교해보겠다.

![image](/images/2018/08/5.png){: width="50%" height="50%"}

figure1에서 보면 다른 training sample들에서 아주 크게 벗어나는 양상을 띄는 2개의 outlier가 있다. 저놈들이 범인인 것 같다. 하여간 쟤들은 여기에서 좋지 않은 data인 것 같다. 사실 대부분의 sample들은 작은 포유동물인 것을 알 수 있는데 거기에 한 두놈 저런애들이 껴있는 것이다. 쟤들을 없애고 작은 포유동물들의 뇌의 크기에 따른 실제 몸무게를 예측하는 프로그램으로 만들어야겠다.

19번과 32, 33번 sample data를 빼고 다시 해보겠다.

![image](/images/2018/08/6.png){: width="50%" height="50%"}

아까보단 scale이 10^1만큼이나 cost function이 작아졌지만 아직도 차이가 많이 나는 것 같다. sample data들을 더 쳐내보자. 아예 뇌의 무게가 20 이하인 것들만 남겨둬보겠다.

![image](/images/2018/08/7.png){: width="50%" height="50%"}

이번엔 alpha를 0.005로 해보았다. 스크린 샷 찍기전에 시행착오 거치면서 정한 것이다. 이번에는 cost가 좀 더 작아졌다. 사실 무게가 g 단위라서 절대숫자가 10, 100 단위씩이나 돼서 cost가 저렇게 큰 것이다. 아무튼 이렇게 linear regression을 한 번 해보았다. gradient descent를 하면서 cost function이 줄어드는것도 직접 관찰해보면서 최종 theta를 정하였다. 그리고 그렇게 정해진 model과 training dataset을 같은 그래프상에 올려놓고 잘 맞아떨어지는지도 보았다.

사실 오늘 한 예제의 dataset이 그다지 좋은 것인지 잘 모르겠다. 애초에 이런 data를 가지고 위의 model을 만드는 것이 합당한 것인가 싶다.

이번 예제는 feature가 한 개 였지만 feature가 여러 개 있는것도 동일하다.