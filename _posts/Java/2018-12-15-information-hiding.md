---
title: 정보 은닉
date: 2018-12-15T12:18:42+09:00
author: SWnomad
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - getter
  - Java
  - private
  - protected
  - public
  - setter
  - 게터
  - 세터
  - 자바
  - 접근 제어자
  - 정보은닉
---
클래스, 메소드, 멤버변수 등을 정의할 때 public 이외에도 다른 접근 제어자들이 있다. 접근 제어자는 이 객체에 접근할 수 있는 권한을 설정하는 명령어이다.

&nbsp;

# 1. 정보 은닉

* * *

~~~ java
package classpart;

public class Student {
   private int stuID; // private 접근제어자
   private String stuName; // private 접근제어자
   int age;
   String addr;
}
~~~

stuID와 stuName 멤버변수는 외부 클래스에서 접근을 할 수 없게 되었다.

~~~ java
package classpart;

public class Student {
    private int stuID; // private 접근제어자
    private String stuName; // private 접근제어자
    int age;
    String addr;

    public void setName(String name) {
        stuName = name;
    }

    public String getName() {
        return stuName;
    }

    public int getStuID(){
        return stuID;
    }
}
~~~

외부 클래스에서 직접 stuName과 stuID에 접근할 수 없게 된 대신에 setName() 메소드와 getName()메소드를 제공하여 이름을 설정하고 읽을 수 있게 해두었다. 하지만 stuID는 학번이기 때문에 바뀔 수 없어 읽기밖에 제공하지 않는다.

위 코드에는 추가하지 않았지만 애초에 생성자에 매개변수로 stuID를 넣어두고 인스턴스를 만들 때 정해지도록 하면 된다.

&nbsp;

# 2. getter와 setter

* * *

일반적으로 멤버 변수는 get()메소드와 set()메소드를 통해서 접근할 수 있도록 하는 것이 일반적이다. 이 메소드들을 getter, setter라고 부른다.

이클립스 IDE는 멤버 변수들에 대해 자동으로 getter와 setter를 만들어주는 기능도 제공한다.

외부 클래스에서 멤버 변수에 접근을 허용하는 것. 접근을 제한하고 setter를 제공하는 것. 둘에는 차이가 있다.

~~~ java
package classpart;

public class Student {
    String stuName;
    private char gender;

    public String getStuName() {
        return stuName;
    }
    public void setStuName(String stuName) {
        this.stuName = stuName;
    }
    public char getGender()
    {
        return gender;
    }
    public void setGender(char gender) {
        if ((gender == 'M') || (gender == 'F')){
            this.gender = gender;
        }
        else {
            System.out.println("invalid gender!!"); // 에러 구문 출력
            return; // setGender() 함수 종료
        }
    }
}
~~~

위 코드를 보면 성별을 설정하는 setGender 메소드를 제공할 때 매개변수를 M(Male) 혹은 F(Female)로 제한을 걸어둔다. 둘 중 하나가 아니면 오류 구문을 출력하고 함수를 종료하도록 한다. 하지만 성별을 public 접근 제어자를 사용해서 선언했으면 어떤 것이든 char 자료형에 맞는 자료는 저장될 수 있었을 것이다.

&nbsp;

#접근 제어자 접근 허용범위#

public : 모든 외부 클래스

protected : 같은 패키지 내부 클래스 혹은 상속 클래스

아무것도 없는 경우(default) : 같은 패키지 내부 클래스

private : 같은 클래스 내부

&nbsp;