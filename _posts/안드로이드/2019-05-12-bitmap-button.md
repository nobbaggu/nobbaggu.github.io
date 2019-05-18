---
title: (안드로이드) 36. 비트맵(Bitmap) 버튼
date: 2019-05-12T16:00:00+09:00
author: SWnomad
layout: post
categories: 안드로이드
tags:
  - bitmap
  - 비트맵
  - 버튼
  - button
---

앱을 만들때는 기본 위젯이 아닌 직접 만든 버튼을 사용하는 경우가 많다. 단순히 배경만 사진으로 해주는 것도 있는데, 이 때는 클릭 이벤트가 발생해도 버튼에 변화가 없어 사용자가 버튼의 상태를 알아채기 힘들다. 따라서 그래픽 이미지로 된 버튼을 직접 만들어서 사용해야 한다. 이 버튼이 바로 **비트맵 버튼**이다.

비트맵은 메모리에 만들어진 이미지 객체라고 생각하면 된다. 일반적으로 버튼 클래스를 상속받아서 만들게 된다.

비트맵에 대해 제대로 이해하기 위해서는 Canvas아 Bitmap, BitmapFactory 클래스에 대해 자세히 다루어야하는데 이는 그래픽 부분에서 자세히 다룰 것이므로 이번에는 비트맵 버튼을 사용하는 방법만 간단하게 알아본다.

먼저 버튼을 상속받아 클래스를 정의한다.

~~~ java
package io.swnomad.samplebitmapwidget;

import android.content.Context;
import android.graphics.Color;
import android.graphics.Typeface;
import android.support.v7.widget.AppCompatButton;
import android.util.AttributeSet;
import android.view.MotionEvent;

public class BitmapButton extends AppCompatButton {

    int iconNormal = R.drawable.bitmap_button_normal; //노멀 상태의 비트맵 이미지
    int iconClicked = R.drawable.bitmap_button_clicked; //클릭 상태의 비트맵 이미지

    public BitmapButton(Context context){
        super(context);
        init(); //객체 생성 후 초기화
    }

    public BitmapButton(Context context, AttributeSet attrs){
        super(context, attrs);
        init();//객체 생성 후 초기화
    }

    public void init(){
        setBackgroundResource(iconNormal); //비트맵 버튼 노말 상태
        setTextColor(Color.WHITE); //텍스트 색상 흰색
        setTypeface(Typeface.DEFAULT_BOLD);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        super.onTouchEvent(event);

        int action = event.getAction();

        switch(action){
            case MotionEvent.ACTION_DOWN: // 버튼 눌렀을 때는 클릭 상태 비트맵 이미지 설정
                setBackgroundResource(iconClicked);
                break;

            case MotionEvent.ACTION_UP: // 버튼에서 손 떼면 노멀 상태 비트맵 이미지 설정
                setBackgroundResource(iconNormal);
                break;
        }

        invalidate(); //다시 그리기

        return true;
    }
}
~~~

<br>
객체를 생성 후 초기화하기 위해 생성자 안에 `init()` 메소드를 넣어주었다. `init()` 메소드 내에서는 버튼의 비트맵 이미지를 노멀 상태의 이미지로 설정하고 텍스트 색상을 흰색, 스타일을 굵은 글씨로 설정한다.

그리고 버튼이 눌리고 떼어질 때의 이벤트 처리를 위해서 `onTouchEvent()` 메소드를 재정의하였다. 각각의 이벤트별로 비트맵 이미지를 다르게 설정하면 된다.

`invalidate()` 메소드는 `View class`의 메소드로 그래픽 설정후 레이아웃에 다시 그릴 때 사용한다.

앱을 실행하면 아래의 실행결과가 나온다.

![1](/images/android/36/1.jpg){: width="30%" height="30%"}
![2](/images/android/36/2.jpg){: width="30%" height="30%"}

첫 번째 사진이 노멀 상태이고 두 번째 사진이 버튼을 클릭했을 때이다. 두 가지 터치 이벤트가 발생할 때마다 버튼의 비트맵 이미지가 다시 그려진다.