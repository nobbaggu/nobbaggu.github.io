---
title: (안드로이드) 14. 인플레이션(Inflation)
date: 2019-04-29T19:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 액티비티
  - activity
  - 인플레이션
  - inflation
---

&nbsp;
### 1. 인플레이션(Inflation)

XML 레이아웃은 화면 배치만 정의할 뿐이다. 여기에 화면의 기능을 담당하는 Java 소스파일이 필요하다. 즉, 하나의 화면이 동작하기 위해서는 메인 레이아웃 XML 파일 하나와 Java 소스파일이 필요하다. 이렇게 화면의 구성과 기능을 분리하는 이유는 화면만 따로 분리하면 이해도 쉽고 관리도 편리하기 때문이다.

처음 앱을 만들었을 때 보이는 화면 역시 Java 소스코드에서 화면을 설정한 것이다. 이 부분은 setContentView 메소드가 담당한다.

~~~ java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main); // activity_main.xml 레이아웃을 앱 화면으로 설정
    }
}
~~~

MainActivity 클래스를 보면 AppCompatActivity 클래스를 상속한다. AppCompatActivity 클래스에는 화면에 필요한 기능들이 정의되어있다.

앱을 실행했을 때 화면이 보인다는 것은 화면 배치 정보(XML 코드)가 메모리에 로딩되어 객체로 존재한다는 것이다. 이 과정을 레이아웃 인플레이션(Inflation)이라고 한다.

&nbsp;
__레이아웃 인플레이션 : XML 레이아웃의 화면배치 정보가 메모리에 로딩되어 객체화 되는 과정__

![layout-inflation](/images/android/14/layout-inflation.jpg){: width="70%" height="70%"}

&nbsp;
Java에서는 객체로 메모리에 존재하지 않는 것을 참조할 수 없다. 만약에 존재하지 않는 것을 참조하려 하면 Null Pointer Exception 에러가 일어나고 프로그램이 중지되는 심각한 오류가 발생한다.

먼저 xml 레이아웃에 버튼을 하나 정의해보자.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="버튼" />

</LinearLayout>
~~~

그리고 setContentView(R.layout.activity_main) 메소드로 xml 파일을 인플레이션 하기 전에 버튼을 참조하여 보자.

~~~ java
package io.swnomad.samplelayoutinflater;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        Button button = findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(getApplicationContext(), "버튼이 눌렸습니다.", Toast.LENGTH_LONG).show();
            }
        });
        setContentView(R.layout.activity_main);
    }
}
~~~

위 Java 코드에서는 setContentView 메소드를 통해 레이아웃 파일을 메모리에 로딩하여 객체로 만들기 전에 레이아웃에 정의 된 Button을 참조하려 한다.

![2](/images/android/14/2.jpg){: width="30%" height="30%"}

앱을 실행하자마자 오류가 나며 프로그램이 종료된다. 앱이 실행되고 findViewById 메소드를 통해 Button을 참조하려는데 메모리에 존재하지 않아 참조하지 못하는 것이다.

좀 더 정확히 알아보기 위해 예외처리 구문을 넣어보았다.

~~~ java
package io.swnomad.samplelayoutinflater;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        try{
            Button button = findViewById(R.id.button);
            button.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Toast.makeText(getApplicationContext(), "버튼이 눌렸습니다.", Toast.LENGTH_LONG).show();
                }
            });
        }catch(NullPointerException e){
            Toast.makeText(getApplicationContext(), "Null Pointer Exception 발생", Toast.LENGTH_LONG).show();
        }

        setContentView(R.layout.activity_main);
    }
}
~~~

![3](/images/android/14/3.jpg){: width="30%" height="30%"}

확실히 Null Pointer Exception이 발생하였다.

정리하자면, setContentView 메소드는 화면에 보일 레이아웃을 설정하고 메모리에 로드하여 객체화 시켜주는 아주 중요한 메소드이다.

&nbsp;
### 2. LayoutInflater 클래스

setContentView 메소드는 액티비티의 전체 화면을 설정하는 역할을 한다. 안드로이드에서는 부분화면을 따로 XML로 정의하여 로드하여 사용할 수 있도록 LayoutInflater 클래스를 제공한다. LayoutInflater는 안드로이드 시스템 서비스에서 제공하므로 다음의 메소드를 통해 인플레이터 객체를 참조하여 사용할 수 있다.

~~~ java
getSystemService(Context.LAYOUT_INFLATER_SERVICE)
~~~

간략하게 설명하자면 시스템 서비스는 안드로이드 디바이스가 실행되면 자동으로 운영체제에서 제공한다. 시스템 서비스가 제공하는 서비스들은 getSystemService() 메소드를 통해 사용하면 된다.

다시 본론으로 돌아와, 부분화면을 XML 레이아웃으로 정의한 후 화면에 띄워주는 과정은 다음의 그림으로 요악된다.

![4](/images/android/14/4.png){: width="70%" height="70%"}

부분화면을 정의한 XML 레이아웃을 LayoutInflater를 통해 인플레이션 시켜 뷰 그룹으로 객체화 시킨다. 이 뷰 그룹을 일반 뷰처럼 메인 레이아웃에 추가하여 사용하면 된다.

먼저 메인 레이아웃의 XML 코드를 다음과 같이 작성한다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="버튼을 누르면 부분화면이 뜹니다."
        android:textSize="20dp"/>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="버튼" />

    <LinearLayout
        android:id="@+id/container"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
    </LinearLayout>

</LinearLayout>
~~~

마지막에 추가된 LinearLayout은 부분화면을 담을 부모 컨테이너로 사용할 것이다.

그리고 부분 화면 레이아웃은 다음과 같이 정의한다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center_vertical"
    android:background="#44ff0000">

    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="인플레이션 된 화면입니다."
        android:gravity="center_horizontal"
        android:textSize="30dp"
        android:textStyle="bold|italic"/>

    <Button
        android:id="@+id/button2"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="눌러보세요!"
        android:layout_marginTop="20dp"/>

</LinearLayout>
~~~

텍스트뷰 하나와 버튼 하나가 있다.

이제 자바 소스코드에서 인플레이션을 하여 부분화면을 띄워본다.

~~~ java
package io.swnomad.samplelayoutinflater;

import android.content.Context;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    LinearLayout container;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        container = findViewById(R.id.container); //부분화면을 띄울 컨테이너 참조
        Button button = findViewById(R.id.button); //버튼 참조

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                /* 인플레이터 객체를 만들고 이를 참조하여 sub_layout.xml 레이아웃을 인플레이션
                 * 한 이후 container 객체에 설정하기 */
                LayoutInflater inflater = (LayoutInflater) getSystemService(Context.LAYOUT_INFLATER_SERVICE);
                inflater.inflate(R.layout.sub_layout, container, true);

                /* sub_layout.xml이 인플레이션 되어 container 객체로 전달되었기 때문에
                   container 객체의 findViewById 메소드를 통해 sub_layout의 뷰를 참조할 수 있다. */
                Button button2 = container.findViewById(R.id.button2);
                button2.setOnClickListener(new View.OnClickListener() {
                    @Override
                    public void onClick(View v) {
                        TextView textView = container.findViewById(R.id.textView);
                        textView.setText("잘했어요!");
                    }
                });
            }
        });

    }
}
~~~

메인 화면의 버튼을 클릭하면 부분화면 레이아웃이 인플레이트(객체화)되어 메모리에 로딩되고, 이를 container 객체에 설정한다. 그럼 부분화면이 나타난다. 부분화면에 정의된 텍스트뷰와 버튼은 메모리에 인플레이션이 되었기 때문에 사용할 수 있게 되었다. 다만 container 객체에 설정되었기 때문에 container 객체의 findViewById() 메소드를 사용해야 한다. 아무튼 부분화면의 버튼을 누르면 부분화면의 텍스트뷰의 텍스트가 바뀔 것이다.

![5](/images/android/14/5.jpg){: width="30%" height="30%"}
![6](/images/android/14/6.jpg){: width="30%" height="30%"}
![7](/images/android/14/7.jpg){: width="30%" height="30%"}