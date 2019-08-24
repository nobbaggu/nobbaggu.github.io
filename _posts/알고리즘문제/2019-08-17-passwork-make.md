---
title: (백준 1759)-암호 만들기
date: 2019-08-17T11:18:34+09:00
author: SWnomad
layout: post
categories: 알고리즘문제
tags:
  - 브루트포스
  - brute force
  - 자바
  - java
  - 백준
  - 재귀
  - recursion
  - 1759
  - 암호
---

[백준 1759 - 암호 만들기](https://www.acmicpc.net/problem/1759){: target="_blank" }

## 1. 풀이 전략
* * *

총 C개의 암호 후보 알파벳들이 있다. 이 후보 알파벳들의 처음부터 마지막까지 하나하나 짚어가면서 1)암호에 들어가는 경우, 2)암호에 들어가지 않는 경우로 나누어 모든 경우를 따져볼 수 있다. 즉, 암호에 추가 혹은 추가하지 않음이라는 두 가지 다른 경로가 있고, 이를 각각 실행하면서 정답을 찾는 것이다. 이러한 논리는 재귀호출 구조에 알맞다. 암호의 갯수가 4개가 되면 모음 1개이상, 자음 2개이상을 포함하는지 체크한 이후 true면 정답을 출력 후 재귀 호출을 종료한다. 또한 마지막까지 훑었는데 아직 4개 미만으로 추가했다면 이 경우는 정답이 아니므로 그냥 재귀호출을 종료한다.

<br>

재귀호출 알고리즘은 크게 세 과정으로 분할하여 짜면 된다.

>1. 더 이상 진행해도 정답일 가능성이 없는 경우
>2. 정답 후보를 찾은 경우
>3. 아직 정답을 찾지 못하여 다음 순서를 호출하는 경우

각각의 과정으로 나누어 생각하면 비교적 쉽게 재귀호출 알고리즘을 짤 수 있다.

조금 더 자세한 내용은 아래의 링크로 이어지는 포스팅에서 볼 수 있다. 그렇게 길지 않으므로 시간적 여유가 된다면 한 번 봐두어 같은 유형의 문제에 계속해서 적용할 수 있다.

<br>
[재귀](https://swnomad.github.io/2019/08/11/recursion/){: target="_blank" }

<br>

문제를 풀기 위해 위 세 과정을 설계해보자.

>1. 더 이상 정답 가능성이 없는 경우 => 모든 알파벳을 훑었지만 아직 암호의 길이가 4 미만인 경우
>2. 정답 후보를 찾은 경우 => 암호의 길이가 4인 경우. 이 때, 모음과 자음의 갯수 기준을 충족하면 출력, 아니면 그냥 호출 종료
>3. 아직 정답을 찾지 못한 경우 => 다음 경우를 호출

<br>

이 때, 다음 경우를 호출할 때는 이번 알파벳을 추가한 경우, 추가하지 않은 경우 나누어 둘 모두 호출해주어야한다.

## 2. 소스 코드
* * *

~~~ java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main{
	
	public static void main(String[] args) throws NumberFormatException, IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		StringTokenizer tk = new StringTokenizer(br.readLine());
		int L = Integer.parseInt(tk.nextToken());
		int C = Integer.parseInt(tk.nextToken());
		
		//암호의 후보 알파벳 C개 입력받아 배열에 저장
		tk = new StringTokenizer(br.readLine());
		char[] chs = new char[C];
		for(int i=0; i<C; i++) {
			chs[i] = tk.nextToken().charAt(0);
		}
		
		//오름차순으로 정렬
		Arrays.sort(chs);
		
		recursion(L, chs, 0, new String(""));
		
	}	
	
	static void recursion(int L, char[] chs, int idx, String pwd) {
		
		//정답의 가능성이 없는 경우
		//chs의 마지막 index를 벗어나는 경우 재귀호출 종료
		if(idx>chs.length-1 && pwd.length()<L)
			return;
		
		//정답 후보를 찾은 경우
		if(pwd.length()==L) {
			if(check(pwd))
				System.out.println(pwd);			
			
			return;
		}
				
		//아직 정답을 찾지 못한 경우 다음 함수 호출
		//1) 이번 idx의 알파벳 집어넣기
		recursion(L, chs, idx+1, pwd+chs[idx]);
				
		//2) 이번 idx의 알파벳 집어넣지 않기
		recursion(L, chs, idx+1, pwd);
	}
	
	//모음 1개 이상, 자음 2개 이상인지 체크
	static boolean check(String str) {
		int mo = 0;
		int ja = 0;
		
		for(int i=0; i<str.length(); i++) {
			char ch = str.charAt(i);
			if(ch=='a' || ch=='e' || ch=='i' || ch=='o' || ch=='u') {
				mo++;
			}else {
				ja++;
			}
		}
		
		return mo>=1 && ja>=2;
	}

}
~~~

여기서 주의할 점은 다음 함수 호출 순서이다. 사전순으로 출력해주어야하므로 이번 index의 알파벳을 집어넣는 경우를 먼저 호출해주어야한다. 이유는 조금만 생각해보면 알 수 있다.