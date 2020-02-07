---
title: (안드로이드) 18. 액티비티의 수명주기(LifeCycle)
date: 2019-05-01T11:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 액티비티
  - activity
  - 수명주기
  - lifecycle
  - onstart
  - onrestart
  - onpause
  - onstop
  - onresume
  - ondestroy
  - oncreate
  - SharedPreferences
---

액티비티는 액티비티 스택(Activity Stack)이라는 것으로 관리된다. 새로운 액티비티를 띄우면 이전의 액티비티는 액티비티 스택에 저장된다. 그리고 가장 상위의 액티비티가 없어지면 액티비티 스택에서 가장 상위의 액티비티가 다시 나타나는 구조이다.

![1](/images/android/18/1.jpg){: width="40%" height="40%"}

결과적으로 액티비티의 생성, 실행, 중지, 그리고 해제되는 과정까지의 상태정보는 시스템이 관리한다. 지금까지 보았던 onCreate() 메소드는 액티비티가 생성될 때 자동으로 호출되는 메소드이다. 그런데 생성 뿐만이 아니라 실행, 중지, 해제등의 상태로 들어서면 자동으로 호출되는 메소드들이 있다.


|메소드|호출|
|---|---|
|**onCreate()**|액티비티 생성 시 호출|
|**onStart()**|액티비티가 화면에 보이기 바로 전에 호출|
|**onResume()**|액티비티가 사용자와 상호작용하기 바로 전에 호출|
|**onRestart()**|액티비티가 중지 되었다가 재시작 되기 직전에 호출. 다음에는 항상 onStart()가 호출됨|
|**onPause()**|다른 액티비티 시작할 때 호출. 화면에 보이나 상위 액티비티 때문에 포커스는 받지못함. 상태 정보가 저장됨|
|**onStop()**|액티비티를 중지시키고 화면에서 내릴 때 호출|
|**onDestroy()**|액티비티 메모리에서 해제 시 호출. finish()메소드를 호출할 때|

![2](/images/android/18/2.png){: width="60%" height="60%"}

위의 생명주기 메소드들은 액티비티의 상태변경에 따라 시스템에 의해 자동으로 호출되는데, 이러한 메소드를 콜백 메소드(Callback method)라고 한다.

앱을 키면 onCreate() - onStart() - onResume() 순서대로 메소드가 호출된다. 그리고 다른 액티비티를 띄울때는 onPause() - onStop() 순서로 메소드가 호출된다. 액티비티가 종료될 때는 onDestroy() 메소드가 호출된다.

액티비티의 생명주기를 확인하기 위한 액티비티를 만들어본다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="메뉴화면 띄우기"
        android:textSize="24dp"
        android:layout_gravity="center"
        android:layout_marginTop="300dp"/>

    <Button
        android:id="@+id/buttonFinish"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="종료하기"
        android:textSize="24dp"
        android:layout_gravity="center"
        android:layout_marginTop="20dp"/>


</LinearLayout>
~~~

~~~ java
package io.swnomad.samplelifecycle;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Toast.makeText(this, "onCreate() 호출", Toast.LENGTH_LONG).show();

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(getApplicationContext(), MenuActivity.class); //메뉴 액티비티를 띄우기 위한 인텐트 생성
                startActivity(intent);//액티비티 띄우기
            }
        });

        Button buttonFinish = findViewById(R.id.buttonFinish);
        buttonFinish.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                finish(); //액티비티 종료
            }
        });
    }

    // 생명주기 메소드가 호출되었는지 알아보기 위해 토스트 메시지 띄우도록 재정의 하기
    @Override
    protected void onStart() {
        super.onStart();
        Toast.makeText(this, "onStart() 호출", Toast.LENGTH_LONG).show();
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        Toast.makeText(this, "onRestart() 호출", Toast.LENGTH_LONG).show();
    }

    @Override
    protected void onStop() {
        super.onStop();
        Toast.makeText(this, "onStop() 호출", Toast.LENGTH_LONG).show();
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        Toast.makeText(this, "onDestroy() 호출", Toast.LENGTH_LONG).show();
    }

    @Override
    protected void onPause() {
        super.onPause();
        Toast.makeText(this, "onPause() 호출", Toast.LENGTH_LONG).show();
    }

    @Override
    protected void onResume() {
        super.onResume();
        Toast.makeText(this, "onResume() 호출", Toast.LENGTH_LONG).show();
    }
}
~~~

MenuActivity 액티비티를 하나 만들고, 첫 번째 버튼을 누르면 MenuActivity 액티비티를 띄운다. 두 번째 버튼을 클릭하면 액티비티를 종료한다. 앱을 실행하여 실행해보면 생명주기 메소드가 호출될 때 띄우는 토스트 메시지를 확인할 수 있다.

특히 onResume() 메소드와 onPause() 메소드는 화면이 보일 때와 보이지 않을 때 항상 호출된다. 따라서 데이터의 저장과 복원을 위해서도 아주 중요한 메소드가 된다. 예를 들어 게임을 하다가 전화가 오면 onPause() 메소드가 항상 먼저 호출된다. onPause() 메소드 안에서 데이터를 저장하면, 전화를 끊고 다시 게임 화면으로 돌아올 때 호출되는 onResume() 메소드 안에서 데이터를 복원하는 코드를 구현하면 된다.

입력상자에 입력 된 내용이 앱을 껐다 켜도 다시 복원되도록 하는 앱을 만들어보자.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <EditText
        android:id="@+id/nameInput"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="내용을 입력하세요"
        android:textSize="24dp"
        android:layout_gravity="center"
        android:layout_marginTop="300dp"/>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="종료하기"
        android:textSize="24dp"
        android:layout_gravity="center"
        android:layout_marginTop="20dp"/>


</LinearLayout>
~~~

버튼을 누르면 애플리케이션이 종료된다.

~~~ java
package io.swnomad.samplelifecycle;

import android.app.Activity;
import android.content.Intent;
import android.content.SharedPreferences;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    EditText nameInput;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        nameInput = findViewById(R.id.nameInput);
        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                finish(); // 버튼을 누르면 액티비티 종료
            }
        });
    }

    @Override
    protected void onPause() {
        super.onPause();
        Toast.makeText(this, "onPause() 호출됨", Toast.LENGTH_LONG).show();
        saveState(); // 액티비티 중지 전 상태 저장
    }

    @Override
    protected void onResume() {
        super.onResume();
        Toast.makeText(this, "onResume() 호출됨", Toast.LENGTH_LONG).show();
        restoreState(); // 액티비티 재시작 전 상태 복원
    }

    protected void saveState(){
        /* 1. "pref"라는 이름의 저장소 생성
           2. "pref" 저장소의 에디터 생성
           3. 에디터를 통해 key:"name", value: 입력상자의 문자열 데이터 저장
           4. 에디터 제출*/
        SharedPreferences pref = getSharedPreferences("pref", Activity.MODE_PRIVATE);
        SharedPreferences.Editor editor = pref.edit();
        editor.putString("name", nameInput.getText().toString());
        editor.commit();
    }
    protected void restoreState(){
        /* 1. "pref" 저장소 참조
           2. 저장소가 비어있지 않고 key가 "name"인 데이터가 있으면
           3. "name" key로 문자열 데이터 얻어와 저장
           4. 얻어온 데이터를 입력상자의 텍스트로 지정 */
        SharedPreferences pref = getSharedPreferences("pref", Activity.MODE_PRIVATE);
        if(pref!=null && pref.contains("name")){
            String name = pref.getString("name", "");
            nameInput.setText(name);
        }
    }

    // 저장소 비우기
    protected void ClearMyPrefs(){
        SharedPreferences pref = getSharedPreferences("pref", Activity.MODE_PRIVATE);
        SharedPreferences.Editor editor = pref.edit();
        editor.clear();
        editor.commit();
    }
}
~~~

앱에서 간단한 데이터를 저장 및 복원할 때는 SharedPreferences 클래스를 사용하면 된다. SharedPreferences는 앱 내부에 파일을 하나 만들어 읽고 쓴다.

![3](/images/android/18/3.jpg){: width="30%" height="30%"}
![4](/images/android/18/4.jpg){: width="30%" height="30%"}
![5](/images/android/18/5.jpg){: width="30%" height="30%"}

버튼을 눌러 앱을 종료했다가 다시 실행했을 때 입력상자의 내용이 복원되어 있는것을 확인할 수 있다.