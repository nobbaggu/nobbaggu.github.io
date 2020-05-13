---
title: (머신러닝) 18. Dimensionality Reduction - PCA
date: 2018-09-01T14:32:22+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - covariant matrix
  - dimensionality reduction
  - highly correlated features
  - k-means algorithm
  - k-means 알고리즘
  - matlab svd
  - octave svd
  - PCA
  - principal component analysis
  - redundant features
  - singular value decomposition
  - svd function
  - svd 함수
---
$$\dpi{120} X = \begin{bmatrix}1&2&7&13 \\ 4&8&9&4\\3&6&11&9 \\ \end{bmatrix}$$

위 행렬에서 두 번째 열은 첫 번째 열에 단순히 2를 곱하면 나온다. 위의 행렬이 dataset이라면 feature $$\dpi{120} x_{1}$$과 $$\dpi{120} x_{2}$$는 완전히 동일한 정보를 담고 있는 것이다. 이처럼 완전히 동일한 정보는 아니더라도, 두 feature가 서로 highly correlated 되어있다면 이 두 feature를 잘 요약해주는 새로운 1개의 feature로 표현하는 것이 효율적이다.

highly correlated한 여러개의 feature가 있을 때 그 수를 줄여 더 적은 수의 feature로 나타내는 방법들이 있다. 그 중에 많이 쓰이는 방법 중 하나가 PCA(Principal Component Analysis)이다.

이러한 알고리즘을 적용하여 feature 수를 줄이면 계산에 필요한 메모리공간이 절약되고 시간복잡도 역시 줄일 수 있다. 그리고 feature를 3개 이하로 줄일 수 있다면 그래프상으로 visulalization하여 볼 수도 있다.

&nbsp;

&nbsp;

&nbsp;

<span style="font-size: 18pt;"><strong>1. PCA(Principal Component Analysis)</strong></span>

* * *

![image](/images/2018/08/no-name-108.png){: width="50%" height="50%"}

기본적인 idea는 이렇다. feature 2개를 어떠한 벡터에 projection하였을 때 수직 거리(error)들의 합이 가장 작은 벡터를 구한다. 이 새로운 vector에 각각의 data를 projection한 것을 새로운 data로 사용한다. 이렇게 하면  $$\dpi{120} x_{1}$$과 $$\dpi{120} x_{2}$$ 두 개의 feature가 1개의 feature로 대체된다. 이 방법은 일반적으로 n개의 feature를 그보다 작은 k개의 feature vector $$\dpi{120} u^{(1)},u^{(2)},\cdot\cdot\cdot,u^{(k)}$$ 로 줄이는 데 사용할 수 있다. PCA의 목적은 이 error의 합의 최소값을 찾는 것이다.

&nbsp;

&nbsp;

<span style="font-size: 14pt;"><strong>1) Pre-processing of the data</strong></span>

PCA 알고리즘을 적용하기 전 먼저 data를 feature normalization을 해준다.

  * i-th data의 j-th feature를 $$\dpi{120} x^{(i)}_{j}$$라고 할 때, 모든 data example의 j-th feature의 평균을 구한다.

$$\dpi{120} \mu_{j}=\frac{1}{m}\sum_{i=1}^{m}x^{(i)}_j$$ 

&nbsp;

  * 모든 데이터를 다음의 데이터로 바꾼다.

$$\dpi{120} x^{(i)}_{j}\rightarrow x^{(i)}_j-\mu_{j}$$ 

&nbsp;

  * feature마다 scale 차이가 크다면 feature scailing을 해준다. 을 한다.

$$\dpi{120} x^{(i)}_{j}\rightarrow \frac{x^{(i)}_j-\mu_{j}}{\sigma_{j}}$$ 

&nbsp;

위의 과정을 거쳐서 얻은 새로운 data matrix  $$\dpi{120} X$$를 사용한다.

&nbsp;

<span style="font-size: 14pt;"><strong>2) PCA algorithm</strong></span>

먼저 PCA는 다음의 작업을 하는 것이라 할 수 있다.

  * n dimensional feature로 이루어진 n dimensional feature로 이루어진 data를 projection 했을 때 projection error가 가장 작은 k개의 벡터 $$\dpi{120} u^{(1)},u^{(2)},\cdot\cdot\cdot,u^{(k)}$$를 찾는다.
  * 위의 vector에 data를 projection하여 k dimensional ffeature로 이루어진 새로운 dataset $$\dpi{120} z^{(1)},z^{(2)},\cdot\cdot\cdot,z^{(m)}$$을 찾는다.

이제 PCA 알고리즘을 사용하여 위의 작업을 해보자.

&nbsp;

<span style="font-size: 14pt;"><strong>a) covariant matrix 계산</strong></span>

$$\dpi{120} \Sigma = \frac{1}{m}\sum_{i=1}^{m}(x^{(i)})(x^{(i)})^T = \frac{1}{m}X^{T}\cdot X$$ 

![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Sigma){: width="50%" height="50%"}

&nbsp;

![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\x^{(i)}&space;:&space;n\times&space;1\qaud\quad\quad&space;\quad(x^{(i)})^{T}:1&space;\times&space;n&space;\\&space;X&space;:&space;m&space;\times&space;n\quad&space;\quad&space;\quad&space;X^{T}:n&space;\times&space;m){: width="50%" height="50%"}

이다. 따라서 covariant matrix는 ![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;n&space;\times&space;n){: width="50%" height="50%"}

&nbsp;

<span style="font-size: 14pt;"><strong>b)  covariant matrix </strong></span><span style="font-size: 14pt;"><strong>![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;U){: width="50%" height="50%"}

covariant matrix의 eigen vector들로 이루어진 matrix  ![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;X){: width="50%" height="50%"}

&nbsp;

<span style="font-size: 14pt;"><strong>c) k dimensional vector z들의 matrix ![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;Z){: width="50%" height="50%"}

먼저 U의 1~k번째 column만으로 이루어진 matrix  ![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;n&space;\times&space;k){: width="50%" height="50%"}

![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;m&space;\times&space;k){: width="50%" height="50%"}

결국 우리는![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;Z){: width="50%" height="50%"}

![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;Z=\begin{bmatrix}&space;---z^{(1)}---&space;\\---z^{(2)}---\\&space;\cdot&space;\\&space;\cdot&space;\\---z^{(m)}---&space;\end{bmatrix}){: width="50%" height="50%"}

Z의 각 data example은 k개의 feature를 가지고 있다. 각각의 ![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;u^{(i)}){: width="50%" height="50%"}

이제 이 dataset을 가지고 K-means 알고리즘이나 다른 unsupervised 알고리즘을 적용하면 된다.

&nbsp;

뭔가 꽤 설명한 것 같지만 다음과 같은 간결하고 몇 줄 되지 않는 matlab 코드로 PCA알고리즘이 구현된다.

`1  Sigma = (1/m) * X' * X; % compute the covariance matrix<br/>
2  [U,S,V] = svd(Sigma);   % compute our projected directions<br/>
3  Ureduce = U(:,1:k);     % take the first k directions<br/>
4  Z = X * Ureduce;        % compute the projected data points`

&nbsp;

&nbsp;

&nbsp;

<span style="font-size: 18pt;"><strong>2. Reconstruction ![image](https://latex.codecogs.com/gif.latex?\dpi{150}&space;Z){: width="50%" height="50%"}

* * *

![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;z^{(i)}=U_{reduce}^{T}\cdot&space;x^{(i)}\quad&space;\quad&space;\quad&space;Z=X\cdot&space;U_{reduce}){: width="50%" height="50%"}

반대로![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}_{approx}=U_{reduce}\cdot&space;z^{(i)}\quad&space;\quad&space;\quad&space;X_{approx}=Z\cdot&space;U_{reduce}^{T}){: width="50%" height="50%"}

&nbsp;

&nbsp;

&nbsp;

<span style="font-family: georgia, palatino, serif; font-size: 18pt;"><strong>3. Choosing the number of Principal Components</strong></span>

* * *

PCA 알고리즘을 사용하면 n개의 feature를 k개의 feature로 줄일 수 있다 하였다. 이 때 k를 몇으로 정하는 것이 좋을지 생각해보지 않을 수 없다.

  *![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{average\quad&space;projection\quad&space;error}{total\quad&space;variation}=\frac{\frac{1}{m}\sum_{i=1}^{m}\left&space;\|&space;x^{(i)}-x^{(i)}_{approx}&space;\right&space;\|^{2}}{\frac{1}{m}\sum_{i=1}^{m}\left&space;\|&space;x^{(i)}&space;\right&space;\|^{2}}\leq&space;0.01){: width="50%" height="50%"}

이 때 99%의 variance가 보존된다고 본다. 0.01 이 아니라 0.05를 사용한다면 95%의 variance가 보존되는 것이다. variance가 보존된다는 것은 기존의 data들의 relevant한 position이 유지된다는 말이다. 되도않는 hyperplane을 골라서 projection하면 기존의 class별 classification 되어있던 data들이 오히려 더 섞여 classification이 악화될 소지가 있는 것이다.

&nbsp;

지금까지의 내용으로 결론을 지어보자.

<table style="width: 100%; border-collapse: collapse; border-style: double; background-color: #ebe8e8;" border="1">
  <tr>
    <td style="width: 100%;">
      1) k=1, 2, 3, ··· 순차적으로 늘려가며 PCA를 적용한다.</p> 
      
      <p>
        2) $$\dpi{120} U_{reduce},z^{(i)},x_{approx}^{(i)}$$를 계산한다.
      </p>
      
      <p>
        3) 99%이상의 variance가 보존되는지 check하여 실패하면 k를 1 늘려서 다시 test한다. 위에서 말한 variance 보존 공식을 적용하는 k 중 가장 작은 값을 찾을 때 까지이다.</td> </tr> </tbody> </table> 
        
        <p>
          결국은 위와 같은 알고리즘을 짜는 것인데, 비효율적이고 귀찮은 작업이라는 느낌이 든다. 아까 말했던 $$\dpi{120} U = svd(\Sigma)$$함수를 기억하는가? 위에서는 U만 썼지만, svd() 함수는 3개의 값을 return 해준다. 코드를 다음과 같이 쓰면 된다.
        </p>
        
        <p>
          $$\dpi{120} [U,S,V] = svd(\Sigma)$$
        </p>
        
        <p>
          $$\dpi{120} U$$는 covariant matrix의 eigen vectors&#8217; matrix이라고 이미 알고 있다. 두 번째 return value인 S를 통해 variance가 얼마나 보존되는지 check할 수 있다.
        </p>
        
        <p>
          ![image](https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{\sum_{i=1}^{k}S_{ii}}{\sum_{i=1}^{n}S_{ii}}\geq&space;0.99){: width="50%" height="50%"}
        </p>
        
        <p>
          아무튼 이렇게 해서 작은 k를 찾으면 성공적으로 feature의 dimension이 줄어들어 계산해야 하는 메모리를 줄이고 모델의 학습시간을 개선시켜 줄 것이다. 또한 운이 좋아 3 이하의 k를 찾는다면 visualization으로 데이터의 패턴을 직관적으로 파악하는 데에도 도움을 줄 수 있다.
        </p>
        
        <p>
          그렇다고 무작정 처음부터 PCA를 적용할 것이 아니라 먼저 모델을 학습시켜보고 필요할 때에만 하는 것이 좋다.
        </p>