---
title: (머신러닝) 16. SVM(Support Vector Machine)
date: 2018-08-24T20:24:11+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - decision boundary
  - gaussian kernel
  - kernel
  - large margin classifier
  - linear kernel
  - margin
  - similarity
  - similarity function
  - support vector machine
  - SVM
  - 마진
  - 서포트 벡터 머신
  - 최대 마진
---
![image](/images/2018/08/no-name-92-300x187.png){: width="50%" height="50%"}

위 그림의 classification 문제에서 decision boundary는 3가지 모두 될 수 있다. 하지만 어떤 decision boundary를 사용하는 것이 새로운 test data에 대해서도 더 좋은 예측을 할까? 직관적으로 당연히 boundary 2가 가장 좋은 분류를 할 것이라고 알 수 있다. decision boundary 1과 2는 boundary와 가장 인접한 data가 간신히 정답으로 분류된 느낌을 준다. 조금만 벗어났어도 결과가 달라질 수 있다. 하지만 decision boundary 2는 조금 더 여유로운 느낌을 준다. 이 상황을 margin이 많이 남는다고 한다. <span style="color: #ff0000;"><strong>support vector machine은 margin이 최대가 되는 decision boundary를 찾아내도록 해주며, 아직까지도 분류 알고리즘으로 엄청 많이 쓰이고있다.</strong></span>

이제부터 SVM 알고리즘을 차근차근 파헤쳐 볼것이다.

&nbsp;

&nbsp;

<span style="font-size: 18pt;"><strong>1. hypothesis function & cost function</strong></span>

* * *

  * <span style="font-size: 14pt;"><strong>classification(logistic regression) hypothesis function & cost function</strong></span>

$$h_{\theta}(x)=\frac{1}{1+e^{-\theta^{T}x}}$$ 

y=1일 때 $$h_{\theta}(x)\approx 1$$ 즉, $$\theta^{T}x\gg 0$$이 되게하는 $$\theta$$를 찾는것이 목적

y=0일 때 $$h_{\theta}(x)\approx 0$$ 즉,$$\theta^{T}x\ll0$$ 이 되게하는 $$\theta$$를 찾는것이 목적

&nbsp;

$$\\J(\theta)=-\frac{1}{m}[y\cdot log(h_{\theta}(x))+(1-y)\cdot log(1-h_{\theta}(x))]\\ \\ = -\frac{1}{m}[y\cdot log(\frac{1}{1+e^{-\theta^{T}x}})+(1-y)\cdot log(1-\frac{1}{1+e^{-\theta^{T}x}})]$$ 

&nbsp;

&nbsp;

  * <span style="font-size: 14pt;"><strong>SVM hypothesis function & cost function</strong></span>

classification에서 각 클래스의 cost는 각각 다음과 같았다.

$$\\y=1:\quad cost_{1}=-log(\frac{1}{1+e^{-\theta^{T}x}})\\ y=0:\quad cost_{0}=-log(1-\frac{1}{1+e^{-\theta^{T}x}})$$ 

하지만 SVM에서는 조금 다른 cost를 사용한다.

$$\\y=1:\quad cost_{1}=max(0,k(1-z))\quad\quad z=\theta^{T}x\\ y=0:\quad cost_{0}=max(0,k(1+z))\quad\quad z=\theta^{T}x$$ 

![image](/images/2018/08/no-name-94.png){: width="50%" height="50%"}

위와같이 새로운 cost function형태의 함수를 hinge loss function이라 부른다.

다시 cost function을 하나의 식으로 정리하면,

$$J(\theta)= \frac{1}{m}[y\cdot cost_{1}(\theta^{T}x)+(1-y)\cdot cost_{0}(\theta^{T}x)]+\frac{\lambda}{2m}\sum_{j=1}^{n}\theta_{j}^{2}$$ 

optimization에 영향이 없으므로 m을 곱하여 식을 간단히 하고 $$C=\frac{1}{\lambda}$$라는 새로운 constant를 곱해주면

$$J(\theta)= C[y\cdot cost_{1}(\theta^{T}x)+(1-y)\cdot cost_{0}(\theta^{T}x)]+\frac{1}{2}\sum_{j=1}^{n}\theta_{j}^{2}$$ 

결국 우리는 위 식이 최소가 되도록 θ를 optimization 하면 된다. $$\lambda$$를 사용하나 C를 사용하나 같다. 결국 어떠한 parameter를 크게 한다는 이 parameter가 곱해진 term의 weight를 올린다는 말이고 이는 최소화 시키는 과정에 더 큰 영향을 받는다는 말이기 때문이다.

&nbsp;

$$h_{\theta}(x)=\begin{Bmatrix} 1\quad\quad\quad if\quad \theta^{T}x\geq 0 \\ 0\quad\quad\quad if\quad \theta^{T}x<0 \end{Bmatrix}$$ 

y=1일 때 $$h_{\theta}(x)\approx 1$$ 즉, $$\theta^{T}x\gg 0$$이 되게하는 $$\theta$$를 찾는것이 목적

y=0일 때 $$h_{\theta}(x)\approx 0$$ 즉,$$\theta^{T}x\ll0$$ 이 되게하는 $$\theta$$를 찾는것이 목적

&nbsp;

SVM의 hypothesis는 logistic regression처럼 class가 1이나 0이 될 확률로 해석하지 않는다. 그저 1이나 0이 될 뿐이다.

&nbsp;

<span style="font-size: 18pt;"><strong>2. decision boundary with Large margin</strong></span>

* * *

![image](/images/2018/08/no-name-93-300x205.png){: width="50%" height="50%"}

위 그림에서 가운데 있는 굵은 선을 decision boundary로 잡는다고 해보자. decision boundary이기 때문에 이 선은$$\theta^{T}x=\theta_{0}+\theta_{1}x_{1}+\theta_{2}x_{2}+\cdot\cdot\codt+\theta_{n}x_{n} = 0$$  의 방정식을 만족한다. 그리고 이 경계선에서 벗어나 있는 점들에 θ를 곱하면 $$\theta^{T}x=c \quad (c\neq 0)$$가 된다. 모든 데이터가 $$\left | \theta^{T}x \right |\geq 1$$을 만족하도록 decision boundary를 잡아 margin을 크게 하여 안정된 예측을 하는 것이 목표이다. $$\left | \theta^{T}x \right |\geq 0$$이 아니라 $$\left | \theta^{T}x \right |\geq 1$$ 인 것에 주목해야한다. 이것이 support vector machine 알고리즘을 사용하는 목적이기 때문이다. 0이 아닌 1을 기준으로 잡은 이유는 넉넉히 여유를 남기고 예측하고 싶어서이다. 여유를 남긴다는 것은 지금까지 본 적 없는 data에 대한 예측 적중률이 올라가는 것을 의미한다.

이를 다른 관점에서 보자.

![image](/images/2018/08/no-name-95.png){: width="50%" height="50%"}

기하학적으로 $$\theta^{T}x$$는 ($$\theta$$ 크기)×($$x$$를 벡터 $$\theta$$에 projection한 벡터의 크기 )이다.

![image](/images/2018/08/123.png){: width="50%" height="50%"}

결국 p가 margin이었다. 즉$$\theta^{T}x\geq 1$$ 이라는 조건은$$(\theta \times margin) \geq 1$$  와 같이 해석이 된다.

&nbsp;

평면 방정식에 대해 잘 모르시는 분들을 위해 평면 방정식에 관해 설명한 간단한 포스팅 하나를 올려두었다.

https://SWnomad.com/%ED%8F%89%EB%A9%B4-%EB%B0%A9%EC%A0%95%EC%8B%9D/

&nbsp;

정리를 하면 다음과 같다.

우리는 $$J(\theta)= C[y\cdot cost_{1}(\theta^{T}x)+(1-y)\cdot cost_{0}(\theta^{T}x)]+\frac{1}{2}\sum_{j=1}^{n}\theta_{j}^{2}$$가 최소가 되도록 $$\theta$$를 최적화 해야한다.

위 식에서 $$\sum_{j=1}^{n}\theta_{j}^{2}$$ 가 작아져야 하므로$$\theta^{T}x=\left | \theta \right |p\geq 1$$ 의 조건을 만족시키는 과정에서 $$\theta$$는 최소화가 되고  p, 즉 마진이 커지게 된다.

**<span style="color: #ff0000; font-size: 14pt;">즉, $$\theta^{T}x=\left | \theta \right |p\geq 1$$의 조건을 만족하면서 $$J(\theta)= C[y\cdot cost_{1}(\theta^{T}x)+(1-y)\cdot cost_{0}(\theta^{T}x)]+\frac{1}{2}\sum_{j=1}^{n}\theta_{j}^{2}$$를 최소화 시키는 작업은 margin이 최대화 된 decision boundary를 구하는 일인 것이다.</span>**

&nbsp;

위의 방정식 문제는 Lagrange Multiplier라는 방법을 써서 풀 수 있다. 하지만 위의 수식에 대해 적용하는 것은 그리 간단한 작업이 아니다. 그리고 우리가 그런 대수적인 증명까지 해야 할 필요는 없다. R, matlab, octave, python 등의 프로그래밍 도구는 이미 전산수학 전문가들이 만들어놓은 훌륭한 라이브러리를 제공한다. 위에서 이해한 내용을 바탕으로 라이브러리를 사용하면 간결한 스크립트 몇 줄만으로 SVM 알고리즘을 구현할 수 있다.

한 가지 생각해볼 게 있다.

![image](/images/2018/08/no-name-99-300x281.png){: width="50%" height="50%"}

위의 그림과 같은 dataset의 경우 두 decision boundary 중 어느 것이 더 좋은 선택일까? data 중 1개가 outlier이다. 이런 일반적인 경향에서 벗어나는 data까지 모두 분류하는 것이 좋은 것인가? 1번은 모든 데이터를 완벽히 분류한다. 하지만 이러한 decision boundary는 training dataset은 모두 완벽히 분류할 지 몰라도 2번 decision boundary가 더 일반적인 분류 패턴을 보이기 때문에 처음 보는 data를 예측할 때에는 2번 boundary에 의한 분류가 더 정확도가 높을 것이라 생각된다. C가 크면 overfitting이 일어나 1번과 같은 decision boundary를 가지게 된다. 이런 경우 C를 낮추어 더 일반적인 decision boundary를 찾아야 한다.

&nbsp;

<span style="font-size: 18pt;"><strong>3. Kernels</strong></span>

* * *

지금까지의 예시에서는 모든 데이터들이 선형적으로 분류가 가능하여 decision boundary로 평면이 될 수 있었다. 하지만 항상 평면으로 모든 data를 분류할 수 있는것이 아니다. 다음과 같은 경우를 보자.

![image](/images/2018/08/no-name-96-300x274.png){: width="50%" height="50%"}

데이터의 분포가 위와 같을 경우는 non-linear한 decision boundary가 필요하다. SVM에서 non-linear한 classification 문제를 풀기 위해서는 Kernel이란 trick을 사용해야한다.

Kernel이란 것은 간단히 말해 원래 주어진 feature$$x$$ 를 어떠한 정해진 규칙에 따라 가공을 하여 새로운 feature $$f$$를 만들어 사용하는 것이다. 그럼 decision boundary의 방정식도 다음과 같이 바뀐다.

&nbsp;

$$\theta^{T}x \rightarrow \theta^{T}f$$                $$\\ y=1:\theta^{T}f\geq 0 \\ y=0:\theta^{T}f< 0$$

아무튼간에 kernel를 통해 feature 변환을 하면 non-linear classifier SVM을 만들 수 있다.

새로운 feature$$f$$ 를 구하는 방법을 알아보자.

총 m개의 training dataset 중 i번째 training data $$x^{(i)}$$에 대해 변환을 하여보자.

$$\\ f^{(i)}_{1}=similarity(x^{(i)},x^{(1)})=exp({-\frac{\left \| x^{(i)}-x^{(1)} \right \|^{2}}{2\sigma^{2}}})\\ f^{(i)}_{2}=similarity(x^{(i)},x^{(2)})=exp({-\frac{\left \| x^{(i)}-x^{(2)} \right \|^{2}}{2\sigma^{2}}}) \\ \cdot \\ \cdot \\f^{(i)}_{m}=similarity(x^{(i)},x^{(m)})=exp({-\frac{\left \| x^{(i)}-x^{(m)} \right \|^{2}}{2\sigma^{2}}})$$ 

&nbsp;

$$x^{(i)}=(x^{(i)}_{1},x^{(i)}_{2},\cdot\cdot\cdot,x^{(i)}_{n})\quad \rightarrow \quad f^{(i)}=(f^{(i)}_{1},f^{(i)}_{2},\cdot\cdot\cdot,f^{(i)}_{m})$$ 

자신을 포함한 다른 모든 training example과의 similarity를 계산하여 하나의 feature로 만든다.

주의해야 할 것은 feature의 갯수가 n에서 m으로 바뀌었다. 따라서 $$\theta$$의 dimension도 바뀐다.

$$x^{(i)}\in \mathbb{R}^{n+1}\quad \rightarrow \quad f^{(i)} \in \mathbb{R}^{m+1}$$ 

We wanted$$\left | \theta^{T}x^{(i)}\right |=\left |\theta_{0}+\theta_{1}x^{(i)}_{1}+\theta_{2}x^{(i)}_{2}+\cdot \cdot \cdot+\theta_{n}x^{(i)}_{n} \right | \geq 1$$ 

But now we want $$\left|\theta^{T}f^{(i)}\right |=\left |\theta_{0}+\theta_{1}f^{(i)}_{1}+\theta_{2}f^{(i)}_{2}+\cdot \cdot \cdot+\theta_{m}f^{(i)}_{m} \right | \geq 1$$

Cost function도 다음과 같이 된다.

$$J(\theta)=\sum_{i=1}^{m}y^{(i)}cost_{1}(\theta^{T}f^{(i)})+(1-y^{(i)})cost_{0}(\theta^{T}f^{(i)})+\frac{1}{2}\sum_{j=1}^{m}\theta_{j}^{2}$$ 

최적화 과정에서 기존의 training example 대신 kernel로 변환 된 새로운 dataset을 사용하면 된다.

참고로 $$f_{i}$$를 계산할 때 사용한 함수는 Gaussian function이다. 따라서 위의 Kernel을 Gaussian Kernel이라고 한다. Gaussian Kernel 이외에도 여러가지 다양한 Kernel 함수들이 있다.

$$\sigma^{2}$$ 은 Gaussian Kernel의 중요한 parameter이다.

higher $$\sigma^{2}$$ : kernel함수 혹은 $$f$$가 smooth하게 변하며 high bias를 유도한다.

low  $$\sigma^{2}$$ :  kernel함수 혹은 $$f$$가 rapid하게 변하며 high variance를 유도한다.

&nbsp;

아무튼 이제 새로운 feature로 분류를 하면 된다. $$\theta^{T}f\geq 0$$이면 positive class,$$\theta^{T}f< 0$$ 이면 negative class이다.

3개의 feature로 부터 얻은 parameter가$$\theta_{0}=-0.5, \theta_{1}=1, \theta_{2}=1, \theta_{3}=0$$ 이라고 해보자. 그리고 새로운 test data가 있다. 이 data는 $$x^{(1)}$$에 근접해있고 다른 데이터에서는 멀다고 해보자. 그렇다면 이 test data는 $$f_{1}\approx 1, f_{2}\approx0,f_{3}\approx0$$이 될 것이다. 그렇다면 $$h_{\theta}(x)=\theta_{0}+\theta_{1}f_{1}+\theta_{2}f_{2}+\theta_{3}f_{3}\approx -0.5 +1 +0+0=0.5> 0$$ 따라서 이 data는 class1으로 분류된다. 이처럼 $$x^{(2)},x^{(3)}$$에 근접해 있는 test data에서도 예측을 하였는데 2 번째 data는 positive class, 마지막 data는 negative class로 분류되었다고 해보자. 이런 경우 $$x^{(1)},x^{(2)}$$근처는 positive class 영역, $$x^{(3)}$$근처는 negative class 영역인 것처럼 보인다. 따라서 decision boundary는 다음과 비슷하게 그려질 것이다.

![image](/images/2018/08/no-name-100.png){: width="50%" height="50%"}

&nbsp;

&nbsp;

&nbsp;

<span style="font-size: 18pt;"><strong>#Tips for using SVM</strong></span>

* * *

  * SVM과 관련한 훌륭한 library가 많기 때문에 이를 사용하면 된다.

&nbsp;

  * parameter C, $$\sigma^{2}$$과 Kernel function의 종류를 정해야 한다. parameter C는 cross validation set을 사용하여 결정하면 된다.

e.g. No kernel = linear kernel : feature의 갯수 n이 크고, training dataset의 갯수 m이 작은 경우 굳이 kernel을 사용할 필요가 없다. data의 수가 적은 경우 kernel을 통해 non-linear classifier를 만들면 high variance(overfitting)문제가 일어날 가능성이 있기 때문이다. kernel을 적용하지 않을 경우, 즉 no kernel인 경우를 linear kernel라고도 부르고 이는 단지 linear classifier일 뿐이다.

&nbsp;

  * Gaussian Kernel을 사용하기 전에 feature scailing은 필수이다.

e.g. feature들의 scale의 차이가 큰 경우 feature scailing이나 feature normalization을 해주지 않으면 linear regression이나 logistic regression의 경우처럼 좋은 훈련 parameter를 얻지 못할 수 있다.

&nbsp;

&nbsp;

  * Gaussian Kernel을 사용할 때 high bias와 high variance의 trade-off 사이에서 적절한 값의 $$\sigma^{2}$$을 선택해야한다.

&nbsp;

  * multi-class classification의 경우에도 이미 만들어져있는 훌륭한 SVM library가 많다. 이 package를 사용하거나 one-vs-all method를 사용해도 된다.

&nbsp;

  * Gaussian Kernel을 언제 쓸까?

&#8211; n large, m small : 이 경우는 안그래도 high variance로 갈 가능성이 큰 데 non-linear kernel까지 사용해서 overfitting을 부추길 필요 없으므로 linear classification을 하는 것이 좋다.

&#8211; n small, m intermediate : 이 경우는 feature 수는 작고 training example 갯수도 SVM 알고리즘을 계산하기에 부담이 없을 경우 non linear kernel를 쓰면 좋은 모델을 만들 수 있을 가능성이 크다.

&#8211; n small, m large : feature 수는 작지만 training example의 갯수가 10만, 혹은 100만 정도로 크다면 계산의 시간 복잡도가 너무 커서 차라리 새로운 feature를 추가하거나 polynomial feature를 새로 만들어서 linear kernel SVM이나 logistic regression으로 푸는 것이 오히려 효율적이다.

&nbsp;

&nbsp;

&nbsp;