---
title: (백준 2178)-미로 탐색
date: 2019-09-01T16:18:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 그래프
  - bfs
  - dfs
  - 완전탐색
  - 단지번호
  - 백준
  - boj
---

[백준 2178 - 미로 탐색](https://www.acmicpc.net/problem/2178){: target="_blank" }

## 1. 풀이 전략
* * *

최단거리를 찾는 문제이므로 BFS를 사용하여 해결할 수 있다. 여기서 한 가지 중요한 것은 거리 정보를 저장하기 위한 2차원 배열 d이다. BFS로 탐색을 하면서 간선을 옮길 때 마다 거리를 1 증가시켜 저장하면 (N,M)지점의 거리를 알 수 있다.

## 2. 소스 코드
* * *

~~~ java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main{
	public static void main(String[] args) throws IOException{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer tk = new StringTokenizer(br.readLine());
		
		// 지도 크기 N, M 입력
		N = Integer.parseInt(tk.nextToken());
		M = Integer.parseInt(tk.nextToken());
		
		// 지도 정보 입력
		map = new int[N][M];
		for(int i=0; i<N; i++) {
			String str = br.readLine();
			for(int j=0; j<M; j++) {
				map[i][j] = Character.getNumericValue(str.charAt(j));
			}
		}
		
		// 시작점부터의 거리를 저장하기 위한 배열
		d = new int[N][M];
		
		bfs(new Pair(0,0));
		
		System.out.println(d[N-1][M-1]);
	}
	
	static int N, M;
	static int[][] map;
	static int[][] d;
	
	static void bfs(Pair start) {
		// 방문한 지점을 저장하기 위한 큐
		Queue<Pair> q = new LinkedList<>();
		
		//시작점 방문 처리
		q.add(start);
		d[start.r][start.c] = 1; //시작위치 거리 1 저장
		
		
		while(!q.isEmpty()) {
			Pair x = q.remove();
			int[] dr = {-1,1,0,0};
			int[] dc = {0,0,1,-1};
			
			//이번 지점과 연결된 상하좌우 탐색
			for(int i=0; i<4; i++) {
				Pair nx = new Pair(x.r+dr[i], x.c+dc[i]);
				
				// 지도 범위안에 있고 & 방문하지 않았고 & 1인 곳 방문
				if(isInRange(nx) && d[nx.r][nx.c]==0 && map[nx.r][nx.c]==1) {
					q.add(nx);
					d[nx.r][nx.c]= d[x.r][x.c] + 1; 
				}
			}
		}
	}
	
	// 지도 범위 안에 있는지 판단하기 위한 함수
	static boolean isInRange(Pair x) {
		if(x.r>=0 && x.r<N && x.c>=0 && x.c<M) {
			return true;
		}
		
		return false;
	}
}

class Pair{
	int r;
	int c;
	
	public Pair(int r, int c) {
		this.r = r;
		this.c = c;
	}
}
~~~