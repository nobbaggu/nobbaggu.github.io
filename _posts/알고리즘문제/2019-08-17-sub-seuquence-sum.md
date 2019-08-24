---
title: (백준 1182)-부분수열의 합
date: 2019-08-17T14:18:34+09:00
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
  - 1182
  - 부분
  - 수열
  - 집합
---

[백준 1182 - 부분수열의 합](https://www.acmicpc.net/problem/1182){: target="_blank" }

## 1. 풀이 전략
* * *

집합의 처음 원소부터 마지막 원소까지 각각을 포함하는 경우와 포함하지 않는 경우를 고려해주면 된다. 따라서 재귀호출을 사용해서 해결할 수 있다. 이 경우도 모든 경우를 따져보는 것이므로 브루트포스 문제이다.

원소를 더했을 때 그 결과가 S이면 카운트를 1 증가시켜주면 된다. 그러나 이 때 호출을 종료하지 않아야한다. 왜냐하면 이후에 원소를 추가적으로 더하면서 또다시 0이 될 수 있는 경우가 나올 가능성이 있기 때문이다.	

<br>

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
		
		//N, S 입력받기
		StringTokenizer tk = new StringTokenizer(br.readLine());
		int N = Integer.parseInt(tk.nextToken());
		int S = Integer.parseInt(tk.nextToken());
		
		//수열 입력받기
		tk = new StringTokenizer(br.readLine());
		int[] arr = new int[N];
		for(int i=0; i<N; i++) {
			arr[i] = Integer.parseInt(tk.nextToken());
		}
		
		recursion(arr, S, 0, 0, false);
		System.out.print(cnt);
	}
	
	static int cnt = 0;
	
	static void recursion(int[] arr, int S, int idx, int sum, boolean notCounted) {
		
		//정답 후보를 찾은 경우
		//집계되지 않았고 합계가 S이면 cnt 1 증가
		if(notCounted && sum==S) {
			cnt++;
			//호출 종료하지 않고 계속
		}
		
		//더 이상 정답일 가능성이 없는 경우
		if(idx >= arr.length)
			return;
		
		//정답을 아직 찾지 못한 경우
		//1)idx 번 째 원소를 더할 경우
		recursion(arr, S, idx+1, sum+arr[idx], true);
		
		//2)idx 번 째 원소를 더하지 않을 경우		
		//전혀 새로운 수열이 아니므로 notCounted에 false를 넣어주어 cnt를 증가시키지 말아야한다
		recursion(arr, S, idx+1, sum, false);
	}
}
~~~

<br>

notCounted 변수는 단어의 의미처럼 아직 카운트되지 않았다는 의미이다. 만약 지금까지의 sum이 0이라서 이미 cnt가 1 증가되었는데, idx번 째 원소를 더하지 않을 경우에는 또다시 sum이 0이다. 그러나 이 경우는 이미 고려를 한 경우이기 때문에 카운트되지 말아야하므로 notCounted 자리에 false를 넣어주어 계산에 포함시키지 말아야한다.