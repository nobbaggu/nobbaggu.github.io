---
title: (안드로이드) 58. 유튜브 플레이어 API를 사용한 동영상 재생
date: 2019-06-09T18:00:00+09:00
author: SWnomad
layout: post
categories: 안드로이드
tags:
  - youtube
  - android
  - player
  - api
  - 유튜브
  - youtubeandroidplayerapi
  - 안드로이드
  - 플레이어
---

일반적인 안드로이드 멀티미디어 패키지에 포함된 비디오뷰로는 유튜브 미디어 플레이어를 가져올 수 없다. 물론 웹뷰를 사용할 수도 있겠지만 구글 개발자 사이트에서는 유튜브 동영상 재생을 위한 성능 좋은 공식 API를 제공한다.

<br>

↓↓↓ 구글 개발자 사이트 YouTubeAndroidPlayerApi 링크

[YouTubeAndroidPlayerApi](https://developers.google.com/youtube/android/player/){: target="_blank" }

<br>

API를 사용하는 간단한 앱을 만들어보려한다. 그 전에 위 링크를 따라가 Download 탭에서 API를 다운받는다.

이제 아래 절차대로 API를 프로젝트에 추가하고 API 키를 발급받으면 라이브러리를 사용할 수 있다.
<br>

+ 압축을 푼 이후 \YouTubeAndroidPlayerApi-1.2.2\libs 폴더에 있는 YouTubeAndroidPlayerApi.jar 파일을 안드로이드 스튜디오에서 프로젝트 파일의 /app/libs 폴더에 끌어다가 복사한다.
![1](/images/android/58/1.png){: width="100%" height="100%"}

<br>
+ 안드로이드 스튜디오 상단 메뉴의 \[File\] - \[Project Structure\] 을 클릭하여 Dependencies 탭의 App으로 가서 \+ 버튼을 클릭하여 Jar Dependency를 클릭한다.
![2](/images/android/58/2.png){: width="100%" height="100%"}

<br>
+ libs/YouTubeAndroidPlayerApi.jar 을 입력하고 OK버튼을 누른다.
![3](/images/android/58/3.png){: width="100%" height="100%"}

<br>
+ gradle 폴더가 아닌 app 폴더의 build.gradle 파일을 열어보면 아래와 같이 dependencies에 라이브러리가 추가된 것이 보인다.
![4](/images/android/58/4.png){: width="100%" height="100%"}

<br>
+ [API키 발급 사이트](https://console.developers.google.com/apis/credentials?hl=ko){: target="_blank" } ← 링크를 타고 들어가서 API키 발급받기를 클릭한다. 그러면 API 키카 생성된다.
![5](/images/android/58/5.png){: width="100%" height="100%"}

<br>
+ AndroidManifest.xml 파일에서 패키지 이름을 복사해서 패키지 이름 입력란에 붙여넣는다. 그리고 SHA-1키를 입력해야 하는데, 이 키는 안드로이드 스튜디오의 오른쪽 벽에 붙어있는 Gradle을 클릭하면 \[App\] - \[Tasks\] - \[android\] - \[signingReport\]를 더블클릭하면 실행이 되면서 안드로이드 스튜디오 아래부분에 나타난다. 이 키를 복사해서 붙여넣고 저장을 하면 된다.
![6](/images/android/58/6.png){: width="100%" height="100%"}
![7](/images/android/58/7.png){: width="100%" height="100%"}

<br>

이제 API를 사용하기 위해 아래 코드를 입력한다.

~~~ xml
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.google.android.youtube.player.YouTubePlayerView
        android:id="@+id/youtubeViewer"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1" />

    <Button
        android:id="@+id/playBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="재생"
        android:textSize="24dp"
        android:layout_gravity="center_horizontal"/>

</LinearLayout>
~~~
레이아웃에 동영상을 재생시킬 영역을 위해 YouTubePlayerView를 추가하고 재생을 위한 버튼을 추가한다.

<br>

~~~ java	
public class MainActivity extends YouTubeBaseActivity {

    private YouTubePlayerView youtubeViewer;
    YouTubePlayer.OnInitializedListener listener;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button playBtn = findViewById((R.id.playBtn));
        youtubeViewer = findViewById(R.id.youtubeViewer);

        listener = new YouTubePlayer.OnInitializedListener(){
            @Override
            public void onInitializationSuccess(YouTubePlayer.Provider provider, YouTubePlayer youTubePlayer, boolean b) {
                /* 초기화 성공하면 유튜브 동영상 ID를 전달하여 동영상 재생*/
                youTubePlayer.loadVideo("SGWohw86Xk8"); // 동영상 ID는 URL 상단의 마지막 부분이다.
            }

            @Override
            public void onInitializationFailure(YouTubePlayer.Provider provider, YouTubeInitializationResult youTubeInitializationResult) {
                Toast.makeText(MainActivity.this, "재생 실패", Toast.LENGTH_LONG).show();
            }
        };

        playBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                /* 파라미터1: API키
                 * 파라미터2: 리스너 객체 */
                youtubeViewer.initialize("AIzaSyB2-ytj3SHdTEmUrf2LTUuA0J6gynwCXJs", listener);
            }
        });
    }

}
~~~

아까 signingReport를 실행한 것 때문에 앱이 아닌 signingReport가 또다시 실행될 수 있으므로 Alt\+Shift\+F10을 눌러 액티비티를 실행한다. 그리고 앱을 실행하면 아래와 같이 유튜브 플레이어가 실행된다.

![8](/images/android/58/8.jpg){: width="40%" height="40%"}