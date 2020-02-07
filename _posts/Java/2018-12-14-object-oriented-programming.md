---
title: (Java) 객체 지향 프로그래밍이란?
date: 2018-12-14T21:00:30+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - object
  - oriented
  - programming
  - 객체
  - 메소드
  - 멤버 변수
  - 지향
  - 클래스
  - 프로그래밍
---
객체란 간단히 말해 어떠한 사물이나 대상을 뜻한다. 눈에 보이는 사람, 자동차 등 부터 눈에 보이지 않는 마우스 클릭, 쇼핑 등도 객체가 될 수 있다.

객체지향 프로그래밍이란 이러한 객체들 사이에 일어나는 유기적인 일들을 구현하여 프로그램을 만드는 것이다.

객체지향 프로그래밍은 클래스를 기반으로 모든게 일어나기 시작한다.

클래스란 객체의 속성이나 행동이 정의되는 하나의 모듈 단위이다. 객체를 만든다는 것은 바로 클래스를 정의한다는 것이다.

클래스는 객체의 <span style="text-decoration: underline;"><strong>속성, 특성</strong>을 나타내는 부분과 하나의 기능을 수행하는 <span style="text-decoration: underline;"><strong>메서드</strong>를 가진다.

가령 사람이라는 객체에 대해 정의한다고 해보자.

속성, 특성에는 나이, 이름, 주소 등이 있을 것이다. 그리고 기능에는 나이를 먹는다, 이름을 개명한다 등이 있을 것이다.

이러한 것을 클래스로 구현하는 것을 간단히 맛보자면 다음과 같다.

~~~ java
package classpart;

public class Person {
    // Person 클래스의 속성
    int age;
    String name;
    String address;

    // 나이 먹는 행위
    void IncreaseAge(int age) {
    this.age++;
    }
    // 차타기
    void rideCar() {
    System.out.println("부릉부릉~");
    }
}
~~~

이러한 클래스를 한 번 정의하면 여러 사람이라는 객체를 찍어내며 만들 수 있다. 이것이 객체지향 프로그래밍의 생산성을 높여주는 부분이다.

&nbsp;

이처럼 클래스를 기반으로 다양한 객체를 만들어 그들 사이에 일어나는 상호작용을 구현하고 프로그래밍 하는것이다.