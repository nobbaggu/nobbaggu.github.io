---
title: (안드로이드) 35 - 나인패치(9-Patch) 이미지
date: 2019-05-12T15:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 나인패치
  - nine patch
  - image
---

일반적인 이미지 파일은 크기를 늘리거나 줄일 때 이미지의 가장자리 부분에 왜곡이 생긴다. 

![1](https://nobbaggu.github.io/images/android/35/1.png){: width="100%" height="100%"}

위 그림에서 이미지를 늘릴수록 곡선형태인 모서리 부분이 가장 심하게 왜곡이 된다. 예를 들면 이미지 버튼에 자신이 구성한 UI를 적용할 때 디바이스마다 다른 해상도로 인해 이미지 크기가 자동으로 늘어나거나 줄어드는 일이 발생할 수 있다.

나인패치는 이러한 이미지의 왜곡을 해결하는 방법을 정의한 것이다.

<br>
#### 나인패치 이미지의 동작 원리

나인패치 이미지는 크게 두 영역으로 구분할 수 있다.

>1. 크기가 늘어나도 되는 영역
>2. 크기가 늘어나지 말아야 할 영역

나인패치 이미지는 두 영역을 구분하기위해 기본적으로 9개의 영역으로 분할하여 저장하는 방식이다.

![2](https://nobbaggu.github.io/images/android/35/2.png){: width="100%" height="100%"}

<br>
나인패치에서는 이미지가 늘어나야 할 영역에 대한 정보를 저장하기 위해 원본이미지 크기에서 왼쪽/오른쪽/위/아래 각각 1픽셀씩 늘린 이미지를 새로 만든다. 그리고 위 1픽셀 라인에는 가로로 늘어날 영역을 기록하고, 왼쪽 1픽셀 라인에는 세로로 늘어날 영역을 기록한다.

![3](https://nobbaggu.github.io/images/android/35/3.png){: width="100%" height="100%"}

이렇게 저장된 나인패치 이미지는 크기가 변할 때 기록된 정보를 토대로 크기 변환을 한다.

안드로이드는 이름에 '.9'가 붙은 이미지 파일을 나인패치 이미지로 인식한다.

<br>
일반이미지와 나인패치 이미지의 비교를 위해 각각에 대해 버튼을 3개씩 다른 크기로 만들어보자.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:orientation="vertical"
        android:gravity="center_horizontal"
        android:background="#ffffff00">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="일반 이미지"
            android:textSize="24dp"
            android:textStyle="bold"
            android:textColor="#ff000000"
            android:layout_gravity="center_horizontal"
            android:layout_marginTop="10dp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="small"
            android:textColor="#ffffffff"
            android:background="@drawable/button_image_01"
            android:layout_marginTop="30dp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="mediummediummeduim"
            android:textColor="#ffffffff"
            android:background="@drawable/button_image_01"
            android:layout_marginTop="40dp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="longlonglonglonglonglonglonglonglong"
            android:textColor="#ffffffff"
            android:background="@drawable/button_image_01"
            android:layout_marginTop="40dp"/>

    </LinearLayout>

    <LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        android:orientation="vertical"
        android:gravity="center_horizontal"
        android:background="#ffff00ff">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="나인패치 이미지"
            android:textSize="24dp"
            android:textStyle="bold"
            android:textColor="#ff000000"
            android:layout_gravity="center_horizontal"
            android:layout_marginTop="10dp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="small"
            android:textColor="#ffffffff"
            android:background="@drawable/button_image_02"
            android:layout_marginTop="30dp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="mediummediummeduim"
            android:textColor="#ffffffff"
            android:background="@drawable/button_image_02"
            android:layout_marginTop="40dp"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="longlonglonglonglonglonglonglonglong"
            android:textColor="#ffffffff"
            android:background="@drawable/button_image_02"
            android:layout_marginTop="40dp"/>

    </LinearLayout>
</LinearLayout>
~~~

일반 이미지 3개, 나인패치 이미지 3개를 각각 다른 크기로 만들어서 앱을 실행해보면 다음과 같은 결과가 나온다.

![4](https://nobbaggu.github.io/images/android/35/4.jpg){: width="30%" height="30%"}

버튼의 배경으로 설정한 이미지가 텍스트의 크기에 따라 자동으로 변한다. 이 때 일반 이미지는 이미지 왜곡이 심하게 일어난다. 그러나 나인패치로 변형한 이미지는 더욱 깔끔하게 크기를 변형하는 것을 알 수 있다.