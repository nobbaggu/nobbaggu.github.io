---
title: (백준 10974)-모든 순열
date: 2019-08-11T18:48:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 브루트포스
  - brute force
  - 자바
  - java
  - 백준
  - 10974
  - 순열
  - 모든
  - next
  - previous
  - permutation
---

[백준 10974 - 모든 순열](https://www.acmicpc.net/problem/10974){: target="_blank" }

## 1. 풀이 전략
* * *

[순열](https://swnomad.github.io/2019/08/11/permutation/){: target="_blank" }

먼저 위 링크에서 순열이 무엇인지, 다음 순열과 이전 순열은 어떻게 구하는지 읽어보아야 한다. 또한 위 링크에 다음 순열과 이전 순열을 구하는 Java 코드가 있으므로, 그 코드를 사용하면 이 문제는 바로 해결할 수 있다.

먼저 처음 순열을 출력하고 다음 순열이 있는 동안, 즉 next_permutation의 결과가 true가 되는한 계속 출력해주면 되는 단순한 문제이다.

<br>

## 2. 소스 코드
* * *

~~~ java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

class Main{
	public static void main(String[] args) throws NumberFormatException, IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int N = Integer.parseInt(br.readLine()); //N 입력
		
		//순열을 위한 배열을 생성하고 순서대로 숫자를 저장하여 첫 순열을 만든다.
		int[] a = new int[N];
		for(int i=0; i<N; i++) {
			a[i] = i+1;
		}
		
		//첫 순열을 한 번 출력해주고 다음 순열이 있는 한 계속해서 출력해준다.
		do {
			for(int i=0; i<N; i++) {
				System.out.print(a[i]+" ");
			}
			System.out.println();
		}while(next_permutation(a));
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