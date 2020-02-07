---
title: (안드로이드) 7. 상대 레이아웃(RelativeLayout)
date: 2019-04-28T09:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 레이아웃
  - 상대
  - 상대 레이아웃
  - RelativeLayout
---

&nbsp;
상대 레이아웃은 부모 컨테이너, 그리고 뷰 사이의 상대적 위치관계를 정의하여 화면을 배치하는 레이아웃이다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <Button
        android:id="@+id/button1"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="Button1"
        android:layout_below="@+id/button3"
        android:layout_above="@+id/button2"
        />

    <Button
        android:id="@+id/button2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button2"
        android:layout_alignParentBottom="true"/>

    <Button
        android:id="@+id/button3"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Button3"/>

</RelativeLayout>
~~~

layout_width와 layout_height 속성이 모두 match_parent인 button1은 layout_below 속성과 layout_above 속성값으로 각각 button3과 button2의 id를 넣어주어 두 버튼의 사이에 위치하게 된다. 다만, 뷰를 만들면 default로 화면의 윗쪽에 자리잡으므로 button2는 layout_alignParentBottom 속성의 값을 true로 해주어 부모 컨테이너의 아랫쪽에 붙여야 한다.

앱의 화면은 아래와 같이 나타난다.

![1](/images/android/7/1.jpg){: width="30%" height="30%"}