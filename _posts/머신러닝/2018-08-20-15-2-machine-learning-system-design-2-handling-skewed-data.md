---
title: (머신러닝) 15-2. Machine Learning System Design - Handling Skewed Data
date: 2018-08-20T18:40:53+09:00
author: SWnomad
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - AI
  - F score
  - F 스코어
  - F 점수
  - hypothesis function
  - machine learning
  - precision
  - recall
  - 가설함수
  - 머신러닝
  - 모델 평가
  - 알고리즘 평가
  - 인공지능
---
암을 진단하는 모델을 test data에 적용했는데 에러율 1%의 정확도로 예측했다고 해보자. 그런데 실제로 암을 가진 환자는 test data의 0.5% 라면? 암이 class1, 암이 아닌경우 class0으로 분류된다고 하면 모든 경우를 class0으로 예측을 한다해도 에러율이 0.5%밖에 되지 않는다. 아무런 알고리즘의 개선 없이 예측 error를 올린 것이다. 따라서 이런 경우 error율 자체로 모델의 성능을 평가하기에는 무리가 있다고 볼 수 있다. 이처럼 class의 data수 차이가 아주 많이 나는 data를 skewed data라고 한다.  skewed data란 직역하자면 &#8216;왜곡된 데이터, 꼬인 데이터&#8217; 정도가 된다. 따라서 일반적으로 모델의 성능을 파악할 수 있는 &#8216;하나의 수로 된 지표&#8217;를 정의하여 사용할 수 있다면 아주 좋을 것이다.

&nbsp;

<span style="font-size: 18pt;"><strong>1. Precision / Recall</strong></span>

* * *

test data를 다음과 같이 분류한다.

true positive(TP) : predicted 1 / actual 1

true negative(TN) : predicted 0 / actual 0

false negative(FN) : predicted 0 / actual 1

false positive(FP) : predicted 1 / actual 0

&nbsp;

1) Precision : 1이라고 예측한 환자들 중 정말로 1인 비율

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;P=\frac{TP}{TP+FP}" alt="\dpi{120} P=\frac{TP}{TP+FP}" align="absmiddle" /> 

&nbsp;

2) Recall : 실제로 1인 환자들 중 제대로 1로 골라낸 수

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;R&space;=&space;\frac{TP}{TP+FN}" alt="\dpi{120} R = \frac{TP}{TP+FN}" align="absmiddle" /> 

&nbsp;

위에서 언급했던 암 환자 예측 모델에서 모든 data에 대해 class0으로 판단한다고 하면 R=0이 된다. 따라서 이 모델은 좋은 모델이 아니라고 할 수 있다.

&nbsp;

<span style="font-size: 18pt;"><strong>2. P와 R 사이 Trade-off</strong></span>

* * *

classification(logistic regression)의 경우를 다시 떠올려보자.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;h_{\theta}(x)&space;<&space;0.5&space;\rightarrow&space;class0\\&space;h_{\theta}(x)&space;\geq&space;0.5&space;\rightarrow&space;class1" alt="\dpi{120} \\ h_{\theta}(x) < 0.5 \rightarrow class0\\ h_{\theta}(x) \geq 0.5 \rightarrow class1" align="absmiddle" />  hypothesis function의 결과값이 0.5를 기준으로 positive / negative class로 구분한다.

&nbsp;

하지만 위의 threshold를 옮기면 어떻게 될까?

(i)   <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;h_{\theta}(x)&space;<&space;0.7&space;\rightarrow&space;class0\\&space;h_{\theta}(x)&space;\geq&space;0.7&space;\rightarrow&space;class1" alt="\dpi{120} \\ h_{\theta}(x) < 0.7 \rightarrow class0\\ h_{\theta}(x) \geq 0.7 \rightarrow class1" align="absmiddle" />                                (ii)  <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\&space;h_{\theta}(x)&space;<&space;0.3&space;\rightarrow&space;class0\\&space;h_{\theta}(x)&space;\geq&space;0.3&space;\rightarrow&space;class1" alt="\dpi{120} \\ h_{\theta}(x) < 0.3 \rightarrow class0\\ h_{\theta}(x) \geq 0.3 \rightarrow class1" align="absmiddle" />

&nbsp;

(i)는 class1로 분류하는 기준을 조금 더 엄격하게 잡은 경우이다. 이 때에는 암이라고 예측한 경우에 대해 맞을 비율이 높아질 것이다. 하지만 실제 암 환자들 중 우리가 골라내는 확률은 낮아진다. 따라서 이 때에는 P가 커지고 R이 작아진다.

(ii)는 class1로 분류하는 기준을 조금 더 안정적으로 잡은 경우이다. 이 때에는 암이라고 예측한 경우에 대해 맞을 확률이 낮아진다. 기준을 너무 널널하게 잡았기 때문이다. 하지만 실제 암 환자들 중 우리가 골라내는 비율은 높아진다. 따라서 이 때에는 P는 작아지고 R이 커진다.

P와 R의 개념을 봤을 때 둘 모두 높은 것이 좋다. 하지만 하나가 커지면 하나는 작아진다.

<img class="aligncenter size-medium wp-image-590" src="/images/2018/08/no-name-80-300x263.png" alt="" width="300" height="263" srcset="/images/2018/08/no-name-80-300x263.png 300w, /images/2018/08/no-name-80.png 440w" sizes="(max-width: 300px) 100vw, 300px" /> 

<table style="border-collapse: collapse; width: 85.4545%;" border="1">
  <tr>
    <td style="width: 15.3939%; text-align: center;">
    </td>
    
    <td style="width: 13.3334%; text-align: center;">
      P
    </td>
    
    <td style="width: 12.606%; text-align: center;">
      R
    </td>
  </tr>
  
  <tr>
    <td style="width: 15.3939%; text-align: center;">
      model 1
    </td>
    
    <td style="width: 13.3334%; text-align: center;">
      0.99
    </td>
    
    <td style="width: 12.606%; text-align: center;">
      0.23
    </td>
  </tr>
  
  <tr>
    <td style="width: 15.3939%; text-align: center;">
      model 2
    </td>
    
    <td style="width: 13.3334%; text-align: center;">
      0.9
    </td>
    
    <td style="width: 12.606%; text-align: center;">
      0.3
    </td>
  </tr>
  
  <tr>
    <td style="width: 15.3939%; text-align: center;">
      model 3
    </td>
    
    <td style="width: 13.3334%; text-align: center;">
      0.6
    </td>
    
    <td style="width: 12.606%; text-align: center;">
      0.5
    </td>
  </tr>
</table>

위와 같은 표가 있을 때 어떠한 알고리즘이 좋은 알고리즘인지 판단할 수 있겠는가? 물론 감으로 선택할 수 있겠지만 모델의 성능을 평가하는 지표가 하나의 숫자로 되어있다면 판단하기 수월하겠다. 따라서 우리는 다음과 같은 모델 성능의 평가 지표인 F score를 정의할 수도 있다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;F=\frac{P+R}{2}" alt="\dpi{120} F=\frac{P+R}{2}" align="absmiddle" /> 

하지만 예를들어 우리가 모든 test datad의 결과를 class1로 예측한다 해보자. 그럼 Recall은 1로 아주 높아지고 좋은 F score를 얻을 수 있다. 즉, P와 R의 평균값을 F score로 쓰는 것은 좋지 않아 보인다. 따라서 F score는 다음과 같이 정의한다.

&nbsp;

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\mathbf{F=2\frac{PR}{P+R}}" alt="\dpi{120} \mathbf{F=2\frac{PR}{P+R}}" align="absmiddle" /> 

&nbsp;

<table style="border-collapse: collapse; width: 85.4545%;" border="1">
  <tr>
    <td style="width: 15.3939%; text-align: center;">
    </td>
    
    <td style="width: 13.3334%; text-align: center;">
      P
    </td>
    
    <td style="width: 12.606%; text-align: center;">
      R
    </td>
    
    <td style="width: 22.3031%; text-align: center;">
      <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;F=\frac{P+R}{2}" alt="\dpi{120} F=\frac{P+R}{2}" align="absmiddle" />
    </td>
    
    <td style="width: 21.876%; text-align: center;">
      <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;F=2\frac{PR}{P+R}" alt="\dpi{120} F=2\frac{PR}{P+R}" align="absmiddle" />
    </td>
  </tr>
  
  <tr>
    <td style="width: 15.3939%; text-align: center;">
      model 1
    </td>
    
    <td style="width: 13.3334%; text-align: center;">
      0.99
    </td>
    
    <td style="width: 12.606%; text-align: center;">
      0.23
    </td>
    
    <td style="width: 22.3031%; text-align: center;">
      <strong>0.61</strong>
    </td>
    
    <td style="width: 21.876%; text-align: center;">
      <strong>0.37</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 15.3939%; text-align: center;">
      model 2
    </td>
    
    <td style="width: 13.3334%; text-align: center;">
      0.9
    </td>
    
    <td style="width: 12.606%; text-align: center;">
      0.3
    </td>
    
    <td style="width: 22.3031%; text-align: center;">
      <strong>0.6</strong>
    </td>
    
    <td style="width: 21.876%; text-align: center;">
      <strong>0.45</strong>
    </td>
  </tr>
  
  <tr>
    <td style="width: 15.3939%; text-align: center;">
      model 3
    </td>
    
    <td style="width: 13.3334%; text-align: center;">
      0.6
    </td>
    
    <td style="width: 12.606%; text-align: center;">
      0.5
    </td>
    
    <td style="width: 22.3031%; text-align: center;">
      <strong>0.55</strong>
    </td>
    
    <td style="width: 21.876%; text-align: center;">
      <strong>0.55</strong>
    </td>
  </tr>
</table>

F score를 위와 같이 정의하여야 가장 합리적인 model3의 F score가 높게 나오는 것을 볼 수 있다. 이러한 Precision과 Recall, 그리고 F score를모델의 평가지표로 활용하면 아주 skewed한 data에 대해서도 충분히 model의 성능을 평가할 수 있다. <span style="color: #ff0000;"><strong>threshold를 바꿔가면서 model을 훈련시키고</strong></span> <span style="color: #ff0000;"><strong>cross validation dataset에 대해 P와 R을 구한다음 F score를 계산하여 가장 높은 점수를 얻은 모델을 선택하면 된다.</strong></span>