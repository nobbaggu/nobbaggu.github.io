---
title: 29. 페이지 슬라이딩(Page Sliding)
date: 2019-05-08T21:00:00+09:00
author: SWnomad
layout: post
categories: 안드로이드
tags:
  - 페이지 슬라이딩
  - page sliding
---

애니메이션(Animation) 기능을 사용한 UI를 다이나믹하게 구성할 수 있는 여러 방법이 있는데, 그 중 하나가 페이지 슬라이딩이다. 가령 '메뉴'버튼을 눌렀을 때 메뉴 창이 위에서 내려오는 경우가 대표적이다. 여러 개의 뷰를 한 번에 하나씩 보여주는 프레임 레이아웃(FrameLayout)과 애니메이션 기능을 동시에 적용하면 페이지 슬라이딩을 구현할 수 있다.

페이지 슬라이딩을 구현하기 위해 먼저 레이아웃을 만든다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:background="#ff5555ff">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Base Area"
            android:textSize="20dp"
            android:textColor="#ffffffff"/>

    </LinearLayout>

    <LinearLayout
        android:id="@+id/page"
        android:layout_width="200dp"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:layout_gravity="right"
        android:background="#ffffff66"
        android:visibility="gone">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Area #1"
            android:textSize="20dp"
            android:textColor="#ff000000"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="Area #2"
            android:textSize="20dp"
            android:textColor="#ff000000"/>

    </LinearLayout>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="열기"
        android:textSize="20dp"
        android:layout_gravity="right|bottom"
        android:layout_marginRight="20dp"
        android:layout_marginBottom="20dp"/>

</FrameLayout>
~~~

첫 번째 리니어 레이아웃은 화면 전체를 채우도록 하였다. 두 번째 리니어 레이아웃이 바로 슬라이딩을 효과를 넣어줄 레이아웃인데, layout_width 속성값을 "200dp"로 지정해주어 화면의 가로길이를 다 채우지 않도록 한다. 그리고 원하는 시점에 보여야 하므로 visibility 속성을 "gone"으로 설정한다.

&nbsp;
그 다음으로 페이지가 오른쪽에서 나오는 애니메이션(translate_left.xml)과 들어가는 애니메이션(translate_right.xml)을 정의한다.

\<translate_left.xml\>

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:fromXDelta="100%p"
        android:toXDelta="0%p"
        android:duration="500"
        android:repeatCount="0"
        android:fillAfter="true" />
</set>
~~~

\<translate_right.xml\>
~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:fromXDelta="0%p"
        android:toXDelta="100%p"
        android:duration="500"
        android:repeatCount="0"
        android:fillAfter="true" />
</set>
~~~


&nbsp;
이제 페이지 슬라이딩 기능을 구현할 코드를 작성한다.

~~~ java
package io.swnomad.samplepagesliding;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.view.animation.Animation;
import android.view.animation.AnimationUtils;
import android.widget.Button;
import android.widget.LinearLayout;

public class MainActivity extends AppCompatActivity {

    boolean isPageOpen = false;
    Animation translateLeft;
    Animation translateRight;
    LinearLayout page;
    Button button;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        page = findViewById(R.id.page);

        /* 왼쪽, 오른쪽으로 슬라이딩하는 애니메이션 객체 로딩 */
        translateLeft = AnimationUtils.loadAnimation(this, R.anim.translate_left);
        translateRight = AnimationUtils.loadAnimation(this, R.anim.translate_right);

        //페이지 열기 리스너 설정
        translateLeft.setAnimationListener(new Animation.AnimationListener() {
            @Override
            public void onAnimationStart(Animation animation) {
                page.setVisibility(View.VISIBLE); // 슬라이딩 시작할 때 페이지 보이게
            }

            @Override
            public void onAnimationEnd(Animation animation) {
                // 페이지 다 열리고 버튼 글자 변경 및 isPageOpen 변수 값 변경
                button.setText("닫기");
                isPageOpen = true;
            }

            @Override
            public void onAnimationRepeat(Animation animation) {

            }
        });

        //페이지 닫기 리스너 설정
        translateRight.setAnimationListener(new Animation.AnimationListener() {
            @Override
            public void onAnimationStart(Animation animation) {

            }

            @Override
            public void onAnimationEnd(Animation animation) {
                page.setVisibility(View.INVISIBLE); //페이지 슬라이딩 끝난 후 페이지 안보이게
                //페이지 다 닫기고 버튼 글자 변경 및 isPageOpen 변수 값 변경
                button.setText("열기");
                isPageOpen = false;
            }

            @Override
            public void onAnimationRepeat(Animation animation) {

            }
        });

        button = findViewById(R.id.button);

        // 버튼 클릭 이벤트 처리
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(isPageOpen){ //페이지 열려있으면
                    page.startAnimation(translateRight); //오른쪽으로 닫기 애니메이션 실행
                }else{ //페이지 닫혀있으면
                    page.startAnimation(translateLeft); //왼쪽으로 열기 애니메이션 실행
                }
            }
        });
    }
}
~~~

페이지가 열리는 애니메이션과 닫히는 애니메이션 객체를 각각 설정하고 AnimationListener 객체를 각각 달았다. 페이지가 왼쪽으로 열리는 리스너에서는 열리기 전에 보이기 시작해야하므로 onAnimationStart() 메소드안에서 visibility 속성을 보이도록 변경하고 닫히는 리스너에서는 닫히고 안보이도록 해야하므로 onAnimationEnd() 메소드에서 visibility 속성을 안보이도록 바꾸었다.

버튼을 클릭했을 때의 이벤트는 isPageOpen 변수의 값에 따라 다르게 처리하면 된다.

앱을 실행하면 아래와 같은 결과를 볼 수 있다.

![1](/images/android/29/1.jpg){: width="30%" height="30%"}
![2](/images/android/29/2.jpg){: width="30%" height="30%"}