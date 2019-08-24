---
title: (백준 15658)-연산자 끼워넣기2
date: 2019-08-18T16:18:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 브루트포스
  - brute force
  - 자바
  - java
  - 백준
  - 재귀
  - recursion
  - 15658
  - 연산자
---

[백준 15658 - 연산자 끼워넣기2](https://www.acmicpc.net/problem/15658){: target="_blank" }

## 1. 풀이 전략
* * *

이 문제는 가능한 모든 연산을 한 이후 최대와 최소를 구할 수 있다.

먼저 숫자 N개가 주어진다. 그리고 N-1개보다 크거나 같은 수의 연산자가 주어진다. 각각의 숫자 자리에 더하기, 빼기, 곱하기, 나누기 중에 무엇을 넣을지 선택하고 중간연산을 한 이후 다음으로 이동한다. 여기서 힌트가 보인다. 중간 연산 결과와 남은 연산자 수를 매개변수로 하여 매개변수로 전달해주는 재귀함수를 사용해서 구현할 수 있겠다. 이러한 과정을 반복하여 N-1번의 연산이 된 경우에는 최대, 최소와 비교하고 호출을 종료하면 될 것이다. 이렇게 하면 모든 경우에 대해 연산결과를 얻을 수 있고, 자연스레 최대값과 최소값을 얻을 수 있다.

재귀호출 알고리즘은 크게 세 과정으로 분할하여 짜면 된다.

>1. 더 이상 진행해도 정답일 가능성이 없는 경우
>2. 정답 후보를 찾은 경우
>3. 아직 정답을 찾지 못하여 다음 순서를 호출하는 경우

각각의 과정으로 나누어 생각하면 비교적 쉽게 재귀호출 알고리즘을 짤 수 있다.

조금 더 자세한 내용은 아래의 링크로 이어지는 포스팅에서 볼 수 있다. 그렇게 길지 않으므로 시간적 여유가 된다면 한 번 봐두어 같은 유형의 문제에 계속해서 적용할 수 있다.

<br>
[재귀](https://swnomad.github.io/2019/08/11/recursion/){: target="_blank" }

<br>

## 2. 소스 코드
* * *

~~~ java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main{
	public static void main(String[] args) throws NumberFormatException, IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		//숫자의 갯수 N 입력
		N = Integer.parseInt(br.readLine());
		
		//숫자 입력
		nums = new int[N];
		StringTokenizer tk = new StringTokenizer(br.readLine());
		for(int i=0; i<N; i++) {
			nums[i] = Integer.parseInt(tk.nextToken());
		}
		
		//연산자 수 입력. 차례대로 +,-,*,/
		ops = new int[4];
		tk = new StringTokenizer(br.readLine());
		for(int i=0; i<4; i++) {
			ops[i] = Integer.parseInt(tk.nextToken());
		}
		
		recursion(nums[0], ops[0], ops[1], ops[2], ops[3], 1);
		
		System.out.print(max+"\n"+min);
		
	}
	
	static int N;
	static int[] nums;
	static int[] ops;
	static int max = -1000000000;
	static int min = 1000000000;
	
	static void recursion(int curResult, int add, int sub, int mul, int div, int idx) {
		
		//정답 후보를 찾은 경우 : idx가 N이면 N개의 숫자에 대해서 연산을 한 것이므로 max, min과 비교하고 호출 종료
		if(idx==N) {
			if(curResult > max) 
				max = curResult;
			if(curResult < min)
				min = curResult;
			
			return;
		}
		
		//다음 재귀 호출
		if(add>0) { //더하기를 하는 경우
			recursion(curResult+nums[idx], add-1, sub, mul, div, idx+1);
		}
		
		if(sub>0) { //빼기를 하는 경우
			recursion(curResult-nums[idx], add, sub-1, mul, div, idx+1);
		}
		
		if(mul>0) { //곱하기를 하는 경우
			recursion(curResult*nums[idx], add, sub, mul-1, div, idx+1);
		}
		
		if(div>0) { //나누기를 하는 경우
			recursion(curResult/nums[idx], add, sub, mul, div-1, idx+1);
		}
	}
}
~~~