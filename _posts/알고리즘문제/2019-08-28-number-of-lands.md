---
title: (백준 4963)-섬의 개수
date: 2019-08-28T21:18:34+09:00
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

[백준 4963 - 섬의 개수](https://www.acmicpc.net/problem/4963){: target="_blank" }

## 1. 풀이 전략
* * *

이 문제는 완전탐색으로 해결할 수 있다. 땅(1)을 그래프의 정점으로 보고 좌우상하, 대각선4방향 총 8방향의 땅과 간선이 연결된 것으로 생각할 수 있다. 1이 붙은 임의의 지점에서 시작해서 인접한 1인 곳을 모두 탐색한다. 그리고 맵을 처음부터 다시 확인하여 방문하지 않은 1이 있으면 거기서 부터 또 한 번 탐색을 시작한다. 이러한 식으로 지도상의 모든 1을 방문할 때 까지 완전탐색을 하면 섬의 수를 구할 수 있다.

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
		
		StringBuilder sb = new StringBuilder(); //결과 저장 버퍼
		while(true) {
			StringTokenizer tk = new StringTokenizer(br.readLine());
			w = Integer.parseInt(tk.nextToken());
			h = Integer.parseInt(tk.nextToken());
			
			if(w==0 && h==0)
				break;
			
			map = new int[h][w];
			for(int i=0; i<h; i++) {
				tk = new StringTokenizer(br.readLine());
				for(int j=0; j<w; j++) {
					map[i][j] = Integer.parseInt(tk.nextToken());
				}
			}
			
			c = new boolean[h][w];

			sb.append(bfs()+"\n");//결과 출력
		}
		
		System.out.print(sb);
	}
	
	static int w, h;
	static int[][] map;
	static boolean[][] c;
	
	static int bfs() {
		
		//탐색을 시작지점(땅) 찾기
		int r0=0, c0=0;
		for(int i=0; i<h; i++) {
			for(int j=0; j<w; j++) {
				if(map[i][j]==1) {
					r0=i; c0=j;
					i=h; j=w; //2중 for문 탈출
				}
			}
		}
		
		//땅이 없으면 0 출력하고 탐색 종료
		if(r0==0 && c0==0 && map[r0][c0]==0) {
			return 0;
		}
		
		Queue<Pair> q = new LinkedList<>();
		
		//탐색 시작 지정 방문 처리
		q.add(new Pair(r0, c0));
		c[r0][c0] = true;
		
		int cnt = 1; //섬의 갯수
		
		boolean clear = false; //지도 상의 모든 땅 방문했는지 체크하기 위한 변수
		while(!clear) {
			while(!q.isEmpty()) {
				Pair x = q.remove();
				int[] dr = {-1,-1,-1,0,0,1,1,1};
				int[] dc = {-1,0,1,-1,1,-1,0,1};
				
				//상하좌우 탐색하여 방문하지 않은 땅이면 q에 저장
				for(int i=0; i<8; i++) {
					Pair nx = new Pair(x.r+dr[i], x.c+dc[i]);
					if(isInRange(nx.r, nx.c) && map[nx.r][nx.c]==1 && !c[nx.r][nx.c]) {
						c[nx.r][nx.c]= true;
						q.add(nx);
					}
				}
			}
			
			clear = true;//모든 땅 탐색했다고 가정
			//아직 방문하지 않은 땅이 있으면 그 지점부터 다시 탐색 시작 . 그리고 섬의 수 1 증가
			for(int i=0; i<h; i++) {
				for(int j=0; j<w; j++) {
					if(!c[i][j] && map[i][j]==1) {
						c[i][j] = true;
						q.add(new Pair(i,j));
						clear = false;
						cnt++; //섬 수 1 증가
						i=h; j=w; //2중 for문 탈출
					}
				}
			}
		}
	
		return cnt; // 섬의 수 반환
	}
	
	static boolean isInRange(int r, int c) {
		if(r>=0 && r<h && c>=0 && c<w)
			return true;
		
		return false;
	}
}

class Pair{
	int r, c;
	
	Pair(int r, int c){
		this.r = r;
		this.c = c;
	}
}
~~~