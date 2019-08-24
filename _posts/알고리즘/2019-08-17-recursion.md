---
title: 재귀(Recursion)
date: 2019-08-17T13:28:34+09:00
author: SWnomad
layout: post
categories: 알고리즘
tags:
  - recursion
  - 재귀
  - 함수
---

## 1. 재귀함수
* * *

재귀 호출이란 어떠한 함수가 자기 자신을 다시 호출하는 것을 말한다. 함수 내부에 자신을 호출하는 코드가 있고, 이러한 호출이 끊임없이 반복된다. 따라서, 함수 내부에 재귀 호출이 중단되도록 하는 *종료조건*을 반드시 포함해주어야한다. 종료조건이 없으면 함수가 끝이 나지 않는다. 일반적인 경우라면 메모리 오버플로가 발생하고 프로그램의 비정상 종료를 일으킨다.

<br>

알고리즘 문제를 풀 때, 특히 모든 경우를 따져보는 브루트 포스 문제들은 재귀함수로 해결할 수 있는 경우가 많다.

재귀 함수는 일반적으로 특정 패턴으로 작성한다. 재귀함수를 이루는 3가지 기본 요소는 아래와 같다.

>1. 더 이상 정답 가능성이 없는 경우 -> 재귀 호출 중단
>2. 정답 후보를 찾은 경우 -> 출력 후 재귀 호출 중단
>3. 아직 정답을 찾지 못한 경우 -> 다음 경우 호출

<br>

어떠한 사람이 숫자 N을 입력했을 때, 이 숫자가 3의 배수인지 찾는 경우를 예로 들어보자.

~~~ java
static void recursion(int N, int sum){		
	//더 이상 정답 가능성이 없을 경우
	if(sum>N) {
		System.out.println("no");
		return;
	}

	//정답 후보를 찾은 경우
	if(sum==N) {
		System.out.println("yes");
		return;
	}

	//아직 정답을 찾지 못한 경우
	recursion(N, sum+3);
}
~~~

이미 정답이 될 가능성이 없는 경우라면 함수를 중단하고, 정답을 찾으면 정답을 출력해주고 재귀호출을 중단한다. 재귀호출을 이루는 알고리즘의 핵심적인 부분은 아직 정답을 찾지 못한 경우의 코드이다. 이 알고리즘이 제대로 정답을 찾아가는 과정이 되도록 코드를 작성해야 한다.

또한 재귀함수에서는 서로 다른 방법을 구분할 수 있게 해주는 것들을 매개변수로 적절히 넣어주는 것이 중요하다. 무슨 말인지는 예제를 하나 더 보면서 생각해보자.

1,2,3 세 숫자만 사용해서 어떠한 숫자 n을 만드는 경우의 수를 출력해보는 문제이다. 예를들어 n이 4라고 하면

1+1+1+1
1+1+2
1+2+1
2+1+1
2+2
1+3
3+1

총 7가지 경우의 수가 존재한다.

위 문제를 일반적인 코드로 표현해보자.

<br>

~~~ java
public class Main{
	
	public static void main(String[] args) throws NumberFormatException, IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(br.readLine());
			
		cnt = 0;
		go(n, cnt);
		System.out.println(cnt);
	}
	
	static int cnt;
	
	static void go(int n, int sum) {
		if(sum==n){
			cnt++;
			return;
		}
		
		if(sum>10)
			return;
		
		go(n, sum+1);
		go(n, sum+2);
		go(n, sum+3);
	}
}
~~~

위 코드에서는 1,2,3을 더하는 경우 각각을 위해 세 번 함수를 호출한다.

1,2,3을 더하는 각각의 방법을 구분되도록 sum이 들어가는 매개변수에 1,2,3을 각각 더해주었다.