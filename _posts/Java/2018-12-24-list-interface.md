---
title: (Java) 43 - List 인터페이스
date: 2018-12-24T18:13:01+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - add
  - ArrayList
  - interface
  - Java
  - LinkedList
  - list
  - remove
  - 리스트
  - 인터페이스
  - 자바
---
List 인터페이스는 Collection 인터페이스의 하위 인터페이스이다. 순차적인 자료를 관리할 때 사용하고 중복을 허용한다. List 인터페이스를 구현한 활용도 높은 클래스 몇 가지에 대해 알아본다.

&nbsp;

# 1. ArrayList

* * *

~~~ java
public class Member {
   
   private int memberId;
   private String memberName;
   
   public Member(int memberId, String memberName) {
      this.memberId = memberId;
      this.memberName = memberName;
   }

   public int getMemberId() {
      return memberId;
   }

   public void setMemberId(int memberId) {
      this.memberId = memberId;
   }

   public String getMemberName() {
      return memberName;
   }

   public void setMemberName(String memberName) {
      this.memberName = memberName;
   }
   
   @Override
   public String toString() {
      return this.memberName + "'s ID is " + this.memberId;
   }
}
~~~

먼저 Member 클래스를 정의하였다. 멤버변수는 Id와 이름 두 개 뿐이고 각각에 대한 getter와 setter 메소드를 지원한다.

&nbsp;

이제 위에서 정의한 Member클래스로 회원을 생성하고 관리하기 위해서 ArrayList를 활용할 것이다.

&nbsp;

~~~ java
package collection.arraylist;
import java.util.ArrayList; //ArrayList 클래스 활용을 위한 import문
import collection.Member; //Member 클래스를 다루기 위한 import문

public class MemberArrayList {
   private ArrayList<Member> arrayList; //ArrayList로 Member 배열 생성
   
   //MemberArrayList 객체 생성과 동시에 arrayList 배열 생성
   public MemberArrayList() {
      arrayList = new ArrayList<>();
   }
   
   //멤버 추가 메소드
   public void addMember(Member member) {
      arrayList.add(member);
   }
   
   
   //멤버ID로 멤버 삭제 메소드
   public boolean removeMember(int memberId) {
      for(int i=0; i < arrayList.size(); i++) {
         Member member = arrayList.get(i);
         int tempId = member.getMemberId();
         if(tempId == memberId) {
            arrayList.remove(i);
            return true;
         }
      }
      
      System.out.println(memberId + " does not exist in list");
      return false;
   }
   
   //모든 멤버 정보 출력 메소드
   public void showAllMember() {
      for(Member member : arrayList) {
         System.out.println(member);
      }
      System.out.println();
   }
}
~~~

arrayList는 명단을 저장하기 위한 ArrayList클래스의 객체 배열이다. 외부 프로그램에서 직접적인 접근을 막기위해 private로 선언하였다. 그러나 addMember(), removeMember(), showAllMember() 메소드로 회원을 추가/삭제 및 열람 기능을 제공하도록 한다.

이제 위에서 ArrayList로 구현한 회원관리 프로그램이 잘 작동하는지 테스트 해보아야한다.

~~~ java
package collection.arraylist;
import collection.Member;

public class MemberArrayListTest {

   public static void main(String[] args) {
      MemberArrayList memberArrayList = new MemberArrayList();
      
      
      Member mem1 = new Member(1001, "Kelly");
      Member mem2 = new Member(1002, "John");
      Member mem3 = new Member(1003, "Sam");
      
      //멤버 3명 추가
      memberArrayList.addMember(mem1);
      memberArrayList.addMember(mem2);
      memberArrayList.addMember(mem3);
      
      memberArrayList.showAllMember();
      
      //멤버 1명 삭제
      memberArrayList.removeMember(mem2.getMemberId());
      
      memberArrayList.showAllMember();
      
   }

}
~~~

멤버를 추가 및 삭제하고 열람하는 메소드를 호출해보았다.

실행결과

Kelly's ID is 1001


John's ID is 1002


Sam's ID is 1003




Kelly's ID is 1001


Sam's ID is 1003

테스트 결과가 예상, 의도한 대로 나온걸로 보아 실제 회원관리 프로그램 클래스가 적절하게 작성되었음을 알 수 있다.

&nbsp;

&nbsp;

# 2. LinkedList

* * *

LinkedList 클래스는 하나의 요소에 '**자료 + 다음 요소의 주소**' 두 가지를 동시에 가진다. 따라서 메모리공간의 물리적인 자료 주소가 연결되어있지 않더라도 논리적으로 앞뒤 순서가 있을 수 있다.

ArrayList는 배열의 자료를 이동시켜서 빈 공간을 만들어 추가하고, 자료를 삭제하면 뒤 요소들을 한칸씩 당겨 빈 공간을 삭제한다. 그러나 LinkedList는 애초에 배열의 순서를 요소가 가진 '다음 요소의 주소'를 가지고 판단한다. 따라서 메모리 어딘가에 저장되어있는 요소를 추가할 때는 앞 index위치 요소의 '다음 요소의 주소'값만 바꿔주면 된다. 삭제할 때도 마찬가지다.

그러나 배열은 요소의 index를 가지고 위치를 추적한다던가 하는 경우에는 ArrayList가 유리하다.

자료의 삽입/삭제 같은 구조의 변경이 많은 자료는 LinkedList로, 그렇지 않은 경우는 ArrayList로 관리하는게 효율적이다.

이번에는 같은 Member 클래스를 LinkedList 클래스를 활용해서 관리하는 프로그램을 작성해본다. 미리 말하자면 ArrayList보다 몇 개의 메소드를 더 지원할 뿐 거의 ArrayList활용방법과 같다.

~~~ java
package collection.linkedlist;
import collection.Member;
import java.util.LinkedList;

public class MemberLinkedList {
   private LinkedList<Member> linkedList;
   
   MemberLinkedList(){
      linkedList = new LinkedList<>();
   }
   
   //멤버 추가
   public void addMember(Member member) {
      linkedList.add(member);
   }
   
   //제일 앞에 멤버 추가
   public void addMemberToFirst(Member member) {
      linkedList.addFirst(member);
   }
   
   //멤버 ID로 멤버 삭제
   public boolean removeMember(int memberId) {
      for(int i=0; i < linkedList.size(); i++) {
         Member member = linkedList.get(i);
         int tempId = member.getMemberId();
         if(tempId == memberId) {
            linkedList.remove(i);
            return true;
         }
      }
      
      System.out.println(memberId + " does not exist");
      return false;
   }
   
   //가장 마지막 멤버 삭제
   public void removeLastMember() {
      linkedList.removeLast();
   }
   
   //모든 멤버 정보 출력
   public void showAllMember() {
      for(Member member : linkedList) {
         System.out.println(member);
      }
   }
}
~~~

&nbsp;

~~~ java
package collection.linkedlist;

import collection.Member;
import collection.arraylist.MemberArrayList;

public class MemberLinkedListTest {

   public static void main(String[] args) {
      
      MemberLinkedList memberLinkedList = new MemberLinkedList();
      
      Member mem1 = new Member(1001, "Kelly");
      Member mem2 = new Member(1002, "John");
      Member mem3 = new Member(1003, "Sam");
      
      memberLinkedList.addMember(mem1);
      memberLinkedList.addMember(mem2);
      memberLinkedList.addMember(mem3);  
      memberLinkedList.showAllMember();
      
      memberLinkedList.removeLastMember();
      memberLinkedList.showAllMember();
      
      memberLinkedList.addMemberToFirst(new Member(1004, "NewMan"));
      memberLinkedList.showAllMember();
      
      
   }

}
~~~

실행결과

Kelly's ID is 1001


John's ID is 1002


Sam's ID is 1003




Kelly's ID is 1001


John's ID is 1002




NewMan's ID is 1004


Kelly's ID is 1001


John's ID is 1002