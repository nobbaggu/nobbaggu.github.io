---
title: (디자인패턴) 6 - 커맨드 패턴(Command Pattern)
date: 2020-06-20T12:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 커맨드
  - command
  - 패턴
  - new
---

## 1. 개요 ##
----

+ 대신 일해주는 디자인 패턴
	+ 직접 일을 하지 않고 원하는 일을 만들고 던져주면 알아서 처리해주는 객체 디자인
	+ 좀 더 고급스러운 표현으로는 **작업 요청의 캡슐화**
	+ ex) Java 스레드에 `Runnable` 객체를 던져주고 `run()`를 호출하면 알아서 일해준다.
		+ `Runnable` 객체를 디자인 패턴에서는 **커맨드 객체(command object) - 작업 정보가 들어있는 객체**라고 부른다.

+ 호출하는 클래스 내에서 직접 작업처리 코드를 작성하지 않고 작업 객체를 받아서 작업 객체의 실행 메소드를 호출만 한다.
	+ 작업 종류의 추가, 작업 방식의 변경이 일어나도 호출하는 코드를 변경할 필요 없다.
	+ 작업의 분리로 인해 컨트롤 클래스와 작업 클래스를 분리할 수 있다.
	+ ex) Java의 `Thread`는 작업이 어떻게 되는지 자세히 알 필요가 없다. 다만 `Runnable`의 실행 메소드를 대신 호출해 줄 뿐이다.
		+ 따라서 작업 방식의 변경이 있을 경우 `Runnable` 객체만 오버라이딩 해주면 `Thread`는 이러한 것들을 신경쓰지 않아도 된다.

## 2. 커맨드 패턴 ##
----

+ 실행 순서
	+ 클라이언트(사용자)가 command 객체 생성
	+ 인보커(invoker)에 command 객체 전달
	+ 인보커가 command 객체의 execute 실행
	+ 리시버(receiver)가 작업 처리

+ 커맨드(command)
	+ 작업의 수행 동작을 가지고 있는 객체

+ 인보커(invoker)
	+ Java 스레드와 같이 작업 요청을 전달받고 대신 실행요청을 해주는 객체

+ 리시버(receiver)
	+ 실제 작업처리 혹은 동작을 하는 객체
	
![class_diagram](https://nobbaggu.github.io/images/designpattern/commandpattern/class_diagram.jpg){: width="50%" height="50%"}

<br>

## 3. 커맨드 패턴 예시 ##
----

+ 전등을 켜고 끄는 예제

~~~ java
public class Light {
    public void on() {
        System.out.println("불이 켜졌습니다.");
    }

    public void off() {
        System.out.println("불이 꺼졌습니다.");
    }
}
~~~

~~~ java
public interface Command {
    void execute();
}
~~~

~~~ java
public class Invoker {
    Command command;

    public Invoker(Command command) {
        this.command = command;
    }

    public void execute() {
        command.execute();
    }

    public void setCommand(Command command) {
        this.command = command;
    }
}
~~~

~~~ java
public class Main {
    public static void main(String[] args) {
		//리시버(receiver) 객체 생성
        Light light = new Light();

		//ConcreteCommand 객체 생성(불켜기)
        Command lightOnCommand = new Command() {
            @Override
            public void execute() {
                light.on();
            }
        };

		//ConcreteCommand 객체 생성(불끄기)
        Command lightOffCommand = new Command() {
            @Override
            public void execute() {
                light.off();
            }
        };

        Invoker invoker = new Invoker(lightOnCommand); //인보커 객체 생성하며 생성자 매개변수로 커맨드 객체 전달
        invoker.execute(); //실행요청
        invoker.setCommand(lightOffCommand); //setCommand() 메소드 호출로 다른 커맨드 객체 설정
        invoker.execute(); //실행요청
    }
}
~~~

+ Command 인터페이스를 익명 객체로 생성했다.
	+ 하나는 `Light` 객체의 `on()` 호출
	+ 하나는 `Light` 객체의 `off()` 호출
	
+ 현실에서는 이런 간단한 상황이 아니라 더 복잡한 상황을 제어해야 할 것이다.
	+ 익명 객체가 아닌 자세한 동작을 구현한 `Command`의 구현 클래스를 작성할 것이다.
	+ `Command`의 구현 객체를 만들어서 `invoker`에 던져주고 실행요청을 하면 된다.
	
<br>

## 4. 정리 및 생각 ##
----

커맨드 패턴을 사용하면 요청을 하는 주체인 클라이언트와 작업 이 둘을 분리할 수 있다. 작업이 변경되면 해당 작업만 수정하면 되며 클라이언트 코드는 건드릴 필요가 없게 된다. 이는 객체지향 디자인 원칙인 변경에는 닫혀있고 확장에는 열려있는 OCP를 지킬 수 있게 도와준다. 지금까지 익숙하게 쓰던 Java의 Thread도 커맨드 패턴을 사용한 것이었다. 스레드가 인보커고 Runnable 객체가 Command 객체였던 것이다. Command 객체를 여러게 만들어 한번에 인보커에 등록해두고 일련의 순서대로 실행할 수도 있다. 이 역시 Java의 `ThreadPoolExecutor`에 잘 구현되어있다. 커맨드 패턴을 쓰면 사용자는 작업이 세부적으로 어떻게 실행되는지 전혀 신경 쓸 필요가 없다. `Command` 인터페이스의 `execute()`만 호출하면 된다.