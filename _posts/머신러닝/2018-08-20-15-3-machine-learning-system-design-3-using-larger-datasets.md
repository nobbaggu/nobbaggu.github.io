---
title: (머신러닝) 15-3. Machine Learning System Design - Using Larger Datasets
date: 2018-08-20T19:58:07+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - AI
  - hypothesis function
  - large dataset
  - larger dataset
  - low bias algorithm
  - low bias model
  - machine learning
  - 가설함수
  - 머신러닝
  - 인공지능
---
머신러닝에서 승자는 &#8220;우수한 알고리즘을 가진 자 가 아닌 많은 데이터를 가진 자&#8221; 라는 말이 있다. 이러한 말이 맞는 경우도 있고 아닌 경우도 있다.

다음과 같은 알고리즘이 있다 해보자.

For breakfast, I ate \_____ eggs.

위의 빈 칸에 들어갈 단어를 [to, two, too] 중에서 맞게 고르는 알고리즘이다. 이러한 경우 앞 뒤의 문맥이 feature x로 들어갈 수 있을 것이다. 하지만 이러한 경우 앞 뒤의 단어만으로 빈 칸에 알맞는 단어를 고를 수 있을 정도로 방대한 양의 training dataset이 필요하다고 생각이 든다.

하지만 다음과 같은 반례를 생각해보자. 집의 가격을 예측하는 알고리즘이다. 그리고 집의 크기만을 feature로 가진다. 집의 연식, 층 수, 방의 수, 등등 아무 것도 고려하지 않는다. 이러한 경우는 아무리 많은 데이터를 넣어줘도 집의 가격을 제대로 예측할 수 없을 것이다.

&nbsp;

따라서 많은 양의 데이터가 알고리즘을 개선시키는데 도움이 될 지 안 될지는 다음을 기준으로 판단할 수 있다.

&#8216;주어진 feature의 정보를 가지고 그 분야의 전문가가 결과 y를 예측하기에 충분한가?&#8217;

예시 1에서는 영어 전문가가 오면 feature(앞 뒤의 문맥)을 가지고 빈 칸에 맞는 단어를 충분히 선택할 수 있다. 따라서 많은 양의 데이터로 훈련을 시킨다면 좋은 모델이 나올 가능성이 높다.

하지만 예시 2에서는 부동산 전문가가 오더라도 feature(집의 크기) 하나만 딸랑 알려준다면 예측이 힘들 것이다. 따라서 이런 경우는 많은 양의 데이터가 알고리즘을 개선하는 데 아무런 도움이 되지 않는다.

&nbsp;

정리하자면, 충분한 가짓 수의 feature와 parameter가 있는 복잡한 알고리즘은 &#8212; 이런 경우 high variance(overfit)이 일어나기 쉬운데 이러한 알고리즘을 low bias(high variance) 알고리즘이라고 부른다 &#8212; 많은 양의 데이터가 알고리즘의 성능을 개선하는데 도움을 준다.

low bias 알고리즘 : 많은 feature와 parameter로 인해서 $$J_{train}$$이 매우 낮을 것이다.

<span style="font-size: 14pt;"><strong>+</strong></span>

large data : low bias(high variance)를 해소시켜$$J_{test}\approx J_{train}$$  이 되도록 도와준다.

위 두 가지를 종합하면 <span style="color: #ff0000;"><strong>복잡한 high bias 알고리즘에 large data를 넣어주면 알고리즘의 성능을 개선하는 데 도움이 된다.</strong></span>