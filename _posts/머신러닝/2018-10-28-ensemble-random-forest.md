---
title: (머신러닝) Ensemble - Random Forest
date: 2018-10-28T21:41:45+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - Bagging
  - Decision Tree
  - machine learning
  - Random Forest
  - 랜덤 포레스트
  - 머신러닝
  - 앙상블
  - 의사결정 트리
---
Random Forest 는 Decision Tree를 앙상블 Bagging을 사용하여 만든 알고리즘이다. 따라서 의사결정 트리와 Ensemble 방법 중 Bagging에 대해 먼저 이해해야 한다. 아래의 두 포스트는 이 두 내용에 대한 설명이며, 그리 길지 않다.

&nbsp;

<blockquote class="wp-embedded-content" data-secret="oTG2C9rELN">
  <p>
    <a href="https://SWnomad.com/decision-tree/">Decision Tree</a>
  </p>
</blockquote>



<blockquote class="wp-embedded-content" data-secret="AKPcjJQHmb">
  <p>
    <a href="https://SWnomad.com/ensemble-bagging/">Ensemble &#8211; Bagging</a>
  </p>
</blockquote>



&nbsp;

# 1. Random Forest 알고리즘

* * *

<table style="border-collapse: collapse; width: 60.1212%;" border="1">
  <tr>
    <td style="width: 12%; text-align: center;">
      feature1
    </td>
    
    <td style="width: 12.1212%; text-align: center;">
      feature2
    </td>
    
    <td style="width: 12.8485%; text-align: center;">
      feature3
    </td>
    
    <td style="width: 13.2122%; text-align: center;">
      feature4
    </td>
    
    <td style="width: 9.96152%; text-align: center;">
      <strong>class</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 12%; text-align: center;">
      YES
    </td>
    
    <td style="width: 12.1212%; text-align: center;">
      NO
    </td>
    
    <td style="width: 12.8485%; text-align: center;">
      YES
    </td>
    
    <td style="width: 13.2122%; text-align: center;">
      YES
    </td>
    
    <td style="width: 9.96152%; text-align: center;">
      <strong>NO</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 12%; text-align: center;">
      YES
    </td>
    
    <td style="width: 12.1212%; text-align: center;">
      YES
    </td>
    
    <td style="width: 12.8485%; text-align: center;">
      NO
    </td>
    
    <td style="width: 13.2122%; text-align: center;">
      EYS
    </td>
    
    <td style="width: 9.96152%; text-align: center;">
      <strong>YES</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 12%; text-align: center;">
      NO
    </td>
    
    <td style="width: 12.1212%; text-align: center;">
      YES
    </td>
    
    <td style="width: 12.8485%; text-align: center;">
      YES
    </td>
    
    <td style="width: 13.2122%; text-align: center;">
      NO
    </td>
    
    <td style="width: 9.96152%; text-align: center;">
      <strong>YES</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 12%; text-align: center;">
      NO
    </td>
    
    <td style="width: 12.1212%; text-align: center;">
      NO
    </td>
    
    <td style="width: 12.8485%; text-align: center;">
      YES
    </td>
    
    <td style="width: 13.2122%; text-align: center;">
      YES
    </td>
    
    <td style="width: 9.96152%; text-align: center;">
      <strong>NO</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 12%; text-align: center;">
      YES
    </td>
    
    <td style="width: 12.1212%; text-align: center;">
      NO
    </td>
    
    <td style="width: 12.8485%; text-align: center;">
      YES
    </td>
    
    <td style="width: 13.2122%; text-align: center;">
      NO
    </td>
    
    <td style="width: 9.96152%; text-align: center;">
      <strong>YES</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 12%; text-align: center;">
      ···
    </td>
    
    <td style="width: 12.1212%; text-align: center;">
      ···
    </td>
    
    <td style="width: 12.8485%; text-align: center;">
      ···
    </td>
    
    <td style="width: 13.2122%; text-align: center;">
      ···
    </td>
    
    <td style="width: 9.96152%; text-align: center;">
      <strong>···</strong>
    </td>
  </tr>
</table>

위와 같은 데이터가 주어졌다.

먼저, 우리는 2개의 feature만 골라서 학습을 시킨다. Bagging Ensembl에 따라 학습 시킬 데이터를 추출한다. 물론, 중복 데이터가 들어갈 수 있다. 이렇게 고른 feature와 data의 subset으로 모델을 학습시킨다.

트리의 수가 100개라고 하면 모델을 100개를 만들어 서로 다른 feature, data로 학습을 시키는 것이다.

그런데 우리는 위에서 2개의 feature만 사용했다. 하지만 3개의 feature를 사용하면 더 좋은 성능을 내지 않을까?

3개의 feature를 사용하여 위의 과정을 반복해보았다.

두 Random Forest의 성능을 비교하여 더 좋은 성능을 내는 Random Forest를 선택하면 된다. 성능을 비교하는 방법은 잠시 후 설명한다.

아무튼 최종적으로 아래와 같이 여러개(100개)의 decision tree가 만들어진다.

![image](https://nobbaggu.github.io/images/2018/10/no-name-8.jpg){: width="50%" height="50%"}

&nbsp;

&nbsp;

&nbsp;

# 2. Random Forest의 성능 평가

* * *

Random Forest의 각각의 Tree는 전체 training dataset 중 중복을 허용하여 일부만을 사용하여 학습한다. 이 때, 어떤 Tree를 학습시키는 데에도 사용되지 않은 data가 있을 것이다. 이러한 data를 &#8216;out of bag data&#8217;라고 한다. 이 out of bag data들을 가지고 Random Forest로 예측을 하여 모델의 정확도를 측정한다.

위에서 말했듯이 이렇게 측정된 Random Forest들 중에서 feature를 3개만 사용한 Random Forest가 가장 좋은 예측성능을 낸다면 이 Random Forest 모델을 사용하면 된다.