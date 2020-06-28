---
title: (디자인패턴) 12 - 스테이트 패턴(State Pattern)
date: 2020-06-28T18:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 스테이트
  - state
  - 패턴
  - pattern
---

## 1. 뽑기기계 코딩 ##
----

+ 아래 그림과 같은 방식으로 동작하는 뽑기 기계가 있다.

![gumball_machine_state](https://nobbaggu.github.io/images/designpattern/statepattern/gumball_machine_state.jpg){: width="50%" height="50%"}

+ '동전 있음' 상태
	+ 동전 반환을 하면 동전이 나오고 '동전 없음' 상태로 변한다.
	+ 손잡이를 돌리면 알맹이가 나오기 시작하고 '알맹이 판매(나오는중)' 상태로 변한다.
	+ 알맹이가 다 나오고 나서 알맹이가 남아있으면 '동전 없음'상태로, 없으면 '알맹이 매진' 상태로 변한다.
	
+ 나머지 상태일때도 다이어그램에 표시된 대로 동작한다.

+ 이제 위 상황을 코드로 작성하면 아래와 같다.

<br>

~~~ java
public class GumballMachine {
    private final static int SOLD_OUT = 0; //알맹이 매진
    private final static int NO_COIN = 1; //도전 없음
    private final static int HAS_COIN = 2; //도전 넣었음
    private final static int SOLDING = 3; //알맹이 나오는중

    private int state = SOLD_OUT; //초기 상태
    private int count = 0; //알맹이 갯수

    public GumballMachine(int count) {
        this.count = count;
        if(count > 0) {
            state = NO_COIN;
        }
    }

    //동전 넣기
    public void insertCoin() {
        if(state == HAS_COIN) {
            System.out.println("이미 코인이 있습니다.");
        } else if(state == NO_COIN) {
            System.out.println("코인을 넣었습니다.");
            state = HAS_COIN;
        } else if(state == SOLD_OUT) {
            System.out.println("상품 매진입니다.");
        } else if(state == SOLDING) {
            System.out.println("상품이 나오는 중입니다. 잠시 후 다시 넣어주세요.");
        }
    }

    //동전 반환
    public void ejectCoin() {
        //...
    }

    //손잡이 돌리기
    public void turnCrank() {
        //...
    }

    //알맹이 꺼내기
    public void dispense() {
        //...
    }
}
~~~

+ `if/else`가 떡칠 된다.

+ 다른 상태나 동작을 추가하면 코드 전 부분을 고쳐야 한다.

<br>

## 2. 스테이트 패턴 ##
----

+ 상태를 캡슐화 한다.
	+ `State` 인터페이스 작성 후 4가지 행위에 대한 메소드 추가
	+ 각 상태별로 `State` 구현하며 메소드 알맞게 정의
	+ 이렇게 하면 클라이언트 코드에서 `if/else`문을 없애고 `state.메소드()` 호출 한 줄만 필요하다.
	+ 캡슐화된 상태 객체에 행동을 위임
	
<br>

![gumballmachine_class_diagram](https://nobbaggu.github.io/images/designpattern/statepattern/gumballmachine_class_diagram.jpg){: width="50%" height="50%"}

<br>

+ 먼저 아래와 같이 `State` 인터페이스를 작성 후 각 상태가 이를 구현하도록 한다.

~~~ java
public interface State {
    void insertCoin();
    void ejectCoin();
    void turnCrank();
    void dispense();
}

public class HasCoinState implements State {

    GumballMachine gumballMachine;

    public HasCoinState(GumballMachine gumballMachine) {
        this.gumballMachine = gumballMachine;
    }

    @Override
    public void insertCoin() {
        System.out.println("이미 동전이 있습니다.");
    }

    @Override
    public void ejectCoin() {
        gumballMachine.setState(gumballMachine.noCoinState);
        System.out.println("동전이 반환되었습니다.");
    }

    @Override
    public void turnCrank() {
        gumballMachine.setState(gumballMachine.soldingState);
        System.out.println("알맹이가 나옵니다.");
    }

    @Override
    public void dispense() {
        gumballMachine.setCount(gumballMachine.getCount() - 1);
        System.out.println("알맹이를 꺼냈습니다.");
        if(gumballMachine.getCount() > 0) {
            gumballMachine.setState(gumballMachine.noCoinState);
        } else {
            gumballMachine.setState(gumballMachine.soldOutState);
        }
    }
}

public class NoCoinState implements State {
	//...
}

public class SoldingState implements State {
	//...
}

public class SoldOutState implements State {
	//...
}
~~~

<br>

+ 이제 `GumballMachine` 코드는 매우 단순해진다.

~~~ java
public class GumballMachine {
    private State state;

    State soldOutState;
    State soldingState;
    State noCoinState;
    State hasCoinState;

    private int count = 0; //알맹이 갯수

    public GumballMachine(int count) {

        soldOutState = new SoldOutState(this);
        soldingState = new SoldingState(this);
        noCoinState = new NoCoinState(this);
        hasCoinState = new HasCoinState(this);

        this.count = count;
        if(count > 0) {
            state = noCoinState;
        } else {
            state = soldOutState;
        }
    }

    /*state, count의 getter와 setter*/
    public State getState() {
        return state;
    }

    public void setState(State state) {
        this.state = state;
    }

    public int getCount() {
        return count;
    }

    public void setCount(int count) {
        this.count = count;
    }

    //동전 넣기
    public void insertCoin() {
        state.insertCoin();
    }

    //동전 반환
    public void ejectCoin() {
        state.ejectCoin();
    }

    //손잡이 돌리기
    public void turnCrank() {
        state.turnCrank();
        state.dispense();
    }

    //알맹이 꺼내기
    public void dispense() {
        state.dispense();
    }
}
~~~

<br>

+ 스테이트 패턴의 공식 정의
	+ 객체 내부 상태에 따라 변해야 하는 객체의 행동을 캡슐화하여 객체의 메소드가 바뀌는 것과 같은 결과를 만들어낼 수 있는 디자인 패턴

![class_diagram](https://nobbaggu.github.io/images/designpattern/statepattern/class_diagram.jpg){: width="50%" height="50%"}


## 3. 정리 ##
----

+ 스테이트 패턴을 적용함으로써 한 일
	+ 상태 변수를 캡슐화 시켰다.
	+ 캡슐화된 상태 객체가 행동을 위임받아서 처리한다.
	+ 지저분한 `if/else` 구조를 없애버렸다.
	+ 새로운 상태가 추가되면 새로운 클래스를 추가하면 된다.
		+ 물론 다른 상태로 영향을 받겠지만 `if/else`로 떡칠했을 때와는 비교도 안된다.
		
<br>

스트래티지(strategy) 패턴과 무엇이 다른가? 스트래티지 패턴에서도 행동을 캡슐화 하여 객체에 행동을 위임시켰다.

그러나 스트래티지 패턴에서는 클라이언트 코드에서 어떤 전략 객체를 사용할지를 직접 지정해 준다. 그러나 스테이트 패턴에서는 행동이 끝난 후 현재 상태가 알아서 업데이트 되고 이에 따라 다음 행동에서 어떤 객체가 행동할지가 정해진다. 클라이언트 측에서는 상태를 전혀 모른다. 행동만 하면 된다.

정리하자면 스트래티지 패턴은 유연성을 위해 상속보다는 캡슐화를 선택한 것이고, 스테이트 패턴은 수많은 조건문을 집어넣는 대신 캡슐화를 선택한 것이다.