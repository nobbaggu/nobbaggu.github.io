---
title: SR 플립플롭
date: 2018-12-08T17:23:36+09:00
author: SWnomad
layout: post
categories: 컴퓨터구조
image: /images/2018/12/no-name-3.jpg
tags:
  - clock
  - edge
  - flipflop
  - level
  - SR
  - trigger
  - 레벨
  - 엣지
  - 클럭
  - 트리거
  - 플립플롭
---
# 1. 클럭(Clock)

* * *

CPU, 메모리, 주변장치 등 컴퓨터의 모든 소자는 클럭에 맞추어 동작한다. 클럭의 특정 주기마다 모든 소자들이 동기화되어 데이터를 교환하고 명령을 수행하는 등의 일을 한다. 따라서 클럭이 없으면 컴퓨터가 동작할 수 없다. 클럭은 주기적으로 상승, 하강 동작을 반복하면서 1과 0의 값을 가진다.

<a href="https://SWnomad.com/sr-%ed%94%8c%eb%a6%bd%ed%94%8c%eb%a1%ad/%ec%a0%9c%eb%aa%a9-%ec%97%86%ec%9d%8c-173/" rel="attachment wp-att-1497"><img class="aligncenter size-full wp-image-1497" src="/images/2018/12/no-name-18.jpg" alt="" width="425" height="154" srcset="/images/2018/12/no-name-18.jpg 425w, /images/2018/12/no-name-18-300x109.jpg 300w" sizes="(max-width: 425px) 100vw, 425px" /></a>

가령 클럭이 1Mhz라는 것은 클럭이 1초에 100만 번 발생한다는 것이다. 따라서 컴퓨터는 1초에 100만 번의 명령어를 수행한다. 따라서 클럭의 주파수가 높다는 것은 훨씬 자주, 빨리 명령을 처리한다는 것이고 일반적으로 컴퓨터의 속도가 높다는 것이다.

&nbsp;

&nbsp;

# 2. SR 플립플롭

* * *

플립플롭은 래치에 clock을 추가한 것이다. 즉, 클럭이 들어올 때에만(1일 때에만) 래치가 동작하도록 한 것이다.

<a href="https://SWnomad.com/sr-%ed%94%8c%eb%a6%bd%ed%94%8c%eb%a1%ad/%ec%a0%9c%eb%aa%a9-%ec%97%86%ec%9d%8c-174/" rel="attachment wp-att-1498"><img class="aligncenter  wp-image-1498" src="/images/2018/12/no-name-19.jpg" alt="" width="363" height="197" srcset="/images/2018/12/no-name-19.jpg 600w, /images/2018/12/no-name-19-300x163.jpg 300w" sizes="(max-width: 363px) 100vw, 363px" /></a>

위 그림을 해석하면 CLK=1이 되었을 때에만 S,R의 입력이 그대로 들어갈 수 있게 SR래치가 동작이 된다. 따라서 위 SR래치는 클럭의 주파수에 맞춰서 주기적으로 일을 한다.

<a href="https://SWnomad.com/sr-%ed%94%8c%eb%a6%bd%ed%94%8c%eb%a1%ad/2-17/" rel="attachment wp-att-1494"><img class="aligncenter  wp-image-1494" src="/images/2018/12/2-1.jpg" alt="" width="545" height="201" srcset="/images/2018/12/2-1.jpg 787w, /images/2018/12/2-1-300x111.jpg 300w, /images/2018/12/2-1-768x283.jpg 768w" sizes="(max-width: 545px) 100vw, 545px" /></a>

위 그림에서 input이 변해도 clock이 상승하여 1이 되기 전 까지는 입력이 감지되지 못한다. 왜냐하면 CLK=0과 AND연산을 하면 (S,R) = (0,0)이 되므로 SR래치의 동작은 &#8216;상태 보존&#8217;이 되기 때문이다. 그러다가 클럭이 발생하여 1이 되면 SR래치의 기능이 수행된다.

이처럼 특정 레벨에서만 소자가 동작하도록 하는 방식을 &#8216;레벨 트리거(level trigger)&#8217; 방식이라고 한다. 그런데 만약 클럭의 주기가 너무 짧아 클럭이 1인 상태에서 input이 2번 바뀌면 output도 두번 바뀔 수가 있다. 이러한 문제 때문에 소자들 간의 동기화가 틀어져 문제가 발생할 수 있다.

위에서 언급한 레벨 트리거 방식의 문제점을 보완하기 위해 실제에서는 &#8216;엣지 트리거(edge trigger)&#8217; 방식을 사용한다. 엣지 트리거 방식에는 클럭이 &#8216;상승&#8217;할 때에만 동작하도록 하는 positive edge trigger 방식과 그 반대인 negative edge trigger 방식을 쓸 수 있다.

위 그림의 SR 플립플롭을 조금 변경하여 edge탐지 회로를 추가하면 SR플립플롭이 엣지 트리거 방식으로 동작한다.

&nbsp;

&nbsp;

# 3. 정리

* * *

<a href="https://SWnomad.com/sr-%ed%94%8c%eb%a6%bd%ed%94%8c%eb%a1%ad/%ec%a0%9c%eb%aa%a9-%ec%97%86%ec%9d%8c-175/" rel="attachment wp-att-1499"><img class="aligncenter size-full wp-image-1499" src="/images/2018/12/no-name-20.jpg" alt="" width="1146" height="264" srcset="/images/2018/12/no-name-20.jpg 1146w, /images/2018/12/no-name-20-300x69.jpg 300w, /images/2018/12/no-name-20-768x177.jpg 768w, /images/2018/12/no-name-20-1024x236.jpg 1024w" sizes="(max-width: 1146px) 100vw, 1146px" /></a>