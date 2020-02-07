---
title: (머신러닝) (ex)신경망을 활용한 숫자인식 모델 만들기
date: 2018-08-18T00:53:54+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - artificial neural network
  - digit recognition
  - digit recognization
  - digit recognize
  - fminunc
  - neural network
  - 숫자인식
  - 신경망
  - 신경망 알고리즘
  - 인공신경망
  - 패턴인식
  - 필기 숫자 인식
---
&nbsp;

<p style="text-align: right;">
  <a role="button" href="/images/2018/08/digitRecog.zip"><span style="font-size: 14pt; color: #0000ff;"><strong><span style="text-decoration: underline;">소스코드.zip</span></strong></span><br/> </a>
</p>

이번 예제는 신경망을 학습하는 이론적인 방법에 대해 포스팅한 아래 링크에 해당하는 내용을 실제로 구현한 것입니다. 이 글의 상단에 소스코드가 첨부되어있으니 받아서 돌려보셔도 좋습니다.

https://SWnomad.com/12-back-propagation역전파-알고리즘

필기숫자 인식은 고전적이지만 머신러닝의 신경망 알고리즘이 적용되는 가장 대표적인 문제입니다.

우리가 숫자인식을 학습시킬 훈련 데이터를 직접 하나하나 만들어내기는 시간적으로 너무 비효율적입니다. 숫자인식 훈련 데이터의 정석이라 불리는 미국의 Yann LeCun 교수가 제공하는 MNIST dataset이 있는데, 오늘은 이것을 활용할 것입니다.

MNIST dataset은 다음의 link에서 받을 수 있습니다.

<http://yann.lecun.com/exdb/mnist/index.html>

그러나 위의 사이트에서 제공하는 .gz 확장자는 일반적인 이미지 format이 아닙니다. 검색을 통해 마침 적절하게 octave에서 바로 load하여 쓸 수 있는 .mat 확장자형태로 변환해 놓은 것이 있는데, 너무 감사한 일입니다. 이 .mat 확장자의 MNIST dataset은 아래의 링크를 타고가면 받을 수 있습니다.

<https://github.com/daniel-e/mnist_octave>

MNIST dataset은 다음과 같은 구성으로 이루어져 있습니다.

60000개의 28×28 pixel의 training data

60000개의 training label. 이 label은 1,2,3,4,5,6,7,8,9,10입니다. class 10은 숫자 0에 해당합니다..

10000개의 28×28 test data. 이 data로 학습된 신경망을 test 해볼 수 있다.

10000개의 test용 label.

<img src="/images/2018/08/no-name-62.png" alt="" width="1034" height="168" /> 

mnist.mat 파일을 저장한 디렉토리로 가서 위의 명령어로 파일을 로드하면 왼쪽과 같이 4개의 행렬이 생깁니다. 여기서 신경망 학습에 사용 될 데이터는 trainX와 label인 trainY구요.  label은 0~9 까지이다. trainX의 60000개의 이미지 중에 99번 째 이미지를 한 번 보죠.

<img src="/images/2018/08/no-name-63.png" alt="" width="1179" height="583" /> 

먼저 reshape 함수를 사용해서 하나의 행으로 늘어져있는 784개 pixel의 명암정보를 28*28 행렬로 변환한 변수 A를 만듭니다. 그리고 imagesc함수를 사용해서 그림으로 표현하는데, colobar를 표시하고 colormap을 gray scale로 설정했습니. 위의 그림에서 colorbar를 보면 숫자 0이 완전한 black, 255가 완전한 white로 표현되는 걸 볼 수 있습니다. 아무튼 image pixel값은 0에서 255사이의 숫자를 사용해서 명암을 표시합니다. 위의 99번째 training data sample은 숫자 3을 그린 것이란 것을 알 수 있습니다.

**0. 데이터 로드**

<img src="/images/2018/08/no-name-65.png" alt="" width="611" height="183" /> 

mnist data의 변수는  uint8형이라 먼저 실수형의 double로 바꿔주어야 합니다. 아니면 곱하기 연산자인 *가 먹히질 않습니다. 그리고 데이터를 60000개 다 쓰지않고 10000개만 쓰겠습니다. 6만개로 하면 계산하는데 시간이 너무오래 걸립니다ㅜㅜ. 알고리즘을 이해하고 구현한다는 것에 의미를 두면 충분합니다.

**1. 신경망의 구조 결정**

input layer : feature가 28*28 = 784이므로 784개의 node

hidden layer : 300개의 node

output layer : label의 수가 10이므로 10개의 node

<img src="/images/2018/08/no-name-64.png" alt="" width="413" height="134" /> 

**2. parameter 초기화**

regression 모델에서는 parameter를 모두 0으로 초기화 해도 상관 없었습니다. 하지만 **신경망 모델에서 파라미터를 0이던 어떤 수인지던간에 같은 값으로 초기화를 하면 forward propagation이던지 back propagation이던지 한 layer의 모든 unit이 같은 값을 가지게 되어서 의미없는 모델이 만들어집니다.** 신경망 모델에서 흔히 쓰이는 초기화 방법으로 행렬의 값들을 랜덤하게 모두 다른 수로 만들어주는 함수를 사용합니다.

<img src="/images/2018/08/no-name-66.png" alt="" width="793" height="158" /> 

input layer와 output layer의 갯수에 의해 결정되는 ε이라는 작은 수를 만들고 위와 같이 rand함수를 사용해서 parameter를 초기화 시킵니다.

**3 & 4. forward propagation & back propagation**

<img src="/images/2018/08/no-name-69.png" alt="" width="1339" height="376" /> 

<img src="/images/2018/08/no-name-67.png" alt="" width="1339" height="549" /> 

우리가 load 한 label y는 0~9의 값을 가지고 있습니다. 하지만 octave의 index는 1부터 시작하죠. 따라서  y의 값 중 0인 것들을 10으로 바꿉니다.

그리고 y\_bin이란 것은 y를 label 벡터화 시킨 것입니다. 예를들어 y가 7이라면 y\_bin은 index 7의 원소만 1이고 나머지는 0인 벡터이죠. 애초에 y는 아래와 같은 형태여야 합니다.

<img src="https://latex.codecogs.com/gif.latex?dpi{120}&space;yin&space;begin&space;{bmatrix}1&space;cdot&space;cdot&space;end{bmatrix},begin&space;{bmatrix}01&space;cdot&space;cdot&space;end{bmatrix},cdotcdotcdot,begin&space;{bmatrix}0&space;cdot&space;cdot1&space;end{bmatrix}" alt="dpi{120} yin begin {bmatrix}1 cdot cdot end{bmatrix},begin {bmatrix}01 cdot cdot end{bmatrix},cdotcdotcdot,begin {bmatrix}0 cdot cdot1 end{bmatrix}" align="absmiddle" /> 

그리고 parameterVector라는 것은 Theta1과 Theta2의 모든 원소를 일렬로 쭉 풀어놓 것입니다. 하나의 벡터로 말입니다. 이후에 cost function을 자동으로 최적화 시켜주는 fminunc같은 종류의 함수를 사용할 때 함수의 variable로써 벡터는 받을 수 있지만 행렬은 받지 못하기 때문입니다.

그리고 cost function J, error를 누적시키는 Δ,  label벡터 y_bin을 만들어주고 초기화 시킵니다.

그 다음 1번째 training data부터 m번째 training data까지 모두 순전파, 역전파를 거치며 cost function과 각 node의 error들을 누적시킵니다. 이 후 for 반복문이 끝나고 J에 regularization 항을 넣어주고 누적시킨 error인 delta로부터 J의 gradient를 구합니다.

아무튼 위의 함수가 return하는 값은 [J grad] 입니다. J는 계산된 cost값이고, grad는 <img src="https://latex.codecogs.com/gif.latex?dpi{120}&space;frac{partial&space;J}{partial&space;Theta}" alt="dpi{120} frac{partial J}{partial Theta}" align="absmiddle" />를 일렬로 쭉 풀어놓은 벡터입니다.

아! 참고로 cost function을 계산하는 지금 함수는 전달인자로 regularization parameter λ가 들어오는데 3으로 넣어줄겁니다 . 다음과 같이 말이에요.

<img src="/images/2018/08/no-name-68.png" alt="" width="1264" height="134" /> 

**5. gradient check**

우리가 gradient를 구하는 알고리즘을 제대로 짠 게 맞는지 안맞는지 확신을 하고가면 훨씬 좋을 것 같습니다. 기껏 시간들여 학습을 시켜놓았는데 예측 정확성이 말도 안되게 낮다거나 다른 문제가 있어 봤더니 실수가 있었다면 헛수고잖아요. 이 때에 우리는 gradient check를 하는거에요.

<img src="/images/2018/08/no-name-71.png" alt="" width="1337" height="621" /> 

gradient check를 할 때에 우리가 학습시킬 모델의 구조와 training dataset을 그대로 사용할 필요 없습니다. 학습시키는데 시간이 좀 오래 걸리거든요. 따라서 위와같이 size가 작고 단순한 새로운 신경망을 만들고 X와 y도 위와 같이 rand함수와 mod함수를 사용해서 새로 만든 간단한 신경망에 맞는 것으로 다시 만듭니다.사실 mod(a,b)는 a를 b로 나눈 나머지를 뱉어내는 함수인데 위처럼 코드를 짜주면 중복되지 않는 수로 이루어진 값으로 y를 만들 수 있습니다. parameter를 초기화 시킵니다.

costFunc라는 것은 costFunction의 parameterVector가 들어갈 자리에 t를 넣음으로써 cost function을 나머지 variable을 상수로 고정시키고 parameterVector만의 함수로 만든거에요. parameterVector의 값을 변경시켜주면서 costfunction의 변화를 보기 위함입니다.

for 반복문에서는 gradient의 근사를 구합니다. perturb가 충분히 작다면 거의 똑같이 나와야합니다.

마지막으로 grad와 approxGrad를 출력하도록 코드를 넣어주면 둘을 비교할 수 있어요. 이 두 벡터가 거의 같아야 우리가 짠 알고리즘이 제대로 gradient를 계산하고 있다는 증거입니다.

**6. parameter update**

<img src="/images/2018/08/no-name-70.png" alt="" width="1169" height="264" /> 

이제 octave에서 제공하는 최적화 함수를 써서 parameter를 학습시킬 차례입니다.

costFunction 함수로부터 nn_costFunction이라는 새로운 함수를 만듭니다. 이 함수는 다른 variale을 상수로 고정시키고 parameterVector의 변화에 따른 costFunction의 변화를 보기위한 함수입니다. @(t)는 &#8220;at t&#8221;라고 읽으며 t에 대한 함수라고 해석할 수 있습니다. 위에서는 parameterVector를 t로 치환하였어요.

그리고 octave에서 기본적으로 제공하는 fminunc라는 함수를 사용하지 않고 fmincg라는 optimization함수를 사용했는데, 이 함수는 octave에서 기본적으로 제공하는 함수가 아닙니다. 외부에 fminuc함수를 조금 조작해서 fmincg라는 함수로 만든 것인데 Standford의 Andrew Ng. 교수의 코드입니다. 굳이 소스를 알 필요 없고 가져다 쓰도록 합시다. fminunc함수를 쓰면 memory 어쩌구 하는 오류가 뜨기 때문입니다. 아무튼 fmincg함수를 사용해서 nn_costFunction이 최소가 되도록 학습시킵니다. 초기값으로는 위에서 우리가 rand함수를 써서 랜덤하게 초기화시킨 parameterVector를 넣어주고, 50번의 학습 iteration을 거치도록 설정한 옵션을 넣어줘요. 이 때 fmincg함수는 최적화된 parameterVector인 optParameterVector와 그 때의 cost값 J를 return합니다. 그리고 이제 optParameterVector로부터 최적화 된 parameter matrix인 optTheta1과 optTheta2를 만들어줍니다.

이제 우리는 trainingdataset을 사용해서 학습시킨 최적화 된 parameter를 가지고 있는겁니다. 과연 50번의 iteration이 충분했는지는 아직은 모릅니다. 지금부터 보도록 하죠. 아! 참고로 학습시키는데 시간이 꽤 오래 걸립니다 ㅜㅜ. 데이터를 60000개중에 10000개만 썼는데도 한 30분 쫌 넘게 걸렸던 거 같습니다.

**7. prediction**

처음에 mnist.mat을 load 했을 때 trainX와 trainY가 아닌 testX와 testY가 있었을겁니다. 각각 10000개의 이미지와 그에 해당하는 10000개의 라벨인데요 이제 이전에 본적없던 이미지인 test 데이터를 이용해서 얼마나 잘 맞추는지 보겠습니다.

<img src="/images/2018/08/no-name-72.png" alt="" width="1034" height="608" /> 

5000개의 데이터에 대해서만 테스트를 해보겠습니다.

<img src="/images/2018/08/no-name-73.png" alt="" width="514" height="70" /> 

91.74%의 확률로 예측을 하네요. 과연 cost function이 수렴하여 최소가 될 때 까지 충분히 iteration을 한 것인가 의문이 생깁니다. 궁금하시면 이 부분은 iteration에 대한 J의 그래프를 그려서 확인해셔도 됩니다.

아무튼 신경망 알고리즘을 설계하고 학습시키는 방법과 실제 구현을 해서 예측하는데 사용해보았습니다. 더 해보고 싶은신게 있으시면 UCL machine learning repository등의 open data 제공 사이트에 가서 직접 구현해보셔도 좋습니다.