---
title: 22. Online Learning
date: 2018-09-12T21:08:12+09:00
author: SWnomad
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - continuous flow of data
  - online learning
  - 온라인 러닝 알고리즘
---
Online Learning 은 실시간으로 흘러들어오는 데이터로 알고리즘을 학습시킬 경우에 사용하는 방법이다.

웹사이트는 방문자들의 행동에 관한 데이터로 그들의 알고리즘을 학습시킨다. 구글, 아마존과 같은 대형 웹사이트는 실시간으로 계속해서 방문자가 찾아온다. 데이터가 계속해서 흘러 들어오는 것이다. 이러한 continuous flow of data를 어떻게 처리하고 알고리즘을 학습시키는지가 지금 알아볼 내용이다.

&nbsp;

# 1. Online Learning

* * *

택배사 홈페이지를 예로 들어보자. 그들이 출발지, 도착지, 금액이 feature이고 그들이 서비스를 이용하는지(y=1) 않는지(y=0)이 결과이다.

그럼 머신러닝 알고리즘에 계속 들어오는 data는 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(x,y)" alt="\dpi{120} (x,y)" align="absmiddle" />이다. 그리고 parameter는 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta" alt="\dpi{120} \theta" align="absmiddle" />이다.

data <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;(x,y)" alt="\dpi{120} (x,y)" align="absmiddle" />가 들어오는 족족 그 data를 가지고 parameter <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta" alt="\dpi{120} \theta" align="absmiddle" />를 학습시키는 것이다.

&nbsp;

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\theta_{j}:=\theta_{j}-\alpha(h_{\theta}(x)-y)x_{j}" alt="\dpi{120} \theta_{j}:=\theta_{j}-\alpha(h_{\theta}(x)-y)x_{j}" align="absmiddle" /> 

우리가 원하는 model은 출발지, 도착지에 따른 금액 조건에서 서비스를 이용할 확률<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;p(y=1|x;\theta)" alt="\dpi{120} p(y=1|x;\theta)" align="absmiddle" />  이다.

한가지 더, 이렇게 한 번 학습에 쓰여진 데이터는 버리고 다시 사용하지 않는다.

그냥 몇만명 정도의 데이터로만 학습을 시키고 그 parameter를 계속 사용해도 되지 않을까? 하는 궁금증이 생길 수도 있다. 하지만 경제상황 등의 상황에 따라 계속 parameter는 달라질 수 밖에 없기 때문에 이런 online learning 알고리즘이 필요한 것이다.

위에서 말한 내용으로 알 수 있는 online learning 알고리즘의 특성이 있다. 바로 변화하는 user preferences에 적응 가능 하다는 것이다. 알고리즘이 스스로 최근의 변화 trend를 따라간다.

&nbsp;

언뜻 보면 stochastic gradient descent와 비슷하다. 하지만 stochastic gradient descent 알고리즘은 가지고 있는 데이터를 하나씩 사용해서 학습시키는 알고리즘인 반면, online learning 알고리즘은 실시간으로 흘러 들어오는 데이터를 가지고 학습시키는 경우 사용한다.

위에서도 말했듯이 데이터와 결과의 패턴, 트렌드가 변하는 경우에도 적응이 가능하여 이러한 변화들이 반영이 되어야하는 모델에 사용한다.

&nbsp;