---
title: (백준 1476)-날짜 계산
date: 2019-08-10T15:28:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 브루트포스
  - brute force
  - 자바
  - java
  - 백준
  - 1476
---

### 1. 풀이 전략
* * *

1,1,1부터 E,S,M이 될 때 까지 증가시키기 보다는 E,S,M이 모두 1,1,1이 될 때 까지 각각 1을 감소시킨다. 이 때 필요한 연산 횟수만큼의 해가 지난 것이므로 year를 쉽게 구할 수 있다.

<br>
 
아래는 E,S,M을 매개변수로 year를 계산하는 함수가 돌아가는 규칙이다.

>1. E,S,M을 입력받아 저장
>2. while문을 사용하여 E,S,M이 모두 1이 될 때 까지 1씩 감소
>3. E,S,M이 1씩 감소될 때 year는 1씩 증가
>4. year를 리턴


### 2. 소스 코드
* * *

~~~ java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

	public static void main(String[] args) throws IOException {
		
		//입력을 받기 위해 BufferedReader 객체 생성
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		//StringTokenizer에 "E S M" 저장
		StringTokenizer tk = new StringTokenizer(br.readLine());
		
		//Integer 클래스의 parseInt 함수를 통해 tk에 저장된 문자열을 정수로 parse
		int E = Integer.parseInt(tk.nextToken());
		int S = Integer.parseInt(tk.nextToken());
		int M = Integer.parseInt(tk.nextToken());
		
		System.out.println(calcYear(E, S, M));
		
	}
	
	/*E,S,M이 주어졌을 때 연도를 계산하는 함수*/
	public static int calcYear(int E, int S, int M) {
		
		int year = 1;
		
		//E,S,M이 모두 1이지 않으면 1씩 감소(1이면 15로 변환)
		while(!(E==1 && S==1 && M==1)) {
			E = (E==1) ? 15 : E-1;
			S = (S==1) ? 28 : S-1;
			M = (M==1) ? 19 : M-1;
			
			year++;//연산 1번마다 연도를 1씩 증가
		}
		
		return year;
	}
	
}
~~~