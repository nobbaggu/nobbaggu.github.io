---
title: (안드로이드) 10 - 프레임 레이아웃(FrameLayout)
date: 2019-04-28T12:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 프레임 레이아웃
  - FrameLayout
---

프레임 레이아웃은 가지고 있는 여러개의 뷰 중 한 번에 하나만 보여주는 레이아웃이다. 무척 단순하지만 꽤 자주 사용되는 레이아웃이다. 프레임 레이아웃에 여러개의 뷰를 차례차례 추가하면 가장 마지막에 추가한 뷰만 화면에 보이는 상태가 된다. 이 때, 프레임 레이아웃에 있는 뷰의 가시성(Visibility) 속성을 설정함으로써 특정 뷰를 보이거나 보이지 않게 함으로써 화면을 전환할 수 있다.

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
        android:layout_gravity="center"
        android:text="이미지 바꾸기"
        android:onClick="onButtonClicked"/>

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <ImageView
            android:id="@+id/imageView1"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@drawable/dream01"
            android:visibility="invisible"/>

        <ImageView
            android:id="@+id/imageView2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:src="@drawable/dream02" />
    </FrameLayout>
</LinearLayout>
~~~

이미지를 전환할 버튼을 하나 넣어주고 onClick 속성의 값으로 onButtonClicked 메소드를 넣어주었다.

그리고 그 아래에 프레임 레이아웃을 넣고 레이아웃 안에 ImageView를 2개 추가하였다. 이렇게 했을 때 중첩 된 ImageView들 중 2번째 ImageView만 화면에 보인다. 이제 뷰를 전환하는 기능을 적용하기 위해서 자바 소스코드를 수정해야한다.

~~~ java
package io.swnomad.sampleframelayout;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.ImageView;

public class MainActivity extends AppCompatActivity {

    ImageView imageView1;
    ImageView imageView2;
    int imageIndex = 2; //현재 FrameLayout에 보이는 이미지 번호

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        imageView1 = findViewById(R.id.imageView1); //imageView1 객체 참조
        imageView2 = findViewById(R.id.imageView2); //imageView2 객체 참조
    }

    /* 버튼 onClick 속성 값 메소드 */
    public void onButtonClicked(View v){
        changeImage();
    }

    /* 프레임 레이아웃에 보이는 이미지 전환 메소드 */
    public void changeImage(){
        if(imageIndex==1){ //현재의 imageIndex가 1이면
            imageView1.setVisibility(View.INVISIBLE); //첫 번째 이미지 안보이게
            imageView2.setVisibility(View.VISIBLE); //두 번째 이미지 보이게
            imageIndex = 2; //현재 imageIndex 2로 변경

        }else if(imageIndex==2){ //현재의 imageIndex가 2면
            imageView1.setVisibility(View.VISIBLE); //첫 번째 이미지 보이게
            imageView2.setVisibility(View.INVISIBLE); //두 번째 이미지 안보이게
            imageIndex = 1; //현재 imageIndex 1로 변경
        }
    }
}
~~~

imageIndex 변수는 현재 화면에 보이는 이미지를 무엇인지 판단하기 위해 정의한 변수이다. 먼저 imageView1, imageView2 변수는 각각 이미지뷰 객체 ImageView1과 ImageView2 객체를 참조한다.

onButtonClicked 메소드에 changeImage() 메소드를 넣어줌으로써 버튼이 눌리면 changeImage() 메소드의 코드가 동작하도록 하였다. 현재의 imageIndex 변수의 값에 따라 ImageView의 visibility 속성을 setVisibility 메소드를 통해 제어해준다. 그리고 화면의 visibility 속성 변경 이후 imageIndex 변수의 값도 변경해주는 것을 잊으면 안된다.

이렇게 했을 때 앱을 실행하면 다음과 같은 화면이 보인다.

![1](https://nobbaggu.github.io/images/android/10/1.jpg){: width="30%" height="30%"}
![2](https://nobbaggu.github.io/images/android/10/2.jpg){: width="30%" height="30%"}

버튼을 누를 때 마다 화면이 전환되지만 이는 단순히 두 개의 이미지가 중첩된 상태에서 보이게 하거나 보이지 않게 하는 것일 뿐이다.