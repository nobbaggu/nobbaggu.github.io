---
title: (안드로이드) 26. 프로그레스바(ProgressBar)
date: 2019-05-07T20:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 프로그레스바
  - progressbar
---

프로그레스바(ProgressBar)란 사용자에게 어떠한 작업이 진행중이라는 것을 알려줄 때 사용한다. 추가적으로 작업의 진행 정도까지 알려줄 수 있다.

|속성|설명|
|--|--|
|막대 모양|작업의 진행 정도를 알려줌|
|원 모양|작업이 진행 중임을 알려줌|

XML 레이아웃에서 <ProgressBar> 태그로 추가할 수 있다. max 속성은 프로그레스바 값의 최대치이며 progress 속성은 현재 값을 의미한다. 그러나 진행률은 계속해서 변하는 값이므로 Java 코드에서 아래 두 메소드를 사용해서 업데이트 해주어야한다.

~~~ java
void setPogress(int progress)
void incrementProgressBy(int diff)
~~~

실제 예제를 보면서 프로그레스바에 대해서 이해해보자.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <ProgressBar
        style="?android:attr/progressBarStyleHorizontal"
        android:id="@+id/progressBar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:indeterminateOnly="false"
        android:minHeight="20dp"
        android:minWidth="20dp"
        android:max="100"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="start"
            android:textSize="15dp"
            android:text="0%"/>

        <TextView
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:gravity="end"
            android:textSize="15dp"
            android:text="100%"/>

    </LinearLayout>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="증가하기"
        android:textSize="20dp"
        android:layout_marginTop="150dp"/>

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="제출하기"
        android:textSize="20dp"
        android:layout_marginTop="50dp"/>

</LinearLayout>
~~~

프로그레스바를 하나 만들고 버튼을 두 개 만들었다. "증가하기" 버튼은 프로그레스바의 진행률을 올리기위해서 사용할 것이고 "제출하기" 버튼은 원형 프로그레스 대화상자를 띄우기 위해 사용할 것이다.

~~~ java
package io.swnomad.sampleprogressbar;

import android.app.ProgressDialog;
import android.content.DialogInterface;
import android.os.Handler;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.ProgressBar;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    ProgressBar progressBar; //막대 프로그레스바
    ProgressDialog dialog; //원형 프로그레스바

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        progressBar = findViewById(R.id.progressBar);
        progressBar.setProgress(50); //프로그레스바 초기값 50

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                progressBar.incrementProgressBy(10); //프로그레스바 진행률 10 증가
            }
        });

        Button button2 = findViewById(R.id.button2);
        button2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                dialog = new ProgressDialog(MainActivity.this); //프로그레스 대화상자 객체 생성
                dialog.setProgressStyle(ProgressDialog.STYLE_SPINNER); //프로그레스 대화상자 스타일 원형으로 설정
                dialog.setMessage("제출 중입니다."); //프로그레스 대화상자 메시지 설정
                dialog.show(); //프로그레스 대화상자 띄우기

                /* 안드로이드 OS 핸들러 객체 사용하여 3초 시간지연 후 대화상자 닫기*/
                Handler handler = new Handler();
                handler.postDelayed(new Runnable(){
                    @Override
                    public void run() {
                        dialog.dismiss(); // 3초 시간지연 후 프로그레스 대화상자 닫기
                        Toast.makeText(MainActivity.this, "제출되었습니다.", Toast.LENGTH_LONG).show();
                    }
                }, 3000);
            }
        });
    }
}
~~~

버튼1을 누르면 incrementProgressBy() 메소드를 사용하여 프로그레스바 진행률을 10 증가시킨다. 버튼2를 클릭하면 프로그레스 대화상자 객체를 생성하고 띄운 후 3초 이후에 닫는다. 앱을 실행시키면 아래 화면을 확인할 수 있다.

![1](/images/android/26/1.jpg){: width="30%" height="30%"}
![2](/images/android/26/2.jpg){: width="30%" height="30%"}
![3](/images/android/26/3.jpg){: width="30%" height="30%"}