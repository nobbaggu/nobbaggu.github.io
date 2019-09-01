---
title: (백준 1707)-이분 그래프
date: 2019-08-27T20:18:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 그래프
  - bfs
  - dfs
  - 완전탐색
  - 이분그래프
  - 연결요소
---

[백준 1707 - 이분 그래프](https://www.acmicpc.net/problem/1707){: target="_blank" }

## 1. 풀이 전략
* * *

그래프의 정점들을 두 집합으로 나누었을 때 같은 집합 이내에 있는 정점끼리는 연결되어있지 않아야 한다. 이러한 집합이 존재할 경우의 그래프를 '이분 그래프'라고 한다는 것이다. 이 문제는 BFS나 DFS와 같은 완전탐색으로 해결할 수 있다. BFS를 사용하면 아래에서 설명하는 사고의 흐름으로 문제를 풀 수 있다.

> 1. 임의의 시작점을 그룹1에 넣는다.
> 2. 연결된 모든 정점들을 큐(Queue)에 넣고 그룹은 2로 한다. 연결되어있기 때문에 항상 다른 그룹이어야 하기 때문이다.
> 3. 큐에서 정점(x)을 하나 가져온다. 연결된 정점(y)들을 탐색하면서 탐색하지 않았던 정점은 탐색을 하고 이미 탐색했던 정점은 그룹을 비교해본다.
> 4. x와 y의 그룹이 같다면 이분 그래프가 아니다. 연결된 정점끼리 같은 그룹이기 때문이다.

<br>
주의할 점은 아직 끝이 아니란 것이다. 그래프의 연결 요소가 1개가 아닐 수 있기 때문이다. 정점 1부터 N까지 방문했는지 확인하고 아직 방문하지 않은 정점이 있다면 해당 정점을 시작점으로 해서 위 과정을 한번 더 거쳐야한다. 모든 연결요소가 이분 그래프라야 전체 그래프가 이분 그래프가 된다.

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
		
		//테스트케이스 수
		int K = Integer.parseInt(br.readLine());
		
		StringBuilder sb = new StringBuilder();
		
		for(int i=0; i<K; i++) {
			StringTokenizer tk = new StringTokenizer(br.readLine());
			
			//그래프의 정점과 간선의 갯수
			N = Integer.parseInt(tk.nextToken());
			M = Integer.parseInt(tk.nextToken());
			
			//그래프 정보 저장을 위한 객체 생성
			graph = new ArrayList[N+1];
			for(int j=1; j<=N; j++) {
				graph[j] = new ArrayList<Integer>();
			}
			
			//그래프의 간선 정보 저장
			for(int j=0; j<M; j++) {
				tk = new StringTokenizer(br.readLine());
				int u = Integer.parseInt(tk.nextToken());
				int v = Integer.parseInt(tk.nextToken());
				
				graph[u].add(v);
				graph[v].add(u);
			}
			
			//방문 체크 배열
			c = new boolean[N+1];
			
			if(bfs(1)) {
				sb.append("YES\n");
			}else {
				sb.append("NO\n");
			}
			
		}
		
		System.out.print(sb);	
		
	}
	
	static int N, M;
	static ArrayList<Integer>[] graph;
	static boolean[] c;
	
	static boolean bfs(int start) {
		//정점 저장 큐
		Queue<Integer> q = new LinkedList<Integer>();
		
		//정점들의 그룹을 표시할 배열
		int[] group = new int[N+1];
		
		//시작점 방문 처리
		q.add(start); c[start] = true;
		group[start] = 1;

		boolean clear = false; //모든 정점 방문 했는지 판별하기 위한 변수
		/*그래프의 요소가 1개가 아닐 수 있으므로 완전 탐색 후에 모든 정점 방문했는지 확인해야 한다.*/
		
		while(!clear) { //연결요소가 2개 이상일 경우 모든 정점을 방문할 때 까지
			while(!q.isEmpty()) {
				int x = q.remove();
				
				//x에서 갈 수 있는 모든 정점 탐색하여 방문하지 않았으면 방문
				for(int i=0; i<graph[x].size(); i++) {
					int y = graph[x].get(i);
					
					if(!c[y]) { //아직 방문하지 않았으면
						q.add(y); c[y] = true; //방문하기
						
						// 연결된 정점끼리는 다른 그룹이어야 한다
						if(group[x]==1) {
							group[y] = 2;
						}else {
							group[y] = 1;
						}
					}else { //이미 방문한 점이면 그룹 비교
						if(group[x] == group[y])
							return false; //연결된 정점이 같은 그룹에 속하므로 이분 그래프가 아니다
					}
				}
			}
			
			clear = true; //모든 정점 방문했다고 가정
			for(int i=1; i<=N; i++) {
				if(!c[i]) { //아직 방문하지 않은 정점이 있으면
					clear = false; //clear 변수 false로
					q.add(i); c[i] = true; // 점점 i 방문
					group[i] = 1;
					break; //for문 탈출 후 새로운 그래프 요소 탐색 시작
				}
			}
		}
		
		
		//모순이 생기지 않고 완전탐색이 끝났으면 이분 그래프이므로 true 반환
		return true;
	}
}
~~~