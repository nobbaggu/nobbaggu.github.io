---
title: (머신러닝) 1. 머신러닝이란?
date: 2018-08-08T20:12:22+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - AI
  - clustering
  - machine learning
  - supervised learning
  - unsupervised learning
  - 머신러닝
  - 분류
  - 비지도학습
  - 예측
  - 인공지능
  - 지도학습
  - 클러스터링
---
# <span style="font-size: 18pt;"><strong>Q. 머신러닝 (Machine Learning) 이란?</strong></span>

* * *

머신러닝이란 인공지능의 한 분야이다.

> <span style="text-decoration: underline;"><em>Arthur Samuel</em></span>
> 
> &#8220;프로그래밍 된 대로만 행동하지 않고(어떠한 입력에 대해 명확히 정해진 출력) <span style="color: #ff0000;">스스로 학습하는 능력을 컴퓨터에게 주는것</span>&#8220;

&nbsp;

머신러닝은 크게 지도학습 / 비지도학습 으로 나뉜다.

&nbsp;

# <span style="font-size: 18pt;"><strong>1. 지도학습(supervised learning)</strong></span>

* * *

컴퓨터에게 입력에 대한 결과를 알려주며 학습시키는 것. 즉, 수많은 [input, output] data를 넣어주면 입력~출력 사이에서 나타나는 일정한 패턴을 찾아낸다. 따라서 **컴퓨터에게 적절한 정답을 알려주면서 학습시킨다하여 지도학습이다****.** 이후 컴퓨터가 처음보는 입력 데이터를 주었을 때 얼마나 잘 예측을 하는지에 따라 알고리즘 성능 판단.

지도학습은 regression(예측) 과 classification(분류) 문제로 나뉜다.

&nbsp;

## **1) 예측(regression)**

훈련을 시키기위해 주어진 데이터들의 관계를 충분히 잘 나타내는 모델을 구하여 이후의 input(입력값)들마다 output(결과)를 예측하는 문제. <span style="color: #ff0000;"><strong>regression 문제는 output이 continuous(연속적)이다</strong>.</span> 이 때 우리는 모델을 1차 다항식, 2차 다항식, 혹은 그 이상의 다항식이나 지수함수, 로그함수 등 데이터set의 패턴에만 잘 들어맞는다면 어떠한 함수도 모델로 삼을 수 있다.

ex1 ) (재산, 집의 평수)의 data set들을 학습시켜 두 변수의 관계를 충분히 잘 나타내는 모델을 구하여 어떠한 사람의 재산이 주어졌을 때 집의 평수를 예측하는 모델

&nbsp;

![image](/images/2018/08/no-name.jpg){: width="50%" height="50%"}

&nbsp;

위의 케이스는 선형 모델로 충분히 좋은 예측이 가능해 보인다. 위의 데이터로 훈련시킨 모델은 재산이 입력되면 집의 평수를 예측해줄 것이다.

이외에도 발사이즈를 보고 그 사람의 키를 예측하는문제나 하루에 운동하는 시간에 따른 근육량을 예측하는 문제 등이 있다.

&nbsp;

&nbsp;

## **2) 분류(classification)**

<span style="color: #ff0000;"><strong>데이터를 알맞은 카테고리에 mapping시키는 문제. classification 문제의 모델은 discrete output(예측값)을 가진다.</strong></span> 카테고리를 class(클래스)라고 부르는데 2 가지의 class 중에서 분류하는 binary classification문제가 분류 문제중 가장 단순하다. 3개 이상의 class가 있는 multiclass classification문제가 일반적이다.

ex) A(a)~C(c)의 알파벳 패턴을 인식하여 3가지 class로 분류

&nbsp;

![image](/images/2018/08/no-name-1.png){: width="50%" height="50%"}

&nbsp;

알파벳 사진이 입력으로 들어오면 pixel단위로 패턴을 인식하여 이미 학습된 알고리즘(모델)에 따라 각각의 알파벳 class로 mapping시키는 패턴인식 예제.

&nbsp;

ex) 스팸메일 필터도 분류가 적용된 대표적인 사례들 중 하나. 우리가 몇 가지 특징에 따라 스팸인지 아닌지 학습을 시키면 이후 다른 메일에서 그 특징을 추출하여 분류

&nbsp;

ex3) 손의 크기와 목소리 톤의 2가지 정보로부터 남자인지 여자인지 분류하는 문제

&nbsp;

![image](/images/2018/08/no-name-1.jpg){: width="50%" height="50%"}

&nbsp;

&nbsp;

&nbsp;

# <span style="font-size: 18pt;"><strong>2. 비지도학습(unsupervised learning)</strong></span>

* * *

컴퓨터에게 일을 어떻게 하는지 가르치지 않는다. 즉, 적절한 해답을 알려주지 않고도 컴퓨터가 스스로 배우도록 하는 것이다. 지도학습의 분류(classification)와 비슷한 행위를 하지만 인간이 적절한 정답을 알려주지 않는다. 데이터 자체들만을 놓고 모종의 구조(structure)를 이끌어내어 클러스터링(clustering)한다. 클러스터링을 굳이 한국말로 번역하자면 구별하여 묶는다 정도로 해석하면 된다.

> **클러스터링 [clustering]**  :: 컴퓨터인터넷IT용어대사전 용어해설 > 컴퓨터/통신  
> 유사성 등의 개념에 기초하여 데이터를 몇몇의 그룹으로 분류하는 수법의 총칭. 문헌 검색, 패턴 인식, 경영 과학 등에 폭넓게 응용되고 있다.  
> -출처 : 네이버 지식백과 &#8211;

&nbsp;

ex1) 1000개의 에쎄이를 어떠한 기준(총 단어 수, 문단의 길이, 특정 단어의 빈도 수 등)에 따라 클러스터링  
ex2) 뉴스기사들을 시사, 연예, 과학, 예술 등의 카테고리로 클러스터링