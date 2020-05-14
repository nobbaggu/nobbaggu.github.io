---
title: (머신러닝) Decision Tree
date: 2018-10-28T15:11:30+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - Decision Tree
  - Gini
  - Gini impurity
  - impurity
  - 의사결정 트리
  - 지니 계수
---
의사결정 트리(Decision Tree)는 직관적이면서 이해하기 쉬운 머신러닝 알고리즘이다. 의사결정 트리 하나만 가지고 예측 모델링을 하기에는 성능이 뛰어난 편이 아니지만 Bagging, Boosting 등의 Ensemble 알고리즘, Random Forest로의 알고리즘 확장을 통해 우수한 성능을 낸다. 특히 요즘 머신러닝 경진대회의 가장 핫한 알고리즘인 XGBoost 알고리즘을 사용하기 위해서도 의사결정 트리에 대한 이해는 필수적이다.

&nbsp;

&nbsp;

# 1. 의사결정 트리 알고리즘

* * *

<table style="border-collapse: collapse; width: 100%; height: 168px;" border="1">
  <tr style="height: 24px;">
    <td style="width: 22.3333%; text-align: center; height: 24px;">
      Like Programming?
    </td>
    
    <td style="width: 21.9697%; text-align: center; height: 24px;">
      Like Math?
    </td>
    
    <td style="width: 19.6667%; text-align: center; height: 24px;">
      Male?
    </td>
    
    <td style="width: 36.0303%; text-align: center; height: 24px;">
      <em><strong>Like Machine Learning?</strong></em>
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 22.3333%; text-align: center; height: 24px;">
      YES
    </td>
    
    <td style="width: 21.9697%; text-align: center; height: 24px;">
      YES
    </td>
    
    <td style="width: 19.6667%; text-align: center; height: 24px;">
      NO
    </td>
    
    <td style="width: 36.0303%; text-align: center; height: 24px;">
      <em><strong>YES</strong></em>
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 22.3333%; text-align: center; height: 24px;">
      NO
    </td>
    
    <td style="width: 21.9697%; text-align: center; height: 24px;">
      YES
    </td>
    
    <td style="width: 19.6667%; text-align: center; height: 24px;">
      NO
    </td>
    
    <td style="width: 36.0303%; text-align: center; height: 24px;">
      <em><strong>NO</strong></em>
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 22.3333%; text-align: center; height: 24px;">
      YES
    </td>
    
    <td style="width: 21.9697%; text-align: center; height: 24px;">
      NO
    </td>
    
    <td style="width: 19.6667%; text-align: center; height: 24px;">
      YES
    </td>
    
    <td style="width: 36.0303%; text-align: center; height: 24px;">
      <em><strong>YES</strong></em>
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 22.3333%; text-align: center; height: 24px;">
      YES
    </td>
    
    <td style="width: 21.9697%; text-align: center; height: 24px;">
      YES
    </td>
    
    <td style="width: 19.6667%; text-align: center; height: 24px;">
      YES
    </td>
    
    <td style="width: 36.0303%; text-align: center; height: 24px;">
      <em><strong>YES</strong></em>
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 22.3333%; text-align: center; height: 24px;">
      NO
    </td>
    
    <td style="width: 21.9697%; text-align: center; height: 24px;">
      NO
    </td>
    
    <td style="width: 19.6667%; text-align: center; height: 24px;">
      YES
    </td>
    
    <td style="width: 36.0303%; text-align: center; height: 24px;">
      <em><strong>NO</strong></em>
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 22.3333%; text-align: center; height: 24px;">
      NO
    </td>
    
    <td style="width: 21.9697%; text-align: center; height: 24px;">
      NO
    </td>
    
    <td style="width: 19.6667%; text-align: center; height: 24px;">
      NO
    </td>
    
    <td style="width: 36.0303%; text-align: center; height: 24px;">
      <em><strong>NO</strong></em>
    </td>
  </tr>
  
  <tr>
    <td style="width: 22.3333%; text-align: center;">
      &#8230;
    </td>
    
    <td style="width: 21.9697%; text-align: center;">
      &#8230;
    </td>
    
    <td style="width: 19.6667%; text-align: center;">
      &#8230;
    </td>
    
    <td style="width: 36.0303%; text-align: center;">
      <em><strong>&#8230;</strong></em>
    </td>
  </tr>
</table>

위의 표는 프로그래밍을 좋아하는지, 수학을 좋아하는지, 그리고 남자인지 여자인지가 나와있는 데이터인데, 이 사람들에게 머신러닝을 좋아하는지 설문조사를 한 결과라고 해보자. 위의 데이터를 가지고 학습시킨 간단한 흐름도 하나는 다음과 같다.

![image](https://nobbaggu.github.io/images/2018/10/no-name-4.jpg){: width="50%" height="50%"}

의사결정 트리는 위 그림처럼 질문 하나하나의 대답에 따라 경로가 결정되며 마지막 예측 까지 도달한다.

파란색으로 된 것은 질문이다. 초록색으로 된 것은 최종 예측 결과이다. 파란색 중 가장 위에 있는 것을 Root Node, 줄여서 <span style="text-decoration: underline;"><strong>Root</strong></span>라 부른다. 그리고 다른 파란색들을 Internal Node, 줄여서 그냥 <span style="text-decoration: underline;"><strong>Node</strong></span>라 부른다. 그리고 초록색으로 된 것들을 Leaf Node, 줄여서 <span style="text-decoration: underline;"><strong>Leaf</strong></span>라 부른다.

의사결정 트리를 학습시킨다는 것은 위의 트리에서 어떤 질문부터 할지를 결정하는 것이다. 그리고 질문이 남아있어도 더 이상 묻지 않는 것이 더 예측 정확도가 높다고 판단되면 결과를 내는 것이다.

여기에 대해서 지금부터 자세히 설명할 것이다.

&nbsp;

&nbsp;

# 2. 의사결정 트리 학습방법

* * *

3가지의 질문들 중 어떤 질문을 먼저 하느냐에 따라 학습 결과는 달라지며, 따라서 모델의 예측 성능도 달라진다.

결론부터 말하자면, 분류의 impurity를 나타내는 Gini계수 라는 것을 계산해 Gini계수가 가장 낮은 질문을 선택한다.

![image](https://nobbaggu.github.io/images/2018/10/1.jpg){: width="50%" height="50%"}

Programming 을 좋아하는 사람들 중 머신러닝을 좋아하거나 그렇지 않은 사람들, 프로그래밍을 좋아하지 않는 사람들 중 머신러닝을 좋아하는 사람과 그렇지 않은 사람의 수는 위와 같이 나타난다.

위의 질문 하나만으로는 완벽히 분류하지 못한다. 만약 위의 질문 하나만으로 ML을 좋아하는 사람과 그렇지 않은 사람이 완벽히 분류된다면 그것을 pure(순수한) 이라 표현한다. 따라서 위 하나의 질문만으로 구성된 의사결정 트리는 impurity(불순도)를 가지고 있으며 이 impurity가 어느정도인지 나타내는 Gini 계수를 계산할 수 있다.

$$\\Gini\quad impurity = 1-(probability\quad of\quad YES)^{2}-(probability\quad of\quad NO)^{2}$$ 

Gini impurity는 위의 공식에 따라 계산한다. 그럼 각각의 leaf에 대해 Gini impurity를 계산해보겠다.

$$Gini\quad of\quad first\quad leaf=1-(\frac{130}{130+47})^{2}-(\frac{47}{130+47})^{2}=0.39$$ 

$$Gini\quad of\quad second\quad leaf=1-(\frac{21}{21+94})^{2}-(\frac{94}{21+94})^{2}=0.30$$ 

첫 번째 leaf의 응답자 수는 130+47 = 177이다. 두 번째 leaf의 응답자 수는 21+94 = 115이다. 따라서 총 응답자의 수는 292이다. 최종 Gini impurity는 다음과 같이 두 leaf의 Gini impurity를 평균내어 계산한다.

$$Gini\quad impurity = 0.39(\frac{177}{292})+0.30(\frac{115}{292})=0.35$$ 

이런 식으로 각각의 질문에 대한 Gini impurity를 구해 Gini impurity가 가장 낮은 질문을 먼저 넣어준다.

이제 또 남은 응답자 수에 대하여 같은 과정을 거쳐 다음 질문을 선택할 수 있다.

![image](https://nobbaggu.github.io/images/2018/10/1-1.jpg){: width="50%" height="50%"}

&nbsp;

이런 과정을 거쳐 최종적으로 다음과 같은 의사결정 트리가 나왔다.

![image](https://nobbaggu.github.io/images/2018/10/no-name-4.jpg){: width="50%" height="50%"}

그런데 왼쪽 줄기에서 Like Math?의 응답으로 NO가 나온 경우 거기서 더 이상 Male/Female? 을 묻지 않고 Leaf로 줄기가 끝이 난다. 이러한 경우는 &#8216;Like Math&#8217;만  Gini impurity보다 &#8216;Male/Female&#8217;의 Gini impurity가 더 높기 때문이다. 더 이상 질문을 만들어봐야 오히려 예측 성능이 떨어지는 모델이 나온다는 말이다.

![image](https://nobbaggu.github.io/images/2018/10/1-2.jpg){: width="50%" height="50%"}

Male/Female로 나누기 전의 Gini impurity는 다음과 같다.

$$1-(\frac{20}{20+66})^{2}-(\frac{66}{20+66})^{2}=0.36$$ 

그런데 오히려 Male/Female로 나누고 난 이후의 Gini impurity가 더 높다. 이런 경우에는 더 이상 나누지 않고 Leaf로 둔다.

&nbsp;

&nbsp;

# 3. 숫자 데이터가 있는 경우의 Gini impurity

* * *

위의 데이터에 다음의 나이를 포함하는 데이터가 추가되었다. 이런 경우 역시 나이별로 분기가 되어야하는데 이 때도 역시 Gini impurity가 필요하다.

<table style="border-collapse: collapse; width: 23.2727%;" border="1">
  <tr>
    <td style="width: 10.3636%; text-align: center;">
      Age
    </td>
    
    <td style="width: 12.9091%; text-align: center;">
      Lime ML?
    </td>
  </tr>
  
  <tr>
    <td style="width: 10.3636%; text-align: center;">
      15
    </td>
    
    <td style="width: 12.9091%; text-align: center;">
      N
    </td>
  </tr>
  
  <tr>
    <td style="width: 10.3636%; text-align: center;">
      22
    </td>
    
    <td style="width: 12.9091%; text-align: center;">
      Y
    </td>
  </tr>
  
  <tr>
    <td style="width: 10.3636%; text-align: center;">
      33
    </td>
    
    <td style="width: 12.9091%; text-align: center;">
      Y
    </td>
  </tr>
  
  <tr>
    <td style="width: 10.3636%; text-align: center;">
      41
    </td>
    
    <td style="width: 12.9091%; text-align: center;">
      N
    </td>
  </tr>
</table>

&nbsp;

이런 경우 나이별로 정렬을 한 이후 각 구간 사이의 평균을 낸다. 그리고 하나하나 적용해가며 어떤 것을 기준으로 사용했을 때의 Gini impurity를 구한 이후 가장 낮은 기준을 사용하면 된다.

![image](https://nobbaggu.github.io/images/2018/10/no-name-5.jpg){: width="50%" height="50%"}