---
title: (디자인패턴) 1 - 스트래티지 패턴(Strategy Pattern)
date: 2020-05-31T12:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 스트래티지
  - 패턴
  - 인터페이스
  - 추상클래스
  - 상속
  - 구현
  - 분리
---

<br>

## 1. 어떤 문제를 해결하기 위한 패턴인가? ##
----

+ 게임 프로그래밍 상황을 가정해보자.
	+ Character 추상클래스를 구현하는 여러 직업별로 각각의 구현 클래스 존재
	+ 모든 캐릭터는 포션을 마시므로 추상클래스에서 `drinkPortion()` 구현 
	+ 각 캐릭터는 전투방식이 다르므로 `fight()`는 abstract로 남겨두고 각자 구현
	
+ 포션 효율 향상 스킬이 있는 직업이 추가되어 `drinkPortion()` 구현이 달라져야한다면?
	+ 그 직업만 오버라이드(override)해서 내용을 바꾸면 되잖아?
	
+ 게임에 펫이 추가되고 캐릭터들이 이를 탈 수 있게 해야 한다면?
	+ Character 추상클래스에 `ridePet()` 메소드를 구현한다.
	+ 펫을 탈 수 없는 직업이 있어야한다면? 걔만 오버라이드 해서 메소드 바디(body)를 비워놓아서 아무 행동도 하지않게 하면 되잖아?
	
+ 위와 같은 방식으로 새로운 구현 클래스가 추가될 때 마다 오버라이드 하다보면 점점 걷잡을 수 없이 코드가 따로놀고 복잡해질 수 있다

<br>
+ 상속은 답이 아닌 것 같다. 그래서 그냥 인터페이스를 사용하기로 했다.
	+ 모든 구현 클래스는 인터페이스를 구현하고 메소드 내용을 채워넣어야한다.
	+ 대부분의 캐릭터에서 공통적으로 동일하게 작동하는 메소드가 있을 텐데 재사용의 이점을 버리자는 건가?
	
<br>
## 2. 해결 방법 ##
----

+ 소프트웨어 디자인 원칙1
	+ '변하는 부분'과 '변하지 않는 부분'을 분리하기

+ 포션을 마시는 행동은 모든 캐릭터 인스턴스에서 동일하고, 전투효과만 다르다고 가정하자.
	+ 변하는 부분인 전투 효과를 클래스로 분리
	+ 추가로 캐릭터의 모습을 표현하는 `display()` 메소드는 각 클래스에서 구현해야한다. 직업별로 모습은 완전히 다르므로
	
<br>
~~~ java
public abstract class Character {
	
	Weapon weapon;
	
	//생긴 모습은 직업별로 많이 다르므로 각 클래스에서 구현
	public abstract display();
	
	public drinkPortion() {
		System.out.println("HP 100 증가");
	}
	
	//대입된 Weapon 객체별로 효과가 다를 것이다.
	//그러나 Character 클래스는 그것을 알 필요 없다.
	public fight() {
		weapon.attack();
	}
	
	//유연한 무기 변경을 위한 setter
	public setWeapon(Weapon weapon) {
		this.weapon = weapon;
	}
}

public class Warrior extends Character {
	
	@Override
	public display() {
		System.out.println("대충 전사답게 생김");
	}
}

public class Magician extends Character {
	
	@Override
	public display() {
		System.out.println("대충 마법사처럼 생김");
	}
}
~~~

<br>
~~~ java

public interface Weapon {
	public void attack();
}

public Sword implements Weapon {
	
	@Override
	public void attack() {
		System.out.println("슉슉~");
	}
}

public Staff implements Weapon {
	
	@Override
	public void attack() {
		System.out.println("퓽퓽~");
	}
}
~~~

<br>
## 3. 무엇이 해결되었나? ##
----

+ 행동을 분리했기때문에 객체만 생성한다면 얼마든지 재사용할 수 있어 상속의 장점을 그대로 가진다.

+ 요구사항의 변화 혹은 기능 추가에 대해 Character 클래스에 손을 대지않고 Weapon 클래스만 변경 혹은 추가하면 되므로 확장이 쉽다.

+ 프로그램 실행 중에서 `setWeapon()` 메소드를 통해 무기를 변경할 수 있어 유연한 행동이 보장된다.

<br>
## 4. 정리 ##
----

+ 상속이 항상 답이 아닐 수 있다.

+ **변하는 부분과 변하지 않는 부분을 분리**하는 것이 중요하다.
	+ 따로 뽑아내서 인터페이스화 하고 구체적인 구현들은 자주 바뀌므로 이를 구현하여 만든다.
	+ 호출을 하는 주체에 종속되지 않고 내용을 변경을 할 수 있다.
	+ 인터페이스를 구현하는 새로운 구현클래스만 만들면 되므로 기능 확장이 쉽다.
	
+ **구현이 아닌 인터페이스에 맞춰 프로그래밍**하라
	+ 위 예제에서는 Sword, Staff가 아닌 Weapon 타입으로 변수를 만들었다.
	+ 모든 무기는 `Weapon`을 구현하므로 전투 중 무기를 바꿔낄 때 `setWeapon()`만 호출하면 무기를 바꿔 낄 수 있다.
	
+ **상속보다는 구성(composite)을 사용**하라.
	+ 알고리즘군(행동)을 별도의 클래스로 캡슐화한다.
	+ 행동이 구현 코드에 의존하지 않게 되어 실행시에 행동을 바꿀 수 있다. 유연해진다.
