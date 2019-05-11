---
title: 디코더와 인코더
date: 2018-12-05T17:56:08+09:00
author: SWnomad
layout: post
categories: 컴퓨터구조
image: /images/2018/12/no-name-3.jpg
tags:
  - coder
  - decoder
  - encoder
  - 디코더
  - 인코더
  - 코더
  - 코덱
---
우리가 잘 아는 코덱은 코더+디코더(Coder + Decoder)의 합성어이다. 영상의 아날로그 신호를 이진 비트로 압축해 저장할 때 인코더(코더)를 사용하고, 이진 코드로 압축된 정보를 영상 등의 아날로그 신호로 풀 때는 디코더를 사용한다.

&nbsp;

# 1. 디코더(Decoder)

* * *

인코더와 반대되는 개념이다.

**2진수를 입력받아 출력신호 중 1개만 1로 set 하고 나머지는 0으로 clear 한다.**

<a href="https://SWnomad.com/%eb%94%94%ec%bd%94%eb%8d%94%ec%99%80-%ec%9d%b8%ec%bd%94%eb%8d%94/%ec%a0%9c%eb%aa%a9-%ec%97%86%ec%9d%8c-171/" rel="attachment wp-att-1457"><img class="aligncenter size-full wp-image-1457" src="/images/2018/12/no-name-16.jpg" alt="" width="1045" height="274" srcset="/images/2018/12/no-name-16.jpg 1045w, /images/2018/12/no-name-16-300x79.jpg 300w, /images/2018/12/no-name-16-768x201.jpg 768w, /images/2018/12/no-name-16-1024x268.jpg 1024w" sizes="(max-width: 1045px) 100vw, 1045px" /></a>

2진코드로 저장된 동영상의 압축 정보를 풀어 아날로그 영상 신호로 내보낸다고 생각해보면 이해하기 수월하다.

&nbsp;

&nbsp;

&nbsp;

# 2. 인코더(Encoder)

* * *

Coder라고도 한다.

**(1개만 1로 set되고 나머지는 0으로 clear 되어있는) 2진 비트열을 입력받아 압축된 2진수를 출력한다.**

<a href="https://SWnomad.com/%eb%94%94%ec%bd%94%eb%8d%94%ec%99%80-%ec%9d%b8%ec%bd%94%eb%8d%94/%ec%a0%9c%eb%aa%a9-%ec%97%86%ec%9d%8c-172/" rel="attachment wp-att-1458"><img class="aligncenter size-full wp-image-1458" src="/images/2018/12/no-name-17.jpg" alt="" width="593" height="150" srcset="/images/2018/12/no-name-17.jpg 593w, /images/2018/12/no-name-17-300x76.jpg 300w" sizes="(max-width: 593px) 100vw, 593px" /></a>