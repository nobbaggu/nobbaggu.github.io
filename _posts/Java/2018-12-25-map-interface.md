---
title: (Java) 46. Map 인터페이스
date: 2018-12-25T16:51:51+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - hashmap
  - Java
  - keyset
  - map
  - treemap
  - values
  - 메소드
  - 인터페이스
  - 자바
  - 트리맵
  - 해쉬맵
  - 해시맵
---
Map 인터페이스는 '자료 쌍'을 다루는 데 용이한 라이브러리이다. 흔히 key-value로 이루어진 자료를 다룬다. 이 자료구조에서 key값은 유일하고, value값은 중복이 허용된다.

자료 쌍의 예로는 학번-이름, 고객번호-이름 등이 있을 수 있다.

Map인터페이스를 구현한 HashMap클래스와 TreeMap 클래스에 대해 알아본다.

&nbsp;

# 1. HashMap 클래스

* * *

HashMap 클래스를 활용하여 회원관리 프로그램을 만든다.

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
   
}
~~~

&nbsp;

~~~ java
package collection.hashmap;

import java.util.HashMap;
import java.util.Iterator;
import collection.Member;

public class MemberHashMap {

   //HashMap은 key, value 각각을 위한 제네릭 자료형 2개를 사용
   private HashMap<Integer, Member> hashMap; //key : 회원ID,  value : 회원
   
   public MemberHashMap() { //객체를 만드는 동시에 hashMap 저장소도 같이 생성
      hashMap = new HashMap<Integer, Member>();
   }
   
   // 회원 추가 - put(key, value) 메소드 사용
   public void addMember(Member member) {
      hashMap.put(member.getMemberId(), member);
   }
   
   // 회원 삭제 - 회원탐색할 때 containsKey(key)메소드 사용
   public boolean removeMember(int memberId) {
      if(hashMap.containsKey(memberId)) {
         hashMap.remove(memberId);
         return true;
      }
      
      System.out.println(memberId + " does not exist");
      return false;
   }
   
   //모든 멤버 출력
   public void showAllMember() {
      Iterator<Integer> ir = hashMap.keySet().iterator(); //keySet() 메소드는 해쉬맵의 모든 key값들을 Set 객체로 반환
      
      while(ir.hasNext()) {
         int memberId = ir.next();
         
         Member member = hashMap.get(memberId);
         System.out.println(member);
      }
      System.out.println();
   }
}
~~~

HashMap을 사용하여 자료를 관리하는 프로그램을 만들 때 key값으로는 중복이 안되는 자료를 해주어야 한다.

HashMap의 keySet() 메소드는 모든 key값들을 Set 자료로 반환해주어 중복이 안된다. 반면 values()메소드는 모든 value값들을 list 자료형으로 반환해주며 list 자료형은 중복을 허용한다.

&nbsp;

이제 테스트를 해볼 차례다.

~~~ java
package collection.hashmap;

import collection.Member;

public class MemberHashMapTest {

   public static void main(String[] args) {
      MemberHashMap memberHashMap = new MemberHashMap();
      
      Member member1 = new Member(1001, "Kim");
      Member member2 = new Member(1002, "Lee");
      Member member3 = new Member(1003, "Son");
      Member member4 = new Member(1004, "Choi");
      
      memberHashMap.addMember(member1);
      memberHashMap.addMember(member2);
      memberHashMap.addMember(member3);
      memberHashMap.addMember(member4);
      
      memberHashMap.showAllMember();
      
      memberHashMap.removeMember(member2.getMemberId());
      
      memberHashMap.showAllMember();
      
      
   }

}
~~~

실행결과

Kim's ID is 1001


Lee's ID is 1002


Son's ID is 1003


Choi's ID is 1004




Kim's ID is 1001


Son's ID is 1003


Choi's ID is 1004

&nbsp;

&nbsp;

# 2. TreeMap클래스

* * *

TreeMap은 TreeSet과 마찬가지로 이진 검색 트리 알고리즘이 구현되어있다. TreeMap은 key값으로 정렬을 한다. 따라서 key값에 해당하는 클래스는 Comparable이나 Comparator 인터페이스를 구현해야한다. 하지만 memberId의 자료형인 Integer 클래스는 이미 Comparable 인터페이스를 구현해놓아서 따로 여기서 할 필요는 없다.

~~~ java
package collection.treemap;

import collection.Member;
import java.util.TreeMap;
import java.util.Iterator;

public class MemberTreeMap {
   TreeMap<Integer, Member> memberTreeMap;
   
   public MemberTreeMap() {
      memberTreeMap = new TreeMap<Integer, Member>();
   }
   
   public void addMember(Member member) {
      memberTreeMap.put(member.getMemberId(), member);
   }
   
   public boolean removeMember(int memberId) {
      if(memberTreeMap.containsKey(memberId)) {
         memberTreeMap.remove(memberId);
         return true;
      }
      
      System.out.println(memberId+" does not exist");
      return false;
   }
   
   public void showAllMember() {
      Iterator<Integer> ir = memberTreeMap.keySet().iterator();
      
      while(ir.hasNext()) {
         int key = ir.next();
         System.out.println(memberTreeMap.get(key));
      }
      System.out.println();
   }
}
~~~

&nbsp;

~~~ java
package collection.treemap;

import collection.Member;

public class MemberTreeMapTest {

   public static void main(String[] args) {
      MemberTreeMap mems = new MemberTreeMap();
      
      Member member1 = new Member(1001, "Kim");
      Member member2 = new Member(1002, "Lee");
      Member member3 = new Member(1003, "Son");
      Member member4 = new Member(1004, "Choi");
      
      mems.addMember(member1);
      mems.addMember(member2);
      mems.addMember(member3);
      mems.addMember(member4);
      mems.showAllMember();
      
      mems.removeMember(member2.getMemberId());
      mems.showAllMember();
   }
}
~~~

실행결과

Son's ID is 1001


Kim's ID is 1002


Choi's ID is 1003


Lee's ID is 1004




Son's ID is 1001


Kim's ID is 1002


Choi's ID is 1003

TreeMap클래스는 Key값에 대해 이진트리를 구현한다. Key에 해당하는 클래스인 Integer는 Comparable인터페이스를 이미 구현하고 있기에 따로 무언가를 할 필요는 없었다.