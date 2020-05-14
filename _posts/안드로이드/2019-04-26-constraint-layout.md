---
title: (안드로이드) 4. 제약 레이아웃(Constraint Layout)
date: 2019-04-26T15:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 제약조건
  - constraint
  - layout
---

&nbsp;
### 1. 제약 조건(Constraint)

제약 레이아웃은 뷰를 배치할 때 제약 조건이라는 것을 사용한다.

제약 조건은 뷰의 위치를 결정할 때 레이아웃 안의 다른 요소와의 상대적 위치로 결정하는 것이다.

뷰는 위,아래,왼쪽,오른쪽에 각각 연결점을 가지고 있다. 이 연결점을 핸들(handle)이라고도 한다. 뷰의 연결점을 타겟(target)의 연결점에 잇는 것으로 제약조건은 완성된다. 타깃으로는 부모 레이아웃, 레이아웃 안의 다른 뷰, 가이드라인이 될 수 있다.

![constraint](https://nobbaggu.github.io/images/android/4/1.png){: width="70%" height="70%"}

위 그림은 제약 레이아웃 안에 버튼의 왼쪽 연결점과 부모 레이아웃의 왼쪽, 버튼의 윗쪽 연결점과 부모 레이아웃의 윗쪽을 연결하여 제약조건을 완성한 것이다. 제약조건은 최소 횡방향, 종방향 각각 1개씩으로 총 2개 이상이면 된다.


&nbsp;
### 2. 마진(Margin)
오른쪽의 attributes 창에서는 연결점과 타깃 사이 거리인 마진을 설정할 수 있다.

버튼을 하나 더 추가하면 다음과 같은 제약조건이 나온다.

![constraint](https://nobbaggu.github.io/images/android/4/2.png){: width="30%" height="30%"}

이 제약조건들이 적용된 XML 코드는 다음과 같다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="80dp"
        android:layout_marginTop="80dp"
        android:text="Button"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="40dp"
        android:layout_marginLeft="40dp"
        android:layout_marginTop="80dp"
        android:text="Button"
        app:layout_constraintStart_toEndOf="@+id/button"
        app:layout_constraintTop_toTopOf="parent" />
</android.support.constraint.ConstraintLayout>
~~~

&nbsp;

&nbsp;
### 3. 바이어스(Bias)
가로 방향 연결선을 2개 모두 연결하거나 세로 방향 연결선을 2개 모두 연결하면 각각 가로방향, 세로방향의 가운데에 위치하게 되고, 이 때에는 마진 값은 고려되지 않는다. 이 때에는 화면의 왼쪽이나 오른쪽, 혹은 위나 아래 쪽으로 치우치게 하기 위해서는 bias를 조절하면 된다. bias는 Attributes 탭에서 설정할 수 있다.

![constraint](https://nobbaggu.github.io/images/android/4/3.png){: width="70%" height="70%"}

바이어스를 결정한 다음 XML 코드를 보면 자동으로 추가가 되어있을 것이다.

~~~ xml
<Button
	...
app:layout_constraintVertical_bias="0.732"
app:layout_constraintHorizontal_bias="0.699"
	... />
~~~

&nbsp;
### 4. 가이드라인(GuideLine)

![constraint](https://nobbaggu.github.io/images/android/4/4.png){: width="70%" height="70%"}

가이드라인은 여러 개의 뷰들을 정렬시킬 때 사용할 수 있다. 가이드라인은 크기가 0으로 실제 만들어진 앱에서는 보이지 않는다.

아래는 가이드라인이 추가 된 XML 코드이다.

~~~ xml
<android.support.constraint.Guideline
    android:id="@+id/guideline"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    app:layout_constraintGuide_begin="65dp" />
~~~

