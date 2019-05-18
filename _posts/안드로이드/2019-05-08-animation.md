---
title: (안드로이드) 28. 애니메이션(Animation)
date: 2019-05-08T20:00:00+09:00
author: SWnomad
layout: post
categories: 안드로이드
tags:
  - animation
  - 애니메이션
  - startAnimation
  - loadAnimation
  - setAnimationListener
  - onAnimationStart
  - onAnimationEnd
---

레이아웃의 모든 뷰에는 애니메이션(Animation) 기능을 설정할 수 있다. 애니메이션 구현방법 중 트윈 애니메이션(Tweened Animation)이라는 방법이 가장 간단하면서도 일반적으로 사용한다. 트윈 애니메이션은 이동, 확대 및 축소, 회전과 같은 일정한 패턴으로 움직이는 애니메이션 기능을 지원한다.

&nbsp;
#### \<트윈 애니메이션 구현 방법\>

![1](/images/android/28/1.png){: width="70%" height="70%"}

**1. 트윈 애니메이션 동장 방식을 XML 파일로 정의**

**2. loadAnimation() 메소드 사용하여 애니메이션 객체 메모리 로딩**

**3. startAnimation() 메소드 사용하여 애니메이션 동작**

&nbsp;

애니메이션을 정의하는 XML 파일은 /res/anim 폴더에 저장해야 한다. 안드로이드는 /res/anim 폴더에 들어있는 XML 파일을 애니메이션이 정의된 리소스로 인식한다.

&nbsp;
애니메이션 기능을 적용한 버튼을 만들기 위한 코드를 작성해보자.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:fromXDelta="100%p"
        android:toXDelta="0%p"
        android:duration="6000"
        android:repeatCount="3"/>

    <alpha
        android:fromAlpha="0.5"
        android:toAlpha="1"
        android:duration="6000"
        android:repeatCount="3"/>
    
</set>
~~~

\<translate\> 태그는 뷰 등의 구성요소의 위치이동 방식을 정의하기 위한 액션 정보 속성이 들어간다. 애니메이션은 fromXDelta 에서 toXDelta 속성까지 이동한다. 위 코드에서는 100%p 부터 0%p 까지이므로 오른쪽 끝에서 왼쪽 끝으로 이동하도록 되어있는 것이다. duration은 애니메이션의 지속시간(ms)이다. repeatCount는 애니메이션의 반복 횟수를 지정한다. 위 코드에서는 1번 + 3번반복으로 총 4번 실행된다.

\<alpha\> 태그는 투명도에 관한 속성을 정의한다. 위 코드에서는 투명도 0.5부터 1까지 6초에 걸쳐 3번 반복하도록 정의했다.

~~~ java
package io.swnomad.samplebuttonanimation;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.view.animation.Animation;
import android.view.animation.AnimationUtils;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    TextView textView;
    Button button;
    Animation flowAnim;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = findViewById(R.id.textView);
        button = findViewById(R.id.button);

        flowAnim = AnimationUtils.loadAnimation(this, R.anim.flow); //애니메이션 XML 파일 메모리에 로딩
        flowAnim.setAnimationListener(new Animation.AnimationListener() { //애니메이션에 리스너 설정(필요할 때만)
            @Override
            public void onAnimationStart(Animation animation) {
                Toast.makeText(MainActivity.this, "애니메이션 시작", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAnimationEnd(Animation animation) {
                Toast.makeText(MainActivity.this, "애니메이션 종료", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAnimationRepeat(Animation animation) {

            }
        });

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                textView.startAnimation(flowAnim); //텍스트뷰로 flowAnim 애니메이션 객체 시작
            }
        });
    }
}
~~~

setAnimationListener() 메소드를 통해 애니메이션 리스너 객체를 설정하여 애니메이션이 시작할 때, 반복될 때, 끝날 때 특정 동작을 정의하여 실행할 수 있다. 버튼을 누르면 텍스트뷰가 애니메이션 동작을 수행한다.

![1](/images/android/28/1.jpg){: width="30%" height="30%"}
![2](/images/android/28/2.jpg){: width="30%" height="30%"}
![3](/images/android/28/3.jpg){: width="30%" height="30%"}
