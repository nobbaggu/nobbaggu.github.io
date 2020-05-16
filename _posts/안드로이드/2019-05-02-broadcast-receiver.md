---
title: (안드로이드) 20 - 브로드캐스트 수신자(Broadcast Receiver)
date: 2019-05-02T19:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - broadcastreceiver
  - 브로드캐스트
  - 수신자
---

브로드캐스트 수신자(Broadcast Receiver)는 안드로이드의 4대 컴포넌트 중 하나이다. 서비스(Service)처럼 UI가 없고 백그라운드에서 동작한다. 브로드캐스트(Broadcast)가 '방송'이란 뜻이니만큼, 브로드캐스트 수신자는 안드로이드 전체에 한번에 뿌려지는 글로벌 이벤트(Global Event)를 받는 애플리케이션의 컴포넌트이다. 글로벌 이벤트로는 전화 수신이나 문자 수신등이 있다.

매니페스트 파일에 브로드캐스트 수신자를 추가하고, 인텐트필터를 통해 받고싶은 인텐트를 설정하면 등록해둔 인텐트가 안드로이드 전체에 뿌려졌을 때 애플리케이션의 브로드캐스트 수신자가 그 인텐트를 가져온다.

브로드캐스트 수신자는 onReceiver() 메소드를 재정의하여야 한다. 이 메소드는 브로드캐스트 수신자의 인텐트에 등록된 인텐트가 도착하면 자동으로 호출된다.

그러면 브로드캐스트 수신자에서 문자 메시지를 받도록 설정을 한 후 메시지가 오면 내용을 가져와 액티비티의 텍스트뷰에 설정하는 애플리케이션을 만들어보자.

~~~ java
package io.swnomad.samplereceiver;

import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.telephony.SmsMessage;
import android.widget.Toast;

import java.util.Date;

public class SmsReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {

        Bundle bundle = intent.getExtras(); //인텐트에서 부가데이터 가져오기

        //bundle에 들어있는 SMS 메시지를 parsing
        SmsMessage[] messages = parseSmsMessage(bundle);

        if(messages != null && messages.length >0){
            String sender = messages[0].getOriginatingAddress(); // 메시지 송신자
            String contents = messages[0].getMessageBody(); // 메시지 내용
            Date receivedDate = new Date(messages[0].getTimestampMillis()); // 수신 날짜
            String date = receivedDate.toString();

            //MainActivity로 보낼 인텐트 만들고 메시지 정보 부가데이터로 넣기
            Intent mIntent = new Intent(context.getApplicationContext(), MainActivity.class);
            mIntent.putExtra("sender", sender);
            mIntent.putExtra("contents", contents);
            mIntent.putExtra("date", date);

            //브로드캐스트 수신자는 화면이 없으므로 새로운 액티비티 화면 새로 띄우려면 인텐트에 FLAG_ACTIVITY_NEW_TASK 추가해야함
            mIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK); // -> onCreate() 메소드에서 처리

            //이미 액티비티가 메모리에 존재하면 새로 만들지 말고 사용 -> onNewIntent() 메소드에서 처리
            mIntent.addFlags(Intent.FLAG_ACTIVITY_SINGLE_TOP | Intent.FLAG_ACTIVITY_CLEAR_TOP);

            /*
            1. 만들어진 액티비티가 없으면 onCreate() 메소드에서 인텐트를 처리
            2. 이미 만들어진 액티비티가 있으면 onNewIntent() 메소드에서 인텐트를 처리
             */

            //액티비티 띄우기
            context.getApplicationContext().startActivity(mIntent);
        }
    }

    /* 1. Bundle 객체에서 PDU 포맷의 메시지 객체 가져오기
           2. 메시지 객체 갯수만큼 SmsMessage 배열 만들기
           3. createFromPdu() 메소드 사용해서 PDU 포맷의 메시지 SmsMessage 객체로 변환하여 SmsMessage 배열에 저장 */
    private SmsMessage[] parseSmsMessage(Bundle bundle){
        Object[] objs = (Object[]) bundle.get("pdus"); //PDU 포맷의 메시지 복원
        SmsMessage[] messages = new SmsMessage[objs.length];

        int smsCount = objs.length;
        for(int i=0; i<smsCount; i++){
            if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.M){ // API 23 이상
                String format = bundle.getString("format");
                messages[i] = SmsMessage.createFromPdu((byte[])objs[i], format);
            }else{
                messages[i] = SmsMessage.createFromPdu((byte[])objs[i]);
            }
        }

        return messages;
    }
}
~~~

브로드캐스트 수신자는 백그라운드에서 동작하다가 인텐트 필터에 등록해놓은 인텐트가 도착하면 onReceive() 메소드가 호출도므로 이 메소드안에 인텐트를 처리하는 코드를 추가하였다. 먼저 위험권한에 대한 부분은 무시하자. 먼저 인텐트로부터 번들 객체를 받는다. 그리고 번들 객체의 내용을 SmsMessage 자료형으로 고쳐야하는데, 이 코드는 parseSmsMessage(Bundle bundle) 메소드로 따로 만들었다.

parseSmsMessage() 메소드에 있는 코드는 안드로이드에서 API에서 정해둔 SMS를 확인하는 일반적인 코드이므로 한 번 만들어두면 나중에 필요할 때 마다 복사해서 사용할 수 있다. 딱히 외울 필요는 없고 createFromPdu 메소드로 Object 객체 형태에서 SmsMessage 객체로 포맷을 변환한다는 핵심내용만 알면 될 것 같다.

아무튼 문자 메시지를 파싱하여 발신자, 내용, 날짜를 읽어와서 이를 또 인텐트에 부가 데이터로 추가하여 MainActivity로 보낸다. 이 때 중요한 것이 있다. 브로드캐스트 수신자는 화면이 없는 컴포넌트이므로 액티비티 화면을 새로 띄우기 위해서는 FLAG_ACTIVITY_NEW_TASK 플래그를 추가해주어야한다. FLAG_ACTIVITY_SINGLE_TOP 플래그도 추가했으므로 이미 떠있는 액티비티가 있으면 이를 다시 사용해야한다. 그러나 이 때는 onCreate() 메소드가 호출되지 않으므로 onNewIntent() 메소드를 재정의하여 인텐트 처리부 코드를 넣어주어야한다.

이제 액티비티에서 이를 처리하는 코드를 보자.

~~~ java
package io.swnomad.samplereceiver;

import android.Manifest;
import android.app.Activity;
import android.content.Intent;
import android.content.SharedPreferences;
import android.content.pm.PackageManager;
import android.os.Build;
import android.support.v4.app.ActivityCompat;
import android.support.v4.content.ContextCompat;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.telephony.SmsMessage;
import android.widget.TextView;
import android.widget.Toast;

import java.util.Date;

public class MainActivity extends AppCompatActivity {

    TextView textView; //메시지를 표시하기 위한 텍스트뷰

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //RECEIVE_SMS는 위험권한이므로 사용자에게 권한 요청을 하기위한 코드
        int permissionCheck = ContextCompat.checkSelfPermission(this, Manifest.permission.RECEIVE_SMS);
        if(permissionCheck == PackageManager.PERMISSION_GRANTED){
            Toast.makeText(this, "SMS 수신권한 있음", Toast.LENGTH_SHORT).show();
        } else {
            Toast.makeText(this, "SMS 수신권한 없음", Toast.LENGTH_SHORT).show();
            if(ActivityCompat.shouldShowRequestPermissionRationale(this,Manifest.permission.RECEIVE_SMS)){
                Toast.makeText(this, "SMS 권한 설정이 필요함", Toast.LENGTH_SHORT).show();
            } else {
                // 권한이 할당되지 않았으면 해당 권한을 요청
                ActivityCompat.requestPermissions(this,new String[] {Manifest.permission.RECEIVE_SMS},1);
            }
        }

        //텍스트뷰 객체 참조하기
        textView = findViewById(R.id.textView);

        Intent intent = getIntent();

        // 인텐트로부터 발신자, 내용, 날짜 정보 받아와서 저장
        String sender = intent.getStringExtra("sender");
        String contents = intent.getStringExtra("contents");
        String date = intent.getStringExtra("date");

        //텍스트뷰에 메시지 정보 설정
        Toast.makeText(getApplicationContext(), sender+ "으로부터 문자 도착", Toast.LENGTH_LONG).show();
        textView.setText("보낸사람: "+sender+"\n내용: "+contents+"\n날짜: "+date);
    }

    // 새로운 인텐트가 도착하면 호출되는 콜백 메소드
    @Override
    protected void onNewIntent(Intent intent) {
        super.onNewIntent(intent);

        // 인텐트로부터 발신자, 내용, 날짜 정보 받아와서 저장
        String sender = intent.getStringExtra("sender");
        String contents = intent.getStringExtra("contents");
        String date = intent.getStringExtra("date");

        //텍스트뷰에 메시지 정보 설정
        Toast.makeText(getApplicationContext(), sender+ "으로부터 문자 도착", Toast.LENGTH_LONG).show();
        textView.setText("보낸사람: "+sender+"\n내용: "+contents+"\n날짜: "+date);
    }
}
~~~

액티비티는 인텐트에서 메시지 정보를 가져와 텍스트뷰에 내용을 표시한다. 액티비티의 레이아웃 구성은 다음과 같다.

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
        android:text="수신받은 메시지가 표시됩니다."
        android:textSize="20dp"
        android:maxLines="40"
        android:layout_marginTop="350dp"
        android:layout_marginLeft="10dp"
        android:layout_marginRight="10dp"/>

</LinearLayout>
~~~

앱을 실행하고 문자메시지를 받아보면 다음과 같이 내용이 텍스트뷰에 표시된다. 그리고 백그라운드에서도 실행되므로 다른 작업을 하다가 문자가 오면 액티비티가 뜬다.

![1](https://nobbaggu.github.io/images/android/20/1.jpg){: width="30%" height="30%"}