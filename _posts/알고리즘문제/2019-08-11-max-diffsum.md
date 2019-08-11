---
title: (백준 10819)-차이를 최대로
date: 2019-08-11T19:18:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 브루트포스
  - brute force
  - 자바
  - java
  - 백준
  - 10819
  - 순열
  - 다음
  - 이전
  - previous
  - next
  - permutation
---

[백준 10819 - 차이를 최대로](https://www.acmicpc.net/problem/10819){: target="_blank" }

## 1. 풀이 전략
* * *

주어진 숫자들로 만들 수 있는 모든 순열들에 대해 차이의 합계를 구해보고 가장 큰 것을 출력해주면 된다. 따라서 이 문제는 모든 경우를 따져보는 브루트포스 알고리즘으로 풀 수 있다. 또한 모든 순열을 구해볼 수 있는 함수는 아래의 링크로 연결되는 포스팅에 설명되어있다.

[순열](https://swnomad.github.io/2019/08/11/permutation/){: target="_blank" }

<br>

## 2. 소스 코드
* * *

~~~ java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

class Main{
	public static void main(String[] args) throws NumberFormatException, IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int N = Integer.parseInt(br.readLine()); //N 입력
	
		//순열 입력을 받을 StringTokenizer 객체 생성
		StringTokenizer tk = new StringTokenizer(br.readLine());
	
		//순열을 위한 배열을 생성하고 StringTokenizer로부터 숫자를 받아 저장
		int[] a = new int[N];
		for(int i=0; i<N; i++) {
			a[i] = Integer.parseInt(tk.nextToken());
		}
		
		Arrays.sort(a); //배열 a를 오름차순으로 정렬
		
		int maxDiffSum = diffSum(a);//차이의 최대값을 a의 diffSum으로 초기화
		
		//다음 순열을 구해서 차이의 합계를 구해서 더 크면 maxDiffSum 갱신
		while(next_permutation(a)) {
			int diffSum = diffSum(a);
			if(maxDiffSum < diffSum)
				maxDiffSum = diffSum;
		}//마지막 순열까지 다 계산하면 끝
		
		//결과 출력
		System.out.print(maxDiffSum);
		
	}
	
	public static int diffSum(int[] a) {
		int sum = 0; //차이를 누적할 변수 생성
		for(int i=0; i<a.length-1; i++) {
			int diff = a[i]-a[i+1];
			sum += (diff > 0) ? diff : -diff; //절대값을 sum에 더하기
		}
		
		return sum;
	}
	
	public static boolean next_permutation(int[] a) {
		
		//길이가 2보다 작으면 다음 순열 없으므로 false 리턴
		if(a.length<2)
			return false;
		
		//1. A[i-1]<A[i]인 가장 큰 i를 찾는다.
		int i = a.length-1;
		while(i>0 && a[i-1]>=a[i]) {
			i--;
		}
		
		//이미 모든 순열이 내림차순일 경우 다음 순열이 없고 이 때 i는 0이 되어있을 것이므로 false 반환
		if(i==0)
			return false;
		
		//2. i<=j이면서 A[i-1]<A[j]를 만족하는 가장 작은 A[j], 즉 가장 큰 j를 찾는다.
		int j=a.length-1;
		while(a[i-1]>=a[j])
			j--;
		
		//3. A[i-1]과 A[j]를 swap한다.
		int temp = a[i-1];
		a[i-1] = a[j];
		a[j] = temp;
		
		//4. A[i]부터 뒤로 있는 순열을 뒤집는다.
		j=a.length-1;
		while(i<j) {
			temp = a[i];
			a[i] = a[j];
			a[j] = temp;
			
			i++;
			j--;
		}
		
		return true;
	}
}
~~~