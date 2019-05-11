---
title: 순차 논리회로와 SR래치
date: 2018-12-08T16:51:30+09:00
author: SWnomad
layout: post
categories: 컴퓨터구조
image: /images/2018/12/no-name-3.jpg
tags:
  - flipflop
  - latch
  - SR
  - 논리
  - 래치
  - 순차
  - 조합
  - 플립플롭
  - 회로
---
# 1. 조합 논리회로

<hr class="wp-block-separator" />
오직 &#8216;입력&#8217; 만이 &#8216;출력&#8217;을 결정. 같은 입력이 들어오는 이상 같은 출력. 따라서 기억소자(메모리)로 사용할 수 없다. &nbsp; &nbsp; 

# 2. 순차 논리회로

<hr class="wp-block-separator" />
&#8216;입력&#8217; + &#8216;회로의 상태&#8217;가 &#8216;출력&#8217;을 결정. 같은 입력이 들어오더라도 회로의 현재 bit 상태(기억)에 따라 다른 &#8216;출력&#8217; 순차 논리회로는 메모리 소자이며, 플립플롭(flip-flop)을 이용한다. 플립플롭에는 SR 플립플롭, JK 플립플롭, D 플립플롭 등 여러가지가 있다. 플립플롭에 대해 알아보기 전에 오늘은 SR래치에 대해 알아본다. &nbsp; &nbsp; 

# 3. SR Latch

<hr class="wp-block-separator" />
비트(bit)의 정보를 저장할 수 있는 소자이다. SR Latch 1개는 1bit 정보를 저장할 수 있다. 따라서 메모리 소자로 사용 가능하다. 

<div class="wp-block-image">
  <figure class="aligncenter is-resized"><img class="wp-image-1484 aligncenter" src="/images/2018/12/1.jpg" alt="" width="240" height="191" /></figure>
</div> 래치의 출력 Q를 Set 혹은 Reset 한다는 의미에서 SR Latch라고 이름 지어졌다. Q&#8217;는 Q와 다른 값을 가지는 값이다. 회로를 보면 연산의 결과, 즉 출력이 다시 입력으로 피드백되어 입력값과 NOR 연산을 진행하여 다시 출력으로 내보낸다. 이것을 고려하여 SR Latch의 입력에 따른 결과를 계산해보자. 초기 상태를 S=0, R=0, Q=0, Q&#8217;=1 이라고 해보자. NOR게이트의 연산을 유념하여 계산을 해보면 안정적으로 이 값이 계속 유지된다. 입력을 S=1, R=0으로 바꿔본다.  S=1로 변하면 현재 Q의 출력 0과의 NOR연산으로 Q&#8217;=0이 된다. 또한 현재 Q&#8217;의 출력 1과 R=0의 NOR연산의 결과로 Q=0이 된다. Q와 Q&#8217;의 값이 같기 때문에 아직 불안정(unstable)한 상태이다. 다시 S=1, Q=0과의 NOR 연산으로 Q&#8217;=0, R=0과 Q&#8217;=0과의 연산으로 Q=1로 된다. 이 상태는 Q와 Q&#8217;의 상태가 서로 반대이며 안정(stable)하다. S=1, R=0은 Latch를 set 하는 것으로, Q=1의 결과를 만들어낸다. 반대로 S=0, R=1 입력은 Latch를 Reset하는 것으로, Q=0으로 clear한다. S와 R 모두 0인 입력은 단순히 회로의 현재 상태를 읽는다(Read). 입력에 따른 출력의 경과를 테이블로 정리하면 다음과 같다. 

<table style="border-collapse: collapse; width: 37.0909%; height: 192px;" border="1">
  <tr style="height: 24px;">
    <td style="width: 6.78788%; height: 24px; text-align: center;">
      <strong>S</strong>
    </td>
    
    <td style="width: 6.78786%; height: 24px; text-align: center;">
      <strong>R</strong>
    </td>
    
    <td style="width: 6.4242%; height: 24px; text-align: center;">
      <strong>Q</strong>
    </td>
    
    <td style="width: 6.54545%; height: 24px; text-align: center;">
      <strong>Q&#8217;</strong>
    </td>
    
    <td style="width: 10.5454%; height: 24px; text-align: center;">
      <strong>stability</strong>
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 6.78788%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.78786%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.4242%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.54545%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 10.5454%; height: 24px; text-align: center;">
      stable
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 6.78788%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 6.78786%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.4242%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.54545%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 10.5454%; height: 24px; text-align: center;">
      unstable
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 6.78788%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 6.78786%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.4242%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 6.54545%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 10.5454%; height: 24px; text-align: center;">
      stable
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 6.78788%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.78786%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.4242%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 6.54545%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 10.5454%; height: 24px; text-align: center;">
      stable
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 6.78788%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.78786%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 6.4242%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.54545%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 10.5454%; height: 24px; text-align: center;">
      unstable
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 6.78788%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.78786%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 6.4242%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.54545%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 10.5454%; height: 24px; text-align: center;">
      stable
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 6.78788%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.78786%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.4242%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 6.54545%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 10.5454%; height: 24px; text-align: center;">
      stable
    </td>
  </tr>
</table> SR Latch는 입력이 들어오면 현재 입력에 맞는 안정한 상태로 수렴할 때 까지 몇 번의 동작 이후 안정한 상태를 유지하며 현재의 정보를 기억한다. 즉, 이것이 메모리이다. 참고로 S=1, R=1은 Q와 Q&#8217;이 동일한 값을 가지게 하며 이를 부정 상태라고 한다. 이러한 입력을 SR 래치에서는 사용하지 않는다. SR래치의 특성표는 다음과 같다. 

<table style="border-collapse: collapse; width: 22.5455%; height: 120px;" border="1">
  <tr style="height: 24px;">
    <td style="width: 2.73973%; height: 24px; text-align: center;">
      <strong>S</strong>
    </td>
    
    <td style="width: 2.73973%; height: 24px; text-align: center;">
      <strong>R</strong>
    </td>
    
    <td style="width: 2.73973%; height: 24px; text-align: center;">
      <strong>Q(t+1)</strong>
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 2.73973%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 2.73973%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 2.73973%; height: 24px; text-align: center;">
      Q(t)
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 2.73973%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 2.73973%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 2.73973%; height: 24px; text-align: center;">
      1
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 2.73973%; height: 24px; text-align: center;">
    </td>
    
    <td style="width: 2.73973%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 2.73973%; height: 24px; text-align: center;">
    </td>
  </tr>
  
  <tr style="height: 24px;">
    <td style="width: 2.73973%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 2.73973%; height: 24px; text-align: center;">
      1
    </td>
    
    <td style="width: 2.73973%; height: 24px; text-align: center;">
      부정
    </td>
  </tr>
</table> Latch를 Enable/Disable 시킬 수 있는 pin을 하나 더 달아서 E=1이면 래치가 동작, E=0이면 래치가 비활성화 되어 어떠한 입력이 들어와도 현재의 상태를 유지할 수 있도록 할 수 있다. 

<div class="wp-block-image">
  <figure class="aligncenter"><img class="wp-image-1488 aligncenter" src="/images/2018/12/2.jpg" alt="" /><figcaption></figcaption></figure>
</div> 위 그림에서 E=0이면 항상 입력으로 S=0, R=0이 들어가는 것과 같은 효과를 내고, 이는 위에서 설명한 바에 의하면 상태를 변화시키지 않고 유지시킨다. 반대로 E=1이면 기존의 SR래치와 같다. 플립플롭을 배울 때에는 Enable pin에 clock source를 달아서 주기적으로 동작하는 소자를 만들 수 있다.