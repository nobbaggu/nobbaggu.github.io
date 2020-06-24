---
title: (디자인패턴) 7 - 어댑터 패턴(Adapter Pattern)
date: 2020-06-24T12:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 어댑터
  - adapter
  - command
  - 패턴
---

##1. 어댑터 패턴이란? ##
----

+ 문제 상황을 가정해보자.
	+ 업체에서 어떠한 클래스를 제공했다.
	+ 우리 개발사에서는 비슷한 기능을 하는 기존 시스템이 존재한다.
	+ 그런데 메소드 이름, 클래스 형태 등이 달라서 그대로 업체에서 제공한 클래스를 적용할 수 없는 상황이다.
	+ 그렇다고 기존 시스템을 변경하거나 업체에서 공급한 클래스를 변경할 수도 없는 상황이다.
	
+ 비슷한 상황
	+ 유럽에는 전기 콘센트 모양이 일자형태로 되어있다.
	+ 유럽에서도 당연히 전기를 공급한다.
	+ 그러나 내가 한국에서 가져간 스마트폰 충전기 플러그 모양과 콘센트 모양이 달라서 그대로 사용할 수 없는 상황이다.
	
+ 위에서 말한 두 상황은 동일하다.
	+ 아래 그림은 유럽식 전기 콘센트를 국제 표준 AC 플러그로 바꿔주는 어댑터 제품이다.
	
![ac_adapter](https://nobbaggu.github.io/images/designpattern/adapterpattern/ac_adapter.jpg){: width="50%" height="50%"}

<br>

+ 객체지향 어댑터 패턴에서 말하는 어댑터란 위 그림의 어댑터와 동일하다.
	+ 어떤 인터페이스를 사용하기 위해 클라이언트 단에서 요구하는 형태로 바꿔주는 역할을 한다.
	+ 이러한 일을 하기 위한 어댑터 클래스를 만들어야한다.
	
![adapter_pattern](https://nobbaggu.github.io/images/designpattern/adapterpattern/adapter_pattern.jpg){: width="50%" height="50%"}

<br>

##2. 예제 코드 ##
----

~~~ java
public interface VideoPlayer {
    void play(String content);
}
~~~

~~~ java
public class SamsungVideoPlayer implements VideoPlayer {
    @Override
    public void play(String content) {
        System.out.println(content);
    }
}
~~~

~~~ java
public interface AudioPlayer {
    void play(String content);
}
~~~

~~~ java
public class SonyAudioPlayer implements AudioPlayer {
    @Override
    public void play(String content) {
        System.out.println(content);
    }
}
~~~

<br>

+ 여기까지는 일반적인 상황이다.
	+ `VideoPlayer` 인터페이스와 구현 클래스인 `SamsungVideoPlayer`
	+ `AudioPlayer` 인터페이스와 구현 클래스인 `SonyAudioPlayer`
	
+ 그런데 클라이언트 코드에서는 비디오 플레이어만 사용한다.
	+ 오디오 플레이어는 사용하지 않는다.
	+ 그런데 노래를 틀고싶다.
	+ 클라이언트에서 타겟 인터페이스(비디오 플레이어)를 사용해서 어댑티(오디오 플레이어)를 사용하고 싶다.
	+ 그래서 아래와 같이 두 개를 연결시켜주는 어댑터를 만들었다.
	
<br>

~~~ java
public class AudioAdapter implements VideoPlayer {
    AudioPlayer audioPlayer;

    public AudioAdapter(AudioPlayer audioPlayer) {
        this.audioPlayer = audioPlayer;
    }

    @Override
    public void play(String content) {
        audioPlayer.play(content);
    }
}
~~~

<br>

+ 이로써 비디오 플레이어 인터페이스를 사용해서 실질적인 오디오 플레이어를 사용할 수 있게 되었다.

+ 어댑터(adapter)
	+ 타겟 인터페이스를 구현한다.
	+ 타겟 인터페이스란 우리가 사용하고자 하는 형태이다. 즉, 위 예제에서는 `VideoPlayer`이다.

+ 어댑티(adaptee)
	+ 어댑터를 가운데 두고 타겟 인터페이스 형태로 변환되어 사용되는 객체
	+ 직접 호출할 수 없어 어댑터로 변환하여 호출되어야 하는 클래스 또는 객체
	+ 위 예제에서는 `AudioPlayer`가 어댑티 이다.
	
+ 어댑터 클래스 안에는 어댑티 클래스가 포함된다.
	+ 위 예제에서는 어댑터 생성자의 매개변수로 전달해주었다.
	+ 어댑터의 메소드를 호출하면 실질적으로 어댑티의 메소드를 호출시켜준다.

## 3. 정리 ##
----

+ 어댑티 코드를 타겟 인터페이스에 맞춰서 고치려면 시스템이 방대할수록 엄청 많은 수정 작업이 필요하다.
	+ 수정에 따르는 새로운 버그나 부작용의 위험이 따른다.
	+ 어댑터 패턴을 사용하면 모든 변경사항을 캡슐화시킨 클래스 한 개만 만들면 된다.
	
+ 일반적으로 어댑터 하나는 하나의 클래스만 변환시킨다.
	+ 사실 여러 클래스의 호출을 편리하게 할 수 있게 여러 어댑티를 감싸는 패턴에는 퍼사드(facade) 패턴이라는 디자인 패턴이 있다.
	
+ 어댑터의 목적은 서로 호환되지 않는 인터페이스를 변환하여 클라이언트에서 편하게 활용할 수 있도록 해주는 것이다.

![class_diagram](https://nobbaggu.github.io/images/designpattern/adapterpattern/class_diagram.jpg){: width="50%" height="50%"}

