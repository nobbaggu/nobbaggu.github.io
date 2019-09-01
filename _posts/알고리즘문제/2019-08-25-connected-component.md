---
title: (백준 11724)-연결 요소의 개수
date: 2019-08-25T16:18:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 그래프
  - bfs
  - dfs
  - 완전탐색
---

[백준 11724 - 연결 요소의 개수](https://www.acmicpc.net/problem/11724){: target="_blank" }

## 1. 풀이 전략
* * *

DFS와 BFS의 목적은 그래프의 모든 정점을 방문하는 것이다. 문제의 솔루션은 이렇다. DFS든 BFS든 사용해서 완전탐색을 한다. 이후 모든 정점을 체크하여 방문하지 않은 지점이 있으면 연결요소의 갯수를 1 증가시킨 다음 해당 정점을 출발 지점으로 다시 설정하고 또 한 번 완전탐색을 한다. 이 과정을 그래프의 모든 정점을 방문할 때 까지 한 이후 연결요소의 갯수를 출력해주면 된다.

## 2. 소스 코드
* * *

~~~ java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main{
	public static void main(String[] args) throws IOException{
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer tk = new StringTokenizer(br.readLine());
		
		//정점갯수 N, 간선갯수 M
		N = Integer.parseInt(tk.nextToken());
		M = Integer.parseInt(tk.nextToken());
		
		//그래프 정보 저장을 위한 ArrayList 배열 생성
		graph = new ArrayList[N+1];
		for(int i=1; i<=N; i++) {
			graph[i] = new ArrayList<Integer>();
		}
		
		//그래프 연결 정보 입력 받기
		for(int i=0; i<M; i++) {
			tk = new StringTokenizer(br.readLine());
			int u = Integer.parseInt(tk.nextToken());
			int v = Integer.parseInt(tk.nextToken());
			
			graph[u].add(v);
			graph[v].add(u);
		}
		
		//방문 체크 배열
		c = new boolean[N+1];
		
		System.out.print(bfs(1));
	}
	
	static int N, M;
	static ArrayList<Integer>[] graph;
	static boolean[] c;
	
	static int bfs(int start) {
		int cnt = 1;//연결요소 갯수
		boolean clear = false; //그래프 모든 정점 방문했는지 체크하기 위한 변수
		
		//방문한 정점을 저장하기 위한 큐
		Queue<Integer> q = new LinkedList<Integer>();		
		
		while(!clear) { //모든 정점 방문하지 않았으면 계속 수행
			//시작지점 방문 처리
			q.add(start); c[start] = true;
			
			while(!q.isEmpty()) {//시작점과 연결된 그래프 요소 완전 탐색
				int x = q.remove(); //q의 가장 앞 원소 꺼내기
				
				//x에서 갈 수 있는 모든 정점 탐색하여 방문하지 않았으면 방문
				for(int i=0; i<graph[x].size(); i++) {
					int y = graph[x].get(i);
					if(!c[y]) {
						q.add(y); c[y] = true;
					}
				}
			}
			
			clear = true; //모든 정점 다 방문했다고 가정		
			//정점을 처음부터 체크하여 방문하지 않은 정점이 있으면 시작점을 해당 정점으로 초기화 하고 break
			for(int i=1; i<=N; i++) {
				if(!c[i]) { //정점 i에 방문하지 않았다면
					start = i; //시작지점 i로 초기화
					clear = false; // 완전탐색 false
					cnt++; //연결 요소 1 증가
					break;
				}
			}
		}
		
		return cnt; //연결요소 갯수 반환		
	}
}
~~~