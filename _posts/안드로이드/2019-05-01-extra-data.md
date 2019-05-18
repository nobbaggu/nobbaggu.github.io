---
title: (안드로이드) 17. 부가 데이터
date: 2019-05-01T10:00:00+09:00
author: SWnomad
layout: post
categories: 안드로이드
tags:
  - 액티비티
  - activity
  - 인텐트
  - intent
  - putExtra
  - startActivity
  - startActivityForResult
  - Parcelable
  - Bundle
  - 번들
---

액티비티를 띄울 때 사용하는 인텐트에는 액션과 데이터 이외에도 부가적인 데이터를 넣어서 전달할 수도 있다. 인텐트 안에는 번들(Bundle) 객체가 들어 있는데, 이 객체는 해시 테이블과 유사하여 key-value 쌍으로 데이터를 입력, 출력할 수 있다.

![1](/images/android/17/1.png){: width="60%" height="60%"}

번들 객체에는 기본 자료형 뿐만 아니라 문자열이나 배열등도 넣어서 전달할 수 있다. 그러나 객체(Object) 자료형은 그냥 전달할 수 없고 바이트 배열로 변환하거나 직렬화(serialization) 한 후 전달할 수 있다. 안드로이드에서는 Serializable 인터페이스와 유사하지만 직렬화한 이후 크기가 작은 Parcelable 인터페이스를 권장한다.

Parcelable 인터페이스를 구현하는 클래스는 다음 두 가지의 메소드를 구현(implement)해야 한다.

~~~ java
public abstract int describeContents()
public abstract void wrtieToParcel(Parcel dest, int flags)
~~~

writeToParcel() : 객체가 가지는 데이터를 Parcel 객체로 만들어주는 역할

&nbsp;
Parcelable 인터페이스를 구현하는 클래스를 하나 만들어보자.
~~~ java
package io.swnomad.sampleparcelable;

import android.os.Parcel;
import android.os.Parcelable;

public class SimpleData implements Parcelable {

    int number;
    String message;

    public SimpleData(int number, String message){
        this.number = number;
        this.message = message;
    }

    public SimpleData(Parcel src){
        number = src.readInt();
        message = src.readString();
    }

    //Parcelable 인터페이스를 구현하는 클래스는 필수로 CREATOR 상수를 정의해야 한다.
    public static final Parcelable.Creator CREATOR = new Parcelable.Creator(){
        public SimpleData createFromParcel(Parcel in){
            return new SimpleData(in);
        }

        public SimpleData[] newArray(int size){
            return new SimpleData[size];
        }
    };

    // Parcelable 인터페이스 구현에서 필수로 정의해야하는 메소드
    @Override
    public int describeContents() {
        return 0;
    }

    // 객체가 가진 데이터를 Parcel 객체로 만들어주는 메소드
    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeInt(number);
        dest.writeString(message);
    }
    /* ******************************************************** */

}
~~~

SimpleData 클래스는 Parcelable 클래스를 구현하므로 생성자를 통해 객체를 만들면 자동으로 Parcel 객체 형태로 만들어진다.

이제 SimpleData를 인텐트에 부가데이터로 담아서 다른 액티비티로 전달해보자.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <EditText
        android:id="@+id/numberInput"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#aaffffff"
        android:hint="숫자를 입력하세요"
        android:textSize="24dp"
        android:layout_margin="10dp"/>

    <EditText
        android:id="@+id/stringInput"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#aaffffff"
        android:hint="문자열을 입력하세요"
        android:textSize="24dp"
        android:layout_margin="10dp"/>

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="메뉴 액티비티 띄우기"
        android:textSize="24dp"
        android:layout_marginTop="200dp"
        android:layout_gravity="center_horizontal"/>

</LinearLayout>
~~~

두 개의 입력상자에 하나는 숫자, 하나는 문자열을 입력받아서 버튼을 눌러서 다른 액티비티로 전달하는 Java 코드를 작성해보자.

~~~ java
package io.swnomad.sampleparcelable;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;

public class MainActivity extends AppCompatActivity {
    public static final String KEY_SIMPLE_DATA = "data";

    Button button;
    EditText numberInput;
    EditText stringInput;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        numberInput = findViewById(R.id.numberInput);
        stringInput = findViewById(R.id.stringInput);
        button = findViewById(R.id.button);

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(getApplicationContext(), MenuActivity.class);

                int num = Integer.parseInt(numberInput.getText().toString());
                String str = stringInput.getText().toString();

                SimpleData data = new SimpleData(num, str); //SimpleData 객체 생성
                // SimpleData 클래스는 Parcelable 인터페이스를 구현하므로 객체가 Parcel 객체 형태로 만들어진다.

                intent.putExtra(KEY_SIMPLE_DATA, data); //인텐트에 부가데이터로 SimpleData 객체 넣기
                startActivity(intent); //액티비티 띄우기
            }
        });

    }
}
~~~

버튼을 클릭하면 두 개의 입력상자로부터 숫자와 문자열을 얻어와서 SimpleData 생성자의 전달인자로 넣어 객체를 생성하는데, Parcelable 인터페이스를 구현했으므로 Parcel 객체로 만들어진다. 그리고 인텐트에 부가데이터로 담아서 MenuActivity로 전달한다.

메뉴 액티비티는 아래와 같은 코드로 구성되어있다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    
    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:layout_marginTop="300dp"
        android:text="전달받은 데이터"
        android:textSize="24dp"/>

</LinearLayout>
~~~

레이아웃에는 전달받은 데이터를 보여줄 텍스트뷰 하나만 있다.

~~~ java
package io.swnomad.sampleparcelable;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.TextView;

public class MenuActivity extends AppCompatActivity {

    public static final String KEY_SIMPLE_DATA = "data";

    TextView textView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_menu);

        textView = findViewById(R.id.textView);

        Intent intent = getIntent(); //인텐트 받기

        Bundle bundle = intent.getExtras(); //인텐트에서 부가 데이터 빼서 bundle 객체로 저장
        SimpleData data = (SimpleData) bundle.getParcelable(KEY_SIMPLE_DATA); //Parcel 객체 빼내서 SimpleData 타입으로 변환

        //텍스트뷰에 전달받은 부가 데이터 표시
        textView.setText("전달받은 데이터\n"+"숫자: "+data.number+"\n"+"문자열: "+data.message);
    }
}
~~~

getIntent() 메소드를 통해 인텐트를 받는다. 그리고 번들 객체로 부가 데이터를 받는다. 그리고 SimpleData 자료형으로 형변환을 하고 텍스트뷰에 데이터가 보이도록 설정한다.

![2](/images/android/17/2.jpg){: width="30%" height="30%"}
![3](/images/android/17/3.jpg){: width="30%" height="30%"}
![4](/images/android/17/4.jpg){: width="30%" height="30%"}