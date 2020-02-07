---
title: (안드로이드) 21. 위험권한 부여
date: 2019-05-03T19:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 위험권한
  - permission
---

안드로이드 API 23 이전에는 앱 설치 시 매니페스트 파일에 등록된 권한이 한 번에 부여되었다. 그런데 API 23 버전부터는 권한이 일반권한과 위험권한으로 분류되고 일반 권한은 앱 설치시에 부여되고 위험 권한은 앱이 처음 실행될 시점에 사용자로부터 부여받도록 되어있다.

위험 권한에는 다음과 같은 것들이 있다.

![1](/images/android/21/1.png){: width="50%" height="50%"}

위험권한을 부여받기 위해서는 매니페스트 파일에 다음 코드를 등록해야 한다.

~~~ xml
<uses-permission android:name="android.permission.RECEIVE_SMS" />
~~~

이제 자바 소스코드로 위험권한을 부여받기 위한 처리부분을 작성해보자.

~~~ java
package io.swnomad.samplereceiver;

import android.Manifest;
import android.content.pm.PackageManager;
import android.support.annotation.NonNull;
import android.support.v4.app.ActivityCompat;
import android.support.v4.content.ContextCompat;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //RECEIVE_SMS 권한 있는지 체크
        int permissionCheck = ContextCompat.checkSelfPermission(this, Manifest.permission.RECEIVE_SMS);

        if(permissionCheck==PackageManager.PERMISSION_GRANTED){//권한 있으면 토스트 메시지 띄우기
            Toast.makeText(this, "SMS 수신 권한 있음", Toast.LENGTH_LONG).show();
        }else{//권한 없으면 권한 요청 대화상자 띄우기
            Toast.makeText(this, "SMS 수신 권한 없음", Toast.LENGTH_LONG).show();
            if(ActivityCompat.shouldShowRequestPermissionRationale(this, Manifest.permission.RECEIVE_SMS)){
                Toast.makeText(this, "SMS 권한 설명 필요함", Toast.LENGTH_LONG).show();
            }else{
                ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.RECEIVE_SMS}, 1);
            }
        }

    }

    // 권한요청에 대한 사용자의 허용 여부 결과를 받아 처리하는 메소드
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {

        switch(requestCode){
            case 1:{

                if(grantResults.length>0 && grantResults[0]==PackageManager.PERMISSION_GRANTED){
                    Toast.makeText(this, "권한 승인됨", Toast.LENGTH_LONG).show();
                }else{
                    Toast.makeText(this, "권한 거부됨", Toast.LENGTH_LONG).show();
                }
            }
        }
    }

}
~~~

이 메소드는 권한을 부여받을 때마다 수정해서 재사용할 수 있다.

앱을 실행하면 다음과 같은 화면이 뜬다.

![2](/images/android/21/2.jpg){: width="30%" height="30%"}

사용자가 허용하거나 거부하면 onRequestPermissionsResult() 메소드에서 결과를 받아서 처리하고 토스트 메시지를 띄운다.

위험 권한은 한 번 허용하면 앱에 부여된 권한 정보를 안드로이드 시스템이 기억하므로 대화상자가 다시 뜨는일은 없다.