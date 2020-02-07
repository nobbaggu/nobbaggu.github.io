---
title: (머신러닝) 6. octave 기초
date: 2018-08-09T08:18:57+09:00
author: nobbaggu
layout: post
categories: 머신러닝
image: /images/2018/08/machine-learning.jpg
tags:
  - matlab
  - octave
  - 매트랩
  - 옥테이브
---
octave 는 MATLAB의 무료버전이라고 보면 되는데, 행렬연산에 좋은 성능을 보인다. 행렬과 관련된 대부분의 연산이 단 한 줄의 함수호출로 이루어진다. 머신러닝의 알고리즘을 구현하는 데 있어서 Python이나 R은 기본적인 변수선언이나 라이브러리 사용법을 익히고 디버깅도 어느정도 할 줄 알아야한다. 하지만 octave는 매우 직관적이고 쉬워 초보자들도 구현을 하면서 배우는것이 가능할 정도이다. 따라서 octave를 사용하면 프로그래밍 언어 자체에 신경쓰지않고 머신러닝의 개념과 구현 알고리즘 자체를 이해하는데에 집중할 수 있다.

아래의 링크를 따라가면 자신에게 맞는 운영체제별로 octave를 설치할 수 있다.

<span style="text-decoration: underline;"><span style="color: #3366ff;"><a style="color: #3366ff; text-decoration: underline;" href="https://www.gnu.org/software/octave/download">https://www.gnu.org/software/octave/download</a></span></span>

&nbsp;

&nbsp;

지금부터 octave의 기본적인 명령어들을 소개하겠다.

&nbsp;

# 1. Basic Operations

* * *

PS1(&#8216;>> &#8216;); %% octave 프롬프트 변경

cd &#8216;c:/path/to/desired/directory name&#8217; %% 작업 디렉토리 변경

3+2 %%더하기

1-4 %%빼기

3*9 %%곱하기

2/5 %%나누기

2^9 %%거듭제곱

1 == 2 %% 같으면 1, 다르면 0 반환

1 ~= 2 %% 다르면 1, 같으면 0 반환

1 && 0 %%bitwise AND 논리

1 || 0 %%bitwise OR 논리

xor(1,0) %%XOR 논리

%% 변수

a = 3; %% a라는 변수에 3 저장. 세미콜론은 실행결과를 나타내고싶지 않을 때 사용

b = &#8216;hi&#8217;; %%b라는 변수에 문자열 저장

c = 3>=1; %%c라는 변수에 논리연산의 결과 저장

% Displaying them:

a = pi %% a에 pi 저장 // 원주율 pi가 정의되어있음

disp(a) %% a의 값 반환

disp(sprintf(&#8216;2 decimals: %0.2f&#8217;, a)) %% %f는 서식문자로서 실수를 나타낼 때 사용하고 0.x의 형태로 x자리수까지 표현 가능

disp(sprintf(&#8216;6 decimals: %0.6f&#8217;, a))

format long %%실수 표현범위 long타입(소숫점 8자리)

a

format short %%실수 표현범위 short타입(소숫점 2자리)

a

%% vectors and matrices

A = [1 2; 3 4; 5 6] %% 세미콜론(;)로 행 구분

v = [1 2 3] %% 행벡터

v = [1; 2; 3] %%열벡터

v = 1:0.1:2 % 1에서 2까지 0.1단위로 커지는 element를 가지는 행벡터

v = 1:6 % stepsize가 없으면 1씩 커짐. 1~6까지의 정수가 원소인 행벡터

C = ones(2,3) %% 2행, 3열의 원소가 모두 1인 행렬. C = [1 1 1; 1 1 1]과 같음

w = ones(1,3)

w = zeros(1,3) %% 1행, 3열의 원소가 모두 0인 행렬 = 행벡터

w = rand(1,3) % 균등분포에 따라 숫자를 추출해서 1&#215;3 행렬 정의

w = randn(1,3)% 정규분포에 따라 숫자를 추출해서 1&#215;3 행렬 정의

hist(w) % w를 히스토그램으로 그려줌. default 막대 10개

hist(w,50) % w를 히스토그램으로 그려줌. 막대 50개로 설정

I = eye(4) % 4&#215;4 단위행렬

% help + 함수이름 : 함수에 대한 도움말 보기

help eye

help rand

* * *

&nbsp;

&nbsp;

# **2. Moving Data Around**

* * *

%% Dimension 계산

sz = size(A) % 행렬 A의 dimension을 반환(행의 수, 열의 수 모두 반환)

size(A,1) % 행의 수 반환

size(A,2) % 열의 수 반환

length(v) % 벡터의 길이 반환

%% 데이터 불러오기

pwd % 현재 디렉토리

cd &#8216;C:\Users\ang\Octave files&#8217; % 디렉토리 변경

ls % 현재 디렉토리에 있는 파일 확인

load q1y.dat % q1y.dat 파일 불러오기

load (&#8220;q1y.dat&#8221;) % q1y.dat파일 불러오기

clear % 모든 변수 clear

v = q1x(1:10); % 벡터 v는 q1x 데이터들 중 처음 10개로 이루어짐

%% 행렬을 index를 사용하여 다루기

A(3,2) % 행렬A의 3행2열의 원소 반환

A(2,:) % :는 &#8220;모든&#8221; 이라는 뜻을 가짐. 이 코드는 2행의 모든 원소 = 2행 자체 반환

A(:,2) % 2열의 모든 원소 = 2열 자체 반환

A(:,2) = [10; 11; 12] % A의 2열을 10, 11, 12로 바꿈

A = [A, [100; 101; 102]]; % A의 끝 열에 [100; 101; 102] 추가

%% 행렬 추가하기

A = [1 2; 3 4; 5 6]

B = [11 12; 13 14; 15 16]

C = [A B] % A와 B를 옆으로 이어붙여 새로운 행렬 C 생성

C = [A; B] % A와 B를 위아래로 이어붙여 새로운 행렬 생성

* * *

&nbsp;

&nbsp;

# **3. Computing on Data**

* * *

A = [1 2;3 4;5 6]

B = [11 12;13 14;15 16]

C = [1 1;2 2]

v = [1;2;3]

%% matrix operations

A * C % 행렬 곱

A .* B % element-wise 행렬곱

% A .\* C or A \* B %% 정의하지 못함. dimension이 맞지 않음

A .^ 2 % element-wise 제곱

1./v % element-wise 역수

log(v) % element-wise 로그취하기

exp(v) % element-wise 지수취하기

abs(v) % element-wise 절대값

v + 1 % 모든 원소 +1

A&#8217; % A의 transpose(전치행렬)

%% misc useful functions

a = [1 15 2 0.5]

val = max(a) % a의 최대값 반환

[val,ind] = max(a) %a의 최대값과 그 index 반환

val = max(A) % 각 열의 최대값 반환

% sum, prod

sum(a) % a의 모든 원소의 합

prod(a) % a의 모든 원소의 곱

sum(A,1) %각 열의 원소의 합

sum(A,2) %각 행의 원소의 합

sum(sum( A .* eye(9) )) %A의 오른쪽아래 대각선방향의 원소의 합 계산

sum(sum( A .* flipud(eye(9)) )) %A의 오른쪽 위 대각선방향의 원소의 합 계산 . flipud : 행렬 위아래 뒤집기

% Matrix inverse (pseudo-inverse)

pinv(A) % inv(A&#8217;_A)_A&#8217; %pseudo 역행렬. 그냥 역행렬이라고 생각해도 무방

* * *

&nbsp;

&nbsp;

# **4. Plotting Data**

* * *

%% plotting

t = [0:0.01:0.98];

y1 = sin(2_pi_4*t);

plot(t,y1);

y2 = cos(2_pi_4*t);

hold on; % hold on 하면 기존의 그래프 없어지지 않고 그 위에 다른 그래프 추가하여 비교 가능

plot(t,y2,&#8217;r&#8217;);

xlabel(&#8216;time&#8217;); %x축 이름

ylabel(&#8216;value&#8217;); %y축 이름

legend(&#8216;sin&#8217;,&#8217;cos&#8217;); % 순서대로 그려진 그래프의 이름

title(&#8216;my plot&#8217;); %그래프 제목

print -dpng &#8216;myPlot.png&#8217; %그래프 현재 디렉토리에 저장

close; % 모든 plot 끄기

figure(1); plot(t, y1); %figure1 창에 plot(t,y1)

figure(2); plot(t, y2); %figure2 창에 plot(t,y2)

subplot(1,2,1); %1&#215;2 규격으로 그래프 칸을 나누고(1,2) // 1번째 그래프에 access

plot(t,y1);

subplot(1,2,2); %1&#215;2 규격으로 그래프 칸을 나누고(1,2) // 2번째 그래프에 access

plot(t,y2);

* * *

&nbsp;

&nbsp;

# **5. Control Loops**

* * *

v = zeros(10,1); % v = [0; 0; 0; 0; 0; 0; 0; 0; 0; 0]  
for i=1:10,  
v(i) = 2^i;  
end;

i = 1;  
while i <= 5,  
v(i) = 100;  
i = i+1;  
end

i = 1;  
while true,  
v(i) = 999;  
i = i+1;  
if i == 6,  
break;  
end;  
end

if v(1)==1,  
disp(&#8216;The value is one!&#8217;);  
elseif v(1)==2,  
disp(&#8216;The value is two!&#8217;);  
else  
disp(&#8216;The value is not one or two!&#8217;);  
end

* * *

&nbsp;

&nbsp;

# **6. Function**

* * *

.m 확장자의 파일에 우리가 함수를 정의하여놓고 사용 가능. 단 파일이 현재 디렉토리에 있어야한다.

ex1)

Function y = squareNum(x); %x를 매개변수로 받아 y를 반환하는 함수

y = x^2; %y와 x의 관계식

> >octave커맨드창에서 squareNum(5)를 입력하면 5^2 = 25 반환

ex2)

Function [y z] = squareAndCubicNum(x); %x를 매개변수로 받아 y와 z를 반환하는 함수

y = x^2;

z = x^3;

> >커맨드창에서 [a b] = squareAndCubicNum(3)을 입력하면 9와 27 반환

* * *

위의 명령어들을 다 외울 필요는 없다. 그저 한 번씩 쳐보고 실행해보면서 어떠한 기능을 하는지 가볍게 파악하면 충분하다. 조만간 이 기능들을 토대로하여 실제로 Linear Regression의 기능을 구현하는 예제를 한 번 보여줄 것이다.