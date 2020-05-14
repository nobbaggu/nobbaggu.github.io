---
title: (안드로이드) 54. SQLiteOpenHelper 클래스
date: 2019-06-08T15:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - sqlite
  - sqlite3
  - database
  - table
  - sql
  - 데이터베이스
  - 테이블
  - SQLiteDatabase
  - SQLiteOpenHelper
---

[SQLite3 사용법](https://github.com/swnomad/rssfeeder){: target="_blank" }

이 포스팅은 위 링크에서 설명한 SQLite 데이터베이스의 기본 사용법을 알고나서 더욱 편리하게 데이터베이스 열기 및 수정을 위한 SQLiteOpenHelper 클래스의 사용 목적과 방법에 대한 글이다.

<br>
## SQLiteOpenHelper 클래스를 사용하는 이유
* * *

구글 플레이스토어에 올린 앱이 SQLite를 사용할 경우 DB의 구조가 변경될 경우가 있다. 예를들면 사람들의 정보를 저장하기위해 '이름', '나이' column만 사용했었는데 이후에 '휴대폰 번호' column을 추가해야 하는 경우이다. 이 때 앱을 버전 업그레이드하면서 새로 배포할 때, 기존의 테이블을 drop하고 새로 만들도록하면 기존 사용자의 데이터가 모두 날아가버리는 불상사가 생겨버린다. 따라서 신규 사용자의 경우는 단순히 새로운 구조의 테이블을 생성하면 되지만, 기존 사용자의 경우 기존 데이터를 유지하면서 업그레이드 해주어야한다.

이러한 경우 사용할 수 있는것이 SQLiteOpenHelper이다.

~~~ java
public SQLiteOpenHelper(Context context, String name, SQLiteDatabase.CursorFactory factory, int version)
~~~

SQLiteOpenHelper 객체를 만들 때 생성자의 파라미터로는 4개의 변수가 전달되는데, name은 데이터베이스의 이름이다. 그리고 마지막 파라미터인 정수 version으로 전달되는 버전정보를 기존의 버전정보와 다르게 지정하여 DB의 구조를 업그레이드 할 수 있다.

SQLiteOpenHelper 클래스의 주 사용 목적은 데이터베이스의 생성 및 업그레이드가 필요할 시에 자동으로 콜백 메소드가 호출된다는 점에서 온다. 이 콜백 메소드를 재정의함으로서 DB의 생성, 업그레이드 등의 상태에 맞게 알맞은 처리가 가능하다.

<br>

~~~ java
public abstract void onCreate(SQLiteDatabase db)
public abstract void onOpen(SQLiteDatabase db)
public abstract void opUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
~~~

<br>

생성자의 파라미터로 전달된 DB의 이름이 없다면 `onCreate()` 메소드가 호출되고 이미 사용중인 DB라면 `onUpgrade()` 메소드가 호출된다.

<br>

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

        <EditText
            android:id="@+id/dbNameInput"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:hint="데이터베이스 이름"
            android:textSize="16dp"
            android:layout_weight="3"
            android:layout_margin="5dp"/>

        <Button
            android:id="@+id/createDbBtn"
            android:layout_width="120dp"
            android:layout_height="wrap_content"
            android:textSize="16dp"
            android:text="DB 생성"
            android:layout_margin="5dp"/>

    </LinearLayout>

</LinearLayout>
~~~

~~~ java
public class MainActivity extends AppCompatActivity {

    SQLiteDatabase database;
    private static final int DB_VERSION = 1; // 데이터베이스 버전

    private DatabaseOpenHelper helper;

    EditText dbNameInput;
    EditText tableNameInput;

    String dbName;
    String tableName;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        /* ********** XML에 정의된 UI객체 참조 ************ */
        dbNameInput = findViewById(R.id.dbNameInput);
        Button createDbBtn = findViewById(R.id.createDbBtn);
        /* ************************************************* */

        /* 데이터베이스 생성 버튼 클릭 이벤트 */
        createDbBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                try{
                    /* 데이터베이스 생성 */
                    dbName = dbNameInput.getText().toString();
                    helper = new DatabaseOpenHelper(MainActivity.this, dbName, null, DB_VERSION);
					
					// helper 객체를 통해 db 불러오기
                    /* 1. dbName과 같은 이름의 DB가 없으면 helper 클래스의 onCreate() 호출
                     * 2. dbName과 같은 이름의 DB가 있으면 version 전달되면서 helper 클래스의 onUprgrade() 호출 */
                    database = helper.getWritableDatabase();

                }catch(Exception e){
                    e.printStackTrace();
                }
            }
        });

    }

    class DatabaseOpenHelper extends SQLiteOpenHelper {

        public DatabaseOpenHelper(Context context, String name, SQLiteDatabase.CursorFactory factory, int version) {
            super(context, name, factory, version);
        }

        @Override
        public void onCreate(SQLiteDatabase db) {
            Toast.makeText(MainActivity.this, "Helper클래스 onCreate() 호출됨", Toast.LENGTH_SHORT).show();

            /* DB가 처음 만들어졌으므로 테이블 생성 */
            if(db!=null)
            {
                try{
                    tableName = tableNameInput.getText().toString();
                    /* 테이블 생성을 위한 SQL문 작성 */
                    String sql = "create table if not exists " + tableName + "(_id integer PRIMARY KEY autoincrement, name text, age integer);";
                    db.execSQL(sql); //sql문 실행
                }catch(Exception e){
                    e.printStackTrace();
                }

            }
        }

        @Override
        public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
            Toast.makeText(MainActivity.this, "Helper클래스 onUpgrade() 호출됨: "
                    + oldVersion +" -> " + newVersion, Toast.LENGTH_SHORT).show();
        }

        @Override
        public void onOpen(SQLiteDatabase db) {
            //Toast.makeText(MainActivity.this, "Helper클래스 onOpen() 호출됨", Toast.LENGTH_SHORT).show();
        }
    }
}
~~~

콜백 메소드를 확인하기 위해 토스트 메시지를 만들었다. 위 앱을 실행하면 아래와 같은 화면이 나타난다.

![1](https://nobbaggu.github.io/images/android/54/1.jpg){: width="30%" height="30%"}

<br>

위 코드에서 버전을 아래와 같이 2로 바꾸고 다시 실행해보자.
~~~ java
private static final int DB_VERSION = 2; // 데이터베이스 버전
~~~

그럼 아래와 같은 화면을 볼 수 있다.

![2](https://nobbaggu.github.io/images/android/54/2.jpg){: width="30%" height="30%"}

<br>

사실 위 2가지 서로 다른 상태 메소드 이후에 곧바로 onOpen() 메소드가 호출된다. 여기에서는 DB 조회와 같이 사용 코드를 작성할 수 있다.

이렇게 버전 정보를 넣어줌으로써 다른 메소드가 호출된다. 상황에 따라 각 메소드를 재정의 함으로써 신규 사용자와 기존 사용자 사이의 유연한 코드 처리가 가능하다. 이 부분이 SQLiteOpenHelper를 사용하는 주 목적이다. 나머지 데이터 조작에 관한 부분은 일반적인 SQLite3 사용법과 동일하다. `execSQL()` 메소드나 `rawQuery()` 메소드를 통해 데이터 추가/삭제 및 조회를 할 수 있다.