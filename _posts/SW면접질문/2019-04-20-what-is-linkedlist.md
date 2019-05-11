---
title: 6. 연결 리스트가 무엇인가?
date: 2019-04-20T18:00:00+09:00
author: SWnomad
layout: post
categories: SW면접질문
tags:
  - 연결 리스트
  - 연결
  - 리스트
  - LinkedList
---

연결 리스트는 배열처럼 여러개의 자료를 저장하는 자료구조이다. 각각의 노드(node)는 자료를 저장하며, 포인터를 하나 가지고 있는데, 이 포인터는 다음 순서 노드의 주소값을 가지고있다. 따라서 각각의 노드는 연결되어있는것과 같다. 주의할 점은, 각 노드들의 실제 물리적 메모리 주소가 연결되어있는 것은 아니다.

<img src = "/images/sw_interview/linkedlist.png">

&nbsp;
~~~ c
#include <stdio.h>
#include <stdlib.h>    // malloc, free 함수가 선언된 헤더 파일

struct NODE {             // 연결 리스트의 노드 구조체
    struct NODE *next;    // 다음 노드의 주소를 저장할 포인터
    int data;             // 데이터를 저장할 멤버
};

int main()
{
    struct NODE *head = malloc(sizeof(struct NODE));    // 머리 노드 생성
                                                        // 머리 노드는 데이터를 저장하지 않음

    struct NODE *node1 = malloc(sizeof(struct NODE));   // 첫 번째 노드 생성
    head->next = node1;                                 // 머리 노드 다음은 첫 번째 노드
    node1->data = 10;                                   // 첫 번째 노드에 10 저장

    struct NODE *node2 = malloc(sizeof(struct NODE));   // 두 번째 노드 생성
    node1->next = node2;                                // 첫 번째 노드 다음은 두 번째 노드
    node2->data = 20;                                   // 두 번째 노드에 20 저장

    node2->next = NULL;                                 // 두 번째 노드 다음은 노드가 없음(NULL)

    struct NODE *curr = head->next;    // 연결 리스트 순회용 포인터에 첫 번째 노드의 주소 저장
    while (curr != NULL)               // 포인터가 NULL이 아닐 때 계속 반복
    {
        printf("%d\n", curr->data);    // 현재 노드의 데이터 출력
        curr = curr->next;             // 포인터에 다음 노드의 주소 저장
    }

    free(node2);    // 노드 메모리 해제
    free(node1);    // 노드 메모리 해제
    free(head);     // 머리 노드 메모리 해제

    return 0;
}
~~~

실행결과

10

20

&nbsp;
여기서 중요한 특징은 **노드는 다음 노드의 주소값을 가리키는 포인터를 가지고 있다는 것이다.** 또한 연결리스트의 머리는 아무런 데이터도 저장하지 않으며, 마지막 노드의 포인터는 아무곳도 가르키지 않는다.

&nbsp;
### 연결리스트 vs 배열

  **1. 자료 참조 속도**
  
  배열은 index만 가지고 포인터 연산만 하면 바로 해당 index의 자료를 참조할 수 있어 매우 빠르다. 그러나 연결리스트는 첫 번째 노드부터 점차 탐색을 해야하므로 일반적인 배열보다 자료를 참조하는 속도가 느리다.
  
  **2. 데이터 삽입 및 삭제**
  
  배열의 경우 마지막이 아닌 처음이나 중간지점에 데이터를 삽입 및 삭제하려면 뒤에오는 메모리 공간을 한 칸씩 뒤로 미루거나 당겨야 한다. 그러나 연결 리스트는 노드가 가지는 포인터의 주소값만 바꾸어주면 그만이므로 배열보다 데이터 삽입 및 삭제 속도가 빠르다.
