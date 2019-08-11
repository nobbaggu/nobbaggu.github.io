---
title: (백준 2309)-일곱 난쟁이
date: 2019-08-10T13:28:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 브루트포스
  - brute force
  - 자바
  - java
  - 백준
  - 2309
---



### 1. 풀이 전략
* * *

9개의 키가 주어지면 그 중 합이 되는 7개를 찾아 오름차순으로 출력해주는 것이다. 모든 경우의 수를 따지는 브루트 포스 알고리즘으로 풀 수 있다.

>1. 9개의 입력을 받아 배열에 저장한다.
>2. 9개의 합을 변수 sum에 저장한다.
>3. 2중 for문을 사용해 2개를 골라 sum에서 빼주어 합이 100이 될 때 까지 찾는다.
>4. 찾을 경우 그 2개의 배열 값을 0으로 만든 후 2중 for문을 탈출한다.
>5. 배열을 오름차순으로 만든 후 0이 아닌 모든 배열의 요소를 출력한다.

### 2. 소스 코드
* * *

~~~ java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {

	public static void main(String[] args) throws IOException {
		
		//입력을 받기 위해 BufferedReader 객체 생성
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int[] dwarfs = new int[9]; //키를 저장할 배열
		
		/*배열에 9명의 난쟁이 키 입력을 저장*/
		for(int i=0; i<dwarfs.length; i++) {
			dwarfs[i] = Integer.parseInt(br.readLine());
		}
		
		br.close(); //입력이 끝난 후 BufferedReader 객체 해제
		
		/*9명의 난쟁이 키의 총합*/
		int sum = 0;
		for(int i=0; i<dwarfs.length; i++) {
			sum += dwarfs[i];
		}
		
		/* 가짜 2명을 찾아서 키를 0으로 만들기 */
		for(int i=0; i<dwarfs.length-1; i++) {
			for(int j=i+1; j<dwarfs.length; j++) {
				if(sum-dwarfs[i]-dwarfs[j]==100) {
					dwarfs[i] = 0;
					dwarfs[j] = 0;
					
					/*조건식이 false가 되게 만들어 반복문 탈출*/
					i=dwarfs.length-1;
					j=dwarfs.length;
				}
			}
		}
		
		/* 오름차순으로 정렬 */
		Arrays.sort(dwarfs);
			
		/* 0이 아닌 것만 출력하여 진짜 7명 난쟁이의 키 출력 */
		for(int i=0; i<dwarfs.length; i++) {
			if(dwarfs[i]!=0) {
				System.out.println(dwarfs[i]);
			}
		}
		
	}
	
}
~~~