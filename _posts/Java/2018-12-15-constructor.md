---
title: (Java) 17 - 생성자
date: 2018-12-15T11:26:27+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - constructor
  - default
  - overload
  - 디폴트
  - 매개변수
  - 생성자
  - 오버로드
  - 컨스트럭터
---
생성자 는 클래스형의 인스턴스를 만들 때 필요한 요소이다. 생성자는 멤버 인스턴스가 만들어 질 때 멤버변수의 초기화에 관여하고 정보은닉과 관계되는 Java의 핵심 내용중 하나이다.

# 1. 생성자

* * *

~~~ java
package classpart;

public class Person {
   String name;
   int age;
}
~~~

위와 같이 정의된 클래스로 인스턴스를 만들 때 다음과 같이 했다.

~~~ java
package classpart;

public class Test {
    public static void main(String[] args) {
       Person per = new Person();
    }
}
~~~

저기서 Person()을 생성자(Constructor)라고 한다.

Person 클래스를 정의한 코드는 컴파일되면서 자동으로 다음과 같이 된다.

~~~ java
package classpart;

public class Person {
   String name;
   int age;

   public Person() {} //default 생성자
}
~~~

클래스를 정의할 때 생성자가 포함되어있지 않으면 디폴트로 생성자를 넣어준다.

디폴트 생성자는 아무런 매개변수도 없고 코드도 없다. 단지 인스턴스를 만들기 위한 용도만 가지고 있다.

&nbsp;

&nbsp;

# 2. 생성자 만들기

* * *

생성자를 프로그래머가 직접 만들 수 있다. 보통 이런 경우는 인스턴스가 만들어지면서 곧바로 멤버 변수들을 초기화 시킬때이다.

~~~ java
package classpart;

public class Person {
   String name;
   int age;
   String addr;

   public Person(String str) {
      name = str;
   } 
}
~~~

위 코드에서는 생성자가 매개변수를 가지고 있다. 매개변수를 받아 멤버변수 name에 값을 저장한다.

위와 같이 만들어진 클래스로 인스턴스를 만드는 방법은 다음과 같다.

~~~ java
package classpart;

public class Test {
    public static void main(String[] args) {
        Person per = new Person("Kim");
        System.out.println(per.name); // "Kim"
    }
}
~~~

인스턴스를 만들면서 바로 String 타입의 문자열을 전달해야 한다. 이 때 전달받은 문자열이 name에 저장된다. 출력해보면 "Kim"이 나온다.

**생성자를 프로그래머가 직접 만든 경우에는 default 생성자가 자동으로 만들어지지 않는다.** 따라서 위에서 만든 생성자뿐이므로 인스턴스를 만들 때 매개변수를 넣어주어야 하고 그렇지 않으면 컴파일 오류가 난다.

디폴트 생성자도 추가해주면 인스턴스는 두 가지 생성자 중에서 하나를 이용해서 만들 수 있다.

&nbsp;

&nbsp;

# 3. 생성자 오버로드

* * *

위에서 매개변수가 있는 생성자나 디폴트생성자 모두 사용할 수 있다고 했다. 이 때에는 디폴트 생성자도 프로그래머가 직접 추가해야한다.

그리고 매개변수가 있는 생성자도 여러개 만들 수 있다.

~~~ java
package classpart;

public class Person {
    String name;
    int age;
    String addr;

    public Person(String pname) {
        name = pname;
    }

    public Person(String pname, int page) {
        name = pname;
        age = page;
    }
}
~~~

위에서는 매개변수가 1개인 생성자와 2개인 생성자가 있다. 이같은 경우를 생성자 **오버로드(OverLoad)**라고 한다.

~~~ java
package classpart;

public class Test {
    public static void main(String[] args) {
        Person per1 = new Person("Kim");
        Person per2 = new Person("Kelly", 20);
    
        System.out.println(per1.name);
        System.out.println(per2.name);
        System.out.println(per2.age);
    }
}
~~~

이렇게 두 생성자중 아무거나 사용하여 인스턴스를 생성할 수 있다.

매개변수가 프로그램 사용자에게 노출되면 안되는 경우 매개변수에 직접 접근하는 것을 제한시키고 인스턴스를 만들 때 정보를 입력받아 초기화 해야하는 경우 등에 매개변수가 있는 생성자를 사용한다.

또한 홈페이지등에 회원가입을 할 때 필수 입력사항같은 항목도 미리 생성자의 매개변수로 지정해두면 편리하다.