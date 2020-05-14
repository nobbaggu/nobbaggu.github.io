---
title: (안드로이드) 46. 스레드(Thread)와 핸들러(Handler)
date: 2019-05-23T17:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 스레드
  - 메인
  - main thread
  - 핸들러
  - 메시지 큐
  - 러너블
  - thread
  - handler
  - message
  - queue
  - obtainMessage
  - sendMessage
  - run
  - Runnable
  - post
  - start
  - join
---

안드로이드는 기본적으로 표준 Java의 Thread를 그대로 사용할 수 있다. 그러나 안드로이드에서는 메인 스레드만 UI요소에 접근 가능하며, 서브 스레드에서 UI에 접근하려면 핸들러(Handler)와 메시지 큐(Queue)를 사용해야한다. 여기서부터 표준 Java와 안드로이드의 처리방식이 달라지기 시작한다.

<br>
## 1. 표준 Java 스레드 사용하기
***

먼저 안드로이드에서 표준 Java의 스레드를 사용 가능한지 화인해보자.

<br>

\<activity_main.xml\>
~~~ xml
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

    <TextView
        android:id="@+id/textView"
        android:layout_width="320dp"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_centerVertical="true"
        android:text="스레드에서 받은 값 : "
        android:textSize="18dp"/>

    <Button
        android:id="@+id/showBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerHorizontal="true"
        android:layout_gravity="center"
        android:layout_marginTop="10dp"
        android:text="보여주기"
        android:textSize="18dp"
        />

</RelativeLayout>
~~~

버튼을 클릭하면 스레드에서 받은 값을 읽어오도록 할 것이다.

<br>

\<MainActivity.java\>
~~~ java
public class MainActivity extends AppCompatActivity {

    int value = 0; //출력할 값
    private boolean isRun = false; //스레드의 실행여부를 위한 boolean 변수 선언

    TextView textView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = findViewById(R.id.textView);
        Button showBtn = findViewById(R.id.showBtn);

        showBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                textView.setText("스레드에서 받은 값 : " + value);
            }
        });

    }

    @Override
    protected void onResume() {
        super.onResume();

        /* 스레드 실행 */
        BackgroundThread thread = new BackgroundThread();
        isRun = true;
        thread.start();
    }

    @Override
    protected void onPause() {
        super.onPause();

        /* 스레드 중지 */
        isRun = false;
        value = 0; //value값 다시 0으로 초기화
    }

    class BackgroundThread extends Thread{

        @Override
        public void run() {
            while(isRun){ // isRun 변수가 true가 되면
                try{
                    Thread.sleep(1000); //1초 지연
                    value++; //value 값 1 증가
                }catch(Exception e){
                    e.printStackTrace();
                }
            }

            try{
                this.join(); // 스레드를 메인 스레드와 합치기
            }catch(Exception e){
                e.printStackTrace();
            }
        }
        /* isRun이 true인 동안은 계속해서 1초마다 value 값을 1씩 증가시킨다.
        * 그러다가 isRun이 false가 되면 스레드가 메인 스레드와 합쳐지고 끝이난다.*/

    }
}
~~~
버튼을 클릭하면 갱신된 value값을 가져와 텍스트뷰에 표시한다. value값을 증가시키는 작업은 새로운 Thread 클래스를 상속받아 새로운 스레드를 만들어서 처리하도록 했다. isRun변수가 false가 되면 `while()`문을 빠져나와서 스레드의 `join()` 메소드를 호출하여 스레드를 다시 메인 스레드와 합친다.

<br>

<br>
## 2. 핸들러(Handler)와 메시지 큐(Message Queue)
***

안드로이드 정책상 데드락(메모리 자원을 여러곳에서 동시에 참조하는 것)을 피하기 위해 애플리케이션의 UI 구성요소에는 메인 스레드만 접근할 수 있다. 그런데 메인 스레드가 아닌 하나의 서브스레드에서 UI 구성요소를 변경해야 하는 경우가 있을 수 있다. 이런 경우에 사용하는 것이 핸들러이다. 핸들러를 사용하면 서브스레드에서 메인 스레드로 UI 처리작업을 요청하는 메시지를 보낼 수 있다.

![2](https://nobbaggu.github.io/images/android/46/2.png){: width="100%" height="100%"}

<br>

위 사진은 핸들러를 통해 서브스레드에서 메인 스레드로 UI 처리작업을 요청하는 3단계를 보여준다.

> 1. `obtainMessage()` 메소드로 메인스레드의 메시지 객체 가져오기
> 2. 가져온 메시지에 정보를 넣고 `sendMessage()` 메소드로 다시 메인 스레드로 보내기
> 3. 메인 스레드가 `handleMessage()` 메소드로 메시지 처리하기

스레드 내부에서 생성자를 통해 핸들러 객체를 만들면 핸들러를 사용할 수 있게 된다. 핸들러는 메시지큐(Message Queue)와 루퍼(Looper) 없이는 아무 의미가 없다. 메시지큐는 메시지를 저장해두고 하나씩 꺼내 작업할 장소라고 생각하자. 이름에서 볼 수 있듯이 메시지큐는 FIFO(First In First Out)을 따르므로 먼저 들어온 메시지를 더 먼저 처리하게 된다. 루퍼(Looper)는 메시지큐를 돌면서 처리할 메시지가 있으면 하나씩 꺼내어 핸들러로 전달하는 역할을 한다.

<br>

아래 사진은 안드로이드 개발 공식 API 문서의 Handler 페이지 앞부분의 설명이다.

![1](https://nobbaggu.github.io/images/android/46/1.png){: width="100%" height="100%"}

<br>

핵심 내용은 이렇다.

> 1. 핸들러는 자신이 포함된 메시지큐의 메시지를 처리하거나 다른 스레드의 메시지큐로 메시지를 보낼 수 있다.
> 2. 핸들러객체는 생성자를 통해 생성된 스레드와 그 스레드의 메시지큐에 연결된다.

<br>

그러면 코드를 통해 서브 스레드에서 UI를 처리하는 과정을 확인해보자.

<br>

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center_vertical|center_horizontal">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="50dp"
        android:hint="무슨동물?"/>

</LinearLayout>
~~~

텍스트뷰가 하나 있다. 이 텍스트뷰의 내용을 메인 스레드가 아닌 서브 스레드를 통해서 설정하는 코드를 작성해보자.

<br>

~~~ java
public class MainActivity extends AppCompatActivity {

    TextView textView;

    final int MESSAGE_ID_CAT = 1;
    final int MESSAGE_ID_DOG = 2;

    Handler handler;
    Thread thread;

    boolean isRun;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = findViewById(R.id.textView);

        /* 핸들러 객체 생성
        * 메인스레드에서 생성되었으므로 메인스레드의 핸들러이다.*/
        handler = new Handler(){
            @Override
            public void handleMessage(Message msg) {
                switch(msg.what){

                    case MESSAGE_ID_CAT: //msg.what == MESSAGE_ID_CAT 인 경우
                        textView.setText(msg.obj.toString()); //텍스트뷰 내용 설정
                        break;

                    case MESSAGE_ID_DOG: //msg.what == MESSAGE_ID_DOG 인 경우
                        textView.setText(msg.obj.toString()); //텍스트뷰 내용 설정
                        break;
                }
            }
        };

        /* 스레드 객체 생성 */
        thread = new Thread(new Runnable() {
            @Override
            public void run() {
                try{

                    /* while문이 실행될 동안 3초마다 반복적으로 두 메시지 번갈아 보내기*/
                    while(isRun){
                        Thread.sleep(3000); // 3초간 시간 지연
                        Message msg = handler.obtainMessage(); //메인스레드 핸들러의 메시지 객체 가져오기
                        msg.what = MESSAGE_ID_CAT; // 메시지 아이디 설정
                        msg.obj = "고양이 입니다."; // 메시지 내용 설정
                        handler.sendMessage(msg); // 메인스레드 핸들러로 메시지 보내기

                        Thread.sleep(3000); //3초간 시간 지연
                        msg = handler.obtainMessage(); //메인스레드 핸들러의 메시지 객체 가져오기
                        msg.what = MESSAGE_ID_DOG; //메시지 아이디 설정
                        msg.obj = "개 입니다."; // 메시지 내용 설정
                        handler.sendMessage(msg); // 메인스레드 핸들러로 메시지 보내기
                    }

                }catch(Exception e){
                    e.printStackTrace();
                }
            }
        });
    }

    @Override
    protected void onResume() {
        super.onResume();
        isRun = true;
        thread.start(); //스레드 시작
    }

    @Override
    protected void onPause() {
        super.onPause();
        isRun = false; // 서브 스레드의 while문 탈출
        try{
            thread.join(); // 스레드를 메인 스레드와 합치기
        }catch(Exception e){
            e.printStackTrace();
        }
    }
}
~~~

먼저 핸들러 객체를 생성하고 `handleMessage()` 메소드를 재정의한다. 이 메소드는 스레드가 루퍼를 통해 메시지큐에서 메시지를 가져오면 처리하는 역할을 한다. `msg.what`을 통해 메시지 ID를 확인하여 텍스트뷰의 내용을 설정하게 된다.

그리고 스레드를 만들어 `run()` 메소드를 재정의한다. `isRun` 변수가 `true`일 동안 3초마다 번갈아가면서 두 개의 메시지를 번갈아 보내도록 했다. 여기서 `msg.what`과 `msg.obj`를 통해 메시지 ID와 내용을 넣어서 `handler.sendMessage()` 메소드를 통해 메인 스레드의 핸들러로 메시지를 보낸다. 그러면 메인스레드의 핸들러가 이 메시지를 확인해서 텍스트뷰의 내용을 확인한다.

앱을 실행하면 "고양이 입니다." 문자열과 "개 입니다." 문자열이 3초마다 번갈아가면서 출력된다.

![3](https://nobbaggu.github.io/images/android/46/3.jpg){: width="30%" height="30%"}
![4](https://nobbaggu.github.io/images/android/46/4.jpg){: width="30%" height="30%"}
![5](https://nobbaggu.github.io/images/android/46/5.jpg){: width="30%" height="30%"}

이 코드에서는 UI 요소에 접근하지 못하도록 되어있는 서브 스레드를 통해 UI를 변경하는 것을 확인해보았다.

<br>
## 3. 핸들러로 Runnable 객체 처리하기
***

위의 예제에서는 아래의 과정을 통해서 UI 요소를 변경했다.

> 1. 서브스레드에서 메인 스레드의 메시지 객체 참조
> 2. 메시지 객체에 메시지 저장 및 메인 스레드로 전송
> 3. 핸들러의 `handleMessage()` 메소드에서 메시지 정보 획득 및 처리(UI 업데이트)

그런데 위의 과정은 코드가 약간 복잡해지는 단점이 존재한다.

<br>
위의 단점을 해결하는 간단한 방법이 있다. 바로 Runnable 객체를 이용하는 것이다. 스레드의 핸들러는 메시지만 주고받고 처리할 수 있는것이 아니다. Runnable 객체도 처리할 수 있다. 즉, 메시지 큐에는 메시지 객체 이외에 Runnable 객체도 들어갈 수 있다. 서브 스레드에서 `handler.post()` 메소드를 사용하여 메인 스레드로 러너를 객체를 전송해주면 러너블 객체의 `run()` 메소드가 메인 스레드에서 실행된다. 따라서 UI 구성요소 역시 변경할 수 있다.

위에서 작성한 예제를 Runnable 객체를 사용한 것으로 바꾸려면 아래와 같이 하면된다.

~~~ java
public class MainActivity extends AppCompatActivity {

    TextView textView;

    Handler handler;
    Thread thread;

    boolean isRun;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = findViewById(R.id.textView);

        /* 핸들러 객체 생성
        * 메인스레드에서 생성되었으므로 메인스레드의 핸들러이다.*/
        handler = new Handler();

        /* 스레드 객체 생성 */
        thread = new Thread(new Runnable() {

            Runnable catRunnable = new Runnable(){ // Runnable 객체 생성
                @Override
                public void run() {
                    textView.setText("고양이 입니다.");
                }
            };

            Runnable dogRunnable = new Runnable(){ // Runable 객체 생성
                @Override
                public void run() {
                    textView.setText("개 입니다.");
                }
            };

            @Override
            public void run() {
                try{
                    /* while문이 실행될 동안 3초마다 반복적으로 두 메시지 번갈아 보내기*/
                    while(isRun){
                        Thread.sleep(3000); // 3초간 시간 지연
                        handler.post(catRunnable); // 메인 스레드로 catRunnable 객체 전송

                        Thread.sleep(3000); // 3초간 시간 지연
                        handler.post(dogRunnable); // 메인 스레드로 dogRunnable 객체 전송
                    }
                }catch(Exception e){
                    e.printStackTrace();
                }
            }
        });
    }

    @Override
    protected void onResume() {
        super.onResume();
        isRun = true;
        thread.start(); //스레드 시작
    }

    @Override
    protected void onPause() {
        super.onPause();
        isRun = false; // 서브 스레드의 while문 탈출
        try{
            thread.join(); // 스레드를 메인 스레드와 합치기
        }catch(Exception e){
            e.printStackTrace();
        }
    }
}
~~~

이로서 핸들러 객체를 생성하면서 `handleMessage()` 메소드를 재정의하여 메시지를 처리하는 부분이 사라졌다. 또한 `run()` 메소드 내부에서 메시지를 구성하여 보내는 코드도 사라졌다. 대신 Runnable 객체를 만들고 `run()` 메소드를 재정의하는 부분이 대신해서 생겼지만 오히려 한 눈에 서브 스레드에서 실행시키고자 하는 코드를 한 눈에 확인할 수 있어 가독성 측면에서 더 좋아진것 같은 느낌이다.