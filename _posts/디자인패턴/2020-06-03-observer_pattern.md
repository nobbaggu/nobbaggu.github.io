---
title: (디자인패턴) 2 - 옵저버 패턴(Observer Pattern)
date: 2020-06-03T12:37:19+09:00
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

## 1. 옵저버 패턴이란? ##
----

+ 한 객체의 상태가 바뀌면 이 객체를 구독하는 객체들에게 자동으로 연락이 가고 구독 객체들의 내용이 갱신된다.
	+ 상태를 저장하는 객체를 **주제(subject)**라고 한다.
	+ 상태를 구독하는 객체를 **옵저버(observer)**라고 한다.
	+ 상태란 객체가 가지고 있는 데이터 값이다.
	+ 일대다(one-to-many)의존성을 가진다.

![observer_pattern](https://nobbaggu.github.io/images/designpattern/observerpattern/observer_pattern.png){: width="50%" height="50%"}

![observer_pattern_class_diagram](https://nobbaggu.github.io/images/designpattern/observerpattern/observer_pattern_class_diagram.png){: width="50%" height="50%"}

<br>

+ 신문사와 정기구독자 비유
	+ 신문사는 주제
	+ 구독자는 옵저버
	+ 어떤 사람이 신문 구독을 원하면 신문사에 구독요청을 한다.
		+ 구독 객체가 주제 객체의 `registerObserver(Observer observer)` 호출
	+ 신문이 더 이상 받기 싫어지면 구독 해지요청을 한다.
		+ 구독 객체가 주제 객체의 `removeObserver(Observer observer)` 호출 
	+ 신문사는 자신의 구독자들에게 정기적으로 신문을 보낸다.
		+ 주제 객체가 자신의 `notify Observers(List<Observer> observers)` 실행
	+ 구독자는 가만히 있으면 알아서 신문을 전달받는다.
		+ 주제 객체에서 구독 객체의 `update(Data data)` 실행
	
<br>

## 2. 옵저버 패턴의 특징 ##
----

+ 옵저버 패턴은 주제와 옵저버 사이에 느슨한 결합이 있다.
	+ '느슨한' 결합이란 두 객체가 상호작용은 하지만 서로에 대해 잘 모른다는 의미
	+ `Observer` 인터페이스만 구현한다면 어떠한 형식의 객체도 옵저버가 될 수 있다.
		+ 주제는 옵저버를 전혀 신경쓸 필요 없이 `update()`만 해주면 된다.

<br>

## 3. 예제 ##
----

//신문사 인터페이스
~~~ java
public interface NewsCompany {

    void registerObserver(Subscriber subscriber);

    void removeObserver(Subscriber subscriber);

    void notifyObservers();

    //뉴스 취재 메소드
    void gotNews(String str);
}
~~~

<br>

//구독자 인터페이스
~~~ java
import java.util.List;

public interface Subscriber {
    void update(String news);
}
~~~

<br>

//조선일보 클래스
~~~ java
public class Chosun implements NewsCompany {

    List<Subscriber> subscribers;

    String news;

    public Chosun() {
        subscribers = new ArrayList<>();
    }

    @Override
    public void registerObserver(Subscriber subscriber) {
        subscribers.add(subscriber);
    }

    @Override
    public void removeObserver(Subscriber subscriber) {
        subscribers.remove(subscriber);
    }

    @Override
    public void notifyObservers() {
        for(int i = 0; i < subscribers.size(); i++) {
            subscribers.get(i).update(news);
        }
    }

    @Override
    public void gotNews(String str) {
        this.news = str;
        notifyObservers();
    }
}
~~~

<br>

//John 클래스
~~~ java
public class John implements Subscriber {

    @Override
    public void update(String news) {
        read(news);
    }

    private void read(String news) {
        System.out.println("John: " + news);
    }
}
~~~

<br>

//Andy 클래스
~~~ java
public class Andy implements Subscriber {
    @Override
    public void update(String news) {
        read(news);
    }

    private void read(String news) {
        System.out.println("Andy thorws away news(" + news + ")");
    }
}
~~~

<br>

//Main
~~~ java
public class Main {
    public static void main(String[] args) {
        NewsCompany chosun = new Chosun();
        Subscriber subscriber1 = new John();
        Subscriber subscriber2 = new Andy();

        chosun.registerObserver(subscriber1);
        chosun.registerObserver(subscriber2);

        chosun.gotNews("내일 비온다");
        chosun.gotNews("주식 폭락");
        chosun.gotNews("새로운 공룡화석 발견");
    }
}
~~~

<br>

+ 주제는 신문사이며 모든 신문사는 `NewsCompany` 인터페이스를 구현해야한다.

+ 모든 구독자는 `Subscriber` 인터페이스를 구현해야한다.

+ 신문사는 간단하게 조선일보 1개만 만들었다.

+ 사람은 John과 Andy 2명이다.
	
+ 실행부에서 존과 앤디 객체를 만들어서 조선일보에 구독자 등록을 했다.

+ 조선일보는 새로운 뉴스를 얻으면 자동으로 `notifyObservers()`를 실행시켜 구독자에게 소식을 전달한다.
	+ John은 업데이트가 되면 신문을 읽는다.
	+ Andy는 신문을 읽지않고 그냥 버린다.
	
실행시켜보면 아래와 같은 결과가 출력된다.

~~~ bash
John: 내일 비온다
Andy thorws away news(내일 비온다)
John: 주식 폭락
Andy thorws away news(주식 폭락)
John: 새로운 공룡화석 발견
Andy thorws away news(새로운 공룡화석 발견)

Process finished with exit code 0
~~~

<br>

## 4. 데이터 획득 방식 ##
----

+ 옵저버 패턴에는 강제성이 있다.
	+ 데이터들 중 필요한 데이터만 가져오지 못한다.
	
+ 지금까지 본 내용에서는 푸시(push) 방식의 데이터 전달을 사용했다.

+ 풀(pull) 방식을 사용하면 옵저버가 원하는 데이터만 가져올 수 있다.
	+ 자바에 내장된 `Observerble`는 게터(getter)를 제공하여 필요한 데이터만 취할 수 있다.

+ `Observable` 클래스
	+ 자바의 내장 주제 클래스
	+ 상속받아 사용
	+ 인터페이스에 맞춰 프로그래밍하라는 객체지향 디자인 원칙에 위배된다.
	+ 이미 다른것을 상속받는다면 `Observable`의 기능을 추가할 수 없다.(Java는 다중상속을 지원하지 않는다...)
	+ getter를 제공
	+ `setChanged()`를 호출하면 `changed`가 true로 바뀐다.
		+ `setChanged()`는 protected이다. 
		+ 따라서 직계 자식 클래스만 호출할 수 있다. 즉, `Observerble`를 인스턴스 변수로 만들고 `setChanged()`를 호출할 수 없다.
		+ 결론적으로 **재사용성에 제약**이 존재한다. 직접 구현해 쓰는게 보통 낫다.
	+ `notifyObservers()` 혹은 `notifyObservers(Object arg)`를 호출하여 옵저버에 알림
		+ notify전에 반드시 `setChanged()`를 호출해서 플래그를 true로 바꿔놓아야한다.
		+ `notifyObservers()` 내부 코드를 보면 `changed`의 값에 따라 `update()`를 호출할지 결정한다.
		
+ `Observer` 인터페이스
	+ 자바의 내장 옵저버 인터페이스
	+ 구현해서 사용
	+ `update(Observable o, Object arg)`를 통해 업데이트
	+ 업데이트할 때 `Observerble`이 전달되므로 getter를 호출하면 필요한 데이터만 사용할 수 있다.
	+ arg에 데이터를 실어서 전달할 수 있다.
	
~~~ java
public class Chosun extends Observable {

    public void gotNews(String str) {
        setChanged(); //changed 플래그 true로 설정
        notifyObservers(str);
    }
}
~~~

<br>

~~~ java
public class John implements Observer {
    @Override
    public void update(Observable o, Object arg) {
        System.out.println((String) arg);
    }
}
~~~

<br>

~~~ java
public class Main {
    public static void main(String[] args) {
        Chosun chosun = new Chosun();
        John john = new John();

        chosun.addObserver(john);

        chosun.gotNews("내일 비온다");
        chosun.gotNews("주식 폭락");
        chosun.gotNews("새로운 공룡화석 발견");
    }
}
~~~

<br>

## 5. 정리 및 생각##
----

옵저버 패턴은 객체 사이에 데이터를 1:다 형태로 전달하기 위한 디자인 패턴이다. 내가 코딩 한달 했을때 이러한 기능을 만든다면 명시적으로 주기를 설정하고 한 번씩 주제 객체의 데이터를 `get()`해서 같으면 버리고 다르면 업데이트 하는 방식을 생각해볼 수 있을 것이다. 그에 반해 옵저버 패턴은 성공한 디자인패턴답게 효율적이다. 이벤트가 발생했을때만 구독자들에게 그것을 알려주기 때문이다. 그리고 안드로이드와 같은 UI 버튼과 리스너도 옵저버 패턴을 사용한다는 것을 처음 알았다. 그러나 안드로이드에서는 클릭 리스너를 1개만 등록할 수 있는걸로 아는데 이것은 1:1 옵저버 패턴이라 할 수 있을까?

+ 옵저버 패턴에서 중요한 디자인 원칙은 
	+ **상호작용하는 객체 사이의 결합도는 가능한 '느슨하게' 해야한다**