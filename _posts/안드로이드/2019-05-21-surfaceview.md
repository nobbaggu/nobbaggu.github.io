---
title: (안드로이드) 45. 서피스뷰(Surface View)
date: 2019-05-21T18:00:00+09:00
author: SWnomad
layout: post
categories: 안드로이드
tags:
  - 서피스뷰
  - serface
  - view
  - serfaceview
  - serfaceholder
  - 서피스
  - 홀더
  - 서피스홀더
  - getHolder
  - lockCanvas
  - unlockCanvasAndPost
  - surfaceholder.callback
  - 콜백
  - callback
---

카메라 어플을 생각해보자. 카메라 어플을 키면 실시간으로 미리보기 화면이 동작한다. 이는 카메라 앱 화면의 뷰가 엄청 짧은 시간내에 아주 많은 프레임을 계속해서 그리기 때문인데, 엄청난 자원이 필요하다. 이런 경우 화면을 그리는데에만 많은 자원이 필요한데, 어플에서 다른 것들을 처리하느라 화면이 버벅거리면서 업데이트가 늦어질 수가 있다. 이와 같은 경우에 사용하는 것이 그래픽 그리기가 빠른 **서피스뷰**이다.

### 1. 서피스뷰의 동작방식
***

일반적인 뷰가 화면에 그려지는 작업은 메인 스레드(Main Thread)에서 실행된다. 하지만 메인 쓰레드는 다른 작업들도 수행해야되므로 다른 작업들의 자원 사용량에 영향을 받는다. 그러나 서피스뷰는 동작 방식이 완전히 다르다. 서피스뷰는 화면을 그리는 동작을 위해 **독립적인 스레드(Thread)를 만들어 실행**시킨다.(쓰레드는 작업이 수행되는 하나의 흐름이다.) 따라서 앱의 자원 사용 상태에 상관없이 실시간으로 뷰를 그린다.

### 2. 서피스를 제어하는 서피스 홀더
***

사실 안드로이드 운영체제의 정책상 메인 스레드이외의 쓰레드는 캔버스(사실 캔버스 뿐만이 아니라 앱의 UI 구성요소)에 직접 접근이 허락되지 않는다. 이는 하나의 프로세스 이내의 여러개의 스레드는 공통의 메모리 자원을 공유하므로 여러 스레드가 같은 자원에 동시에 접근했을 때 일어나는 데드락(Dead Lock)을 방지하기 위함이다. 따라서 서피스뷰를 그리기 위해서는 다른 방법을 써야하는데, 서피스홀더(Surface Holder)라는 것을 사용한다. 서피스뷰는 뷰가 그려지는 버퍼일 뿐이다. 직접적으로 그림을 그리고 서피스뷰의 상태를 제어하는 것은 서피스뷰의 서피스홀더(Surface Holder)이다. 아래 그림은 이러한 서피스뷰와 서피스홀더의 관계를 보여준다.

![1](/images/android/45/1.png){: width="100%" height="100%"}

서피스홀더는 서피스뷰에서 `getHolder()` 메소드를 사용해서 참조할 수 있다. 서피스홀더를 참조하고나면 서피스홀더의 `addCallback()` 메소드를 통해서 SurfaceHolder.Callback 인터페이스를 설정해주어야한다.

~~~ java
public SurfaceHolder getHolder()
public void addCallback(Callback callback);
~~~

<br>
콜백 인터페이스를 설정해주면 서피스뷰의 상태가 변화할 때의 정보가 서피스홀더로 전달되어 아래 세 가지의 메소드가 자동으로 호출된다.

~~~ java
@Override /* 서피스뷰 객체 생성시 호출 */
public void surfaceCreated(SurfaceHolder holder)

@Override /* 서피스뷰 객체 변경시 호출 */
public void surfaceChanged(SurfaceHolder holder, int format, int width, int height)

@Override /* 서피스뷰 객체 해제 시 호출 */
public void surfaceDestroyed(SurfaceHolder holder)
~~~

<br>

### 3. 서피스 홀더의 핵심기능
***

서피스홀더의 3가지 콜백 메소드 중에서 주로 사용되는건 `surfaceCreated()`와 `surfaceDestroyed()` 정도이다. 보통은 `surfaceCreated()` 내에서 새로운 스레드를 생성하고 시작하여 그리는 작업을 수행하는 방식으로 코드를 디자인한다. 그리고 `surfaceDestroyed()`에서 스레드를 다시 메인 스레드와 합쳐주는 작업을 하고 서피스를 해제한다.

<br>

서피스홀더의 핵심 기능은 **캔버스 락 언락 제어**이다. 서피스홀더의 `lockCanvas()` 메소드를 통해서 잠겨진 캔버스를 참조할 수 있다. 이후 이 캔버스에 그림을 그린 후 `unlockCanvasAndPost()` 메소드를 통해서 락을 해제해주면 캔버스에 그렸던 뷰가 서피스를 통해 화면에 보여지게 된다. 이 방식은 그림을 미리 그려놓고 나중에 화면에 띄우기만 하는 더블 버퍼링 방식인데, 이거때문에 서피스홀더의 뷰 그리기 속도는 향상된다.

서피스홀더의 코드 디자인 패턴은 크게 아래의 버주를 벗어나지 않는다.
 
> 1. SurfaceView 상속 및 SurfaceHolder.Callback 인터페이스 구현
> 2. 서피스뷰 상태관련 메소드 3개 재정의
> 3. 쓰레드 상속한 클래스 정의 및 `run()` 메소드 재정의
> 4. 쓰레드 생성 및 뷰 그리기 및 화면에 표시

<br>

`Thread.start()` 메소드가 호출되었을 때 `run()` 메소드 내부의 코드가 실행되므로 `run()` 메소드를 재정의하여 실행시킬 코드를 추가하면 된다.

<br>
### 4. 서피스뷰 예제 - 움직이는 공 그리기
***

~~~ java
public class Ball {

    Context mContext;
    int x, y; //공의 위치
    int dx, dy; //공이 한 번에 움직일 거리
    int rad; //공의 반지름

    static int max_width; //디바이스 화면의 폭을 저장할 변수
    static int max_height; // 디바이스 화면의 높이를 저장할 변수

    public Ball(Context context, int rad) { //공 객체를 생성할 때 반지름을 파라미터로 전달해준다.
        mContext = context;
        init(rad); //초기화
    }

    public void init(int rad){
        //휴대폰 단말의 스크린 폭, 높이 가져와서 max_width, max_height에 대입
        WindowManager manager = (WindowManager) mContext.getSystemService(Context.WINDOW_SERVICE);
        Display display = manager.getDefaultDisplay();
        Point sizePoint = new Point();
        display.getSize(sizePoint);
        max_width = sizePoint.x;
        max_height = sizePoint.y;

        this.rad = rad; //반지름 초기화

        /* 난수 생성메소드를 사용해서 위치값 및 위치변화량 값 랜덤으로 생성 */
        x = (int) (Math.random() * (max_width - rad) + 3);
        y = (int) (Math.random() * (max_height - rad) + 3);
        dx = (int)(Math.random()*30 + 1);
        dy = (int)(Math.random()*30 + 1);

    }

    //파라미터로 전달받은 캔버스에 공 그리기
    public void drawBall(Canvas canvas){

        //왼쪽, 오른쪽 벽에 닿았을 때
        if(x < rad || x > max_width - rad){
            dx *= -1; //x방향 변경
        }

        //위, 아래 벽에 닿았을 때
        if(y < rad || y > max_height - rad){
            dy *= -1; //y방향 변경
        }

        //공 위치 이동
        x += dx;
        y += dy;

        //빨간색 페인트로 공 그리기
        Paint paint = new Paint();
        paint.setColor(Color.RED);
        canvas.drawCircle(x, y, rad, paint);
    }
}
~~~

`Math.random()` 메소드는 0~1 사이의 수 중에서 하나를 랜덤으로 생성해준다. x, y는 공을 그릴 좌표값이다. 그리고 `drawBall()`가 호출될 때 마다 공의 위치를 이동시키고, 벽에 닿으면 반사되도록 한 이후 공을 그려준다. 이 메소드는 스레드의 `run()` 메소드 안에서 호출되면서 캔버스에 그림을 그리기 위한 것이다.

<br>

~~~ java
public class MySurfaceView extends SurfaceView implements SurfaceHolder.Callback {

    SurfaceHolder holder;
    Thread mThread;
    Ball balls[]; //공 10개를 저장할 배열 생성
    Context mContext;

    public MySurfaceView(Context context) {
        super(context);
        init(context);
    }

    private void init(Context context){
        mContext = context;
        holder = getHolder(); //서피스뷰 객체 참조
        holder.addCallback(this); //콜백 인터페이스 설정

        //공 배열에 공 객체 생성 및 저장
        balls = new Ball[20];
        for(int i=0; i<20; i++){
            balls[i] = new Ball(mContext, (int)(Math.random()*50 + 50));
        }
    }


    @Override /* 서피스뷰 객체 생성시 호출 */
    public void surfaceCreated(SurfaceHolder holder) {
        mThread = new MyThread(holder); //새로운 스레드 생성;
        ((MyThread) mThread).setThreadRun(true);
        mThread.start(); //스레드 실행
    }

    @Override /* 서피스뷰 객체 변경시 호출 */
    public void surfaceChanged(SurfaceHolder holder, int format, int width, int height) {
    }

    @Override /* 서피스뷰 객체 해제 시 호출 */
    public void surfaceDestroyed(SurfaceHolder holder) {
        ((MyThread) mThread).setThreadRun(false);
        boolean retry = true;

        /* 스레드 메인스레드에 다시 합치기 */
        while(retry){
            try{
                mThread.join(); //메인 스레드에 합치기
                retry = false;
            }catch(InterruptedException e){
                e.printStackTrace();
            }
        }
    }

    //내부 클래스로 스레드 정의
    public class MyThread extends Thread{

        SurfaceHolder surHolder;
        private boolean threadRun = true;

        public MyThread(SurfaceHolder holder){
            surHolder = holder;
        }

        @Override
        public void run() {
            while(threadRun){
                Canvas canvas = null;
                try{
                    canvas = surHolder.lockCanvas(null); //서피스홀더의 캔버스 객체 참조
                    canvas.drawColor(Color.BLUE); // 캔버스 배경 파란색으로 설정
                    synchronized (surHolder){
                        for(Ball ball : balls){
                            ball.drawBall(canvas);//캔버스에 공 그리기
                        }
                    }
                }finally{
                    if(canvas != null){
                        surHolder.unlockCanvasAndPost(canvas); // 화면에 그려진 뷰 표시하기
                    }
                }
            }
        }

        public void setThreadRun(boolean threadRun) {
            this.threadRun = threadRun;
        }
    }
}
~~~

내부 클래스로 Thread로 상속한 클래스를 만들고 `run()` 메소드를 재정의하여 그리는 코드를 추가한다. 이 때 `lockCanvas()` 메소드를 통해서 서피스홀더의 잠겨진 캔버스를 참조할 수 있다. 이렇게 참조한 캔버스위에 `drawBall()` 메소드를 호출하여 공을 그려준다. 이 때 `while()`문을 사용하여 스레드가 실행되는동안 계속해서 그림을 업데이트 해주어야한다.

이제 `surfaceCreated()` 메소드 내에서 스레드를 생성하고 시작시키면 화면에 움직이는 공이 그려진다. 또한 `surfaceDestroyed()` 메소드에서는 스레드를 다시 메인 스레드와 합쳐주어 자원의 낭비를 막아야한다.

앱을 실행시키면 다음의 화면이 나타난다.

![2](/images/android/45/2.jpg){: width="30%" height="30%"}