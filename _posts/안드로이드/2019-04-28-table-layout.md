---
title: 8. 테이블 레이아웃(TableLayout)
date: 2019-04-28T10:00:00+09:00
author: SWnomad
layout: post
categories: 안드로이드
tags:
  - 레이아웃
  - 테이블
  - 테이블 레이아웃
  - TableLayout
---

테이블 레이아웃은 표의 형태와 비슷한 생김새의 레이아웃이다. 사실 이후에 배울 그리드 뷰라는 것이 테이블 레이아웃을 대체하는데다가, 더욱 간편한 기능을 지원하기 때문에 테이블 레이아웃은 사용할 일이 거의 없다.

테이블 레이아웃은 TableRow라는 태그가 있는데, 이것은 하나의 행을 의미한다. 그리고 TableRow 안에 있는 뷰의 갯수가 열의 갯수가 된다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TableRow>

        <Button
            android:id="@+id/button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button1" />

        <Button
            android:id="@+id/button2"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button2" />

        <Button
            android:id="@+id/button3"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button3" />
    </TableRow>

    <TableRow>

        <Button
            android:id="@+id/button4"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button4" />

        <Button
            android:id="@+id/button5"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button5" />

        <Button
            android:id="@+id/button6"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Button6" />
    </TableRow>
</TableLayout>
~~~

두 개의 TableRow를 추가하고 각각에 button을 3개씩 추가했다. 즉, 행은 2개이고 열이 3개가 된다. 앱을 실행하면 아래와 같은 화면이 나온다.

![1](/images/android/8/1.jpg){: width="30%" height="30%"}

오른쪽에 여유공간이 남는데, 세 개의 버튼이 이 여유공간을 나누어 가지면서 공간을 채우도록 하려면 TableLayout의 stretchColumns 속성을 사용하면 된다.

~~~ xml
android:stretchColumns="0,1,2"
~~~

TableLayout에 위 속성을 추가했다. 3개의 열이 모두 여유공간을 나누어 가지도록 0, 1, 2 인덱스를 모두 넣어주었다.

![2](/images/android/8/2.jpg){: width="30%" height="30%"}

3개의 버튼이 여유공간을 나누어 채운 것이 보인다.

비슷하게 shirinkColumns 속성은 컨테이너를 벗어나는 뷰를 강제로 축소시켜 컨테이너에 맞출 수 있도록 해준다.

추가적인 속성을 확인하기 위해 다음 코드를 보자.

~~~xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:stretchColumns="0,1,2">

    <TableRow>
        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="이름 : "/>

        <EditText
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_span="3"/>
    </TableRow>

    <TableRow>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="아니오"
            android:layout_column="2"/>

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="예" />
    </TableRow>
</TableLayout>
~~~

layout_span은 뷰가 몇 개의 열을 차지할 것인지 설정할 수 있는 속성이다. 첫 번째 행의 입력상자가 총 3개의 열얼 차지하도록 하였다.

그리고 layout_column은 뷰의 열 인덱스를 지정한다. 두 번째 행의 첫 번째 버튼은 layout_column 속성의 값이 2이므로 다음 열의 인덱스는 3이 된다. 즉, 0,1 인덱스의 위치에는 아무런 뷰도 없는 것이다.

화면을 실행하면 다음과 같이 된다.

![3](/images/android/8/3.jpg){: width="30%" height="30%"}