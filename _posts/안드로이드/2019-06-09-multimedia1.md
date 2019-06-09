---
title: (안드로이드) 55. 멀티미디어1 - 오디오와 동영상 재생
date: 2019-06-09T13:00:00+09:00
author: SWnomad
layout: post
categories: 안드로이드
tags:
  - MediaPlayer
  - VideoView
  - 미디어 플레이어
  - 비디오뷰
---

안드로이드에서는 오디오나 동영상 재생 및 녹음, 녹화를 위한 API인 android.media 패키지를 제공한다. 그 중 핵심은 MediaPlayer 클래스로 오디오와 동영상의 재생에 관련된 부분을 담당한다.

<br>
## 1. 오디오 재생
* * *

MediaPlayer를 사용한 오디오의 재생을 위해서는 크게 3가지 단계가 필요하다.

~~~java
setDataSource() //재생 대상 설정
prepare() // 재생 준비
start(); // 재생 시작
~~~

<br>
재생 대상을 지정하는 방법에는 크게 3가지가 있다. 

> 1. 온라인 URL 지정
> 2. 프로젝트 assets 폴더에 저장 후 지정
> 3. 디바이스 SD카드에 저장 후 지정

일반적으로는 단말에 있는 소스를 재생하는 일이 많으므로 3번째 방법이 자주 쓰인다. 그러나 여기에서는 온라인의 샘플 오디오 재생을 위한 URL을 지정할 것이다.

`prepare()` 메소드를 실행하면 지정된 소스의 몇 프레임을 미리 읽어들이고 정보를 확인한다. 마지막으로 `start()` 메소드를 호출하면 지정된 대상 오디오를 재생한다.

<br>

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center_horizontal">

    <Button
        android:id="@+id/playBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="재생"
        android:textSize="24dp"
        android:textStyle="bold"
        android:layout_margin="50dp"/>

    <Button
        android:id="@+id/pauseBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="중지"
        android:textSize="24dp"
        android:textStyle="bold"
        android:layout_margin="50dp"/>

    <Button
        android:id="@+id/restartBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="재시작"
        android:textSize="24dp"
        android:textStyle="bold"
        android:layout_margin="50dp"/>
</LinearLayout>
~~~

재생, 중지, 재시작을 위한 3가지 버튼을 만들었다.

<br>

~~~ java
public class MainActivity extends AppCompatActivity {

    static final String AUDIO_URL = "https://sites.google.com/site/ubiaccessmobile/sample_audio.amr";
    private MediaPlayer mediaPlayer;
    private int playbackPosition = 0; //재생위치를 저장할 변수

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button playBtn = findViewById(R.id.playBtn);
        Button pauseBtn = findViewById(R.id.pauseBtn);
        Button restartBtn = findViewById(R.id.restartBtn);

        playBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                playAudio(AUDIO_URL);
            }
        });

        pauseBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                pauseAudio();
            }
        });

        restartBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                restartAudio();
            }
        });

    }

    private void playAudio(String url){
        /* 애플리케이션 내에서 미디어플레이어는 하나만 사용가능하므로
        이미 사용되고 있을 경우 미디어 플레이어 리소스를 해제해주어야한다. */
        if(mediaPlayer != null){ // 미디어 플레이어가 사용되고 있을경우
            try{
                mediaPlayer.release(); //미디어 플레이어 리소스 해제
            }catch(Exception e){
                e.printStackTrace();
            }
        }

        try{
            mediaPlayer = new MediaPlayer();//미디어 플레이어 객체 생성
            mediaPlayer.setDataSource(url); //대상 재생파일 지정
            mediaPlayer.prepare(); //재생 준비
            mediaPlayer.start(); //재생 시작
        }catch(Exception e){
            e.printStackTrace();
        }


        Toast.makeText(this, "재생 시작", Toast.LENGTH_LONG).show();
    }

    private void pauseAudio(){
        if(mediaPlayer!=null && mediaPlayer.isPlaying()){
            playbackPosition = mediaPlayer.getCurrentPosition();
            mediaPlayer.pause();
            Toast.makeText(this, "일시중지", Toast.LENGTH_LONG).show();
        }
    }

    private void restartAudio(){
        if(mediaPlayer != null && !mediaPlayer.isPlaying()){
            mediaPlayer.start();
            mediaPlayer.seekTo(playbackPosition);
            Toast.makeText(this, "재시작", Toast.LENGTH_LONG).show();
        }
    }

    @Override
    protected void onDestroy() { // 액티비티 종료 시 미디어 플레이어 리소스 해제
        super.onDestroy();
        if(mediaPlayer != null){
            try{
                mediaPlayer.release();
            }catch(Exception e){
                e.printStackTrace();
            }
        }
    }
}
~~~

여기서 눈여겨 볼 점은 중지를 할 때 `getCurrentPosition()` 메소드를 사용해서 중지될 때의 재생 위치를 가져와서 저장한다는 것이다. 그리고 재시작 버튼을 눌렀을 때 `seekTo()` 메소드를 통해 이전에 재생 중지 했던 위치로 찾아가도록 하는 것을 볼 수 있다. 그리고 액티비티의 `onDestroy()` 메소드를 재정의하여 액티비티가 종료될 때 미디어 리소스를 해제해준다.

앱을 실행하고 재생버튼을 누르면 아래 화면을 확인할 수 있다.

![1](/images/android/55/1.jpg){: width="30%" height="30%"}

<br>
## 2. 동영상 재생
* * *

동영상 재생은 레이아웃에 VideoView 객체를 추가하는 것으로 매우 간단하게 구현할 수 있다.

< br>

~~~ xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <Button
        android:id="@+id/startBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:width="180dp"
        android:layout_gravity="center"
        android:text="재생"/>

    <VideoView
        android:id="@+id/videoView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_marginTop="10dp"/>

</LinearLayout>
~~~

재생버튼과 동영상 디스플레이 영역을 위한 VideoView 위젯 두 개만 있다.

<br>

~~~ java
public class MainActivity extends AppCompatActivity {

    private VideoView videoView;
    static final String VIDEO_URL = "https://sites.google.com/site/ubiaccessmobile/sample_video.mp4";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        videoView = findViewById(R.id.videoView);
        MediaController mc = new MediaController(this); //미디어 컨트롤러 객체 생성
        videoView.setMediaController(mc); //비디오뷰에 미디어 컨트롤러 설정

        videoView.setVideoURI(Uri.parse(VIDEO_URL)); //비디오 재생 대상 URI 설정
        videoView.requestFocus();

        Button startBtn = findViewById(R.id.startBtn);

        startBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                videoView.seekTo(0); //재생 위치 처음으로
                videoView.start(); //재생 시작
            }
        });
    }
}
~~~

비디오뷰에 미디어 컨트롤러 객체를 설정하면 비디오뷰 영역을 터치했을 때 동영상 재생을 제어할 수 있는 위젯이 나타난다.

앱을 실행하고 재생버튼을 누르면 아래 화면을 확인할 수 있다.

![2](/images/android/55/2.jpg){: width="30%" height="30%"}