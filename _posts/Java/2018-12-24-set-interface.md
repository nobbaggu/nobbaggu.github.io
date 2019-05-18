---
title: (Java) Set 인터페이스
date: 2018-12-24T21:03:32+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - add
  - class
  - comparable
  - equal
  - hashCode
  - hashset
  - interate()
  - interface
  - iterator
  - Java
  - remove
  - treeset
  - 인터페이스
  - 자바
  - 클래스
---
Set 인터페이스는 Collection 인터페이스의 하위 인터페이스이다. 중고등학교 수학시간에 배운 집합을 생각하면 된다. 어떠한 요소가 있는지가 중요하고 순서는 중요하지 않다. 그리고 중복을 허용하지 않는다.

오늘은 Set 인터페이스를 구현한 클래스인 HashSet과 TreeSet 클래스에 대해 알아본다.

&nbsp;

&nbsp;

# 1. HashSet

* * *

~~~ java
package collection;

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

   // 멤버ID가 같은 경우 같은 객체로 인식시키기 위해 hashCode() 재정의
   @Override
   public int hashCode() {
      // TODO Auto-generated method stub
      return this.memberId;
   }

   // 멤버 ID가 같은 경우 논리적으로 같은 객체라고 인식시키기 위한 equals() 재정의
   @Override
   public boolean equals(Object obj) {
      if(obj instanceof Member) {
         Member member = (Member)obj;
         if(this.memberId == member.memberId) {
            return true;
         }
         else {
            return false;
         }
      }
      
      return false;
   }

}
~~~

&nbsp;

~~~ java
package collection.hashset;
import collection.Member;
import java.util.HashSet;
import java.util.Iterator;

public class MemberHashSet {
   private HashSet<Member> hashSet;
   
   //MemberHashSet 객체가 생성되는 동시에 HashSet 저장소 생성
   MemberHashSet(){
      hashSet = new HashSet<>();
   }
   
   //멤버 추가
   public void addMember(Member member) {
      hashSet.add(member);
   }
   
   //멤버 삭제
   public boolean removeMember(int memberId) {
      Iterator<Member> ir = hashSet.iterator(); //hashSet의 요소를 순회
      
      while(ir.hasNext()) { //hasNext()는 다음 요소가 있으면 true, 없으면 false 반환
         Member member = ir.next(); //다음 요소 있으면 다음 용소를 member에 대입
         int tempId = member.getMemberId();
         if(tempId == memberId) {
            hashSet.remove(member);
            return true;
         }
      }
      
      System.out.println(memberId+" does not exists");
      return false;
   }
   
   //모든 멤버 출력
   public void showAllMember() {
      for(Member member : hashSet) {
         System.out.println(member);
      }
      System.out.println();
   }
}
~~~

ArrayList, LinkedList 클래스와 메소드 사용법이 거의 일치한다. 다만 집합은 index가 없어서 Iterator 클래스를 활용해서 HashSet의 요소를 하나씩 순회탐색하여 비교한 이후 삭제하는 removeMember()메소드를 정의하였다.

&nbsp;

~~~ java
package collection.hashset;
import collection.Member;

public class MemberHashSetTest {
   public static void main(String[] args) {
      
      MemberHashSet memberHashSet = new MemberHashSet();
      
      Member mem1 = new Member(1001, "Lim");
      Member mem2 = new Member(2222, "KongKong");
      Member mem3 = new Member(1003, "Choi");
      Member mem4 = new Member(1004, "Kang");
      Member mem5 = new Member(1004, "Jung"); //mem3와 같은 ID
      
      memberHashSet.addMember(mem1);
      memberHashSet.addMember(mem2);
      memberHashSet.addMember(mem3);
      memberHashSet.addMember(mem4);
      memberHashSet.addMember(mem5);
      
      memberHashSet.showAllMember();
   }
}
~~~

&nbsp;

실행결과

Lim's ID is 1001  
Choi's ID is 1003  
Kang's ID is 1004  
KongKong's ID is 2222

Member 클래스에서 hashCode()메소드와 equals()메소드를 재정의 하여 ID가 같으면 같은 객체로 인식하도록 하였다. 따라서 mem5는 mem4와 같은 ID인 1004를 가지므로 추가되지 않았고 이는 Set인터페이스를 구현한 HashSet은 중복객체를 허용하지 않는다는 것을 보여준다.

&nbsp;

&nbsp;

# 2. TreeSet

* * *

~~~ java
package collection;

public class Member implements Comparable<Member> {
   
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

   @Override
   public int hashCode() {
      // TODO Auto-generated method stub
      return this.memberId;
   }

   @Override
   public boolean equals(Object obj) {
      if(obj instanceof Member) {
         Member member = (Member)obj;
         if(this.memberId == member.memberId) {
            return true;
         }
         else {
            return false;
         }
      }
      
      return false;
   }

   //compareTo 메소드 재정의 하여 정렬방식 선택
   @Override
   public int compareTo(Member member) {
      
      return this.memberId-member.memberId; // 0보다 크므로 오름차순 정렬
   }
   
}
~~~

&nbsp;

~~~ java
package collection.treeset;
import collection.Member;
import java.util.TreeSet;
import java.util.Iterator;

public class MemberTreeSet {
   TreeSet<Member> treeSet;
   
   MemberTreeSet(){
      treeSet = new TreeSet<>();
   }
   
   public void addMember(Member member) {
      treeSet.add(member);
   }
   
   public boolean removeMember(int memberId) {
      Iterator<Member> ir = treeSet.iterator();
      
      while(ir.hasNext()) {
         Member member = ir.next();
         int tempId = member.getMemberId();
         if(tempId == memberId) {
            treeSet.remove(member);
            return true;
         }
      }
      
      System.out.println(memberId+" does not exists");
      return false;
   }
   
   public void showAllMember() {
      for(Member member : treeSet) {
         System.out.println(member);
      }
      System.out.println();
   }
   
}
~~~

TreeSet은 HashSet과 사용법은 같다. 그러나 주의해야 할 것은 TreeSet을 사용할 때에는 요소로 추가되는 참조 자료형 클래스가 Comparable 인터페이스를 구현해야 한다는 것이다. Comparable 인터페이스를 구현하면 compareTo() 메소드를 재정의 해야한다. 이 메소드는 TreeSet이 '오름차순' 혹은 '내림차순' 중 어떤 규칙으로 자료를 정리할 것인지 정해주는 것이며, 0보다 크면 오름차순, 작으면 내림차순, 0이면 자료를 추가하지 않는다.

위 코드에서 this.memberId는 새로 추가되는 멤버의 ID이다. 이것과 이미 TreeSet에 있는 자료의 memberId를 하나하나 탐색비교해가며 새로 추가되는 자료의 위치를 찾아 정렬시킨다. 이유는 기본적으로 TreeSet은 이진탐색 알고리즘을 사용하기 때문이다.

&nbsp;