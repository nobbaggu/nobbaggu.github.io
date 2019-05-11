---
title: 논리식 / 논리도 / 진리표
date: 2018-12-05T15:26:15+09:00
author: SWnomad
layout: post
categories: 컴퓨터구조
image: /images/2018/12/no-name-3.jpg
tags:
  - logic diagram
  - logic expression
  - truth table
  - 논리도
  - 논리식
  - 부울식
  - 진리표
---
논리 회로를 표현하는 방식에는 대표적인 3가지가 있다. A+B&#8217;C를 각각의 방법을 사용해서 표현해보자.

# 1. 논리식

* * *

부울 대수를 사용해서 논리를 부울식으로 표현하는 방식

A+B&#8217;C

&nbsp;

&nbsp;

# 2. 논리도

* * *

논리 게이트를 사용해서 회로로 표현하는 방식

<a href="https://SWnomad.com/%eb%85%bc%eb%a6%ac%ec%8b%9d-%eb%85%bc%eb%a6%ac%eb%8f%84-%ec%a7%84%eb%a6%ac%ed%91%9c/%ec%a0%9c%eb%aa%a9-%ec%97%86%ec%9d%8c-161/" rel="attachment wp-att-1441"><img class="aligncenter size-full wp-image-1441" src="/images/2018/12/no-name-6.jpg" alt="" width="407" height="107" srcset="/images/2018/12/no-name-6.jpg 407w, /images/2018/12/no-name-6-300x79.jpg 300w" sizes="(max-width: 407px) 100vw, 407px" /></a>

&nbsp;

논리식이 주어졌을 때, 식을 차례대로 게이트를 사용해서 표현하면서 풀면 논리도로 바꿀 수 있다.

논리도가 주어졌을 때는 역순으로 하면 된다.

논리식과 논리도 사이에는 완벽히 1대1 대응관계가 성립한다.

&nbsp;

# 3. 진리표

* * *

모든 입력의 경우에 수에 대한 결과 테이블

<table style="width: 23.5152%; border-collapse: collapse; background-color: #ede8e8;" border="1">
  <tr>
    <td style="width: 7.18182%; text-align: center;">
      A
    </td>
    
    <td style="width: 7.30305%; text-align: center;">
      B
    </td>
    
    <td style="width: 7.06065%; text-align: center;">
      C
    </td>
    
    <td style="width: 4.40529%; text-align: center;">
      F=A+B&#8217;C
    </td>
  </tr>
  
  <tr>
    <td style="width: 7.18182%; text-align: center;">
    </td>
    
    <td style="width: 7.30305%; text-align: center;">
    </td>
    
    <td style="width: 7.06065%; text-align: center;">
    </td>
    
    <td style="width: 4.40529%; text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="width: 7.18182%; text-align: center;">
    </td>
    
    <td style="width: 7.30305%; text-align: center;">
    </td>
    
    <td style="width: 7.06065%; text-align: center;">
      1
    </td>
    
    <td style="width: 4.40529%; text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="width: 7.18182%; text-align: center;">
    </td>
    
    <td style="width: 7.30305%; text-align: center;">
      1
    </td>
    
    <td style="width: 7.06065%; text-align: center;">
    </td>
    
    <td style="width: 4.40529%; text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="width: 7.18182%; text-align: center;">
    </td>
    
    <td style="width: 7.30305%; text-align: center;">
      1
    </td>
    
    <td style="width: 7.06065%; text-align: center;">
      1
    </td>
    
    <td style="width: 4.40529%; text-align: center;">
    </td>
  </tr>
  
  <tr>
    <td style="width: 7.18182%; text-align: center;">
      1
    </td>
    
    <td style="width: 7.30305%; text-align: center;">
    </td>
    
    <td style="width: 7.06065%; text-align: center;">
    </td>
    
    <td style="width: 4.40529%; text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="width: 7.18182%; text-align: center;">
      1
    </td>
    
    <td style="width: 7.30305%; text-align: center;">
    </td>
    
    <td style="width: 7.06065%; text-align: center;">
      1
    </td>
    
    <td style="width: 4.40529%; text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="width: 7.18182%; text-align: center;">
      1
    </td>
    
    <td style="width: 7.30305%; text-align: center;">
      1
    </td>
    
    <td style="width: 7.06065%; text-align: center;">
    </td>
    
    <td style="width: 4.40529%; text-align: center;">
      1
    </td>
  </tr>
  
  <tr>
    <td style="width: 7.18182%; text-align: center;">
      1
    </td>
    
    <td style="width: 7.30305%; text-align: center;">
      1
    </td>
    
    <td style="width: 7.06065%; text-align: center;">
      1
    </td>
    
    <td style="width: 4.40529%; text-align: center;">
      1
    </td>
  </tr>
</table>

논리식, 논리도를 진리표로 바꾸는 방법은 단순히 모든 입력의 경우에 대해 출력을 구해서 적어주면 된다.

하지만 진리표가 주어졌을 때 가장 간결한 논리식이나 논리도로 바꾸는 방법은 단순하지만은 않다. 여기에는 몇 가지 통용되는 방법이 있는데, 이에 대해서는 다음 포스팅에서 자세히 다룬다.