---
title: 큐(Queue)와 스택(Stack)
date: 2019-09-03T21:28:34+09:00
author: SWnomad
layout: post
categories: 자료구조
tags:
  - 자료구조
  - 스택
  - 큐
  - stack
  - queue
  - java
  - 자바
  - FIFO
  - LIFO
  - 선입선출
  - 후입선출
---

## 1. 스택(Stack)
***

스택은 사전적으로 '쌓아올린 것'이란 의미를 가지고있다. 스택 자료구조는 데이터를 쌓아올린 형태로 저장한다. 따라서 데이터를 다시 꺼낼 때 가장 마지막에 넣은 데이터부터 꺼낸다. 이를 후입선출, 영어로 LIFO(Last In First Out)이라고 한다.

<br>

![스택](/images/datastructure/stack.jpg){: width="30%" height="30%"}

<br>
가장 최근에 입력된 데이터를 top, 가장 처음에 입력된 데이터를 bottom이라고 한다.

<br>
스택을 구현하기 위해서 배열 혹은 연결리스트(LinkedList)를 사용한다.

<br>

***1) 배열을 사용한 스택 구현***

~~~java
class Stack{
	private int top; //가장 마지막에 추가한 데이터의 index
	private int size; //스택의 크기
	private Object[] stackArray; //스택을 구현하기 위한 배열
	
	public Stack() {
		this.size = 100; //default 크기 100
		this.stackArray = new Object[size]; //스택을 위한 배열 생성
		this.top = -1; //자료가 하나도 없는 상태
	}
	
	public Stack(int size) {
		this.size = size; // 사용자가 입력한 스택의 크기
		this.stackArray = new Object[size]; //크기가 size인 배열 생성
		this.top = -1; //자료가 하나도 없는 상태
	}
	
	/*스택의 중요한 메소드 구현*/	
	//1. 스택이 비어있는지 확인하는 메소드
	public boolean isEmpty() {
		return (top==-1); // top이 -1이면 스택은 비어있다
	}
	
	//2. 스택 자료의 갯수 반환
	public int size() {
		return (top+1);
	}
	
	//3. top 위치의 데이터 return하는 메소드
	public Object peek() {
		if(isEmpty()) { //비어있으면 ArrayIndexOutOfBoundsException 예외 발생
			throw new ArrayIndexOutOfBoundsException(top);
		}
		
		return stackArray[top];
	}
	
	//4. 스택에 자료 저장
	public void push(Object data) {
		if(top==size-1) { // 스태이 가득 찼으면
			Object[] tmp = stackArray; //스택의 자료를 임시로 저장할 배열
			size += 100; // 스택 크기 100 증가
			stackArray = new Object[size]; // 스택을 위한 배열 새로 생성
			for(int i=0; i<tmp.length; i++) { //데이터 복사
				stackArray[i] = tmp[i];
			}
		}
		
		stackArray[++top] = data;
	}
	
	//5. 스택에서 자료 반환하면서 제거
	public Object pop() {
		if(isEmpty()) { //비어있으면 ArrayIndexOutOfBoundsException 예외 발생
			throw new ArrayIndexOutOfBoundsException(top);
		}
		
		return stackArray[top--];
	}
	
}
~~~

<br>

***2) 연결리스트를 사용한 스택 구현***

~~~ java
class Stack{
	
	private Node top; //최상위 노드
	private int size; //스택의 현재 크기
	
	private class Node{
		private Object data; // 노드가 가진 데이터
		private Node next; // 다음 노드
		
		public Node(Object data) {
			this.data = data;
			this.next = null;
		}
	}
	
	public Stack() {
		this.top = null;
		this.size = 0;
	}
	
	//1. 스택이 비어있는지 확인하는 메소드
	public boolean isEmpty() {
		return (top==null);
	}
	
	//2. 스택의 크기 반환
	public int size() {
		return size;
	}
		
	//3. top 위치의 데이터 return하는 메소드
	public Object peek() {
		if(isEmpty()) {
			throw new ArrayIndexOutOfBoundsException();
		}
		
		return top.data;
	}
	
	//4. 스택에 자료 저장
	public void push(Object data) {
		Node newNode = new Node(data);
		newNode.next = top;
		top = newNode; //최상위 노드 갱신
		size++;
	}
	
	//5. 스택에서 자료 반환하면서 제거
	public Object pop() {
		if(isEmpty()) {
			throw new ArrayIndexOutOfBoundsException();
		}
		
		Object item = peek();
		top = top.next;
		
		return item;
	}
}
~~~

<br>

## 2. 큐(Queue)
***

큐 자료구조는 데이터가 순서대로 들어가고 나온다. 따라서 데이터를 다시 꺼낼 때 가장 처음에 넣은 데이터부터 나온다. 이를 선입선출, 영어로 FIFO(First In First Out)이라고 한다.

<br>

![큐](/images/datastructure/queue.jpg){: width="30%" height="30%"}

<br>
***1) 배열을 사용한 큐 구현***

~~~ java
class Queue{
	private int front; //가장 앞 데이터
	private int rear; //가장 뒤 데이터
	private int size; //큐의 크기
	private Object[] queueArray; //큐를 구현하기 위한 배열
	
	public Queue() {
		this.front = 0;
		this.rear = -1;
		this.size = 100; //default 크기 100
		this.queueArray = new Object[size]; //스택을 위한 배열 생성
	}
	
	/*큐의 중요한 메소드 구현*/
	//1. 큐가 비어있는지 확인
	public boolean isEmpty() {
		return (front==rear+1);
	}
	
	//2. 큐 자료의 갯수 반환
	public int size() {
		return (rear-front+1);
	}
	
	//3. front 데이터 return
	public Object peek() {
		if(front==rear+1) {
			throw new ArrayIndexOutOfBoundsException();
		}
		
		return queueArray[front];
	}
	
	//4. 큐에 자료 저장
	public void add(Object data) {
		if(front+rear==size-1) { //큐가 가득 찼으면
			Object[] tmp = queueArray; //큐의 자료를 임시로 저장할 배열
			size += 100; //큐 크기 증가
			queueArray = new Object[size]; //큐를 위한 배열 새로 생성
			for(int i=0; i<tmp.length; i++) { //데이터 복사
				queueArray[i] = tmp[i];
			}
		}
		
		queueArray[++rear] = data;
	}
	
	//5. 큐에서 자료 반환하면서 제거
	public Object remove() {
		if(front==rear+1) {
			throw new ArrayIndexOutOfBoundsException();
		}
		
		return queueArray[front++];
	}
}
~~~

<br>

***2) 연결리스트를 사용한 큐 구현***

~~~ java
class Queue{
	
	private class Node{
		private Object data; //노드에 저장된 데이터
		private Node next; //다음 노드
		
		public Node(Object data) {
			this.data = data;
			this.next = null;
		}
	}
	
	private Node front; //가장 앞에 추가된 노드
	private Node rear; //가장 마지막에 추가된 노드
	private int size; //큐의 현재 크기
	
	public Queue() {
		this.front = null;
		this.rear = null;
		this.size = 0;
	}
	
	/*큐의 중요한 메소드 구현*/
	//1. 큐가 비어있는지 확인
	public boolean isEmpty() {
		return (front==null);
	}
	
	//2. 큐 자료의 갯수 반환
	public int size() {
		return size;
	}
	
	//3. front 데이터 return
	public Object peek() {
		if(isEmpty()) {
			throw new ArrayIndexOutOfBoundsException();
		}
		
		return front.data;
	}
	
	//4. 큐에 자료 저장
	public void add(Object data) {
		Node newNode = new Node(data);

		if(isEmpty()) {
			this.front = newNode;
			this.rear = newNode;
		}else {
			rear.next = newNode;
			rear = newNode;
		}
	}
	
	//5. 큐에서 자료 반환하면서 제거
	public Object remove() {
		if(isEmpty()) {
			throw new ArrayIndexOutOfBoundsException();
		}
		
		Object item = front.data;
		front = front.next;
		
		if(front==null) 
			rear = null;
		
		return item;
	}
}
~~~