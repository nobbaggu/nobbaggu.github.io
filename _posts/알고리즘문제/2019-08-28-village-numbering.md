---
title: (백준 2667)-단지번호 붙이기
date: 2019-08-28T20:18:34+09:00
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

[백준 2667 - 단지번호 붙이기](https://www.acmicpc.net/problem/2667){: target="_blank" }

## 1. 풀이 전략
* * *

이 문제는 완전탐색으로 해결할 수 있다. 집이 있는 곳을 그래프의 정점으로 보고 좌우상하 네 집과 간선이 연결된 것으로 생각할 수 있다. 1이 붙은 임의의 지점에서 시작해서 인접한 1인 곳을 모두 탐색한다. 그리고 맵을 처음부터 다시 확인하여 방문하지 않은 1이 있으면 거기서 부터 또 한 번 탐색을 시작한다. 이러한 식으로 지도상의 모든 1을 방문할 때 까지 완전탐색을 하면 마을의 수와 마을 당 집의 수를 구할 수 있다.

## 2. 소스 코드
* * *

~~~ java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Collections;
import java.util.LinkedList;
import java.util.Queue;

public class Main{
	public static void main(String[] args) throws IOException{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		//지도 크기 N
		N = Integer.parseInt(br.readLine());
		
		//지도 정보 입력
		map = new int[N][N];
		for(int i=0; i<N; i++) {
			String str = br.readLine();
			for(int j=0; j<N; j++) {
				map[i][j] = Character.getNumericValue(str.charAt(j));
			}
		}
		
		//방문한 지점 체크를 위한 배열
		c = new boolean[N][N];
		
		bfs();
	}
	
	static int N;
	static int[][] map;
	static boolean[][] c;
	
	static void bfs() {
		
		//결과를 저장하기 위한 ArrayList
		ArrayList<Integer> res = new ArrayList<>();
		
		//집이 있는 곳을 찾아 최초 시작점으로 설정
		int r0=0, c0=0;
		for(int i=0; i<N; i++) {
			for(int j=0; j<N; j++) {
				if(map[i][j]==1) { //집이 있는 곳 찾으면
					r0 = i; c0 = j;
					i=N; j=N; //2중 for문 탈출
				}
			}
		}
		
		//지도에 집이 있는 곳이 존재하지 않으면 0 출력 후 탐색 종료
		if(r0==0 && c0==0 && map[0][0]==0) {
			System.out.print(0);
			return;
		}
		
		Pair x = new Pair(r0, c0);//map[r0][c0]을 시작점으로
		Queue<Pair> q = new LinkedList<>(); //방문한 곳을 저장하기 위한 큐
		q.add(x); c[x.r][x.c]= true; //x 방문
		
		boolean clear = false; //지도의 모든 곳 탐색했는지 체크하기 위한 변수
		while(!clear) {
			
			int cnt = 0;
			while(!q.isEmpty()) {
				x = q.remove(); cnt++; //q에서 가져올 때 집 갯수 카운트
				
				//인접한 네 지점 탐색
				int[] dr = {-1,0,1,0};
				int[] dc = {0,1,0,-1}; 
				for(int i=0; i<4; i++) {
					//방문하지 않았으면 방문 처리
					if(isInRange(x.r+dr[i], x.c+dc[i]) && map[x.r+dr[i]][x.c+dc[i]]==1 && !c[x.r+dr[i]][x.c+dc[i]]) {
						c[x.r+dr[i]][x.c+dc[i]] = true;
						q.add(new Pair(x.r+dr[i], x.c+dc[i]));
					}
				}
			}
			
			res.add(cnt);//마을의 집 수를 res에 저장
			
			clear = true; //clear는 true라 가정
			for(int i=0; i<N; i++) {
				for(int j=0; j<N; j++) {
					//아직 방문하지 않은 지점 있으면 해당 지점부터 다시 인접한 곳 탐색 시작
					if(!c[i][j] && map[i][j]==1) {
						c[i][j] = true;
						q.add(new Pair(i,j));
						clear = false;
						i=N; j=N; //2중 for문 탈출
					}
				}
			}
		}
		
		//결과 오름차순 정렬
		Collections.sort(res);
		
		//모든 지점 탐색완료 후 결과 출력
		System.out.println(res.size()); //마을의 수 출력
		for(int i=0; i<res.size(); i++) { //마을 당 집 수 출력
			System.out.println(res.get(i));
		}
	}
	
	static boolean isInRange(int r, int c) {
		if(r>=0 && c>=0 && r<N && c<N)
			return true;
		
		return false;
	}
}

class Pair{
	int r; //행
	int c; //열
	
	public Pair(int r, int c) {
		this.r = r;
		this.c = c;
	}
}
~~~