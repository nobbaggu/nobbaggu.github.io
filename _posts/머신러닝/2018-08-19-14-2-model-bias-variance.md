---
title: (머신러닝) 14-2. Bias vs Variance
date: 2018-08-19T01:08:37+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - cross validation
  - high bias
  - high variance
  - overfitting
  - regularization
  - training data
  - underfitting
  - 가설함수
  - 머신러닝
  - 비용함수
---
cross validation에서 model의 hyper-parameter들을 어떻게 fit하여 모델을 개선하는지에 대해 설명한다. 결국 이 parameter들의 값을 조정한다는 것은 high bias(underfit)과 high variance(overfit) 사이에서 어떻게 균형을 맞춰나가는 지에 대한 것이다. 이러한 작업을 하는 것 역시 어느정도 시간을 들여야 하는 작업이지만, 우리가 만든 모델이 생각보다 큰 예측에러를 가지고 있는 경우 대부분은 high bias나 high variance때문인 것을 생각한다면 충분히 가치있는 일이다.

![image](/images/2018/08/no-name-75-1024x364.png){: width="50%" height="50%"}

먼저 기호를 한 번 정리하겠다.

  * d : 가설함수의 최고차항의 차수
  * m : training dataset의 갯수
  * λ : regularization parameter

&nbsp;

high bias와 high variance는 training data의 cost function$$\dpi{120} J_{train}$$ 과 validation data의 cost function $$\dpi{120} J_{cv}$$의 그래프를 그려놓고 보면 파악 할 수 있다. 일단 overfit인지 underfit인지 알면 위에서 말한 보조 parameter를 줄이거나 키워 조절 할 수 있게 된다.

# 

# 1. d에 따른 bias vs. variance

* * *

가설함수의 최고차항인 d를 $$\dpi{120} d=1,d=2,d=3,\cdot\cdot\cdot$$ 와 같이 크게하면 모델의 복잡도가 증가해 training dataset에 점점 잘 들어맞게 되면서$$\dpi{120} J_{train}$$은 작아질 것이다. 또한 어느 정도까지는 모델이 개선되기 때문에 validation data에 대한 예측 정확도도 증가할 것이다. 하지만 어느 순간을 넘어가면 high variance(overfit)이 일어난다. 이 때부터는 가설함수가 새로운 데이터에 일반화되지 못하고 오히려 validation data에 대한 예측 정확도는 떨어지면서 $$\dpi{120} J_{cv}$$는 증가할 것이다.

![image](/images/2018/08/no-name-76-300x231.png){: width="50%" height="50%"}

  * high bias(underfit) : $$\dpi{120} J_{train}\approx J_{cv}$$
  * high variance(overfit) : $$\dpi{120} J_{train}\ll J_{cv}$$1)

&nbsp;

1) $$\dpi{120} d=1,d=2,d=3,\cdot\cdot\cdot$$ d 각각의 d에 대해 training dataset으로 모델 학습

2) 위에서 구한 각각의 모델로 cross validation dataset의 error $$\dpi{120} J_{cv}$$ 계산

3) $$\dpi{120} J_{cv}$$가 최소가 되는 d를 선택

4) test dataset의 결과를 예측하여 모델의 성능을 판단

&nbsp;

&nbsp;

# **2. λ에 따른 bias  vs.  variance**

* * *

$$\dpi{120} \lambda=0, \lambda=0.1, \lambda=0.3, \lambda=0.9, \lambda=0.27, \lambda=0.81, \cdot \cdot \cdot$$ 

보통 위와 같이 λ를 3배씩 늘려가면서 확인한다. λ는 hypothesis function의 parameter Θ를 죽여서 overfit을 해결하는 역할이다. λ가 너무 크면 오히려 high bias(underfit)이 일어나고 λ가 너무 작으면 high variance(overfit)이 일어난다.

![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;\lambda=0,&space;\lambda=0.1,&space;\lambda=0.3,&space;\lambda=0.9,&space;\lambda=0.27,\cdot&space;\cdot&space;\cdot){: width="50%" height="50%"}

2) 위에서 구한 각각의 모델로 cross validation dataset의 error $$\dpi{120} J_{cv}$$ 계산

3) $$\dpi{120} J_{cv}$$가 최소가 되는 λ를 선택

4) test dataset의 결과를 예측하여 모델의 성능을 판단

&nbsp;

&nbsp;

&nbsp;

# **3. m에 따른 bias  vs.  variance**

* * *

training dataset의 갯수 m에 따른 training data와 validation data의 error를 그려 보는 것으로 우리가 더 많은 dataset을 필요로 하는지 파악할 수 있는데, 이 그래프를 <span style="font-size: 14pt; color: #ff0000;"><strong>learning curves</strong></span>라고 한다.

  * <span style="font-size: 14pt;"><strong>high bias(underfit)</strong></span>

model이 high bias 되어있다고 가정해보자. 이 때 training dataset의 갯수 m을 점점 늘리면 error $$\dpi{120} J_{train}$$은 증가할 것이다. training data를 늘려봐야 단순한 모델로는 따라가지 못하기 때문에 error만 쌓이는 것이다. 그리고 $$\dpi{120} J_{cv}$$는 m을 늘릴수록 그래도 조금씩은 작아질 것이다. 하지만 단순한 모델일수록 data에 둔감하여 새로운 데이터가 들어오더라도 모델이 잘 따라가지 않는다. 따라서 어느 시점부터는 데이터 size m이 많아지더라도 단순한 모델로는 그 데이터 패턴에 일반화되지 못해 $$\dpi{120} J_{cv}$$  curve가 flat하게 되고 더 줄어들지 않는다. 평균적으로 $$\dpi{120} J_{train}$$과 $$\dpi{120} J_{cv}$$ 모두 높다는 것이 high bias의 특징이다.

![image](/images/2018/08/no-name-79-300x218.png){: width="50%" height="50%"}

  * <span style="font-size: 14pt;"><strong>high variance(overfit)</strong></span>

모델이 high variance 되어있다면  $$\dpi{120} J_{train}$$과 $$\dpi{120} J_{cv}$$의 차이가 크다. 왜냐하면 overfit이라는 것은 training dataset에는 과하게 들어맞도록 만들어 $$\dpi{120} J_{train}$$는 작겠지만 새로운 데이터를 예측하는 성능은 오히려 떨어져 $$\dpi{120} J_{cv}$$는 크기 때문이다. 이 때에는 training data의 갯수 m을 늘려가면 overfit이 조금씩 해결이 되고 $$\dpi{120} J_{train}$$과 $$\dpi{120} J_{cv}$$ 사이의 gap이 줄어들 것이다.

![image](/images/2018/08/3-3-300x209.png){: width="50%" height="50%"}

&nbsp;

위의 내용들을 토대로 model을 학습시켜 high bias나 high variance되지 않는 균형잡인 model을 만들면 새로운 data에 대한 예측확률이 올라갈 것이다.

&nbsp;

&nbsp;

<span style="font-size: 18pt;"><strong>4. neural network(신경망)의 high bias  vs.  high variance</strong></span>

* * *

인공 신경망은 구조가 단순하고 parameter 갯수가 적을수록 high bias(underfit)이 일어나기 쉽고 구조가 복잡하고 parameter 갯수가 많을수록 high variance(overfit)이 일어나기 쉬운 경향이 있다. 이 때에는 신경망의 node의 갯수를 늘리거나 layer를 추가하는 식으로 high bias를 해결하고 regularization을 통해 high variance를 해결한다.

![image](/images/2018/08/no-name-78-1024x443.png){: width="50%" height="50%"}

&nbsp;

여기서 한 가지 tip을 얻을 수 있다.

**simple model** : **<span style="color: #ff0000;">data에 둔감</span>** → input X 추가에 영향을 별로 받지않음 → <span style="color: #ff0000;"><strong>high bias</strong></span>(underfit)

**complex model**  : <span style="color: #ff0000;"><strong>data에 민감</strong></span> → input X 추가에 큰 영향을 받음 → <span style="color: #ff0000;"><strong>high variance</strong></span>(overfit)

&nbsp;

신경망의 구조를 복잡하게 한 후 일반화 될 때 까지 regularization을 하여 모델을 만드는 것은 좋은 선택이다. 하지만 신경망의 구조가 복잡해질수록 계산의 시간 복잡도가 크게 증가한다. 따라서 무조건 좋은 성능을 위해 신경망을 더 복잡한 구조로 선택하는 것 보다는 hidden layer의 수를 1부터 차례로 증가시키면서 cross validation dataset에 대해 최적화시켜 hidden layer의 수를 정하는 것이 좋겠다.