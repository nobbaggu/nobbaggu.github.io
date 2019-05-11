---
title: 부울 대수 법칙
date: 2018-12-05T16:23:21+09:00
author: SWnomad
layout: post
categories: 컴퓨터구조
image: /images/2018/12/no-name-3.jpg
tags:
  - bool
  - boolean algebra
  - 부울 대수
---
부울 대수는 19세기 중반 영국의 수학자가 만든 대수 체계인데, 컴퓨터 디지털 연산의 바탕 이론이 되었다.

부울 대수는 일반적인 사칙연산의 법칙들과 유사하므로 한 번 정리하는 느낌으로 보면 될 것이다.

&nbsp;

&nbsp;

# 1. 부울 대수 법칙

* * *

### 1) 교환 법칙

x+y=y+x

xy=yx

&nbsp;

### 2) 결합법칙

(x+y)+z=x+(y+z)

(xy)z=x(yz)

&nbsp;

### 3) 분배법칙

x+yz = (x+y)(x+z)

x(y+z)=xy+xz

&nbsp;

### 4) 보수의 법칙

x+x&#8217;=1

xx&#8217;=0

&nbsp;

### 5) 멱등 법칙

x+x=x

x*x=x

&nbsp;

### 6) 흡수 법칙

x+xy=x(1+y)=x

x(x+y)=x

&nbsp;

### 7)드 모르간 법칙

(ab)&#8217;=a&#8217;+b&#8217;

(a+b)&#8217;=a&#8217;b&#8217;