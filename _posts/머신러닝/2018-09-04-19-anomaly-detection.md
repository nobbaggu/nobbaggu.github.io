---
title: (머신러닝) 19. Anomaly Detection
date: 2018-09-04T22:04:26+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - anomaly
  - anomaly detection
  - gaussian distribution
  - 가우스 분포
  - 이상 감지
  - 이상 감지 알고리즘
  - 정규분포
---
정상적인 범주 내에 있는 normal한 data들의 분포에서 많이 벗어나는 data를 anomaly 라고 한다. anomaly 는 &#8216;변칙&#8217;이란 뜻을 가지고 있다.

![image](/images/2018/09/no-name-1.png){: width="50%" height="50%"}

위와 같은 비이상적 data인 anomaly 를 detect 하는 알고리즘은 제품 공정에서 불량을 찾거나 데이터 센터에서 소자의 이상한 움직임을 감지한다던지 하는데에 적용할 수 있다.

&nbsp;

&nbsp;

# <span style="font-size: 18pt;"><strong>1. Gaussian Distribution</strong></span>

* * *

일반적인 normal 데이터들의 분포는 보통 Gaussian distribution(가우스 분포) &#8211; normal distribution(정규분포)라고도 한다- 를 따른다. 가우스 분포는 다음과 같다.

![image](/images/2018/09/no-name-3.png){: width="50%" height="50%"}

위는 mean(평균)이 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\mu" alt="\dpi{120} \mu" align="absmiddle" />이고 variance(분산)이 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\sigma^{2}" alt="\dpi{120} \sigma^{2}" align="absmiddle" />인 가우스 분포이다.

어떤 데이터의 집합이 가우스 분포를 따른다고 할 때 다음과 같이 나타낸다.   <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}\sim&space;N(\mu,\sigma^{2})" alt="\dpi{120} x^{(i)}\sim N(\mu,\sigma^{2})" align="absmiddle" />

&nbsp;

&nbsp;

# <span style="font-size: 18pt;"><strong>2. Algorithm</strong></span>

* * *

training dataset <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(1)},x^{(2)},\cdot\cdot\cdot,x^{(m)}" alt="\dpi{120} x^{(1)},x^{(2)},\cdot\cdot\cdot,x^{(m)}" align="absmiddle" />이 있다고 해보자.

먼저 anomaly 에 대해서는 p가 작고 normal data에 대해서는 p를 크게 해 줄 것 같은 적절한 feature를 선정한다. feature의 수는 n이라고 하고<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(x^{(i)}\in&space;\mathbb{R}^{n})" alt="\dpi{120} (x^{(i)}\in \mathbb{R}^{n})" align="absmiddle" />  data들이 가우스 분포를 따른다고 하면 model 식은 다음과 같다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;\\p(x;\mu,\sigma^{2})=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^{2}}{2\sigma^{2}}}" alt="\dpi{150} \\p(x;\mu,\sigma^{2})=\frac{1}{\sigma\sqrt{2\pi}}e^{-\frac{(x-\mu)^{2}}{2\sigma^{2}}}" align="absmiddle" />                      <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;\mu=\frac{1}{m}\sum_{i=1}^{m}x^{(i)}&space;\\&space;\sigma^{2}=\frac{1}{m}\sum_{i=1}^{m}(x^{(i)}-\mu)^{2}" alt="\dpi{120} \\ \mu=\frac{1}{m}\sum_{i=1}^{m}x^{(i)} \\ \sigma^{2}=\frac{1}{m}\sum_{i=1}^{m}(x^{(i)}-\mu)^{2}" align="absmiddle" />

각 data의 feature은 각각이 서로 독립적이라고 가정하면 모델은 다음과 같이 표현 가능하다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;p(x;\mu,\sigma^{2})=p(x_{1};\mu_{1},\sigma_{1^{2}})p(x_{2};\mu_{2},\sigma_{2}^{2})\cdot\cdot\cdotp(x_{n};\mu_{n},\sigma_{n}^{2})=\prod_{j=1}^{n}p(x_{j};\mu_{j},\sigma_{j}^{2})" alt="\dpi{120} p(x;\mu,\sigma^{2})=p(x_{1};\mu_{1},\sigma_{1^{2}})p(x_{2};\mu_{2},\sigma_{2}^{2})\cdot\cdot\cdotp(x_{n};\mu_{n},\sigma_{n}^{2})=\prod_{j=1}^{n}p(x_{j};\mu_{j},\sigma_{j}^{2})" align="absmiddle" /> 

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;\mu_{j}=\frac{1}{m}\sum_{i=1}^{m}x^{(i)}_{j}&space;\\&space;\sigma^{2}_{j}=\frac{1}{m}\sum_{i=1}^{m}(x^{(i)}_{j}-\mu_{j})^{2}" alt="\dpi{120} \\ \mu_{j}=\frac{1}{m}\sum_{i=1}^{m}x^{(i)}_{j} \\ \sigma^{2}_{j}=\frac{1}{m}\sum_{i=1}^{m}(x^{(i)}_{j}-\mu_{j})^{2}" align="absmiddle" /> 

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;p(x^{(i)})" alt="\dpi{120} p(x^{(i)})" align="absmiddle" /> 는<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}" alt="\dpi{120} x^{(i)}" align="absmiddle" /> 가 normal한 data일 확률이다. 여기에 새로운 data  <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x_{test}" alt="\dpi{120} x_{test}" align="absmiddle" />가 들어왔을 때 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;p(x_{test})" alt="\dpi{120} p(x_{test})" align="absmiddle" /> 우리가 정한 어떤 수 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\epsilon" alt="\dpi{120} \epsilon" align="absmiddle" />보다 작으면 anomaly 라고 판단한다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;p(x_{test})>&space;\epsilon:normal(y=0)&space;\\&space;p(x_{test})<&space;\epsilon:anomaly(y=1)" alt="\dpi{120} \\ p(x_{test})> \epsilon:normal(y=0) \\ p(x_{test})< \epsilon:anomaly(y=1)" align="absmiddle" /> 

&nbsp;

&nbsp;

# <span style="font-size: 18pt;"><strong>3. model evaluation</strong></span>

* * *

model을 세운다고 끝이 아니다. 모델의 성능이 쓸만해야한다.

normal한 data가 10,000개 있고 anomaly data가 20개 있을 때 모든 데이터를 모델 훈련에 쓰는 것이 아니라 다음과 같이 나눈다.

&nbsp;

**training data : normal 6,000개<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(1)},x^{(2)},\cdot\cdot\cdot,x^{(m)}" alt="\dpi{120} x^{(1)},x^{(2)},\cdot\cdot\cdot,x^{(m)}" align="absmiddle" />** 

**cv(cross validation) data : normal 2,000개 / anomaly 10개 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),\cdot\cdot\cdot,(x^{(m_{cv})},y^{(m_{cv})})" alt="\dpi{120} (x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),\cdot\cdot\cdot,(x^{(m_{cv})},y^{(m_{cv})})" align="absmiddle" />**

**test data : normal 2,000개 / anomaly 10개 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),\cdot\cdot\cdot,(x^{(m_{test})},y^{(m_{test})})" alt="\dpi{120} (x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}),\cdot\cdot\cdot,(x^{(m_{test})},y^{(m_{test})})" align="absmiddle" />**

&nbsp;

training data는 normal한 데이터만 가지고 이를 사용해 모델을 구한다.

그리고 cv dataset을 사용해서<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\epsilon" alt="\dpi{120} \epsilon" align="absmiddle" /> 를 조정한다. model의 성능을 확인하는 지표로 F score를 활용하면 된다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;F=\frac{PR}{P+R}\quad&space;(P:precision\quad&space;R:recall)" alt="\dpi{120} F=\frac{PR}{P+R}\quad (P:precision\quad R:recall)" align="absmiddle" /> 

ex) 여러 개의 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\epsilon" alt="\dpi{120} \epsilon" align="absmiddle" />으로 model <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;p(x)" alt="\dpi{120} p(x)" align="absmiddle" />를 학습시켜 각각의 model의 F score를 비교하여 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\epsilon" alt="\dpi{120} \epsilon" align="absmiddle" /> 결정

ex) 어떤 featrue를 포함 시킬지 말지 결정해야할 때, 포함시킨 경우의 F score와 포함시키지 않은 경우 F score 비교

&nbsp;

&nbsp;

&nbsp;

# <span style="font-size: 18pt;"><strong>4. Anomaly Detection   vs.   Supervised Learning</strong></span>

* * *

언뜻 보면 supervised learning의 binary classification 알고리즘을 사용하여 cv data와 test data가 normal data인 경우를 y=0, anomaly data인 경우를 y=1로 놓고 학습을 시켜 모델을 만들 수도 있을 것 같다. 그도 그럴것이 cv dataset과 test dataset은 label(y=1,y=0)을 가지고 있다. 이는 실제로 결함이 있는 data에 대해 예측을 하여 모델의 성능을 평가해야하기 때문에 특별히 실제로 결함인지 아닌지 판단하기 위해 label을 넣은 것이다. 하지만 두 알고리즘은 서로 다른 상황에서 적용된다. 그 차이를 짚고 넘어가겠다.

&nbsp;

**1) Anomaly Detection**

  * 매우 적은 수의 anomaly data / 아주 많은 수의 normal data
  * 적은 수의 anomaly data로는 모델을 학습시키기가 힘들다. anomaly에는 많은 종류가 있기 때문에 현재 가진 anomaly 로 학습을 시켜도 미래에 들어올 data의 anomaly 가 이와 비슷한 종류라는 보장이 없다. 가령, 비행기 엔진 생산라인에서 anomaly detection 알고리즘으로 결함이 있는 엔진을 가려낼 때, 엔진의 파손이나 동작이상에는 매우 많은 종류의 원인이 있을 것이다.

&nbsp;

**2) Supervised Learning**

  * 많은 수의 positive class / negative class 데이터
  * 충분한 수의 positive data가 있고 이는 abnormal한 경우가 아니라 다른 하나의 class일 뿐이기 때문에 미래에 들어올 data가 positive class에 속한다면 현재 가지고 있는 positive class와 비슷할 것이라는 자신감이 있다.

&nbsp;

# <span style="font-size: 18pt;"><strong>5. Tips</strong></span>

* * *

histogram을 그리든 다른 방법을 쓰든 feature가 Gaussian을 따르는지 확인을 해서 따르지 않는 경우는 데이터를 좀 주물러서(?) Gaussian 형태가 되도록 하는 방법이 있다.

  *<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;log(x)" alt="\dpi{120} log(x)" align="absmiddle" /> 
  *<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;log(x+c)" alt="\dpi{120} log(x+c)" align="absmiddle" /> 
  *<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\sqrt{x}" alt="\dpi{120} \sqrt{x}" align="absmiddle" /> 
  *<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{p}" alt="\dpi{120} x^{p}" align="absmiddle" /> 

위의 수식을들 통해 data를 변형해서 Gaussian을 따르도록 한 새로운 feature를 사용하면 된다.

&nbsp;

우리의 최종 목표는 normal한 data에 대해서는 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;p(x)" alt="\dpi{120} p(x)" align="absmiddle" />가 매우 크고 anomaly 에 대해서는 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;p(x)" alt="\dpi{120} p(x)" align="absmiddle" />가 매우 작게 하는 모델을 만드는 것이다.