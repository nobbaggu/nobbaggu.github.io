---
title: 컬렉션 프레임워크
date: 2018-12-24T15:58:14+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - collection
  - framework
  - interface
  - Java
  - map
  - 맵
  - 인터페이스
  - 자바
  - 컬렉션
  - 프레임워크
---
컬렉션 프레임워크(Collection Framework)란 자료를 효율적으로 생성,관리할 수 있도록 자바 JDK에서 제공하는 라이브러리이다.

컬렉션 프레임워크는 크게 2가지로 나뉜다. 1) 컬렉션 인터페이스 2) 맵 인터페이스

&nbsp;

<a href="https://SWnomad.com/%ec%bb%ac%eb%a0%89%ec%85%98-%ed%94%84%eb%a0%88%ec%9e%84%ec%9b%8c%ed%81%ac%eb%9e%80/%ec%a0%9c%eb%aa%a9-%ec%97%86%ec%9d%8c-194/" rel="attachment wp-att-1638"><img class="aligncenter size-full wp-image-1638" src="/images/2018/12/no-name-39.jpg" alt="" width="433" height="255" srcset="/images/2018/12/no-name-39.jpg 433w, /images/2018/12/no-name-39-300x177.jpg 300w" sizes="(max-width: 433px) 100vw, 433px" /></a>

&nbsp;

# 1. Collection 인터페이스

* * *

컬렉션 인터페이스는 하위 인터페이스로 List, Set이 있다.

## 1) List

리스트 인터페이스는 **순차적인 자료**를 관리하는 데 사용한다. 또한 **중복을 허용**한다.

List 인터페이스를 구현한 클래스로는 ArrayList, LinkedList, Vector 등이 있다.

&nbsp;

## 2) Set

셋 인터페이스는 중고등학교 교과시간에 배운 집합을 생각하면 된다. **순서가 정해져 있지 않고** **중복이 허용되지 않는다**.

&nbsp;

Collection 인터페이스에는 다양한 추상 메소드가 정의되어 있는데, 자료를 추가/제거 하거나 자료의 길이를 반환하는 등의 기능을 클래스에서 구현한다.

&nbsp;

# 2. Map 인터페이스

* * *

맵 인터페이스는 하나의 자료가 아닌 key-value쌍이라고 하는 '쌍 자료'를 관리하는 데 사용한다. 예를들면 백화점의 고객번호 - 이름 자료 쌍이 있을 수 있다. 여기서 key는 고객번호, value는 이름이다. 고객 번호는 중복될 수 없지만 동명이인의 고객이 있을 수 있으므로 value는 중복이 될 수 있을 것이라고 상식적으로 생각할 수 있다.

**key는 중복이 허용되지 않고, value는 중복이 허용된다**.

&nbsp;

Map 인터페이스에도 다양한 추상 메소드가 정의되어 있으며 key값을 이용하여 객체를 생성하고 value를 추가한다거나 모든 key집합, value집합 반환등의 기능이 구현되어 있다.