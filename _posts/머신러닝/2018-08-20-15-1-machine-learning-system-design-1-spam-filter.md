---
title: (머신러닝) 15-1. Machine Learning System Design - Spam Filter
date: 2018-08-20T16:41:59+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - AI
  - classification
  - error analysis
  - logistic regression
  - machine learning
  - spam claasifier
  - spam filter
  - 머신러닝
  - 머신러닝 시스템
  - 스팸 필터
  - 스팸메일 분류
  - 인공지능
---
<span style="font-size: 18pt;"><strong>1. Spam Filter</strong></span>

* * *

Spam Filter : Classification(Logistic Regression)

x : features of e-mail

y : label as spam(1) / non-spam(0)

feature vector x는 spam / non-spam을 판단할 수 있는 단어가 e-mail에 본문 내용에 포함되었는지 안되어있는지 여부를 나타낸다.

ex ) buy , sale, price 등은 spam으로 판단하게 하는 단어. Andrew(수신자 이름), now, respond 등은 non-spam으로 판단하도록 하는 단어. 그럼 vector x는 다음과 같은 형태이다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x=\begin{bmatrix}1\\1\\0\\1\\0\\1\\&space;\cdot&space;\\&space;\cdot&space;\end{bmatrix}&space;\begin{bmatrix}buy\\sale\\now\\price\\Andrew\\responde\\&space;\cdot\\&space;\cdot&space;\end{bmatrix}" alt="\dpi{120} x=\begin{bmatrix}1\\1\\0\\1\\0\\1\\ \cdot \\ \cdot \end{bmatrix} \begin{bmatrix}buy\\sale\\now\\price\\Andrew\\responde\\ \cdot\\ \cdot \end{bmatrix}" align="absmiddle" /> 

이러한 feature vector를 spam filter 모델에 넣어주면 스팸인지 아닌지 classfy 한다.

보통 spam의 feature vector의 size는 10,000 ~ 50,000인데 엄청 방대한 스팸메일의 데이터에서 가장 많이 나오는 단어들을 추린 것이다.

&nbsp;

&nbsp;

<span style="font-size: 18pt;"><strong>2. Spam Filter 성능 향상 방법</strong></span>

* * *

1) 엄청 많은 데이터를 모은다 : usually help, but not always help

2) e-mail routing information 같은 sophisticated feature 이용

3) 본문의 내용에 비슷한 단어가 있어도 spam으로 취급 ex)buy, buying / sale, sales 등 같은 단어로 취급할 수 있는 알고리즘 개선

4) 의도적 misspelling 검출 알고리즘 개선 ex w4tches → watches, med1cine → medicine, m0rtgage → mortgage

&nbsp;

위의 방법 들 이외에도 여러가지 방법이 있겠다. 그런데 spam filter의 성능이 예상했던 것 보다 너무 좋지 않게 나온 경우 보통 사람들은 체계적인 방법이나 근거 없이 자신의 감에 의지해 이것 저것 건드리며 개선하려 시도해본다. 하지만 이는 좋지 않은 방법일 뿐더러, 시간을 많이 빼앗는다. 그럼 어떤 옵션을 선택하여 개선해야 할 지 체계적인 판단을 할 수 있는 방법은 뭘까?

&nbsp;

<span style="font-size: 18pt;"><strong>3. Error Analysis</strong></span>

* * *

지금은 spam filter를 예로 들었지만 어떠한 머신러닝 알고리즘을 구현할 때에도 적용될 수 있는 효과적으로 error를 줄이고 시간을 아껴서 알고리즘 성능을 개선할 수 있는 일반적인 tip에 대해 소개해드리겠다.

<span style="font-size: 14pt;"><strong>1) 구조가 간단한 알고리즘으로 시작한다. 이후 cross-validation data를 통해 평가/개선한다.</strong></span>

<span style="font-size: 14pt;"><strong>2) learning curve를 이용해 우리가 더 많은 training dataset이 필요한지 판단한다.</strong></span>

일단은 간단한 알고리즘으로 시작하여 high bias인지 high variance인지 판단하여 개선하는 이유는 애초에 알고리즘을 구현하고 시작하기 전에는 우리가 어떤 parameter를 수정하여 알고리즘을 개선할 수 있는지 알 방법이 없기 때문이다. 일단은 빠르게 구현할 수 있는 알고리즘으로 시작한 이후 자신의 감에 의지한 개선이 아닌 cross validation dataset을 사용해 정량적인 error를 일으키는 원인을 판단하고 수정해 나가는 것이 시간을 절약할 수 있는 길이다.

<span style="font-size: 14pt;"><strong>3) error analysis</strong></span>

validation dataset의 결과를 예측했을 때 error를 일으킨 validation data(ex. 잘못 분류된 e-mail)을 분석한다. 이렇게 error를 일으킨 data들이 공통적으로 가지고 있는 패턴이 있는지 찾아낸다.

(ex.1) 종류별로 분류

  * 약 광고 12
  * 짝퉁 제품 광고 4
  * 피싱메일 53
  * 기타 31

error를 일으킨 100개의 e-mail을 종류별로 분류하였을 때 위와 같다면 피싱메일을 판단하는 알고리즘을 개선하는 것이 가장 합리적일 것이다.

(ex.2) 에러를 일으킨 메일의 특징별로 분류

  * 고의적인 철자 에러(m0rgage,med1cine, . . .) 5
  * 평범하지 않은 e-mail 라우팅 16
  * 평범하지 않은 punctuation(과도한 !, ? 등의 기호 사용)  32

이번같은 경우에는 평범하지 않은 punctuation을 걸러낼 수 있는 기능에 관한 알고리즘을 개선해야 한다고 판단할 수 있다.

&nbsp;

아무튼 보통은 위와 같은 절차에 따라 하지않고 처음부터 복잡한 알고리즘을 세워서 시작하는 사람들이 있다. 하지만 이는 어느 부분을 개선해야 할지도 모르는 상태에서 시간만 잡아먹을 것이다. 시스템이 복잡할수록 계산의 시간복잡도 역시 증가한다. 처음에는 간단하고 조금은 지저분할 수도 있는 모델을 세우고 학습시켜 cross validation dataset을 사용해 고쳐야 할 부분을 파악하고 개선해 나가는 것이 현명한 방법이라 할 수 있다.