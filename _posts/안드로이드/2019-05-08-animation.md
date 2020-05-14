---
title: (안드로이드) 28. 애니메이션(Animation)
date: 2019-05-08T20:00:00+09:00
author: nobbaggu
layout: post
categories: 안드로이드
tags:
  - animation
  - 애니메이션
  - startAnimation
  - loadAnimation
  - setAnimationListener
  - onAnimationStart
  - onAnimationEnd
---

<br>
## 1. 트윈 애니메이션 사용 방법
* * *

레이아웃의 모든 뷰에는 애니메이션(Animation) 기능을 설정할 수 있다. 애니메이션 구현방법 중 트윈 애니메이션(Tweened Animation)이라는 방법이 가장 간단하면서도 일반적으로 사용한다. 트윈 애니메이션은 이동, 확대 및 축소, 회전과 같은 일정한 패턴으로 움직이는 애니메이션 기능을 지원한다.

<br>

트윈 애니메이션을 구현하는 방법은 아래와 같다.

<br>

![1](https://nobbaggu.github.io/images/android/28/1.png){: width="70%" height="70%"}

**1. 트윈 애니메이션 동장 방식을 XML 파일로 정의**

**2. loadAnimation() 메소드 사용하여 애니메이션 객체 메모리 로딩**

**3. startAnimation() 메소드 사용하여 애니메이션 동작**

<br>

애니메이션을 정의하는 XML 파일은 /res/anim 폴더에 저장해야 한다. 안드로이드는 /res/anim 폴더에 들어있는 XML 파일을 애니메이션이 정의된 리소스로 인식한다.

<br>

아래는 애니메이션 액션정보를 정의한 XML 리소스 파일이다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
        android:fromXDelta="100%p"
        android:toXDelta="0%p"
        android:duration="6000"
        android:repeatCount="3"/>

    <alpha
        android:fromAlpha="0.5"
        android:toAlpha="1"
        android:duration="6000"
        android:repeatCount="3"/>
    
</set>
~~~

\<translate\> 태그는 뷰 등의 구성요소의 위치이동 방식을 정의하기 위한 액션 정보 속성이 들어간다. 애니메이션은 fromXDelta 에서 toXDelta 속성까지 이동한다. 위 코드에서는 100%p 부터 0%p 까지이므로 오른쪽 끝에서 왼쪽 끝으로 이동하도록 되어있는 것이다. duration은 애니메이션의 지속시간(ms)이다. repeatCount는 애니메이션의 반복 횟수를 지정한다. 위 코드에서는 1번 + 3번반복으로 총 4번 실행된다.

\<alpha\> 태그는 투명도에 관한 속성을 정의한다. 위 코드에서는 투명도 0.5부터 1까지 6초에 걸쳐 3번 반복하도록 정의했다.

~~~ java
public class MainActivity extends AppCompatActivity {

    TextView textView;
    Button button;
    Animation flowAnim;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        textView = findViewById(R.id.textView);
        button = findViewById(R.id.button);

        flowAnim = AnimationUtils.loadAnimation(this, R.anim.flow); //애니메이션 XML 파일 메모리에 로딩
        flowAnim.setAnimationListener(new Animation.AnimationListener() { //애니메이션에 리스너 설정(필요할 때만)
            @Override
            public void onAnimationStart(Animation animation) {
                Toast.makeText(MainActivity.this, "애니메이션 시작", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAnimationEnd(Animation animation) {
                Toast.makeText(MainActivity.this, "애니메이션 종료", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAnimationRepeat(Animation animation) {

            }
        });

        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                textView.startAnimation(flowAnim); //텍스트뷰로 flowAnim 애니메이션 객체 시작
            }
        });
    }
}
~~~

setAnimationListener() 메소드를 통해 애니메이션 리스너 객체를 설정하여 애니메이션이 시작할 때, 반복될 때, 끝날 때 특정 동작을 정의하여 실행할 수 있다. 버튼을 누르면 텍스트뷰가 애니메이션 동작을 수행한다.

![1](https://nobbaggu.github.io/images/android/28/1.jpg){: width="30%" height="30%"} ![2](https://nobbaggu.github.io/images/android/28/2.jpg){: width="30%" height="30%"} ![3](https://nobbaggu.github.io/images/android/28/3.jpg){: width="30%" height="30%"}

<br>

<br>
## 2. 트윈 애니메이션의 종류
* * *

트윈 애니메이션에는 여러가지 종류가 있다.

|애니메이션|태그|
|------|---|
|위치이동|translate|
|확대/축소|scale|
|회전|rotate|
투명도|alpha|

<br>

> 1. 위치이동

위치 이동은 \<translate\> 태그로 지정한다. fromXDelta와 fromYDelta는 애니메이션의 시작위치이고, toXDelta와 toYDelta는 애니메이션의 종료 위치이다. 그리고 duration 속성을 통해 애니메이션의 실행 시간을 지정할 수 있다.

<br>

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">

    <translate
        android:fromXDelta="0%p"
        android:toXDelta="-100%p"
        android:duration="2000"
        android:repeatCount="3"
        android:fillAfter="true"/>

</set>
~~~

fromXDelta가 0%p이므로 뷰의 원래 위치의 X좌표이다. toXDelta가 -100%p이므로 화면에서 뷰의 크기만큼 왼쪽으로 사라진다. duration은 ms 단위이므로 2초간 애니메이션이 지속된다. repeatCount는 반복횟수를 의미하는데, 3이므로 3회 추가반복을 한다. fillAfter 속성이 true이므로 애니메이션이 끝나고 원래의 위치로 되돌아온다.

<br>

> 2. 확대/축소

확대/축소는 \<rotate\> 태그로 지정한다. pivotX, pivotY는 확대, 축소의 중심이 되는 위치를 의미하며 fromXScale과 fromYScale은 애니메이션의 시작 크기, toXScale과 toYScale은 애니메이션의 종료 크기를 의미한다.

<br>

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">

    <scale
        android:duration="2500"
        android:pivotY="50%"
        android:pivotX="50%"
        android:toYScale="2.0"
        android:toXScale="2.0"
        android:fromYScale="1.0"
        android:fromXScale="1.0"/>

    <scale
        android:startOffset="2500"
        android:duration="2500"
        android:pivotY="50%"
        android:pivotX="50%"
        android:toYScale="0.5"
        android:toXScale="0.5"
        android:fromYScale="1.0"
        android:fromXScale="1.0"/>

</set>
~~~

뷰를 중심으로 X, Y방향 모두 원래의 크기부터 2배까지 늘린다. 두 번째 태그에서는 startOffset이 2500이기 때문에 2.5초부터 시작한다. 정리하자면 이 뷰는 2.5초동안 2배로 커졌다가 2.5초동안 다시 원래의 크기로 돌아온다.

<br>

> 3. 회전

회전은 \<rotate\> 태그로 지정한다. fromDegrees와 toDegrees 속성은 각각 시작 각도와 종료 각도를 의미한다. 여기서도 pivotX, pivotY를 통해 회전의 중심축을 지정할 수 있다.

<br>

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">

    <rotate
        android:pivotX="50%"
        android:pivotY="50%"
        android:fromDegrees="0"
        android:toDegrees="360"
        android:duration="2000"/>
</set>
~~~

위 회전 속성은 뷰의 중앙을 중심축으로 하여 0도(원래의 뷰)에서 360도(한 바퀴) 회전한다.

<br>

> 4. 투명도

투명도는 \<alpha\> 태그로 지정한다. fromAlpha는 시작 투명도, toAlpha는 종료 투명도이다. 0은 완전투명, 1은 완전 불투명을 의미한다.

<br>

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">

    <alpha
        android:fromAlpha="0.2"
        android:toAlpha="1"
        android:duration="3000"/>
</set>
~~~

위 코드는 투명도를 0.2부터 1.0까지 3초동안 변화시킨 것이다.

<br>

<br>

버튼을 4개 만들고 위에서 정의한 4가지 서로다른 애니메이션 속성을 적용해보자.

<br>

~~~ java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        /* 위치이동 애니메이션 */
        Button buttonTranslate = findViewById(R.id.buttonTranslate);
        buttonTranslate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Animation translateAnim = AnimationUtils.loadAnimation(MainActivity.this, R.anim.translate);
                v.startAnimation(translateAnim);
            }
        });

        /* 확대, 축소 애니메이션 */
        Button buttonScale = findViewById(R.id.buttonScale);
        buttonScale.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Animation scaleAnim = AnimationUtils.loadAnimation(MainActivity.this, R.anim.scale);
                v.startAnimation(scaleAnim);
            }
        });

        /* 회전 애니메이션 */
        Button buttonRotate = findViewById(R.id.buttonRotate);
        buttonRotate.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Animation rotateAnim = AnimationUtils.loadAnimation(MainActivity.this, R.anim.rotate);
                v.startAnimation(rotateAnim);
            }
        });

        /* 투명도 애니메이션 */
        Button buttonAlpha = findViewById(R.id.buttonAlpha);
        buttonAlpha.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Animation alphaAnim = AnimationUtils.loadAnimation(MainActivity.this, R.anim.alpha);
                v.startAnimation(alphaAnim);
            }
        });


    }
}
~~~

앱을 실행하고 각각의 버튼을 클릭해보면 적용한 애니메이션이 동작하는 것을 볼 수 있다.

![4](https://nobbaggu.github.io/images/android/28/4.jpg){: width="30%" height="30%"}