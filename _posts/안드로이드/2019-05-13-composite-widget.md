---
title: (안드로이드) 40. 복합 위젯(Composite Widget)
date: 2019-05-13T21:00:00+09:00
author: SWnomad
layout: post
categories: 안드로이드
tags:
  - 복합 위젯
  - Composite Widget
  - 커스텀뷰
  - customview
  - 상속
---

리스트뷰와 그리드뷰를 만들 때 하나의 아이템이 표시되는 레이아웃을 하나의 위젯으로 만들어 사용했었다. 이와 같이 뷰를 상속하여 재정의 하는 방식으로 안드로이드에서 제공하는 기본 위젯을 조합하면 더 복잡한 위젯을 만들 수 있고, 재사용성이 높아지고 코드도 간결해진다.

<br>
안드로이드 API는 날짜선택 위젯과 시간선택 위젯을 각각 제공한다. 두 위젯을 한 번에 보여주는 복합위젯을 만들어보겠다.

<br>
### 1. 복합 위젯 레이아웃

\<picker.xml\>
~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:gravity="center">

    <DatePicker
        android:id="@+id/datePicker"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">
    </DatePicker>

    <CheckBox
        android:id="@+id/enableTimeCheckBox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="시간 보이기"/>

    <TimePicker
        android:id="@+id/timePicker"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">
    </TimePicker>

</LinearLayout>
~~~

<br>
### 2. 복합위젯 컨트롤 코드

\<DateTimePicker.java\>
~~~ java
package io.swnomad.sampledatetimepicker;

import android.content.Context;
import android.util.AttributeSet;
import android.view.LayoutInflater;
import android.view.View;
import android.widget.CheckBox;
import android.widget.CompoundButton;
import android.widget.DatePicker;
import android.widget.LinearLayout;
import android.widget.TimePicker;

import java.util.Calendar;

public class DateTimePicker extends LinearLayout {

    //OnDateChangedListener 리스너와 OnTimeChangedListener 리스너의 이벤트 처리를 한 번에 하기위한 새로운 인터페이스 정의
    public interface OnDateTimeChangedListener{
        void onDateTimeChanged(DateTimePicker view, int year, int monthOfYear, int dayOfYear, int hourOfDay, int minute);
    }

    private OnDateTimeChangedListener listener;
    private DatePicker datePicker;
    private TimePicker timePicker;
    private CheckBox enableTimeCheckBox;

    public DateTimePicker(Context context){
        super(context);
        init(context); //객체 초기화
    }

    public DateTimePicker(Context context, AttributeSet attrs){
        super(context, attrs);
        init(context);
    }

    private void init(Context context){
        //레이아웃 인플레이션
        LayoutInflater inflater = (LayoutInflater) context.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
        inflater.inflate(R.layout.picker, this  , true);

        //Calendar 객체를 사용해 시간 정보 참조
        Calendar calendar = Calendar.getInstance();
        final int curYear = calendar.get(Calendar.YEAR);
        final int curMonth = calendar.get(Calendar.MONDAY);
        final int curDay = calendar.get(Calendar.DAY_OF_MONTH);
        final int curHour = calendar.get(Calendar.HOUR_OF_DAY);
        final int curMinute = calendar.get(Calendar.MINUTE);

        //날짜 선택 위젯 초기화 및 이벤트 처리 리스너 설정
        datePicker = findViewById(R.id.datePicker);
        datePicker.init(curYear, curMonth, curDay, new DatePicker.OnDateChangedListener() {
            @Override
            public void onDateChanged(DatePicker view, int year, int monthOfYear, int dayOfMonth) {
                if(listener!=null){ //새로 정의한 OnDateTimeChangedListener 리스너로 이벤트 전달
                    listener.onDateTimeChanged(DateTimePicker.this, year, monthOfYear, dayOfMonth,
                            timePicker.getCurrentHour(), timePicker.getCurrentMinute());
                }
            }
        });

        //체크박스 이벤트 처리 리스너 설정
        enableTimeCheckBox = findViewById(R.id.enableTimeCheckBox);
        enableTimeCheckBox.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                timePicker.setEnabled(isChecked); //체크 되어있으면 TimePicker 동작
                timePicker.setVisibility(isChecked ? View.VISIBLE : View.INVISIBLE); //체크 되어있으면 TimePicker 보이기
            }
        });

        timePicker = findViewById(R.id.timePicker);

        //시간 선택 위젯 초기화
        timePicker.setCurrentHour(curHour);
        timePicker.setCurrentMinute(curMinute);
        timePicker.setVisibility(View.GONE);

        //시간 선택 위젯 이벤트 처리 리스너 설정
        timePicker.setOnTimeChangedListener(new TimePicker.OnTimeChangedListener() {
            @Override
            public void onTimeChanged(TimePicker view, int hourOfDay, int minute) {
                if(listener!=null){//새로 정의한 OnDateTimeChangedListener 리스너로 이벤트 전달
                    listener.onDateTimeChanged(DateTimePicker.this, datePicker.getYear(), datePicker.getMonth(),
                            datePicker.getDayOfMonth(), hourOfDay, minute);
                }
            }
        });
    }

    // 이 클래스의 객체에 날짜,시간 변경 이벤트 리스너를 설정하기 위한 메소드 정의
    public void setOnDateTimeChangedListener(OnDateTimeChangedListener listener){
        this.listener = listener;
    }

}
~~~

Calendar 객체를 사용하면 현재 년도, 월, 일, 시간, 분 정보를 가져올 수 있다. 이것들을 가지고 날짜선택 위젯과 시간선택 위젯을 초기화하였다.

그리고 체크박스의 선택 여부에따라 시간선택 위젯을 보이거나 보이지 않게 하였다.

이 코드에서 중요한 부분은 DatePicker와 TimePicker의 날짜/시간 변경 이벤트 리스너가 호출되면 직접 정의한 OnDateTimeChangedLister 리스너로 전달해주는 것이다. 이렇게 하면 날짜와 시간 이벤트를 함께 처리할 수 있다.

<br>

이제 MainActivity에서 이 복합 위젯을 화면에 띄워보겠다.

<br>
\<activity_main.xml\>
~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="10dp"/>

    <ScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <io.swnomad.sampledatetimepicker.DateTimePicker
            android:id="@+id/picker"
            android:layout_width="match_parent"
            android:layout_height="match_parent"/>

    </ScrollView>


</LinearLayout>
~~~

텍스트뷰는 DateTimePicker의 날짜/시간을 텍스트로 표시해주기 위해 만들었다.

<br>
\<MainActivity.java\>
~~~ java
package io.swnomad.sampledatetimepicker;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.TextView;

import java.text.SimpleDateFormat;
import java.util.Calendar;

public class MainActivity extends AppCompatActivity {

    final SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy년 MM월 dd일 HH시 mm분");

    TextView textView;
    DateTimePicker picker;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = findViewById(R.id.textView);
        picker = findViewById(R.id.picker);

        //텍스트뷰 표시 시간 초기화
        Calendar calendar = Calendar.getInstance();
        textView.setText(dateFormat.format(calendar.getTime()));

        //날짜, 시간 변경 이벤트 처리 리스너 설정
        picker.setOnDateTimeChangedListener(new DateTimePicker.OnDateTimeChangedListener() {
            @Override
            public void onDateTimeChanged(DateTimePicker view, int year, int monthOfYear, int dayOfYear, int hourOfDay, int minute) {
                Calendar calendar = Calendar.getInstance();
                calendar.set(year, monthOfYear, dayOfYear, hourOfDay, minute);

                //설정한 시간 텍스트뷰에 표시
                textView.setText(dateFormat.format(calendar.getTime()));
            }
        });
    }
}
~~~

SimpleDateFormat 클래스는 날짜를 우리가 원하는 포맷으로 표시해주는 역할을 한다. 날짜,시간 변경 이벤트 리스너로 이벤트가 전달되면 텍스트뷰에 우리가 정의한 포맷으로 날짜, 시간이 표시되도록 하였다.

<br>
앱을 실행하면 아래의 화면을 확인할 수 있다.

![1](/images/android/40/1.jpg){: width="30%" height="30%"}
![2](/images/android/40/2.jpg){: width="30%" height="30%"}