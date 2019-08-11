---
title: (백준 14500)-테트로미노
date: 2019-08-10T16:28:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 브루트포스
  - brute force
  - 자바
  - java
  - 백준
  - 14500
---

### 1. 풀이 전략
* * *
모든 가능한 모양에 대해 계산하여 최대값을 구하는 브루트포스 알고리즘을 통해 해결할 수 있다.

테트로미노에는 총 5가지 모양이 있고, 각각에 대해 대칭, 회전을 고려하면 19가지 모양이 나온다. 각각에 대해 NxM 종이 위에 올라갈 수 있는 모든 경우에 대해 숫자의 합을 계산하면 된다.

<br>

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
		
		/*첫째 줄을 입력받아 N,M 저장 */
		StringTokenizer tk = new StringTokenizer(br.readLine());
		int N = Integer.parseInt(tk.nextToken());
		int M = Integer.parseInt(tk.nextToken());
		
		/* N x M의 2차원 배열을 만들고 정수를 저장
		 * index는 1~N, 1~M을 사용한다.*/
		int[][] map = new int[N+1][M+1];
		for(int i=1; i<=N; i++) {
			tk = new StringTokenizer(br.readLine());
			for(int j=1; j<=M; j++) {
				map[i][j] = Integer.parseInt(tk.nextToken());
			}
		}
		
		int max = 0;
		
		// ㅁㅁ
		// ㅁㅁ
		for(int i=1; i<=N-1; i++) {
			for(int j=1; j<=M-1; j++) {
				int sum = map[i][j]+map[i+1][j]+map[i][j+1]+map[i+1][j+1];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁㅁㅁ
		//    ㅁ
		for(int i=1; i<=N-1; i++) {
			for(int j=1; j<=M-2; j++) {
				int sum = map[i][j]+map[i][j+1]+map[i][j+2]+map[i+1][j+2];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁㅁㅁ
		//    ㅁ x충 대칭
	
		for(int i=2; i<=N; i++) {
			for(int j=1; j<=M-2; j++) {
				int sum = map[i][j] + map[i][j+1] + map[i][j+2] + map[i-1][j+2];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁㅁㅁ
		//    ㅁ 90도 회전
		for(int i=3; i<=N; i++) {
			for(int j=1; j<=M-1; j++) {
				int sum = map[i][j] + map[i][j+1] + map[i-1][j+1] + map[i-2][j+1];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁㅁㅁ
		//    ㅁ 90도 회전 + x축 대칭
		for(int i=1; i<=N-2; i++) {
			for(int j=1; j<=M-1; j++) {
				int sum = map[i][j] + map[i][j+1] + map[i+1][j+1] + map[i+2][j+1];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁㅁㅁ
		//    ㅁ 180도 회전
		for(int i=1; i<=N-1; i++) {
			for(int j=1; j<=M-2; j++) {
				int sum = map[i][j] + map[i+1][j] + map[i+1][j+1] + map[i+1][j+2];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁㅁㅁ
		//    ㅁ 180도 회전 + x축 대칭
		for(int i=2; i<=N; i++) {
			for(int j=1; j<=M-2; j++) {
				int sum = map[i][j] + map[i-1][j] + map[i-1][j+1] + map[i-1][j+2];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁㅁㅁ
		//    ㅁ 270도 회전
		for(int i=1; i<=N-2; i++) {
			for(int j=2; j<=M; j++) {
				int sum = map[i][j] + map[i][j-1] + map[i+1][j-1] + map[i+2][j-1];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁㅁㅁ
		//    ㅁ 270도 회전 + x축 대칭
		for(int i=1; i<=N-2; i++) {
			for(int j=1; j<=M-1; j++) {
				int sum = map[i][j] + map[i+1][j] + map[i+2][j] + map[i+2][j+1];
				if(sum > max)
					max = sum;
			}
		}
		
		//ㅗ
		for(int i=1; i<=N-1; i++) {
			for(int j=2; j<=M-1; j++) {
				int sum = map[i][j] + map[i+1][j] + map[i+1][j-1] + map[i+1][j+1];
				if(sum > max)
					max = sum;
			}
		}
		
		//ㅏ
		for(int i=2; i<=N-1; i++) {
			for(int j=2; j<=M; j++) {
				int sum = map[i][j] + map[i][j-1] + map[i-1][j-1] + map[i+1][j-1];
				if(sum > max)
					max = sum;
			}
		}
		
		//ㅓ
		for(int i=2; i<=N-1; i++) {
			for(int j=1; j<=M-1; j++) {
				int sum = map[i][j] + map[i][j+1] + map[i-1][j+1] + map[i+1][j+1];
				if(sum > max)
					max = sum;
			}
		}
		
		//ㅜ
		for(int i=2; i<=N; i++) {
			for(int j=2; j<=M-1; j++) {
				int sum = map[i][j] + map[i-1][j] + map[i-1][j-1] + map[i-1][j+1];
				if(sum > max)
					max = sum;
			}
		}
		
		//ㅡ
		for(int i=1; i<=N; i++) {
			for(int j=1; j<=M-3; j++) {
				int sum = map[i][j] + map[i][j+1] + map[i][j+2] + map[i][j+3];
				if(sum > max)
					max = sum;
			}
		}
		
		//ㅣ
		for(int i=1; i<=N-3; i++) {
			for(int j=1; j<=M; j++) {
				int sum = map[i][j] + map[i+1][j] + map[i+2][j] + map[i+3][j];
				if(sum > max)
					max = sum;
			}
		}
	
		
		// ㅁ
		// ㅁㅁ
		//   ㅁ
		for(int i=1; i<=N-2; i++) {
			for(int j=1; j<=M-1; j++) {
				int sum = map[i][j] + map[i+1][j] + map[i+1][j+1] + map[i+2][j+1];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁ
		// ㅁㅁ
		//   ㅁ y축 대칭
		for(int i=1; i<=N-2; i++) {
			for(int j=2; j<=M; j++) {
				int sum = map[i][j] + map[i+1][j] + map[i+1][j-1] + map[i+2][j-1];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁ
		// ㅁㅁ
		//   ㅁ 90도 회전
		for(int i=2; i<=N; i++) {
			for(int j=1; j<=M-2; j++) {
				int sum = map[i][j] + map[i][j+1] + map[i-1][j+1] + map[i-1][j+2];
				if(sum > max)
					max = sum;
			}
		}
		
		// ㅁ
		// ㅁㅁ
		//   ㅁ 90도 회전 + y축 대칭
		for(int i=1; i<=N-1; i++) {
			for(int j=1; j<=M-2; j++) {
				int sum = map[i][j] + map[i][j+1] + map[i+1][j+1] + map[i+1][j+2];
				if(sum > max)
					max = sum;
			}
		}
		
		System.out.print(max);
	}
	
}
~~~