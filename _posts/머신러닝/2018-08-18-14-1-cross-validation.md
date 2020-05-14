---
title: (머신러닝) 14-1. Cross Validation
date: 2018-08-18T21:06:53+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - cost function
  - cross validation
  - hypothesis
  - model selection
  - 가설함수
  - 비용함수
---
# **1. model evaluation**

* * *

지금까지 머신러닝 알고리즘을 학습시켜 training dataset에 잘 들어맞는 model을 구했다. 하지만 결국 머신러닝 알고리즘을 학습시키는 최종목표는 새로운 data가 들어왔을 때의 결과 예측이다.

  1.  $$\Theta$$ 를 학습시켜 $$J_{train}(\Theta)$$ 최소화
  2. 모델을 test dataset에 적용, $$J_{test}(\Theta)$$ 계산하여 모델의 예측성능 평가

training data에 대해서 $$J_{train}(\Theta)$$을 최소화 시켰다고 해도 새로운 test data의 결과를 잘 예측한다고 보장할 수 없다. 예를 들면 모델이 overfitting(high variance)인 경우 $$J_{train}(\Theta)$$은 아주 작지만 오히려 새로운 data에 대해서는 일반화(generalization) 되지못하고 성능이 떨어져 $$J_{test}(\Theta)$$는 커질 가능성이 높다. 만약 예측 성능이 떨어진다면 우리는 무엇을 할 수 있을까?

  * training data를 더 모은다
  * feature의 갯수를 늘리거나 줄인다(overfit &#8211; 줄인다 / underfit &#8211; 늘린다)
  * higher order polynomial term을 추가한다(underfit인 경우)
  * regularization parameter λ를 조절한다(overfit &#8211; 늘인다 / underfit &#8211; 줄인다)

하지만 무작정 위의 해결책을 random하게 적용하는 것은 시간낭비가 될 수 있다. 특히 training data를 더 모으는건 시간이 아주 많이 걸리는 일이다. 지금부터는 model을 조금 더 효과적으로 고르는 방법과 위의 해결책을 적절히 적용하는 방법들에 대해서 다루어 모델의 성능을 향상시키는 방법에 대해 설명하고자 한다.

&nbsp;

&nbsp;

# **2. model selection : Train / Validation / Test**

* * *

가지고 있는 data를 모두 모델을 학습시키는데 사용하지 말고 70%는 training dataset, 30%는 test dataset으로 나눈다고 하자.

그리고 우리는 하나의 parameter를 더 추가하는데, hypothesis function 최고차항의 차수인 d(degree of polynomial) 이다.

$$\\ h_{\theta}(x) = \theta_{0}+\theta_{1}x \rightarrow d=1 \\ h_{\theta}(x) = \theta_{0}+\theta_{1}x+\theta_{2}x^{2} \rightarrow d=2 \\ h_{\theta}(x) = \theta_{0}+\theta_{1}x+\theta_{2}x^{2}+\theta_{3}x^{3} \rightarrow d=3 \\ \cdot \\ \cdot$$ 

training data를 가지고 d=1부터 해서 d를 올려가면서 test한다 생각해보자. d가 커질수록 더 높은 차수의 항으로 인해 training dataset의 cost function은 낮아질 것이다. 하지만 어느 순간부터는 d가 커질수록 overfit이 일어나 test data에 대한 예측은 오히려 나빠지고 test data의 cost function은 높아질 것이다.

![image](https://nobbaggu.github.io/images/2018/08/no-name-74-300x232.png){: width="50%" height="50%"}

위 그림에서 포물선 모양의$$J_{test}$$ 가 최소가 되는 점에서 최고차항의 차수인 d를 선택해야한다.

하지만 이 d라는 parameter 역시 test data에 대해서만 최적화 된 것이다. 즉, 이렇게 training data와 test data로 나누어서 d를 정하였는데, 이를 새로운 data에 적용하여 검증하지 않으면 의미가 없는 것이다.

결론적으로 training dataset / test dataset의 2분류보다는 3 분류로 나누는게 가장 적절하다.

<span style="font-size: 14pt;"><strong>60% : training   dataset</strong></span>

<span style="font-size: 14pt;"><strong>20% : cross validation dataset</strong></span>

<span style="font-size: 14pt;"><strong>20% : test dataset</strong></span>

위의 3분류의 dataset을 가지고 다음의 절차에 따라 model 선택을 진행한다.

&nbsp;

&nbsp;

## **1) train**

training dataset을 사용하여 model parameter 학습

$$\\ J_{train}(\Theta) = \frac{1}{2m_{train}}\sum_{i=1}^{m_{train}}[h_{\Theta}(x_{train}^{(i)})-y_{train}^{(i)}]^{2}$$ 

&nbsp;

&nbsp;

## **2) cross validation**

validation dataset은 가설함수의 최고차항의 차수(d), regularization parameter(λ), training data의 갯수(m) 등 model의 보조 parameter를 fit하는 데 사용한다. 여러가지 크기의 d나 λ, m에 대하여 모델을 train한 후 각각의 모델로 cross validation dataset의 결과를 예측하여 $$J_{cv}$$가 최소인 d, λ, m 을 사용하면 된다.

$$J_{cv}(\Theta) = \frac{1}{2m_{cv}}\sum_{i=1}^{m_{cv}}[h_{\Theta}(x_{cv}^{(i)})-y_{cv}^{(i)}]^{2}$$ 

&nbsp;

## 

## **3) test**

마지막으로 위에서 고른 model로 test dataset의 결과를 얼마나 잘 예측하는지 확인한다.

$$J_{test}(\Theta) = \frac{1}{2m_{test}}\sum_{i=1}^{m_{test}}[h_{\Theta}(x_{test}^{(i)})-y_{test}^{(i)}]^{2}$$