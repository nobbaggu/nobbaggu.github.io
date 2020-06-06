---
title: (디자인패턴) 3 - 데코레이터 패턴(Decorator Pattern)
date: 2020-06-06T12:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 데코레이터
  - 패턴
  - 인터페이스
  - 추상클래스
  - 상속
  - 구현
  - 분리
---

## 0. 어떤 상황에 쓰는가? ##
----

+ 커피 주문 시스템
	+ 기본 커피
	+ 추가적인 토핑(휘핑, 모카 등)
	
+ 상속을 사용한다면?
	+ 기본 커피 클래스
	+ 모든 조합 예시에 대한 각각의 클래스가 필요
		+ 모카를 추가한 카페라떼 클레스는 카페라떼를 상속받고 모카 가격을 더하는 방식
		+ 새로운 토핑메뉴가 생길 경우 새로운 클래스를 작성하기위해 코드를 수정해야한다.
		+ 메뉴에 없는 주문(극단적으로 카페라떼에 휘핑크림 3개, 모카 10개, 계피 4개)는 주문할 수 없다.
	+ 가격이 바뀔 경우 코드의 `cost()` 메소드를 직접 수정해야한다.
	+ 시간이 가고 메뉴가 다양화될수록 모든 클래스를 관리하기 어려워 질것이다.
	
![only_extends](https://nobbaggu.github.io/images/designpattern/decoratorpattern/only_extends.jpg){: width="50%" height="50%"}

<br>
	
+ 코드를 수정하지 않고 동적으로 기능을 추가하고 확장하기 위한 디자인 패턴이 필요하다.
	+ OCP(Open-Closed Principle)
		+ 변경에는 닫혀있고 확장에는 열려있는 객체지향 디자인 원칙
		+ 단순히 모든 개별적인 경우에 대해 상속을 받아야 한다면 이것을 지킬 수 없고 시시떼떼로 코드를 수정하고 클래스를 수정하는 작업이 필요하다.
	+ 이를 위해서 데코레이터 패턴을 사용한다.

<br>

## 1. 데코레이터 패턴이란? ##
----

+ 객체에 추가적인 요건을 동적으로 추가하는 디자인 패턴
	+ 코드 수정없이 새로운 기능을 추가하여 확장이 가능
	+ 서브클래스를 만드는 것을 통해 기능을 확장
	
+ **데코레이터 객체는 상위 클래스를 감싸는 래퍼 객체(Wrapper)**로 만든다.
	+ 여러 번 감쌀 수 있기때문에 여러 기능을 마음대로 추가할 수 있다.
	
<br>

![class_diagram](https://nobbaggu.github.io/images/designpattern/decoratorpattern/class_diagram.jpg){: width="50%" height="50%"}

<br>

+ 구성요소
	+ `Component`
		+ 최상위 추상 클래스
	+ `ConcreteComponent`
		+ `Component`를 구현해 만든 기본 아이템(아메리카노, 에스프레소, 라떼 등)
	+ `Decorator`
		+ 추가적인 기능들의 최상위 추상 클래스
		+ `Component`를 상속받는다
	+ `ConcreteDecorator`
		+ `Decorator`를 구현해 만든 여러가지 추가 기능들(휘핑크림, 모카 등)
		
+ 결국 모든 구성요소 클래스가 단 하나 `Component`의 하위 클래스들이다.
	+ 데코레이터는 `Component`를 감싸는 객체로 정의된다.
	+ 몇번을 감싸도 결국 `Component`의 하위 객체이기에 여러번 감쌀 수가 있다.
	+ 따라서 원하는대로 구성할 수 있다.
	
+ 데코레이터는 감싸는 객체의 메소드를 확장한다.
	+ 감싸는 객체의 메소드를 호출하면서 추가적으로 무언가를 더한다.
	+ 이것이 데코레이터가 기능을 확장하는 방법이다.

![wrapper](https://nobbaggu.github.io/images/designpattern/decoratorpattern/wrapper.jpg){: width="50%" height="50%"}

<br>

![class_diagram_example](https://nobbaggu.github.io/images/designpattern/decoratorpattern/class_diagram_example.jpg){: width="50%" height="50%"}

<br>

## 2. 예시 ##
----

~~~ java
//Component
public abstract class Beverage {
    String description;

    public String getDescription() {
        return description;
    }

    public abstract int cost();
}
~~~

<br>

~~~ java
//Decorator
public abstract class Topping extends Beverage {
    Beverage beverage;
}
~~~

<br>

~~~ java
//ConcreteComponent
public class Americano extends Beverage {

    public Americano() {
        description = "아메리카노";
    }

    @Override
    public int cost() {
        return 2000;
    }
}

//ConcreteDecorator
public class Mocha extends Topping {

    public Mocha(Beverage beverage) {
        this.beverage = beverage;
        description = beverage.getDescription() + ", 모카";
    }

    @Override
    public int cost() {
        return 500 + beverage.cost();
    }
}
~~~

<br>

~~~ java
//ConcreteDecorator
public class Whipping extends Topping {
    public Whipping(Beverage beverage) {
        this.beverage = beverage;
        description = beverage.getDescription() + ", 휘핑";
    }

    @Override
    public int cost() {
        return 800 + beverage.cost();
    }
}

//ConcreteDecorator
public class Mocha extends Topping {

    public Mocha(Beverage beverage) {
        this.beverage = beverage;
        description = beverage.getDescription() + ", 모카";
    }

    @Override
    public int cost() {
        return 500 + beverage.cost();
    }
}
~~~

<br>

~~~ java
public class Main {
    public static void main(String[] args) {
        Beverage americano = new Americano();
        americano = new Mocha(americano);
        americano = new Mocha(americano);
        americano = new Whipping(americano);
        System.out.println(americano.getDescription() + ": " + americano.cost()+ "원");

        Beverage latte = new Latte();
        latte = new Whipping(latte);
        System.out.println(latte.getDescription() + ": " + latte.cost() + "원");
    }
}
~~~

실행결과

~~~ console
아메리카노, 모카, 모카, 휘핑: 3800원
라떼, 휘핑: 3300원
~~~

<br>

## 3. 특징 ##
----

+ 상속을 통해 행동을 물려받을 목적보다는 형식을 맞추는 것이 포인트
	+ 데코레이션 한 것도 `Component` 객체이기때문에 반복적인 데코레이션이 가능

+ 데코레이터 클래스에 위임된 행동 이외의 새로운 메소드를 추가해서 사용할 수 있다.

+ 자잘한 클래스들이 많이 추가되고 코드가 복잡해질 수 있다는 단점이 존재한다.
	+ Java의 I/O 라이브러리
	+ 단지 `InputStream`을 감싸는 데코레이터일 뿐이므로 복잡할 것 없다
	
+ 꽤 많은 데코레이터로 감싸서 초기화 해야할 경우가 생긴다.
	+ 데코레이터 패턴은 일반적으로 팩토리, 빌더와 같이 사용해서 이러한 문제점을 해결한다.

<br>

## 4. 정리 및 생각 ##
----

상속을 통해 형식을 맞춘다. 새로운 객체를 만들 때 생성자에서 같은 형식의 객체를 받아 그것을 감싸서 데코레이션을 할 수 있다. 데코레이션을 해서 나오는 객체 역시 똑같은 객체이다. 이것은 또다시 데코레이션 될 수 있단 의미이다. 이런식의 디자인을 통해 필요한 기능을 동적으로 추가해서 사용하는 일이 가능해진다. 데코레이터 패턴을 사용하면 **확장에는 열려있고 변경에는 닫혀있는 OCP 객체지향 디자인 원칙**이 지켜진다.
Java의 I/O 패키지도 데코레이션 패턴으로 되어있다. 기본 스트림들은 `ConcreteComponent`에 속하고 `FilterInputStream`은 `Decorator`추상 클래스에 해당한다. `FilterInputStream`을 확장한 `ConcreteDecorator` 클래스의 예시로는 `BufferedInputStream`, `LineNumberInputStream` 등이 있다.
