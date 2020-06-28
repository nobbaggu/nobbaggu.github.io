---
title: (디자인패턴) 9 - 템플릿 메소드 패턴(Templete Method Pattern)
date: 2020-06-27T15:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 템플릿
  - template
  - 메소드
  - method
  - 패턴
  - pattern
---

## 1. 템플릿 메소드란? ##
----	

+ 템플릿(template)의 정의
	+ 일정한 틀, 형식
	+ 파워포인트 템플릿을 생각하면 된다.
		+ 전체적인 틀, 디자인을 잡아놓았지만 채워넣을 내용, 글꼴, 사진등은 작성자가 채워넣는다.
		
<br>

+ 템플릿 메소드
	+ 알고리즘의 구조, 뼈대를 정의하는 메소드
	+ 템플릿 메소드에서 호출하는 각각의 메소드들은 하위 클래스에서 재정의해서 사용한다.	
	
<br>

~~~ java
public abstract class AbstractClass {
    
	//알고리즘의 구조를 템플릿 메소드로 정의한다.
    final void templateMethod() {
        primitiveOperation1();
        primitiveOperation2();
		concreteMethod1();
    }
	
	//실제 알고리즘은 하위 클래스에서 재정의하여 사용하는 추상 메소드
	abstract void primitiveOperation1();
	abstract void primitiveOperation2();
	
	void concreteMethod1() {
		//구현
	}
}
~~~

<br>

+ 템플릿 메소드 패턴
	+ 템플릿 메소드를 사용하여 알고리즘의 골격을 정의한다.
	+ 여러 단계 중 변하지 않는 공통적인 부분은 직접 구현한다.
	+ 하위 클래스별로 다른 부분은 하위 클래스에서 직접 구현해서 사용한다.
	+ 알고리즘이 변하는것을 방지하기 위해 템플릿 메소드는 `final`로 선언한다.
	
![class_diagram](https://nobbaggu.github.io/images/designpattern/templatemethodpattern/class_diagram.jpg){: width="50%" height="50%"}

<br>

## 2. 예시 ##
----

+ 웨이트 트레이닝 운동 플래너
	+ 수준별로 초급자, 고급자로 나눈다.
	
<br>

~~~ java
public abstract class TrainingPlan {

    //알고리즘의 구조를 정의한 템플릿 메소드
    final void doExercise() {
        squate();
        getRest();

        deadlift();
        getRest();

        benchpress();
        getRest();
    }

    abstract void squate(); //스쿼트
    abstract void deadlift(); //데드리프트
    abstract void benchpress(); //벤치프레스

    //휴식은 공통적으로 1분간 쉬므로 직접 구현
    void getRest() {
        System.out.println("1분간 휴식");
    }
}
~~~

초급자 플랜

~~~ java
public class BeginnerPlan extends TrainingPlan {
    @Override
    void squate() {
        System.out.println("30kg으로 스쿼트를 합니다.");
    }

    @Override
    void deadlift() {
        System.out.println("30kg으로 데드리프트를 합니다.");
    }

    @Override
    void benchpress() {
        System.out.println("30kg으로 벤치프레스를 합니다.");
    }
}
~~~

<br>

고급자 플랜

~~~ java
public class AdvancedPlan extends TrainingPlan {
    @Override
    void squate() {
        System.out.println("60kg으로 스쿼트를 합니다.");
    }

    @Override
    void deadlift() {
        System.out.println("60kg으로 데드리프트를 합니다.");
    }

    @Override
    void benchpress() {
        System.out.println("60kg으로 벤치프레스를 합니다.");
    }
}
~~~

<br>

## 3. hook 메소드 ##
----

~~~ java
public abstract class AbstractClass {
    
    final void templateMethod() {
        primitiveOperation1();
        primitiveOperation2();
		concreteMethod1();
		hook2(); //후크
	}
	
	abstract void primitiveOperation1();
	abstract void primitiveOperation2();
	
	void concreteMethod1() {
		//구현
	}
	
	void hook(){}
}
~~~

+ hook 메소드
	+ 아무것도 하지 않거나 기본적인 내용만 구현되어 있는 메소드
	+ 서브클래스에서 오버라이딩 해도 되고 안해도 된다.
	+ 서브 클래스에서 재정의하면 알고리즘에 끼어들 수 있다.

<br>

+ 운동 플래너에서 후크의 활용 예시
	+ 운동이 힘들수록 쿨다운은 안전을 위해 중요해진다.
	+ 초급자 플랜은 쿨다운 운동을 하지 않아도 되기때문에 그대로 둔다.
	+ 고급자 플랜은 쿨다운 운동을 해야 하므로 재정의해서 사용한다.

~~~ java
public abstract class TrainingPlan {

    //알고리즘의 구조를 정의한 템플릿 메소드
    final void doExercise() {
        squate();
        getRest();

        deadlift();
        getRest();

        benchpress();
        getRest();
    
		coolDown();
	}

    abstract void squate(); //스쿼트
    abstract void deadlift(); //데드리프트
    abstract void benchpress(); //벤치프레스

    //휴식은 공통적으로 1분간 쉬므로 직접 구현
    void getRest() {
        System.out.println("1분간 휴식");
    }
	
	//후크
	void coolDown(){}
}
~~~

<br>

+ 후크를 쓰는 대신 추상 메소드를 만들어두고 쓰면 되지 않은가?
	+ 알고리즘의 특정 단계를 꼭 제공해야 하는 경우 추상 클래스를 사용한다.
	+ 알고리즘이 선택적으로 동작해도 되는 경우 후크를 쓴다.

<br>

## 4. 장점 ##
----

+ 공통, 중복 코드를 재사용

+ 여러 곳에서 동작이 조금씩 다른 형태로 필요할 경우에 유용하다.

+ 알고리즘의 실행 절차가 바뀌면 템플릿 메소드만 변경하면 된다.

+ 새로운 하위 클래스를 쉽게 추가할 수 있는 프레임워크를 재공해준다.
	+ 하위 클래스에 의존하는 부분만 코딩하면 된다.
	
+ 알고리즘에 대한 지식이 템플릿 메소드에 집중되어있어 파악하기가 편리하다.

<br>

## 5. 정리 ##
----

템플릿 메소드를 사용함으로써 전체적인 로직의 뼈대를 정해놓는다. 세부적인 알고리즘들 중 공통적인 부분은 추상 클래스에서 직접 구상 메소드를 정의하고 하위 클래스마다 달라져야 하는 부분은 확장할 때 구현해서 사용한다. 팩토리 메소드 패턴과 헷갈릴 수 있다. 팩토리 메소드 패턴에서는 객체 생성을 서브클래스에 위임하는 것이 목적이었다. 그러나 템플릿 메소드 패턴에서는 알고리즘 뼈대를 `final`로 선언한 템플릿 메소드로 일관적이게 고정시키되, 세세한 알고리즘의 구현은 서브클래스로 위임하는 게 목표다. 이렇게 하면 클라이언트는 구현이 아닌 추상 클래스에 의존하게 되고 중요한 디자인 원칙인 의존성 역전 원칙(DIP)를 지킬 수 있다.