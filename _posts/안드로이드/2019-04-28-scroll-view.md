---
title: (안드로이드) 9. 스크롤 뷰(ScrollView)
date: 2019-04-28T11:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 스크롤 뷰
  - ScrollView
---

스크롤뷰는 뷰가 한 화면에 다 보이지 않을 때 사용하며, 스크롤 기능을 지원한다. 단지 뷰를 스크롤뷰 안에 넣는 것 만으로 스크롤 기능이 자동으로 적용되기 때문에 무척 편리하다.

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

    <HorizontalScrollView
        android:id="@+id/horScrollView"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <ScrollView
            android:id="@+id/scrollView"
            android:layout_width="match_parent"
            android:layout_height="match_parent">

            <ImageView
                android:id="@+id/imageView"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content" />

        </ScrollView>
    </HorizontalScrollView>

</LinearLayout>
~~~

이미지를 번갈아가면서 보여주기 위해 버튼을 하나 만들었다. 그리고 스크롤뷰는 기본적으로 수직 방향을 지원하기 때문에 HorizontalScrollView도 하나 만들어 수직, 수평 방향 모두 스크롤을 할 수 있도록 하였다. 마지막으로 스크롤뷰 안에 이미지를 보여주기 위한 ImageView를 넣었다. 이제 자바 소스코드를 통해서 이미지를 바꾸어 보여주는 앱을 만들어보자.

~~~ java
package io.swnomad.samplescrollview;

import android.content.res.Resources;
import android.graphics.Bitmap;
import android.graphics.drawable.BitmapDrawable;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.ImageView;
import android.widget.ScrollView;

public class MainActivity extends AppCompatActivity {

    ScrollView scrollView;
    ImageView imageView;
    BitmapDrawable bitmap;

    int imageNumber = 1; //현재 imageView의 그림 번호

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        scrollView = findViewById(R.id.scrollView); //스크롤뷰 참조
        imageView = findViewById(R.id.imageView); //이미지뷰 참조

        scrollView.setHorizontalScrollBarEnabled(true);// 수평 스크롤바 사용 가능하게

        /* drawable 폴더에 있는 image01 그림을 bitmap 변수에 저장 */
        Resources res = getResources();
        bitmap = (BitmapDrawable) res.getDrawable(R.drawable.image01);
        int bitmapWidth = bitmap.getIntrinsicWidth();
        int bitmapHeight = bitmap.getIntrinsicHeight();

        imageView.setImageDrawable(bitmap); //ImageView 그림 설정
        imageView.getLayoutParams().width = bitmapWidth; //ImageView 가로 크기 설정
        imageView.getLayoutParams().height = bitmapHeight; //ImageView 세로 크기 설정
    }

    /* button의 onClick 속성으로 설정한 메소드 */
    public void onButtonClicked(View v){
        changeImage();
    }

    /* imageView의 그림을 바꾸는 메소드 */
    private void changeImage(){
        Resources res = getResources();
        if(imageNumber==1){ //imageNumber 변수 값이 1이면

            /* imageView에 image02 그림 설정 */
            bitmap = (BitmapDrawable) res.getDrawable(R.drawable.image02);
            int bitmapWidth = bitmap.getIntrinsicWidth();
            int bitmapHeight = bitmap.getIntrinsicHeight();

            imageView.setImageDrawable(bitmap);
            imageView.getLayoutParams().width = bitmapWidth;
            imageView.getLayoutParams().height = bitmapHeight;
            imageNumber = 2; //imageNumber 변수 값 2로 설정
        }else{ //imageNumber 변수 값이 1이 아니면

            /* imageView에 image01 그림 설정 */
            bitmap = (BitmapDrawable) res.getDrawable(R.drawable.image01);
            int bitmapWidth = bitmap.getIntrinsicWidth();
            int bitmapHeight = bitmap.getIntrinsicHeight();

            imageView.setImageDrawable(bitmap);
            imageView.getLayoutParams().width = bitmapWidth;
            imageView.getLayoutParams().height = bitmapHeight;
            imageNumber = 1; //imageNumber 변수 값 1로 설정
        }
    }
}
~~~

findViewById 메소드에 XML 코드에서 만든 뷰의 id를 전달하면 뷰 객체를 참조할 수 있다. 그리고 /res 파일에 있는 리소스들은 getResources() 메소드를 통해서 참조할 수 있다. getResources()로 참조한 Rescources 객체에서 getDrawable 메소드를 사용하여 image01 혹은 image02를 불러와 BitmapDrawable객체로 강제 형변환을 하여 변수에 저장했다. BitmapDrawable 객체는 getIntrinsicWidth(), getIntrinsicHeight() 메소드를 통해서 크기를 참조할 수 있다. 이후 imageView에 setImageDrawable 메소드를 통해 이미지를 설정하고, getLayoutParams().width, getLayoutParams().height를 통해 이미지뷰의 크기를 설정할 수 있다.

changeImage() 메소드에서는 두 이미지를 번갈아가며 보여주기 위한 코드를 설정했다. 그리고 이 메소드를 버튼의 onClick 속성으로 설정한 메소드에 넣어주어 기능을 완성했다.

![1](https://nobbaggu.github.io/images/android/9/1.jpg){: width="30%" height="30%"}
![2](https://nobbaggu.github.io/images/android/9/2.jpg){: width="30%" height="30%"}