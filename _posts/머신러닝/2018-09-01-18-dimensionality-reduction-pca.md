---
title: (머신러닝) 18. Dimensionality Reduction - PCA
date: 2018-09-01T14:32:22+09:00
author: SWnomad
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
<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;X&space;=&space;\begin{bmatrix}1&2&7&13&space;\\&space;4&8&9&4\\3&6&11&9&space;\\&space;\end{bmatrix}" alt="\dpi{120} X = \begin{bmatrix}1&2&7&13 \\ 4&8&9&4\\3&6&11&9 \\ \end{bmatrix}" align="absmiddle" />

위 행렬에서 두 번째 열은 첫 번째 열에 단순히 2를 곱하면 나온다. 위의 행렬이 dataset이라면 feature <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x_{1}" alt="\dpi{120} x_{1}" align="absmiddle" />과 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x_{2}" alt="\dpi{120} x_{2}" align="absmiddle" />는 완전히 동일한 정보를 담고 있는 것이다. 이처럼 완전히 동일한 정보는 아니더라도, 두 feature가 서로 highly correlated 되어있다면 이 두 feature를 잘 요약해주는 새로운 1개의 feature로 표현하는 것이 효율적이다.

highly correlated한 여러개의 feature가 있을 때 그 수를 줄여 더 적은 수의 feature로 나타내는 방법들이 있다. 그 중에 많이 쓰이는 방법 중 하나가 PCA(Principal Component Analysis)이다.

이러한 알고리즘을 적용하여 feature 수를 줄이면 계산에 필요한 메모리공간이 절약되고 시간복잡도 역시 줄일 수 있다. 그리고 feature를 3개 이하로 줄일 수 있다면 그래프상으로 visulalization하여 볼 수도 있다.

&nbsp;

&nbsp;

&nbsp;

<span style="font-size: 18pt;"><strong>1. PCA(Principal Component Analysis)</strong></span>

* * *

<img class="aligncenter size-full wp-image-669" src="/images/2018/08/no-name-108.png" alt="" width="934" height="306" srcset="/images/2018/08/no-name-108.png 934w, /images/2018/08/no-name-108-300x98.png 300w, /images/2018/08/no-name-108-768x252.png 768w" sizes="(max-width: 934px) 100vw, 934px" /> 

기본적인 idea는 이렇다. feature 2개를 어떠한 벡터에 projection하였을 때 수직 거리(error)들의 합이 가장 작은 벡터를 구한다. 이 새로운 vector에 각각의 data를 projection한 것을 새로운 data로 사용한다. 이렇게 하면  <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x_{1}" alt="\dpi{120} x_{1}" align="absmiddle" />과 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x_{2}" alt="\dpi{120} x_{2}" align="absmiddle" /> 두 개의 feature가 1개의 feature로 대체된다. 이 방법은 일반적으로 n개의 feature를 그보다 작은 k개의 feature vector <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;u^{(1)},u^{(2)},\cdot\cdot\cdot,u^{(k)}" alt="\dpi{120} u^{(1)},u^{(2)},\cdot\cdot\cdot,u^{(k)}" align="absmiddle" /> 로 줄이는 데 사용할 수 있다. PCA의 목적은 이 error의 합의 최소값을 찾는 것이다.

&nbsp;

&nbsp;

<span style="font-size: 14pt;"><strong>1) Pre-processing of the data</strong></span>

PCA 알고리즘을 적용하기 전 먼저 data를 feature normalization을 해준다.

  * i-th data의 j-th feature를 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}_{j}" alt="\dpi{120} x^{(i)}_{j}" align="absmiddle" />라고 할 때, 모든 data example의 j-th feature의 평균을 구한다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\mu_{j}=\frac{1}{m}\sum_{i=1}^{m}x^{(i)}_j" alt="\dpi{120} \mu_{j}=\frac{1}{m}\sum_{i=1}^{m}x^{(i)}_j" align="absmiddle" /> 

&nbsp;

  * 모든 데이터를 다음의 데이터로 바꾼다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}_{j}\rightarrow&space;x^{(i)}_j-\mu_{j}" alt="\dpi{120} x^{(i)}_{j}\rightarrow x^{(i)}_j-\mu_{j}" align="absmiddle" /> 

&nbsp;

  * feature마다 scale 차이가 크다면 feature scailing을 해준다. 을 한다.

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}_{j}\rightarrow&space;\frac{x^{(i)}_j-\mu_{j}}{\sigma_{j}}" alt="\dpi{120} x^{(i)}_{j}\rightarrow \frac{x^{(i)}_j-\mu_{j}}{\sigma_{j}}" align="absmiddle" /> 

&nbsp;

위의 과정을 거쳐서 얻은 새로운 data matrix  <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;X" alt="\dpi{120} X" align="absmiddle" />를 사용한다.

&nbsp;

<span style="font-size: 14pt;"><strong>2) PCA algorithm</strong></span>

먼저 PCA는 다음의 작업을 하는 것이라 할 수 있다.

  * n dimensional feature로 이루어진 n dimensional feature로 이루어진 data를 projection 했을 때 projection error가 가장 작은 k개의 벡터 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;u^{(1)},u^{(2)},\cdot\cdot\cdot,u^{(k)}" alt="\dpi{120} u^{(1)},u^{(2)},\cdot\cdot\cdot,u^{(k)}" align="absmiddle" />를 찾는다.
  * 위의 vector에 data를 projection하여 k dimensional ffeature로 이루어진 새로운 dataset <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;z^{(1)},z^{(2)},\cdot\cdot\cdot,z^{(m)}" alt="\dpi{120} z^{(1)},z^{(2)},\cdot\cdot\cdot,z^{(m)}" align="absmiddle" />을 찾는다.

이제 PCA 알고리즘을 사용하여 위의 작업을 해보자.

&nbsp;

<span style="font-size: 14pt;"><strong>a) covariant matrix 계산</strong></span>

<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Sigma&space;=&space;\frac{1}{m}\sum_{i=1}^{m}(x^{(i)})(x^{(i)})^T&space;=&space;\frac{1}{m}X^{T}\cdot&space;X" alt="\dpi{120} \Sigma = \frac{1}{m}\sum_{i=1}^{m}(x^{(i)})(x^{(i)})^T = \frac{1}{m}X^{T}\cdot X" align="absmiddle" /> 

<img class="alignleft" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Sigma" alt="\dpi{120} \Sigma" align="absmiddle" /> 를 합계 기호와 혼동하지 말아야한다. 하나의 행렬이다.

&nbsp;

<img class="alignleft" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\\x^{(i)}&space;:&space;n\times&space;1\qaud\quad\quad&space;\quad(x^{(i)})^{T}:1&space;\times&space;n&space;\\&space;X&space;:&space;m&space;\times&space;n\quad&space;\quad&space;\quad&space;X^{T}:n&space;\times&space;m" alt="\dpi{120} \\x^{(i)} : n\times 1\qaud\quad\quad \quad(x^{(i)})^{T}:1 \times n \\ X : m \times n\quad \quad \quad X^{T}:n \times m" align="absmiddle" /> 

이다. 따라서 covariant matrix는 <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;n&space;\times&space;n" alt="\dpi{120} n \times n" align="absmiddle" /> 행렬이다.

&nbsp;

<span style="font-size: 14pt;"><strong>b)  covariant matrix </strong></span><span style="font-size: 14pt;"><strong><img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\Sigma" alt="\dpi{120} \Sigma" align="absmiddle" /> 의 eigen-vectors&#8217; matrix  <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;U" alt="\dpi{120} U" align="absmiddle" /> 계산</strong></span>

covariant matrix의 eigen vector들로 이루어진 matrix  <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;U" alt="\dpi{120} U" align="absmiddle" />를 구하려면 singular value decomposition을 하면된다. 이는 복잡한 수식계산의 과정을 거쳐야 한다. 다양한 머신러닝 라이브러리에서는 이러한 계산을 해주는 함수를 지원한다. octave에서는 svd() 함수를 사용하면 된다. →<img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;U&space;=&space;svd(\Sigma)" alt="\dpi{120} U = svd(\Sigma)" align="absmiddle" />      <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;U" alt="\dpi{120} U" align="absmiddle" /> 는 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;n&space;\times&space;n" alt="\dpi{120} n \times n" align="absmiddle" /> 행렬이다. 그리고 바로 이 <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;U" alt="\dpi{120} U" align="absmiddle" />가 우리의 <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;X" alt="\dpi{120} X" align="absmiddle" />를 projection 할 line과 같은 것이다.

&nbsp;

<span style="font-size: 14pt;"><strong>c) k dimensional vector z들의 matrix <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;Z" alt="\dpi{120} Z" align="absmiddle" />를 구한다.</strong></span>

먼저 U의 1~k번째 column만으로 이루어진 matrix  <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;U_{reduce}" alt="\dpi{120} U_{reduce}" align="absmiddle" />를 구한다. <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;U_{reduce}" alt="\dpi{120} U_{reduce}" align="absmiddle" />는 <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;n&space;\times&space;k" alt="\dpi{120} n \times k" align="absmiddle" /> 행렬이다.

<img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;z^{(i)}=U_{reduce}^{T}\cdot&space;x^{(i)}\quad&space;\quad&space;\quad&space;Z=X\cdot&space;U_{reduce}" alt="\dpi{120} z^{(i)}=U_{reduce}^{T}\cdot x^{(i)}\quad \quad \quad Z=X\cdot U_{reduce}" align="absmiddle" />  는<img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;m&space;\times&space;k" alt="\dpi{120} m \times k" align="absmiddle" />  행렬이다. projection을 하여 새로운 k-dimensional featured data를 얻어내었다.

결국 우리는<img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;m&space;\times&space;n" alt="\dpi{120} m \times n" align="absmiddle" /> 행렬 <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;X" alt="\dpi{120} X" align="absmiddle" />에서 <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;m&space;\times&space;k" alt="\dpi{120} m \times k" align="absmiddle" />행렬 <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;Z" alt="\dpi{120} Z" align="absmiddle" />를 얻어냈다.

<img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;Z=\begin{bmatrix}&space;---z^{(1)}---&space;\\---z^{(2)}---\\&space;\cdot&space;\\&space;\cdot&space;\\---z^{(m)}---&space;\end{bmatrix}" alt="\dpi{120} Z=\begin{bmatrix} ---z^{(1)}--- \\---z^{(2)}---\\ \cdot \\ \cdot \\---z^{(m)}--- \end{bmatrix}" align="absmiddle" /> 

Z의 각 data example은 k개의 feature를 가지고 있다. 각각의 <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;z^{(i)}" alt="\dpi{120} z^{(i)}" align="absmiddle" />는 <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}" alt="\dpi{120} x^{(i)}" align="absmiddle" />를 vector <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;u^{(i)}" alt="\dpi{120} u^{(i)}" align="absmiddle" />에 projection한 data이다.

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

<span style="font-size: 18pt;"><strong>2. Reconstruction <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;X" alt="\dpi{150} X" align="absmiddle" /> from <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{150}&space;Z" alt="\dpi{150} Z" align="absmiddle" /></strong></span>

* * *

<img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;z^{(i)}=U_{reduce}^{T}\cdot&space;x^{(i)}\quad&space;\quad&space;\quad&space;Z=X\cdot&space;U_{reduce}" alt="\dpi{120} z^{(i)}=U_{reduce}^{T}\cdot x^{(i)}\quad \quad \quad Z=X\cdot U_{reduce}" align="absmiddle" />  의 관계가 되었었다.

반대로<img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;x^{(i)}_{approx}=U_{reduce}\cdot&space;z^{(i)}\quad&space;\quad&space;\quad&space;X_{approx}=Z\cdot&space;U_{reduce}^{T}" alt="\dpi{120} x^{(i)}_{approx}=U_{reduce}\cdot z^{(i)}\quad \quad \quad X_{approx}=Z\cdot U_{reduce}^{T}" align="absmiddle" /> 의 관계식을 이용하여 Z로부터 X를 복구할 수 있다. 하지만 완벽히 100% 복구되지는 못한다. 이미 근사(approximation)하여 어느정도의 error를 무시해버렸기 때문에 이로부터 복원되는 것도 X의 근사일 뿐이다.

&nbsp;

&nbsp;

&nbsp;

<span style="font-family: georgia, palatino, serif; font-size: 18pt;"><strong>3. Choosing the number of Principal Components</strong></span>

* * *

PCA 알고리즘을 사용하면 n개의 feature를 k개의 feature로 줄일 수 있다 하였다. 이 때 k를 몇으로 정하는 것이 좋을지 생각해보지 않을 수 없다.

  *<img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{average\quad&space;projection\quad&space;error}{total\quad&space;variation}=\frac{\frac{1}{m}\sum_{i=1}^{m}\left&space;\|&space;x^{(i)}-x^{(i)}_{approx}&space;\right&space;\|^{2}}{\frac{1}{m}\sum_{i=1}^{m}\left&space;\|&space;x^{(i)}&space;\right&space;\|^{2}}\leq&space;0.01" alt="\dpi{120} \frac{average\quad projection\quad error}{total\quad variation}=\frac{\frac{1}{m}\sum_{i=1}^{m}\left \| x^{(i)}-x^{(i)}_{approx} \right \|^{2}}{\frac{1}{m}\sum_{i=1}^{m}\left \| x^{(i)} \right \|^{2}}\leq 0.01" align="absmiddle" /> 를 만족하는 가장 작은 k를 선택한다.

이 때 99%의 variance가 보존된다고 본다. 0.01 이 아니라 0.05를 사용한다면 95%의 variance가 보존되는 것이다. variance가 보존된다는 것은 기존의 data들의 relevant한 position이 유지된다는 말이다. 되도않는 hyperplane을 골라서 projection하면 기존의 class별 classification 되어있던 data들이 오히려 더 섞여 classification이 악화될 소지가 있는 것이다.

&nbsp;

지금까지의 내용으로 결론을 지어보자.

<table style="width: 100%; border-collapse: collapse; border-style: double; background-color: #ebe8e8;" border="1">
  <tr>
    <td style="width: 100%;">
      1) k=1, 2, 3, ··· 순차적으로 늘려가며 PCA를 적용한다.</p> 
      
      <p>
        2) <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;U_{reduce},z^{(i)},x_{approx}^{(i)}" alt="\dpi{120} U_{reduce},z^{(i)},x_{approx}^{(i)}" align="absmiddle" />를 계산한다.
      </p>
      
      <p>
        3) 99%이상의 variance가 보존되는지 check하여 실패하면 k를 1 늘려서 다시 test한다. 위에서 말한 variance 보존 공식을 적용하는 k 중 가장 작은 값을 찾을 때 까지이다.</td> </tr> </tbody> </table> 
        
        <p>
          결국은 위와 같은 알고리즘을 짜는 것인데, 비효율적이고 귀찮은 작업이라는 느낌이 든다. 아까 말했던 <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;U&space;=&space;svd(\Sigma)" alt="\dpi{120} U = svd(\Sigma)" align="absmiddle" />함수를 기억하는가? 위에서는 U만 썼지만, svd() 함수는 3개의 값을 return 해준다. 코드를 다음과 같이 쓰면 된다.
        </p>
        
        <p>
          <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;[U,S,V]&space;=&space;svd(\Sigma)" alt="\dpi{120} [U,S,V] = svd(\Sigma)" align="absmiddle" />
        </p>
        
        <p>
          <img src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;U" alt="\dpi{120} U" align="absmiddle" />는 covariant matrix의 eigen vectors&#8217; matrix이라고 이미 알고 있다. 두 번째 return value인 S를 통해 variance가 얼마나 보존되는지 check할 수 있다.
        </p>
        
        <p>
          <img class="alignnone" src="https://latex.codecogs.com/gif.latex?\dpi{120}&space;\frac{\sum_{i=1}^{k}S_{ii}}{\sum_{i=1}^{n}S_{ii}}\geq&space;0.99" alt="\dpi{120} \frac{\sum_{i=1}^{k}S_{ii}}{\sum_{i=1}^{n}S_{ii}}\geq 0.99" align="absmiddle" /> 이 식을 만족한다면 variance가 99%이상 보존되는 것이다. k만 입력해주면 알아서 계산을 해주니 편하고 시간도 절약된다.
        </p>
        
        <p>
          아무튼 이렇게 해서 작은 k를 찾으면 성공적으로 feature의 dimension이 줄어들어 계산해야 하는 메모리를 줄이고 모델의 학습시간을 개선시켜 줄 것이다. 또한 운이 좋아 3 이하의 k를 찾는다면 visualization으로 데이터의 패턴을 직관적으로 파악하는 데에도 도움을 줄 수 있다.
        </p>
        
        <p>
          그렇다고 무작정 처음부터 PCA를 적용할 것이 아니라 먼저 모델을 학습시켜보고 필요할 때에만 하는 것이 좋다.
        </p>