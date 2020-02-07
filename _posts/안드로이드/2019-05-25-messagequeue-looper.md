---
title: (안드로이드) 47. 메시지 큐(Message Queue)와 루퍼(Looper)
date: 2019-05-25T13:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 스레드
  - thread
  - 루퍼
  - looper
  - sendMessage
  - obtain
  - handleMessage
  - Looper.prepare
  - Looper.loop
---

이전 포스팅에서는 스레드와 핸들러, 메시지 큐에 대해 공부했었다. 그리고 서브 스레드에서 메인 스레드로 메시지나 러너블 객체를 전달해서 UI 구성요소를 변경하는 방법을 알았다. 그런데 역으로 메인 스레드에서도 서브 스레드로 메시지나 러너블 객체를 전달할 수 있다.

단, 다른 점은 서브 스레드에서 핸들러를 사용하기 위해서는 루퍼를 따로 생성해주어고 실행시켜 주어야 한다는 것이다. 메인 스레드에서는 이런 과정이 생략되더라도 자동으로 루퍼가 생성되고 메시지큐를 모니터링하면서 새로운 메시지나 러너블 객체가 전달되면 그것을 가져와서 실행했었다.

루퍼는 다음의 코드 두 줄만으로 생성할 수 있다.

~~~ java
Looper.prepare();
Looper.loop();
~~~

이번에는 단순히 서브 스레드에서 루퍼를 만들어 메인 스레드와 서브 스레드간에 메시지를 주고받는 간단한 예제를 하나 보면서 루퍼를 생성하는 방법을 알아보겠다.


<br>

~~~ xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >

    <LinearLayout
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dp"
        >
        <TextView
            android:id="@+id/textView1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="메인 스레드에서 받은 데이터 : "
            android:textSize="18dp"
            />
        <EditText
            android:id="@+id/editText1"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Hello"
            android:textSize="18dp"
            />
    </LinearLayout>
    <LinearLayout
        android:orientation="horizontal"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dp"
        >
        <TextView
            android:id="@+id/textView2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="스레드 #1 에서 받은 데이터 : "
            android:textSize="18dp"
            />
        <EditText
            android:id="@+id/editText2"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text=""
            android:textSize="18dp"
            />
    </LinearLayout>
    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_marginTop="20dp"
        android:text="시작하기"
        android:textSize="18dp"
        />

</LinearLayout>
~~~

버튼을 클릭하면 입력상자1의 텍스트를 가져와서 메인 스레드에서 서브 스레드로 메시지 객체에 담아서 전송을 한다. 그리고 서브 스레드에서는 이 메시지 객체에 " from SubThread"라는 문자열을 덧붙여 다시 메인스레드로 전송한다. 그러면 메인스레드에서는 입력상자2의 텍스트를 설정하도록 할 것이다.

<br>

~~~ java
public class MainActivity extends AppCompatActivity {

    EditText editText1;
    EditText editText2;

    Handler handler;
    Handler subHandler;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        editText1 = findViewById(R.id.editText1);
        editText2 = findViewById(R.id.editText2);

        /* 메인 스레드 핸들러 객체 생성
        * 메시지를 받으면 입려상자2의 텍스트로 설정*/
        handler = new Handler(){
            @Override
            public void handleMessage(Message msg) {
                String str = msg.obj.toString();
                editText2.setText(str);
            }
        };

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                /* 메인 스레드에서 서브 스레드로 메시지 보내기 */
                Message msg = Message.obtain();
                msg.obj = editText1.getText();
                subHandler.sendMessage(msg);
            }
        });

        Thread subThread = new SubThread(); //서브스레드 생성
        subThread.start();//서브스레드 시작
    }

    /* 스레드 상송받아 서브스레드 클래스 정의 */
    class SubThread extends Thread{

        /* 서브스레드 생성될 때 핸들러 생성*/
        public SubThread() {
            subHandler = new Handler(){
                @Override
                public void handleMessage(Message msg) {
                    /*  메시지 받자마자 입력상자1 텍스트로 설정
                     * 이어서 "Mike!!" 문자열 덧붙여서 메인 스레드로 전송*/
                    Message resultMsg = Message.obtain();
                    resultMsg.obj = msg.obj + " from SubThread";
                    handler.sendMessage(resultMsg);
                }
            };
        }

        @Override
        public void run() {
            Looper.prepare(); //루퍼 생성
            Looper.loop(); //루퍼 돌리기
        }
    }
}
~~~

이 코드에서 봐야할 건 `Looper.prepare()` 부분과 `Looper.loop()` 메소드 부분이다. 이 메소드를 통해 루퍼를 생성하고 실행시켜주지 않으면 서브 스레드의 핸들러와 메시지큐를 사용할 수 없다.

앱을 실행하고 입력상자1에 내용을 적어넣고 버튼을 클릭하면 입력상자2에 " from SubThread"라는 문자열이 덧붙여져서 설정될 것이다.

![1](/images/android/47/1.jpg){: width="30%" height="30%"}
![2](/images/android/47/1.jpg){: width="30%" height="30%"}