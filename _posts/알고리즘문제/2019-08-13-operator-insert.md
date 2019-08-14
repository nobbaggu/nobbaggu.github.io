---
title: (백준 14888)-연산자 끼워넣기
date: 2019-08-13T21:18:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 브루트포스
  - brute force
  - 자바
  - java
  - 백준
  - 14888
  - 순열
  - 연산자 끼워넣기
---

[백준 14888 - 연산자 끼워넣기](https://www.acmicpc.net/problem/14888){: target="_blank" }

## 1. 풀이 전략
* * *

\+, \-, x, / 차례대로 1, 2, 3, 4로 둔다. 그리고 N-1개의 배열을 만들어 연산자의 갯수대로 1,2,3,4를 추가하는 것이다. 예를들어 \+ 2개, \- 1개, x 3개, / 2개라고 하면 {1, 1, 2, 3, 3, 3, 4}가 되는 것이다. 그리고 이 배열을 다음 순열 next_permutation을 사용해 처음부터 끝까지 돌리면서 각각의 경우에 숫자에 대해서 연산을 하여 결과를 구한다. 이렇게 해서 나온 최대값, 최소값을 출력해주면 된다.

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
		
		//수의 갯수
		int N = Integer.parseInt(br.readLine());
		
		//N개의 수 입력 받기
		StringTokenizer tk = new StringTokenizer(br.readLine());
		int[] a = new int[N];
		for(int i=0; i<N; i++) {
			a[i] = Integer.parseInt(tk.nextToken());
		}
		
		//차례대로 +, -, x, / 연산자 갯수 입력받기
		tk = new StringTokenizer(br.readLine()); br.close();
		int[] ops = new int[N-1]; //연산자를 저장할 배열
		int idx = 0; //연산자를 넣을 배열의 index
		for(int i=1; i<=4; i++) {//1,2,3,4 차례대로 +, -, x, /
			int opNum = Integer.parseInt(tk.nextToken()); //연산자 i의 갯수
			while(opNum>0) { //각 연산자의 갯수만큼 ops 배열에 연산자 추가
				ops[idx++] = i;
				opNum--;
			}
		}
		
		//최대값, 최소값 초기화
		int max = Integer.MIN_VALUE;
		int min = Integer.MAX_VALUE;
		
		do {
			int result = a[0];
			for(int i=1; i<N; i++) {
				switch(ops[i-1]) {
					case 1:
						result += a[i];
						break;
					
					case 2:
						result -= a[i];
						break;
					
					case 3:
						result *= a[i];
						break;
					
					case 4:
						result /= a[i];
						break;
						
					default:
						break;
				}
			}
			
			//이전 최대값, 최소값과 비교
			max = (result > max) ? result : max;
			min = (result < min) ? result : min;
		}while(next_permutation(ops));
		
		System.out.print(max + "\n" + min);
		
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