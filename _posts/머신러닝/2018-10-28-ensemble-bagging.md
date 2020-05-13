---
title: (머신러닝) Ensemble - Bagging
date: 2018-10-28T17:01:32+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - Bagging
  - bootstrap aggregating
  - Ensemble
  - 머신러닝
  - 배깅
  - 앙상블
---
앙상블이란 새로운 머신러닝 알고리즘이 아닌 여러개의 모델을 합쳐서 예측 결과를 평균내는 것이다. 이러한 학습법은 서로 다른 dataset을 사용하기 때문에 overfitting을 막아주고 평균적으로 더 좋은 예측 성능을 낸다. 오늘은 Ensemble 방법 중 Bagging 에 대해 다룬다.

&nbsp;

# 1. Bagging

* * *

** 여러개의 모델을 <span style="text-decoration: underline;">같은 알고리즘</span>으로 <span style="text-decoration: underline;">서로 다른 dataset</span>을 이용해서 학습시켜 합치는 것**이다.

![image](/images/2018/10/no-name-6.jpg){: width="50%" height="50%"}

각각의 model은 training dataset이 n개 중 랜덤으로 n'(<n)개를 뽑아 모델을 학습 시킨다. 이 때 중요한 것이 있다.

**n&#8217;개의 데이터에는 중복되는 데이터가 포함될 수 있다.** n개 중 중복을 허용하여 n&#8217;개의 데이터를 선정하여 모델을 학습시키는 것이다.

각각의 model이 예측한 결과를 평균을 내어 최종 예측 결과가 나온다.

Bagging을 사용하면 서로 다른 dataset으로 학습을 시키기 때문에 overfitting을 막을 수 있다.