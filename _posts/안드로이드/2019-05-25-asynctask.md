---
title: (안드로이드) 48 - AsyncTask
date: 2019-05-25T14:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - asynctask
  - background
  - onpreexecute
  - onprogressupdate
  - doinbackground
  - onpostexecute
  - execute
  - thread
  - main
  - sub
  - 스레드
  - 백그라운드
---

서브 스레드에서 UI작업을 하기위해 핸들러를 사용하여 메인 스레드로 message나 Runnable 객체를 전달하는 방법이 있다. 하지만 이 방법은 코드가 복잡해지고 불편한 구석이 많다. 그런데 AsyncTask 클래스를 사용하면 UI작업과 백그라운드 작업을 동시에 할 수 있어 엄청 편리하다. AsyncTask객체는 메인스레드에서 처리할 작업과 서브스레드에서 처리할 작업을 서로 다른 메소드로 분리해놓았으므로 적당한 메소드에 처리할 작업의 코드를 작성하여 필요할 때 호출하여 실행하면 된다.

AsyncTask의 동작방식을 그림으로 보자.

![1](https://nobbaggu.github.io/images/android/48/1.png){: width="100%" height="100%"}

각 메소드에 대한 설명을 보자.

|메소드|설명|
|----|--|
|doInBackground()|서브 스레드에서 백그라운드 작업을 수행한다. AsyncTask객체의 execute() 메소드를 호출할 때 사용된 파라미터를 전달받는다.|
|onPreExecute()|백그라운드 작업 수행 중 호출. 초기화 작업에 사용된다.|
|onProgressUpdate()|백그라운드 작업 중간중간 UI객체에 접근할 때 사용하며 호출을 하기위해서는 백그라운드 작업 중 publishProgressed() 메소드를 호출하면 된다.|
|onPostExecute()|백그라운드 작업이 끝난 후 호출된다. 사용된 리소스의 해제 작업 등에 사용된다. 백그라운드 작업의 결과가 Result 타입의 파라미터로 전달된다.|

<br>

추가적으로 알아야 할 메소드가 2개 더 있다.
~~~ java
AsyncTask.cancel() //태스크가 실행되고 있는 중에 강제로 끝내기
publishProgress() // onProgressUpdate() 메소드 호출
~~~

<br>

실제로 사용하면서 이해해보자. 프로그레스바를 만들어 백그라운드에서 진행률을 증가시킬 것이고 이 값을 통해 메인 스레드에서는 프로그레스바의 진행상태를 업데이트하는 간단한 앱을 만들어볼 것이다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="진행"
        android:textSize="20dp"
        android:textColor="#ff000000"
        android:textStyle="bold"
        android:layout_margin="10dp"/>

   <ProgressBar
       android:id="@+id/progressBar"
       style="?android:attr/progressBarStyleHorizontal"
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:layout_margin="10dp"
       android:max="100"
       android:background="#22000000" />

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal"
        android:layout_marginTop="10dp"
        android:gravity="center_horizontal">

        <Button
            android:id="@+id/executeBtn"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="실행"
            android:textSize="24dp"
            android:layout_marginRight="10dp"
            android:textStyle="bold"/>

        <Button
            android:id="@+id/cancelBtn"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="취소"
            android:textSize="24dp"
            android:layout_marginLeft="10dp"
            android:textStyle="bold"/>

    </LinearLayout>


</LinearLayout>
~~~

텍스트뷰는 현재 진행 정도를 나타내기위한 것이다. 진행 버튼을 클릭하면 태스크 시작, 취소 버튼을 누르면 중도에 태스크를 취소하도록 할 것이다.

<br>

~~~ java
public class MainActivity extends AppCompatActivity {

    TextView textView;
    ProgressBar progressBar;
    Backgroundtask task;
    int value;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = findViewById(R.id.textView);
        progressBar = findViewById(R.id.progressBar);

        Button executeBtn = findViewById(R.id.executeBtn);
        Button cancelBtn = findViewById(R.id.cancelBtn);

        /* 실행 버튼을 클릭하여 새로운 태스크 생성 후 실행 */
        executeBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                task = new Backgroundtask();
                task.execute(100);
            }
        });

        /* 취소 버튼을 누르면 태스크 취소 */
        cancelBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                task.cancel(true);
            }
        });
    }

    /* AsyncTask 클래스 상속하여 백그라운드 태스크 클래스 정의 */
    class Backgroundtask extends AsyncTask<Integer, Integer, Integer>{

        /* 백그라운드 작업 이전에 프로그레스바 진행률 0으로 초기화 */
        @Override
        protected void onPreExecute() {
            value = 0;
            progressBar.setProgress(value);
        }

        /* 백그라운드 작업 정의
         * 1. value값 1 증가
          * 2. publishProgress() 호출 -> onProgressUpdate() 호출 -> UI 업데이트
          * 3. 100ms 간격 주기 */
        @Override
        protected Integer doInBackground(Integer... integers) {
            while(!isCancelled()){
                value++; //진행률 1 증가
                if(value>100){
                    break;
                }else{
                    // value값 파라미터로 전달하면서 onProgressUpdate() 메소드 호출
                    publishProgress(value);
                }

                try{
                    Thread.sleep(100); // 100ms 시간 지연
                }catch(Exception e){
                    e.printStackTrace();
                }
            }

            return value;
            /* 백그라운드 작업 완료 후 return 한 것은 onPostExecute() 메소드의 파라미터로 전달된다. */
        }

        /* 백그라운드 작업 중간에 프로그레스바 진행률 업데이트
         * 전달받은 값이 value 하나이므로 values[0]으로 value값 참조 */
        @Override
        protected void onProgressUpdate(Integer... values) {
            progressBar.setProgress(values[0].intValue());
            textView.setText("현재 진행률: " + values[0].toString());
        }

        /* 백그라운드 작업 끝난 이후 호출
         * result는 doInBackground() 메소드의 리턴값이다. */
        @Override
        protected void onPostExecute(Integer result) {
            progressBar.setProgress(result);
            textView.setText("완료!");
        }

        /* 취소 버튼 눌렸을 때 호출 */
        @Override
        protected void onCancelled() {
            progressBar.setProgress(0);
            textView.setText("취소됨");
        }
    }
}
~~~

먼저 AsyncTask 클래스를 상속하여 BackgroundTask 클래스를 정의하였다. 클래스를 정의할 때 제네릭 자료형을 3개 정의한다.
* 첫 번째 제네릭은 `doInBackground()` 메소드의 파라미터로 전달될 자료형이다.
* 두 번재 제네릭은 `onProgressUpdate()` 메소드의 파라미터로 전달될 자료형으로, `publishProgress()` 메소드를 호출하면서 파라미터로 전달하면 `onProgressUpdate()` 메소드가 호출되면서 파라미터로 전달된다.
* 세 번째 제네릭은 `onPostExecute()` 메소드의 파라미터로 전달될 자료형이다. `doInBackground()` 메소드가 종료되면서 이 자료형의 데이터를 return하면 `onPostExecute()` 메소드에서 파라미터로 전달받는다.

코드를 보면 백그라운드 작업을 하면서 100ms마다 한 번씩 value 값을 1씩 증가시키고 `publishProgress()` 메소드를 호출하면서 파라미터로 전달한다. 그러면 `onProgressUpdate()` 메소드가 호출되면서 프로그레스바와 텍스트뷰 UI에 접근하여 업데이트한다. 이 메소드의 코드는 메인 스레드에서 처리되므로 UI객체에 접근할 수 있는것이다.

그리고 `doInBackground()` 메소드가 끝이나면 value를 리턴한다. 이후 이 값이 `onPostExecute()` 메소드가 호출되면서 파라미터로 전달된다. 이어서 프로그레스바의 값으로 설정이 되고 텍스트뷰의 텍스트가 "완료!"로 설정된다.

진행 도중에 취소버튼을 누르면 `task.cancel()` 메소드가 호출되고 `isCancelled()` 메소드가 false를 반환하여 `doInBackground()` 메소드의 while문을 탈출한다. 이어서 자동으로 `onCancelled()` 메소드가 호출되어 프로그레스바의 값을 0으로 초기화시키고 텍스트뷰의 텍스트를 "취소됨"으로 설정한다.

앱을 실행하면 다음의 결과를 볼 수 있다.

![2](https://nobbaggu.github.io/images/android/48/2.jpg){: width="30%" height="30%"}
![3](https://nobbaggu.github.io/images/android/48/3.jpg){: width="30%" height="30%"}
![4](https://nobbaggu.github.io/images/android/48/4.jpg){: width="30%" height="30%"}