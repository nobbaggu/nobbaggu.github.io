---
title: (안드로이드) 6 - 리니어 레이아웃(LinearLayout)
date: 2019-04-27T12:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 레이아웃
  - 리니어
  - 리니어 레이아웃
  - LinearLayout
---

&nbsp;
### 1. 리니어 레이아웃

리니어 레이아웃은 대표적인 레이아웃들 중에서도 가장 중요한 레이아웃이다. 개념은 간단하지만, 여러 개의 리니어 레이아웃을 중첩시켜 사용하면 왠만한 복잡한 화면도 다 만들 수 있다.

리니어 레이아웃은 orientation 속성을 필수로 가지는데, vertical과 horizontal을 값으로 가질 수 있다. vertical은 세로 방향의 뷰 배치를, horizontal은 가로 방향의 뷰 배치를 한다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">


    <Button
        android:id="@+id/button"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button" />

    <Button
        android:id="@+id/button2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button" />

    <Button
        android:id="@+id/button3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button" />
</LinearLayout>
~~~

위 xml 코드에서는 리니어 레이아웃의 방향(orientation) 속성 값으로 vertical을 설정하였다. 그리고 버튼을 3개 배치하였다. 앱을 실행하면 다음과 같은 화면을 볼 수 있다.

![1](https://nobbaggu.github.io/images/android/6/1.jpg){: width="30%" height="30%"}

우리가 만든 레이아웃은 자바 코드를 통해 액티비티의 화면으로 설정된 것이다. MainActivity.java 소스코드를 보자.

~~~ java
package io.swnomad.samplelinearlayout;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main); //activity_main.xml을 액티비티 화면으로 설정
    }
}
~~~

위 코드에서 setContentView 메소드의 전달인자로 R.layout.activity_main과 같이 XML로 정의된 레이아웃 리소스 파일을 전달함으로써 아까 만든 레이아웃이 액티비티 화면으로 설정된 것이었다.

&nbsp;
&nbsp;
### 2. 자바 코드로 화면 구성하기
XML로 정의된 레이아웃 리소스를 사용하지 않고 자바 소스코드에서 직접 화면을 구성할 수 있다. 동적으로 화면 구성이 달라져야 하는 경우는 이렇게 하는것이 더욱 효과적이므로 알아두는 것이 좋다.

새로운 LayoutCodeActivity.java 파일을 만들어서 다음과 같이 소스코드를 입력하였다.

~~~ java
package io.swnomad.samplelinearlayout;

import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.widget.Button;
import android.widget.LinearLayout;

public class LayoutCodeActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        LinearLayout mainLayout = new LinearLayout(this); // 리니어 레이아웃 객체 생성
        mainLayout.setOrientation(LinearLayout.VERTICAL); // 방향 속성을 세로로 설정

        //버튼을 위한 속성 미리 설정하기
        LinearLayout.LayoutParams params = new LinearLayout.LayoutParams(
                        LinearLayout.LayoutParams.MATCH_PARENT, //Layout_width
                        LinearLayout.LayoutParams.WRAP_CONTENT); //Layout_height

        Button button1 = new Button(this); //버튼 객체 생성
        button1.setText("Button1"); //버튼 Text 설정
        button1.setLayoutParams(params); //버튼 속성 설정

        Button button2 = new Button(this); //버튼 객체 생성
        button2.setText("Button2"); //버튼 Text 설정
        button2.setLayoutParams(params); //버튼 속성 설정

        Button button3 = new Button(this); //버튼 객체 생성
        button3.setText("Button3"); //버튼 Text 설정
        button3.setLayoutParams(params); //버튼 속성 설정

        mainLayout.addView(button1);
		mainLayout.addView(button2);
        mainLayout.addView(button3);

        setContentView(mainLayout); //액티비티 화면으로 mainLayout 설정
    }
}
~~~

그리고 AndroidManifest.xml 파일에서 첫 화면을 LayoutCodeActivity로 변경해준다.

~~~ xml
	...
<activity android:name=".LayoutCodeActivity">
	...
~~~

그리고 앱을 실행하면 다음과 같은 화면이 만들어진다.

![2](https://nobbaggu.github.io/images/android/6/2.jpg){: width="30%" height="30%"}

여기에서는 자바 소스 코드상으로 뷰그룹 객체인 mainLayout을 만들고 거기에 3개의 뷰(버튼)을 추가하였다. 마지막으로 setContentView 메소드로 뷰그룹 객체를 전달함으로써 액티비티 화면으로 뷰그룹 객체를 띄울 수 있었다.

&nbsp;
**※Context 객체란?**

안드로이드에서는 뷰 객체를 만들 때 생성자에 항상 Context 객체가 전달되어야 한다. AppCompatActivity 클래스는 Context 클래스를 상속하므로 this로 Context 객체를 전달할 수 있다.

&nbsp;
&nbsp;
### 3. 뷰 정렬하기

뷰를 정렬하기 위한 속성에는 다음의 두 가지가 있다.

**1)layout_gravity**

부모 컨테이너의 여유 공간에서 포함하는 뷰들을 정렬한다. 뷰의 layout_width나 layout_height 속성이 wrap_content인 경우 사용할 수 있다.

**2)gravity**

뷰에 담긴 내용물을 정렬한다.

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
        android:text="left"
        android:layout_gravity="left"/>

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="center"
        android:layout_gravity="center"/>

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button"
        android:gravity="right"
        android:layout_gravity="right"/>

    <Button
        android:id="@+id/button4"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button"
        android:gravity="right"/>
</LinearLayout>
~~~

![3](https://nobbaggu.github.io/images/android/6/3.jpg){: width="30%" height="30%"}

vertical 속성의 리니어 레이아웃에 버튼을 4개 추가하였다. 처음 3개의 버튼은 각각 layout_gravity 속성이 left, center, right 이므로 레이아웃 안에서 뷰의 위치가 각각 왼쪽, 중간, 오른쪽으로 설정되었다. 그리고 4번째 버튼은 layout_width 속성값이 match_parent라서 가로방향으로 공간이 많이 남는다. 이 버튼의 gravity 속성값으로 right를 주었으므로 버튼 안의 내용물, 즉 텍스트가 오른쪽으로 정렬된다.

&nbsp;
&nbsp;
### 4. 여백 설정(마진 & 패딩)

마진(Margin)은 뷰의 테두리 바깥 부분의 여백의 크기를 결정하며 패딩(Padding)은 뷰의 테두리 안쪽의 여백을 설정한다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="텍스트"
        android:textSize="24dp"
        android:padding="20dp"
        android:background="#ffff0000"/>

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="텍스트"
        android:textSize="24dp"
        android:layout_margin="30dp"
        android:background="#ff00ff00"/>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="텍스트"
        android:textSize="24dp"
        android:background="#ff0000ff"
        android:paddingBottom="200dp"/>
</LinearLayout>
~~~

마진과 패딩은 위 코드처럼 4 방향을 한꺼번에 설정할 수도 있고 특정 방향만 설정할 수도 있다.

![4](https://nobbaggu.github.io/images/android/6/4.jpg){: width="30%" height="30%"}

&nbsp;
### 5. 여유공간 분할

부모 컨테이너에 뷰들을 추가했을 때, 여유공간이 남았다면 이들을 기존에 추가한 뷰에 layout_weight 속성을 이용하여 나누어 할당할 수 있다. 예를들어 여유공간을 두 개의 뷰에 1:2로 나누어 할당했을 때, 여유 공간의 1/3과 2/3이 각각의 뷰의 크기에 추가된다. 주의해야 할 것은 뷰 자체의 크기가 1:2가 되는 것이 아니라, 여유 공간만 1:2로 할당하여 추가로 더해준다는 것이다. 또한 여유공간을 할당할 때에는 뷰의 그 방향 크기 속성이 wrap_content일 때에만 가능하다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="#ffffff00"
            android:text="텍스트"
            android:textSize="24dp"
            android:layout_weight="1"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="#ff00ffff"
            android:text="텍스트"
            android:textSize="24dp"
            android:layout_weight="1"/>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="#ffffff00"
            android:text="텍스트"
            android:textSize="24dp"
            android:layout_weight="1"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="#ff00ffff"
            android:text="텍스트"
            android:textSize="24dp"
            android:layout_weight="2"/>
    </LinearLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="#ffffff00"
            android:text="텍스트"
            android:textSize="24dp"
            android:layout_weight="1"/>

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:background="#ff00ffff"
            android:text="텍스트"
            android:textSize="24dp"
            android:layout_weight="3"/>
    </LinearLayout>
</LinearLayout>
~~~

수직방향 리니어 레이아웃 안에 수평방향 리니어 레이아웃을 3개 추가하였다. 그리고 각각의 수평방향 레이아웃에는 두 개의 텍스트뷰(TextView)를 추가하고 layout_weight 속성을 각각 1:1, 1:2, 1:3으로 주었다.

![5](https://nobbaggu.github.io/images/android/6/5.jpg){: width="30%" height="30%"}

