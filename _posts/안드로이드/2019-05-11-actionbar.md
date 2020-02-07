---
title: (안드로이드) 32. 액션바(ActionBar)
date: 2019-05-11T11:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 액션바
  - actionbar
  - onCreateOptionsMenu
  - onOptionsItemSelected
  - 메뉴
  - menu
---

안드로이드 액션바(ActionBar)는 기본적으로 앱의 타이틀을 보여주는 곳이다. 또한 여러가지 옵션 메뉴(Option Menu)들을 표시할 수도 있다.

![1](/images/android/32/1.png){: width="30%" height="30%"}

<br>
액션바를 보이거나 감추고 싶다면 다음의 메소드를 사용하면 된다.

~~~java
ActionBar actionbar = getActionBar(); //액션바 객체 참조
actionbar.hide(); //액션바 숨기기
actionbar.show(); //액션바 보이기
~~~

<br>
옵션 메뉴는 각각의 액티비티마다 설정할 수 있다. 옵션 메뉴를 설정하려면 아래의 메소드를 사용하면 된다.

~~~ java
boolean onCreateOptionsMenu(Menu menu) //액티비티가 만들어지면서 호출
boolean onOptionsItemSelected(MenuItem item) //메뉴의 아이템을 클릭했을 때 호출
~~~

이 메소드는 액티비티가 생성될 때 자동으로 호출되어 액티비티에 메뉴를 추가한다.

먼저 메뉴를 위한 XML 레이아웃을 만들어야 한다. /res 폴더에 'menu' 폴더를 추가하고 /res/menu 폴더에 메뉴를 위한 XML 레이아웃 리소스 파일을 생성하고 각각의 메뉴를 위한 \<item\> 태그를 추가하면 된다. 이 XML 파일을 `onCreateOptionsMenu()` 메소드 안에서 인플레이션 한다. 그러면 액티비티가 만들어질 때 액션바에 자동으로 메뉴가 포함된다. 그리고 각각의 메뉴 아이템을 클릭했을 때 나타나기를 원하는 기능은 `onOptionsItemSelected()` 메소드에서 처리하면 된다.

<br>
~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item android:id = "@+id/menu_refresh"
        android:title="새로고침"
        android:icon="@drawable/menu_refresh"
        app:showAsAction="always" />

    <item android:id = "@+id/menu_search"
        android:title="새로고침"
        android:icon="@drawable/menu_search"
        app:showAsAction="always" />

    <item android:id = "@+id/menu_settings"
        android:title="새로고침"
        android:icon="@drawable/menu_settings"
        app:showAsAction="always" />

</menu>
~~~

메뉴를 정의한 XML에는 '새로고침', '찾기', '설정' 3개의 아이템을 만들었다. 이제 이 레이아웃을 인플레이션하고 아이템을 클릭했을 때 처리하는 코드를 작성해보자.

<br>
~~~java
package io.swnomad.sampleoptionmenu;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

    }

    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_main, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int curId = item.getItemId();

        switch(curId){
            case R.id.menu_refresh:
                Toast.makeText(this, "새로고침", Toast.LENGTH_LONG).show();
                break;
            case R.id.menu_search:
                Toast.makeText(this, "찾기", Toast.LENGTH_LONG).show();
                break;
            case R.id.menu_settings:
                Toast.makeText(this, "설정", Toast.LENGTH_LONG).show();
                break;
            default:
                break;
        }

        return super.onOptionsItemSelected(item);
    }
}
~~~

`onCreateOptionsMenu()` 메소드와 `onOptionsItemSelected()` 메소드를 재정의 해주었다. 각 아이템이 클릭되면 간단하게 토스트 메시지를 띄우도록 하였다.

앱을 실행하면 아래의 결과를 확인할 수 있다.

![2](/images/android/32/2.jpg){: width="30%" height="30%"}