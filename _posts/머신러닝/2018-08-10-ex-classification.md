---
title: (머신러닝) (ex) classification
date: 2018-08-10T05:35:01+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - classification
  - cost function
  - gradient descent
  - hypothesis function
  - machine learning
  - 가설함수
  - 경사하강법
  - 머신러닝
  - 분류
  - 비용함수
---
<p style="text-align: right;">
  <span style="text-decoration: underline; color: #0000ff; font-size: 14pt;"><strong><a style="color: #0000ff; text-decoration: underline;" href="/images/2018/08/classification-함수파일.zip">classification 함수파일</a></strong></span>
</p>

UCI machine learning repository는 양질의 다양한 dataset을 무료로 제공한다.

<span style="text-decoration: underline; color: #0000ff;"><a style="color: #0000ff; text-decoration: underline;" href="http://archive.ics.uci.edu/ml/datasets/Glass+Identification">http://archive.ics.uci.edu/ml/datasets/Glass+Identification</a></span>

오늘 사용할 dataset은 위의 링크에서 가져왔다.

각각의 카테고리에 굴절률, Na함량, Mg함량, Al함량, Si함량, K함량, Ca함량, Ba함량, Fe함량의 총 9개 feature에 따라서 어떻게 분류가 되는지 조사한 것이다.

<class>

  1. building window(float processed)
  2. building window(non float processed)
  3. vehicle window(float processed)
  4. vehicle window(non float processed)
  5. container
  6. tableware
  7. headlamp

위와 같이 총 7개의 class로 분류가된다.

그리고 training dataset의 갯수는 총 214개이다.

<img class="aligncenter size-full wp-image-369" src="/images/2018/08/no-name-16.png" alt="" width="604" height="466" srcset="/images/2018/08/no-name-16.png 604w, /images/2018/08/no-name-16-300x231.png 300w" sizes="(max-width: 604px) 100vw, 604px" /> 

가장 첫 번째 column은 index일뿐이므로 필요없다. 따라서 다음과 같이 data를 불러와야한다.

<img class="aligncenter size-full wp-image-370" src="/images/2018/08/no-name-17.png" alt="" width="281" height="73" /> 

이제 hypothesis function을 세우고 cost function을 만들어보겠다. 먼저 hypothesis function이 sigmoid함수의 형태이기때문에 미리 함수로 만들어놓으면 편리하다.

<img class="aligncenter size-full wp-image-371" src="/images/2018/08/no-name-18.png" alt="" width="304" height="169" srcset="/images/2018/08/no-name-18.png 304w, /images/2018/08/no-name-18-300x167.png 300w" sizes="(max-width: 304px) 100vw, 304px" /> 

참고로 위 함수에 행렬이나 벡터가 들어오면 각각의 성분에 함수가 적용된다.

다음으로 데이터를 불러오고 X,y에 저장하고 초기 theta를 설정하는 과정을 하나의 setData라는 파일에 담았다.

<img class="aligncenter size-full wp-image-372" src="/images/2018/08/no-name-19.png" alt="" width="931" height="549" srcset="/images/2018/08/no-name-19.png 931w, /images/2018/08/no-name-19-300x177.png 300w, /images/2018/08/no-name-19-768x453.png 768w" sizes="(max-width: 931px) 100vw, 931px" /> 

class라는 변수를 octave의 command window에서 우리가 n의 값을 바꿔가며 설정해주기 위해 위처럼 정의하였다. 그리고 초기 theta벡터의 모든 element는 0으로 초기화시킨다.

<img class="aligncenter size-full wp-image-373" src="/images/2018/08/no-name-20.png" alt="" width="542" height="286" srcset="/images/2018/08/no-name-20.png 542w, /images/2018/08/no-name-20-300x158.png 300w" sizes="(max-width: 542px) 100vw, 542px" /> 

cost function을 구하는 함수이다. linear regression파트에서 이미 한 번 보여주었던 코드이다. 앞서정의하였던 sigmoid function을 사용해서 hypothesis vector h를 만들고 cost function의 수식에 적용만 하면 된다.

<img class="aligncenter size-full wp-image-374" src="/images/2018/08/no-name-21.png" alt="" width="1254" height="226" srcset="/images/2018/08/no-name-21.png 1254w, /images/2018/08/no-name-21-300x54.png 300w, /images/2018/08/no-name-21-768x138.png 768w, /images/2018/08/no-name-21-1024x185.png 1024w" sizes="(max-width: 1254px) 100vw, 1254px" /> 

마지막으로 octave에서 지원하는 fminunc함수를 사용해서 costFunction의 최소값과 그 때의 parameter(θ)를 구하는 과정을 하나의 함수로 만들어서 저장한다.

<img class="aligncenter size-full wp-image-375" src="/images/2018/08/no-name-22.png" alt="" width="588" height="348" srcset="/images/2018/08/no-name-22.png 588w, /images/2018/08/no-name-22-300x178.png 300w" sizes="(max-width: 588px) 100vw, 588px" /> 

그리고 n을 바꿔가면서 여러번 실행을 해주면서 1~7의 각 class마다 model을 세워야 하기때문에 위와같이 run이라는 파일에 필요한 함수 명령어들을 모아두면 편리하다. 처음에 clear명령어로 먼저 앞에서 선언했던 모든 변수들을 깔끔하게 지워주는것이 좋다.

자 이제 돌려보자.

<img class="aligncenter size-full wp-image-377" src="/images/2018/08/no-name-24.png" alt="" width="880" height="364" srcset="/images/2018/08/no-name-24.png 880w, /images/2018/08/no-name-24-300x124.png 300w, /images/2018/08/no-name-24-768x318.png 768w" sizes="(max-width: 880px) 100vw, 880px" /> 

&nbsp;

이런식으로 n=7까지 늘려가며 계속 해주면 된다.

<img class="aligncenter size-full wp-image-378" src="/images/2018/08/no-name-25.png" alt="" width="880" height="578" srcset="/images/2018/08/no-name-25.png 880w, /images/2018/08/no-name-25-300x197.png 300w, /images/2018/08/no-name-25-768x504.png 768w" sizes="(max-width: 880px) 100vw, 880px" /> 

위의 명령어를 저장해놓고 한 번에 실행하면 편하다. 참고로 theta_matrix라는 행렬을 만들어 한 모든 theta들이 각 열에 포함되게 만들었다.

이제 우리가 구한 데이터로 예측을 할 수도 있겠다. training data에 포함된 data를 다시 한 번 넣어보면서 제대로 나왔는지 보자.

<img class="aligncenter size-full wp-image-379" src="/images/2018/08/no-name-26.png" alt="" width="932" height="456" srcset="/images/2018/08/no-name-26.png 932w, /images/2018/08/no-name-26-300x147.png 300w, /images/2018/08/no-name-26-768x376.png 768w" sizes="(max-width: 932px) 100vw, 932px" /> 

200번째 데이터는 실제로는 class7에 포함된다. 우리가 만든 model로 예측을 하려면 200번째 데이터 matrix를 theta_matrix와 곱하고 sigmoid함수에 집어넣어서 각 class의 hypothesis의 예측값을 보니 99%의 확률로 class7에 포함된다.

100번째 데이터에도 똑같이 적용을 해보니 38%의 확률로 class2에 속할 확률이 가장 높았고 실제로도 class2로 분류된다. 사실 혼자 코드를 만들어서 실제 데이터와 예측 결과를 보니 214개의 데이터 중에서 154개가 맞아떨어지고 60개가 맞아떨어지지 않았다.

썩 좋은 모델은 아니지만 multiclass인 경우에서 classification을 어떻게 하는지 확인한 것으로 만족해야겠다. 사실 알고리즘을 구현하면서 model의 feature도 신중하게 정해야하고 식의 차수나 형태 역시 잘 골라야한다. 그리고 overfit이 일어나는 경우라면 feature의 갯수를 줄이거나 regularization까지 해주어야한다.