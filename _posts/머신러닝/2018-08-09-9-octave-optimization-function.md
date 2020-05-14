---
title: (머신러닝) 9. Octave Optimization Function
date: 2018-08-09T23:32:39+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - cost function
  - fminunc
  - gradient descent
  - machine learning
  - 경사하강법
  - 머신러닝
  - 선형회귀
---
gradient descent의 식을 직접 작성하지 않아도 numerical computing분야의 전문가들이 만들어놓은 최적 parameter를 찾는 함수들이 존재한다. 오늘은 octave에서 지원하는 함수 중 하나를 소개할 것인데, function이 최소값을 가지도록 하는 parameter를 자동으로 계산해준다. 단지 라이브러리에서 가져다가 사용하면 충분하다.

어떠한 cost function이 다음과 같이 주어진다고 해보자.

$$J(\theta ) = ({ { \theta }_{ 0 }-1 })^{ 2 }+({ { \theta }_{ 1 }-4 })^{ 2 }+({ { \theta }_{ 2 }-6 })^{ 2 }+({ { \theta }_{ 3 }-3 })^{ 2 }$$ 

&nbsp;

위와같은 함수는 4개의 parameter가 (1,4,6,3)의 값을 가질 때 최소값을 가질것이다. 그럼 이를 octave에서 지원하는 함수를 이용하기 전에 위의 함수를 정의해야겠다.

![image](https://nobbaggu.github.io/images/2018/08/no-name-13.png){: width="50%" height="50%"}

&nbsp;

이 함수의 output은 cost function의 값과 이 함수의 gradient(미분)이다.

이제 우리가 위에서 작성한 함수의 최소값(혹은 최소값에 근접한 값)과 그 값에서의 parameter를 구하기 위해서 octave에서 지원하는 fminunc() 라는 함수를 사용할 것이다. 더 자세한 내용이 궁금하다면 octave command window에서 help fminunc 라는 명령어를 입력해보자.

&nbsp;

![image](https://nobbaggu.github.io/images/2018/08/no-name-15.png){: width="50%" height="50%"}

&nbsp;

위에서 options라는 변수는 우리가 fminunc라는 함수를 사용할 때의 option을 미리 setting하여 저장해둔 것이다. &#8216;GradObj&#8217;, &#8216;on&#8217; 은 함수의 gradient를 사용해 gradient descent를 쓰기 위한 설정이다. 그리고 initialTheta에 theta의 초기값으로 사용할 값을 저장해놓았다. 그리고 fminunc라는 함수를 사용해서 J_Func함수의 값을 최소로 만드는 theta를 찾는다.

fminunc의 첫 번째 인자인 @(t)(J\_Func(t))는 J\_Func의 pointer로 J\_Func의 함수가 저장된 메모리공간을 참조한다. 해석을 하자면 @(t)는 t에 대하여인데 theta가 t로 치환된 것이다. 어떠한 함수를 최적화 할 때 그 함수의 variable중 어떤 값에 대하여 최적화를 하고싶은지에 따라 함수 괄호 안에서 그 variale을 t로 바꾸면 된다. 그리고 두 번째 인자는 J\_Func 함수의 theta의 초기값을 initial_theta로 가져다 쓰겠다는 말이다.

optimize function의 첫 번째 반환값은 J\_Func함수가 최소가 되게하는 theta값이고, 두 번째는 이 theta값에서의 J\_Func의 값, 즉 최소값이다.

위의 fminunc함수가 반환한 값을 보아라. 우리가 예상한대로 theta = (1,4,6,3)이 된다. 그리고 이 때 J_Func의 값은 거의 0에 가까운 값을 가진다.