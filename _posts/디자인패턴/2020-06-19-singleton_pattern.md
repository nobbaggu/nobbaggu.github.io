---
title: (디자인패턴) 5 - 싱글톤 패턴(Singleton Pattern)
date: 2020-06-19T12:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 싱글톤
  - 싱글턴
  - singleton
  - 패턴
  - new
---

## 1. 개요 ##
----

+ 싱글톤 패턴은 인스턴스를 단 하나만 만들기 위한 디자인 패턴이다.

+ 언제 필요한가요?
	+ 인스턴스를 단 하나만 만들어야 할 때
	+ ex) 캐시, 스레드풀, 설정용 객체, 기록용 로거(logger), 디바이스 드라이버 객체 등등
	+ 여러 객체가 동시에 돌아가면 문제가 생기는 상황에서 사용
	
+ 전역 변수로 객체를 한번만 만들어서 사용하는 것과 다른가요?
	+ 다르다.
	+ 전역변수로 만들면 프로그램 실행 시 무조건 객체가 만들어진다.
		+ 한 번도 사용하지 않게 되더라도..
		+ 사실 OS에 따라 달라질 수 있다. 어떤 JVM에서는 전역변수로 객체를 선언해도 필요시에 생성하기도 한다.
		+ 여러 클래스에서 전역변수로 객체를 여러번 만들면 여러개가 만들어질 수 있어 이런 실수를 하지 않는다는 보장이 없다.
		
	+ 싱글턴 패턴을 사용하면 최초 객체 호출시에 단 한 번 객체가 생성되고 이후에는 전역 참조가 가능하다.
		+ 코드적으로 객체 생성이 단 한번만 일어나도록 보호장치를 설치해둔다.
		+ 이처럼 객체를 필요시에 생성하는 것을 **게으른 객체 생성(lazy instantiation)**이라고 한다.

<br>

## 2. 싱글톤 패턴 ##
----

+ 아래는 싱글턴 객체를 만들기 위한 클래스 형태를 보여준다.

~~~ java
public class MyClass {
    //1. 다른 패키지에서 객체를 참조할 수 있도록 public으로 선언
    //2. static method에서 참조하기 위해 static으로 변수 선언
    public static MyClass instance;
    
    //외부에서 new 연산자로 객체 생성 불가능하도록 private로 생성자 선언
    private MyClass() {}
    
    //유일한 객체를 얻을 수 있는 메소드
    //1. 다른 패키지에서 호출할 수 있도록 public 선언
    public static MyClass getInstance() {
        //게으른 객체 생성
        if(instance == null) {
            instance = new MyClass();
        }
        
        return instance;
    }
}
~~~

+ 코드에서 중요한 포인트
	+ 외부에서 `new` 연산자를 통해 객체 생성을 막기위해 생성자의 접근 제어자를 `private`으로 하였다.
	+ 외부에서 최초로 호출하는 시점에서 객체를 생성한다.
	+ 외부에서 메소드 호출을 통해 객체를 얻을 수 있도록 메소드 접근 제어자를 `public`으로 하였다.
	+ 객체를 만들 수 없으므로 클래스명을 통해 메소드를 호출할 수 있도록 메소드를 `static`으로 선언하였다.
	
## 3. 실제 적용 사례 ##
----

+ 초콜릿 공장의 초콜릿 가열 제어 장치
	+ 초콜릿이 끓는동안 또다시 초콜릿을 넣거나 다 끓지않은 초볼릿을 부어버리면 안된다.
	+ 실수로 여러 객체가 동시에 제어하면 이런 상황이 발생할 확률이 존재한다.
	+ 싱글턴 패턴을 적용해야 할 때이다.
	
~~~ java
public class ChocolateBoiler {
    
    public static ChocolateBoiler instance;

    boolean empty; //솥이 비어있는지 체크하는 flag
    boolean boiled; //초콜릿이 다 끓었는지 체크하는 flag
    
    private ChocolateBoiler() {}

    public static ChocolateBoiler getInstance() {
        //게으른 객체 생성
        if(instance == null) {
            instance = new ChocolateBoiler();
        }
        
        return instance;
    }
    
    //솥에 초콜릿을 채우는 메소드
    public void fill() {
        //솥이 비어있을 때만 초콜릿을 채운다.
        if(isEmpty()) {
            setEmpty(false);
            setBoiled(false);
            System.out.println("초콜릿을 채웁니다.");
        }
    }
    
    //다 끓은 초콜릿을 붓는 메소드
    public void drain() {
        //솥에 초콜릿이 있고 다 끓었는지 확인 후 비운다
        if(!isEmpty() && isBoiled()) {
            System.out.println("솥에서 초콜릿을 비웁니다.");
            setEmpty(true);
            setBoiled(false);
        }
    }
    
    //초콜릿 끓이는 메소드
    public void boil() {
        //솥에 초콜릿이 있고 끓지 않았는지 확인 후 끓인다
        if(!isEmpty() && !isBoiled()) {
            System.out.println("초콜릿을 끓입니다.");
            setBoiled(true);
        }
    }

    public boolean isEmpty() {
        return empty;
    }
    
    public void setEmpty(boolean empty) {
        this.empty = empty;
    }

    public boolean isBoiled() {
        return boiled;
    }
    
    public void setBoiled(boolean boiled) {
        this.boiled = boiled;
    }
}
~~~

<br>

## 4. 치명적 문제점 ##
----

+ 사실 위에서 보았던 싱글턴 패턴은 완벽하지 않으며 결정적인 문제점을 가지고 있다.
	+ 힌트 : 멀티 스레드를 사용할 때 어떤일이 발생할까?

+ 위의 초콜릿 공장 예시에서 문제가 생겼다.
	+ 초콜릿이 끓고 있는데 또다른 초콜릿을 솥에 채우는 동작이 발생해서 초콜릿이 넘쳤다.
	+ 여러개의 스레드를 사용하도록 고친 것 이외에는 아무것도 손대지 않았다...
	+ 어떤 부분이 문제일까?
	
~~~ java
public class MyClass {
    public static MyClass instance;
    
    private MyClass() {}
    
    public static MyClass getInstance() {
        if(instance == null) { //우연찮게 두 스레드 모두 동시에 이 부분을 check하였다!
            instance = new MyClass();
        }
        return instance;
    }
}
~~~

+ 두 스레드에서 우연찮게 동시에 객체를 호출했다.
	+ 마침 객체는 아직까지 한 번도 호출되지 않은 시점이었다.
	+ 동시에 두 스레드가 동시에 `instance == null` 부분을 확인했고 객체가 없다고 판단해 객체 생성 코드를 실행시켰다.
	+ 객체가 두 개 만들어진것이다!
	+ 해결책 : `getInstance()` 메소드에 `synchronized` 키워드 추가
	
<br>

~~~ java
public class MyClass {
    public static MyClass instance;
    
    private MyClass() {}
    
    public static synchronized MyClass getInstance() {
        if(instance == null) { //우연찮게 두 스레드 모두 동시에 이 부분을 check하였다!
            instance = new MyClass();
        }
        return instance;
    }
}
~~~

+ `synchronied` 키워드를 추가함으로써 한 번에 하나의 자원만 이 메소드를 호출할 수 있다.
	+ 하나의 스레드가 메소드 내부 코드에 진입했다면 다른 스레드는 먼저 메소드를 호출한 스레드의 작업이 끝날 때까지 기다려야 한다.
	+ 하나의 스레드 작업이 끝나고 나면 객체는 생성되었기 때문에 다른 스레드 진입시에 새로운 객체를 만드는 부분은 실행되지 않는다.
	
<br>

## 5. synchronized의 문제점 ##
----

+ 메소드를 동기화 시키면 오버헤드가 크다.
	+ 매우 잦은 호출 시에 성능이 매우 떨어진다.
	
+ 속도가 별로 중요하지 않다면 그냥 두자.

+ 아예 처음부터 만들어버릴 수 있다.
	+ `instance` 변수 선언 시 객체를 생성해서 대입하는 방법이 있다.
	+ 객체 호출이 분명히 일어난다면 어차피 객체를 생성해야하므로 이 방법이 괜찮다.
	+ 이렇게 하면 코드가 아래와 같이 된다.
	
~~~ java
public class MyClass {
    public static MyClass instance = new MyClass();
    
    private MyClass() {}
    
    public static synchronized MyClass getInstance() {
        return instance;
    }
}
~~~

<br>

+ DCL(Double-Checking Locking)을 사용한다.
	+ Java 1.5 버전부터 지원
	+ 객체가 없을 때만 동기화되고 객체 생성 이후부터는 동기화가 해제되어 성능이 복구된다.
	
~~~ java
public class MyClass {

    public static MyClass instance;

    private MyClass() {}

    public static MyClass getInstance() {
		//null인지 확인하고 동기화 블럭 진입
		//객체 생성부를 동기화 블럭으로 보호하여 두 개 이상의 객체 생성을 막는다.
		//객체가 생긴 이후부터는 동기화 블럭을 진입할 일이 없어 성능이 좋아진다.
        if(instance == null) {
            synchronized (MyClass.class) {
                if(instance == null) {
                    instance = new MyClass();
                }
            }
        }
        return instance;
    }
}
~~~
