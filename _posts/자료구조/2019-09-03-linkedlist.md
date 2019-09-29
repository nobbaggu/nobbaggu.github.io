---
title: 연결리스트(LinkedList)
date: 2019-09-03T19:28:34+09:00
author: SWnomad
layout: post
categories: 자료구조
tags:
  - 자료구조
  - 자바
  - java
  - 연결리스트
  - LinkedList
  - node
  - 노드
  - 데이터
  - 자료
---

## 1. 연결리스트란?
* * *

연결리스트는 저장된 자료들이 서로 논리적으로 연결된 자료구조이다. 연결리스트는 데이터를 담는 노드라는 자료구조를 하나씩 가진다. 각 노드는 데이터를 하나 저장할 수 있고, 다음 노드를 가르키는 포인터를 하나 가지고 있다. 노드는 class를 사용해 만든다.

<br>

![연결리스트](/images/datastructure/linkedlist.jpg){: width="30%" height="30%"}

<br>

~~~ java
private class Node{
	private Object data; //실제 저장할 데이터
	private Node next; //다음 데이터를 가르키는 포인터
	
	public Node(Object data) {
		this.data = data;
		this.next = null;
	}
}
~~~

<br>
위 자료구조를 이용하면 서로 연결된 리스트 형태의 자료구조를 만들 수 있다.

<br>

***연결리스트의 특징***

<br>

1) 배열과 비해 검색 속도가 느리다. 배열은 index를 알면 바로 해당 데이터의 위치로 찾아가는것에 비해, 연결리스트는 앞에서부터 순차적으로 노드를 탐색해야 하기 때문이다.

2) 배열에 비해 자료의 삽입 및 삭제가 빠르다. 배열은 앞이나 중간에 새로운 데이터가 추가될 경우, 뒷부분의 데이터들이 한 칸씩 뒤로 밀리거나 삭제 시 빈자리를 채우기 위해 한 칸씩 앞으로 이동해야한다. 하지만 연결리스트는 해당 위치 노드 사이의 연결관계만 끊고 새로운 노드를 사이에 집어넣어주면 되므로 배열보다 빠르다. 그러나 데이터가 많아질수록 결국 삽입할 위치의 인덱스까지 처음부터 순회해야 하는 시간이 길어져 효율이 떨어진다.

3) 크기가 동적으로 변한다. 또한 특정 자료가 삭제되어 더 이상 참조를 하지 않으면 가비지 컬렉터가 수거하기 때문에 메모리 측면에서 배열보다 효율적일 수 있다. 그러나 저장할 데이터의 크기가 작은 경우, 오히려 다음 노드를 참조하는 변수를 위한 메모리가 오히려 더 클 수가 있다.

<br>
<br>

##2. Java를 통한 연결리스트 구현
* * *

연결리스트를 자바로 구현해보자.

먼저 연결리스트 객체를 생성할 때 기본적으로 head라는 노드가 생성된다. 이 노드는 데이터를 저장할 노드가 아니고 항상 가장 처음 들어오는 데이터를 가르키도록 할 노드이다.

그러면 연결리스트가 가지는 중요한 메소드들을 구현하여 실제로 사용가능한 연결리스트를 만들어보자.

<br>

~~~ java
class LinkedList{
	
	private class Node{
		private Object data;
		private Node next;
		
		public Node(Object data) {
			this.data = data;
			this.next = null;
		}
	}
	
	private Node head;
	private int size;
	
	public LinkedList() {
		this.head = new Node(null);
		this.size = 0;
	}
	
	//1. 연결리스트가 비어있는지 확인
	public boolean isEmpty() {
		return (head.next==null);
	}
	
	//2. 저장된 자료 갯수
	public int size() {
		return size;
	}

	
	//3. **특정 노드를 얻기 위한 private 메소드**//
	private Node getNode(int index) {
		
		if(index<0 || index>=size)
			throw new ArrayIndexOutOfBoundsException();
		
		Node node = head.next;
		for(int i=0; i<index; i++) {
			node = node.next;
		}
		
		return node;
	}
	
	//4. 데이터의 index 가져오기
	public int getIndex(Object obj) {
		if(isEmpty())
			return -1;
		
		int index = 0;
		Node node = head.next;
		Object data = node.data;
		
		while(!data.equals(obj)) {
			node = node.next;
			data = node.data;
			
			if(node==null)
				return -1;
			
			index++;
		}
		
		return index;
	}
	
	//5. 데이터 검색
	public Object get(int index) {
		
		if(index<0 || index>=size)
			throw new ArrayIndexOutOfBoundsException(index);
		
		return getNode(index).data;
	}
	
	//6. 처음 데이터 검색
	public Object getFirst() {
		return get(0);
	}
	
	//7. 마지막 데이터 검색
	public Object getLast() {
		if(isEmpty()) {
			throw new ArrayIndexOutOfBoundsException();
		}
		
		return get(size-1);
		
	} 
	
	//8. 데이터 삽입
	public void add(int index, Object data) {
		Node newNode = new Node(data);
		newNode.next = getNode(index+1);
		getNode(index-1).next = newNode;
		size++;
	}
	
	//9. 제일 앞에 데이터 삽입
	public void addFirst(Object data) {
		if(isEmpty()) {
			Node newNode = new Node(data);
			head.next = newNode;
			size++;
		}else {
			Node newNode = new Node(data);
			newNode.next = getNode(0);
			head.next = newNode;
			size++;
		}
	}
	
	//10. 마지막에 데이터 삽입
	public void addLast(Object data) {
		if(isEmpty()) {
			Node newNode = new Node(data);
			head.next = newNode;
			size++;
		}else {
			Node newNode = new Node(data);
			getNode(size-1).next = newNode;
			size++;
		}
	}	
	
	//11. 데이터 삭제(index 위치의 자료)
	public Object remove(int index) {
		if(index<0 || index>=size)
			throw new ArrayIndexOutOfBoundsException(index);
		
		Object item = get(index);
		getNode(index-1).next = getNode(index+1);
		size--;
		
		return item;
	}
	
	//12. 데이터 삭제(값이 data인 자료)
	public boolean remove(Object data) {
		int index = getIndex(data);
		
		if(isEmpty() || index==-1)
			return false;
		
		remove(index);
		
		return true;
	}
	
	//13. 처음 데이터 삭제
	public Object removeFirst() {
		if(isEmpty())
			throw new ArrayIndexOutOfBoundsException();
		
		Object item = getNode(0).data;
		
		if(size>1) {
			head.next = getNode(1);
		}else {
			head.next = null;
		}
		
		size--;
		
		return item;
	}
	
	//13. 마지막 데이터 삭제
	public Object removeLast() {
		if(isEmpty())
			throw new ArrayIndexOutOfBoundsException();
		
		Object item = getLast();
		
		if(size==1) {
			head.next = null;
		}
		
		if(size>1) {
			getNode(size-2).next = null;
		}
		size--;
		
		return item;
	}
}
~~~