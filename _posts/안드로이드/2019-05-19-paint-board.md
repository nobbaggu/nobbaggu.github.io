---
title: (안드로이드) 44 - 페인트보드 만들기
date: 2019-05-19T18:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - 페인트보드
  - paintboard
  - canvas
  - 캔버스
  - 페인트
  - paint
  - 비트맵
  - bitmap
---

페인트보드를 만들기 위해서는 기본적으로 터치하여 그림을 그릴 공간이 필요하기 때문에 아래 코드를 작성한다.

\<PaintBoard.java\>
~~~ java
public class PaintBoard extends View {

    int lastX = -1;
    int lastY = -1;
    Canvas mCanvas;
    Paint mPaint;
    Context mContext;
    Bitmap mBitmap;

    public PaintBoard(Context context) {
        super(context);
        mContext = context;
    }

    public PaintBoard(Context context, AttributeSet attrs) {
        super(context, attrs);
        mContext = context;
    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        mCanvas = new Canvas();
        mPaint = new Paint();
        mPaint.setStrokeWidth(5.0F);
        mBitmap = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_8888);
        mCanvas.setBitmap(mBitmap); //비트맵 객체에 캔버스 달아 그림 그릴 수 있도록 하기위한 설정
        /* 이후부터는 mCanvas 위에 그리는 그림은 mBitmap에 적용된다. */
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        int action = event.getAction(); // 액션 정보 가져오기
        int x = (int) event.getX(); // X좌표
        int y = (int) event.getY(); // Y좌표

        switch(action){

            /* 이동중에 이전 좌표값과 현재 좌표값 연결해서 선 그리기*/
            case MotionEvent.ACTION_MOVE:
                if(lastX != -1){
                    mCanvas.drawLine(lastX, lastY, x, y, mPaint);
                }
                lastX = x;
                lastY = y;

                break;

            /* 손을 떼면 최종 좌표값 (-1, -1)로 리셋*/
            case MotionEvent.ACTION_UP:
                lastX = -1;
                lastY = -1;

                break;
        }

        invalidate(); // 다시 onDraw() 메소드 호출하여 그리기

        return true;
    }

    @Override
    protected void onDraw(Canvas canvas) {
        /* 그림이 그려진 비트맵을 화면에 표시하기 */
        canvas.drawBitmap(mBitmap, 0, 0, mPaint);
    }

    /* 페인트 색 설정을 위한 메소드 */
    public void setPaintColor(@ColorInt int color){
        mPaint.setColor(color);
    }
}
~~~
그림을 그리기위해서 비트맵과 캔버스, 페인트 객체를 만들었다. 비트맵 객체에 캔버스를 설정하여 컨버스에 그림을 그리면 비트맵 객체에 적용되도록 했다. 페인트 객체는 그림의 색, 선의 두께 등을 지정하기 위한 것이다.

그 다음으로 커스텀뷰의 `onTouchEvent()` 메소드를 구현하여 터치를 통해 그림이 그려지도록 했다. 터치 이벤트의 액션정보를 읽어서 어떤 이벤트인지 알 수 있다. `MotionEvent.ACTION_UP`을 통해 손을 떼면 (X,Y) 좌표가 (-1, -1)이 되도록 한다. 그리고 `MotionEvent.ACTION_MOVE` 일 때 X나 Y가 하나라도 -1이 아니면 계속 그림을 중이라는 의미이므로 캔버스의 `drawLine()` 메소드를 통해 그림을 그린다. 이 때 이전 좌표와 현재 좌표를 잇는 라인을 그리도록 했다.

`setPaintColor(int color)` 메소드가 호출되면 파라미터로 전달되는 색을 통해 페인트의 색을 설정한다.

<br>
<br>

애플리케이션을 시작하면 보일 화면으로 아래의 XML 코드를 작성한다.

\<activity_paint_board\>
~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <LinearLayout
        android:id="@+id/toolsLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        android:padding="5dp"
        android:background="#ffffffff">

        <Button
            android:id="@+id/colorBtn"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:drawableTop="@drawable/colorpalette"
            android:drawablePadding="2dp"
            android:text="색상"/>

    </LinearLayout>

    <LinearLayout
        android:id="@+id/boardLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">
    </LinearLayout>

</LinearLayout>
~~~
색을 바꾸기 위한 버튼이 하나 있고 그 아래에는 위에서 만든 커스텀뷰를 추가할 공간이 있도록 구성하였다.

<br>
<br>

\<PaintBoardActivity.java\>
~~~ java
public class PaintBoardActivity extends AppCompatActivity {
    public final static int REQUEST_CODE_COLOR_PALETTE_DIALOG = 101;

    LinearLayout boardLayout;
    Button colorBtn;
    PaintBoard board;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_paint_board);

        /* 페인트보드 레이아웃에 페인트보드 뷰 객체 추가 */
        boardLayout = findViewById(R.id.boardLayout);
        board = new PaintBoard(this);
        boardLayout.addView(board);

        colorBtn = findViewById(R.id.colorBtn);

        colorBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent(getApplicationContext(), ColorPaletteDialog.class);
                startActivityForResult(intent, REQUEST_CODE_COLOR_PALETTE_DIALOG);
            }
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        if(requestCode == REQUEST_CODE_COLOR_PALETTE_DIALOG){
            if(resultCode == RESULT_OK){
                @ColorInt int color = data.getIntExtra("color", 0xff000000);
                board.setPaintColor(color);
            }
        }
    }
}
~~~
레이아웃에 아까 만든 PaintBoard 객체를 추가했다. 그리고 버튼을 눌렀을 때 색 선택을 위한 대화상자를 띄우도록 했다. 대화상자에서 색 선택을 완료했을 때 이 정보를 받아서 처리하기 위해 `onActivityResult()` 메소드를 재정의하였다. 여기서는 인텐트를 통해 어떤 색을 선택했는지 정보를 받는다. 그리고 `board.setPaintColor(color)` 메소드를 통해 페인트 색을 설정한다.

<br>
<br>
그러면 색을 선택할 대화상자의 뷰를 위해 아래의 XML 코드를 작성한다.

\<activity_color_palette_dialog\>
~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="#ff000000">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="색상 선택"
        android:textColor="#ffffffff"
        android:textSize="20dp"
        android:layout_margin="10dp"/>

    <GridView
        android:id="@+id/colorGrid"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_margin="20dp"
        android:numColumns="7">
    </GridView>

    <Button
        android:id="@+id/closeBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:drawableLeft="@drawable/cancel_btn"
        android:drawablePadding="4dp"
        android:text="완료"
        android:layout_gravity="center_horizontal"/>

</LinearLayout>
~~~

그리드뷰를 통해 여러개의 색을 보여준다. 그리고 버튼을 클릭하면 색 설정이 완료되도록 할 것이다. 그리드뷰의 데이터와 뷰를 관리하기 위해 어댑터를 먼저 만들어야한다. 아래는 색 설정 어댑터 코드이다.

\<ColorAdapter.java\>
~~~ java
package io.swnomad.jcgpaintboard;

import android.content.Context;
import android.view.View;
import android.view.ViewGroup;
import android.widget.BaseAdapter;
import android.widget.GridView;
import android.widget.TextView;

public class ColorAdapter extends BaseAdapter {

    Context mContext;
    public static final int[] colors = {0xff000000,0xff00007f,0xff0000ff,0xff007f00,0xff007f7f,0xff00ff00,0xff00ff7f,
            0xff00ffff,0xff7f007f,0xff7f00ff,0xff7f7f00,0xff7f7f7f,0xffff0000,0xffff007f,
            0xffff00ff,0xffff7f00,0xffff7f7f,0xffff7fff,0xffffff00,0xffffff7f,0xffffffff};


    int rowCount;
    int colCount;

    public ColorAdapter(Context context) {
        super();
        mContext = context;

        rowCount = 3;
        colCount = 7;
    }

    @Override
    public int getCount() {
        return rowCount * colCount;
    }

    @Override
    public Object getItem(int position) {
        return colors[position];
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {

        GridView.LayoutParams params = new GridView.LayoutParams(GridView.LayoutParams.MATCH_PARENT
                , GridView.LayoutParams.MATCH_PARENT);

        /* 그리드뷰의 각 아이템이 보이는 뷰 설정*/
        TextView view = new TextView(mContext);
        view.setText(" ");
        view.setLayoutParams(params);
        view.setPadding(4, 4, 4, 4);
        view.setBackgroundColor(colors[position]);
        view.setHeight(120);

        return view;
    }
}
~~~
배열에 21가지의 색상을 저장했다. 그리고 각각의 아이템이 보이는 뷰는 따로 XML로 작성하지 않고 `getView()` 메소드 내에서 간단하게 텍스트뷰로 만들었다. 텍스트뷰의 배경은 각 아이템이 보여줄 색으로 지정했다. 그리고 이 뷰를 리턴한다.

<br>

이제 색상 선택을 위한 대화상자를 위한 자바 코드를 아래와 같이 작성한다.
\<ColorPaletteDialog.java\>
~~~ java
public class ColorPaletteDialog extends Activity {

    GridView colorGrid;
    Button closeBtn;
    TextView textView;

    ColorAdapter adapter;

    Context mContext;
    @ColorInt int selectedColor;

    public ColorPaletteDialog() {
        super();
        mContext = this;
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_color_palette_dialog);

        colorGrid = findViewById(R.id.colorGrid);
        closeBtn = findViewById(R.id.closeBtn);
        textView = findViewById(R.id.textView);
        adapter = new ColorAdapter(this);
        colorGrid.setAdapter(adapter);

        colorGrid.setOnItemClickListener(new AdapterView.OnItemClickListener() {
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                selectedColor = (int)adapter.getItem(position);
                textView.setTextColor(selectedColor);
            }
        });

        closeBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent intent = new Intent();
                intent.putExtra("color", selectedColor);
                setResult(RESULT_OK, intent);
                ((ColorPaletteDialog)mContext).finish();
            }
        });
    }

}
~~~

어댑터 객체를 만들어 그리드뷰에 설정해준다. 그리고 그리드뷰에 아이템 클릭 리스너를 설정해주고 `onItemClick()` 메소드에서는 선택된 색상 정보를 변수에 저장한 후 텍스트뷰의 색을 바꿔주어 사용자가 색이 선택되었음을 알 수 있게 하였다.

마지막으로 '완료' 버튼을 클릭하면 인텐트에 선택된 색 정보를 부가데이터로 담아서 부모 액티비티로 응답을 보낸다. 그러면 아까 본 것처럼 `PaintBoardActivity`에서는 이 색 정보를 사용해 페인트의 색을 설정한다.

또한 매니페스트 파일에서 앱을 시작했을 때 처음으로 동작시킬 액티비티를 `PaintBoardActivity`로 바꾸면 앱이 완성된다.

앱을 실행하면 아래의 결과를 확인할 수 있다.

![1](https://nobbaggu.github.io/images/android/44/1.jpg){: width="30%" height="30%"}
![2](https://nobbaggu.github.io/images/android/44/2.jpg){: width="30%" height="30%"}
![3](https://nobbaggu.github.io/images/android/44/3.jpg){: width="30%" height="30%"}
![4](https://nobbaggu.github.io/images/android/44/4.jpg){: width="30%" height="30%"}
![5](https://nobbaggu.github.io/images/android/44/5.jpg){: width="30%" height="30%"}