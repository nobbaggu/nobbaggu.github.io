---
title: (디자인패턴) 8 - 퍼사드 패턴(Facade Pattern)
date: 2020-06-27T12:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 퍼사드
  - facade
  - 패턴
---

## 1. 퍼사드 패턴이란? ##
----

+ 인터페이스를 단순화시키기 위해 인터페이스를 변경하는 것
	+ 복잡한 여러 클래스의 여러 메소드 호출을 단순하고 쉽게 할 수 있도록 도와준다.
	+ 어댑터가 호환성을 이유로 인터페이스를 변경했다면 퍼사드 패턴에서는 '단순화'가 목적이다.

![class_diagram](https://nobbaggu.github.io/images/designpattern/facadepattern/class_diagram.jpg){: width="50%" height="50%"}

<br>

## 2. 예제 ##
----

+ 홈 씨어터 시스템 구축
	+ 아래는 홈 씨어터를 위한 여러 클래스들의 다이어그램을 보여준다.
	+ 매우 복잡하다.

![home_theater_class_diagram](https://nobbaggu.github.io/images/designpattern/facadepattern/home_theater_class_diagram.jpg){: width="50%" height="50%"}

<br>

+ 아래는 영화를 보기위한 순서다.

~~~ java
popper.on(); //팝콘 기계 전원을 켠다.
popper.pop(); //팝콘을 튀긴다.

lights.dim(10); //전등 밝기 10% 줄인다.

screen.down(); //스크린을 내린다.

projector.on(); //프로젝트를 킨다.
projector.setInput(dvd); //DVD 신호를 입력한다.
projector.wideScreenMode(); // 와이드 스크린 모드로 설정한다.

amp.on(); //앰프를 켠다.
amp.setDvd(dvd); //앰프 입력을 DVD로 전환한다.
amp.setSurroundSound(); //앰프를 서라운드 음향 모드로 설정한다.
amp.setVolume(5); //앰프의 볼륨을 5로 설정한다.

dvd.on(); //DVD플레이어의 전원을 켠다.
dvd.play(movie); //영화를 재생한다.
~~~

+ 영화를 끌때는 역순으로 진행한다.

<br>

+ 퍼사드 클래스를 만들어 위의 과정을 일괄적으로 처리해주는 메소드를 만들면 된다.
	+ 이외에도 여러가지 기능을 일괄적으로 묶어서 하나의 메소드만 호출하면 되도록 만들 수 있다.
	
+ 퍼사드 패턴은 서브시스템의 일련의 인터페이스들의 통합 인터페이스를 제공해준다.
	+ 클라이언트 측에서 단순화된 인터페이스를 사용할 수 있어 문제가 간단해진다.

![home_theater_facade_pattern_class_diagram](https://nobbaggu.github.io/images/designpattern/facadepattern/home_theater_facade_pattern_class_diagram.jpg){: width="50%" height="50%"}
	
<br>

## 3. 최소지식 원칙 ##
----

퍼사드 패턴 덕분에 지킬 수 있는 중요한 디자인 원칙이 있다.

+ 최소 지식 원칙
	+ 친찬 친구하고만 얘기해라.

+ 최소 지식 원칙에서 허용하는 메소드 호출
	+ 객체 자체의 메소드
	+ 메소드에 매개변수로 전달된 객체의 메소드
	+ 객체 내에서 직접 생성한 객체의 메소드
	+ 객체에 속하는 구성요소의 메소드
	
<br>

최소지식 원칙 위배

~~~ java
public float getTemperature() {
	//다른 객체가 return해준 객체의 메소드를 호출하므로 최소지식 원칙에 위배된다.
	Thermometer thermometer = station.getThermometer();
	return thermometer.getTemperature();
}

public float getTemperature() {
	return station.getThermometer().getTemperature();
}
~~~

<br>

최소지식 원칙을 따르는 경우

~~~ java
public float getTemperature() {
	return station.getTemperature();
}
~~~

+ 상호작용하는 객체의 수를 줄였다.
	+ `station`에서 온도계 객체를 받고 그 객체에서 또다시 메소드를 호출했었다.
	+ 차라리 `station` 코드에서 자신의 온도계로 온도를 얻어서 리턴해주는 게 낫다.

<br>

+ 홈씨어터 예제와의 관련성
	+ 복잡한 서브 시스템의 객체를 일일이 다루지 않고 퍼사드 객체와만 상호작용한다.

## 4. 정리 ##
----

퍼사드 패턴은 인터페이스를 단순화 시켜 클라이언트 코드를 단순화 시켜준다. 또한 서브 시스템과 클라이언트를 분리한다. 말인즉슨, 서브시스템에 새로운 기능이 추가되거나 변경된다면 클라이언트 코드는 고칠 필요 없이 퍼사드 클래스 코드만 건드리면 된다. 어댑터 패턴과의 차이점을 잘 알아두어야겠다. 어댑터 패턴의 목적은 호환성이 없는 클래스의 메소드 호출을 위한 래핑(wrapping)이라면 퍼사드 패턴의 목적은 인터페이스의 단순화이다. 두 패턴 모두 여러 클래스를 감쌀 수 있다.