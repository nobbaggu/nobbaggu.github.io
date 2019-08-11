---
title: (백준 10971)-외판원 순회
date: 2019-08-11T20:18:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 브루트포스
  - brute force
  - 자바
  - java
  - 백준
  - 10971
  - 순열
  - 외판원
  - 순회
  - 외판원 순회
---

[백준 10971 - 외판원 순회](https://www.acmicpc.net/problem/10971){: target="_blank" }

## 1. 풀이 전략
* * *

1~N의 도시를 순회하는 모든 경우의 수에 대한 비용을 계산하여 최소비용을 출력해주면 된다. 모든 순회 경로를 따지기 위해서는 순열을 사용하면 된다. 즉, 1~N 도시를 나열하고 마지막에 처음 출발한 도시를 추가해준 다음 비용을 계산한다. 다음 순열이 있는 경우 다음 순열에 대해서도 같은 과정을 반복해준다. 이렇게 해서 나온 비용들 중 최소 비용을 출력해주면 된다.

이 문제를 풀기 위한 핵심적인 알고리즘은 모든 순열을 계산하는 것이다. 아래 링크를 따라가면 다음 순열, 이전 순열에 대한 설명과 Java코드로 표현한 알고리즘을 볼 수 있다.

[순열](https://swnomad.github.io/2019/08/11/permutation/){: target="_blank" }

<br>

## 2. 소스 코드
* * *

~~~ java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

class Main{
	public static void main(String[] args) throws NumberFormatException, IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int N = Integer.parseInt(br.readLine()); //N 입력
	
		//도시간의 이동비용을 저장하기 위한 2차원 배열 생성 및 입력 저장
		int[][] w = new int[N][N];
		for(int i=0; i<N; i++) {
			StringTokenizer tk = new StringTokenizer(br.readLine());
			for(int j=0; j<N; j++) {
				w[i][j] = Integer.parseInt(tk.nextToken());
			}
		}
		
		//순열을 위한 배열 생성 및 오름차순으로 초기화
		int[] city = new int[N];
		for(int i=0; i<N; i++) {
			city[i] = i;
		}
		
		int minCost = Integer.MAX_VALUE; //최소 비용 저장을 위한 변수
		
		//다음 순열이 있는 한 경로에 대한 비용 계산
		do {
			boolean check = true;//올바른 경로인지 판단하기 위한 변수
			int sum = 0;
			
			//경로를 이동하는 데 드는 총 비용 계산
			for(int i=0; i<N-1; i++) {
				int cost = w[city[i]][city[i+1]]; 
				if(cost==0) {
					check = false;
					break;
				}else {
					sum += cost;
				}
			}
			
			//출발지로 돌아오기 위한 비용 추가
			int cost = w[city[N-1]][city[0]];
			if(cost==0) {
				check = false;
			}else {
				sum += cost;
			}
				
			if(check && sum < minCost)
				minCost = sum;
				
		}while(next_permutation(city));
		
		System.out.print(minCost);
	}
	
	//다음 순열을 계산하는 메소드
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