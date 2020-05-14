---
title: (Java) 51. 예외 throws
date: 2018-12-30T17:26:45+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - catch
  - exception
  - Java
  - throws
  - try
  - 다중
  - 던지기
  - 미루기
  - 예외
  - 자바
  - 처리
---
# 1. 예외 던지기

* * *

예외를 해당 메소드의 코드에서 처리하지 않고 이후 객체의 메소드를 호출하여 사용하는 부분에서 처리하도록 미루는 것이 있는데, 이를 예외 던지기, 예외 미루기 라고 한다.

~~~ java
package exception;

import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class ArrayException {
   public static void main(String[] args) throws FileNotFoundException {
      
      FileInputStream fis = new FileInputStream("a.txt");
   }
}
~~~

위 코드에서는 FileNotFoundException 예외를 main() 메소드를 호출하여 사용하는 부분으로 넘긴다. 그런데 main()메소드는 운영체제의 자바 가상머신(JVM)에서 호출하므로 프로그램이 비정상 종료된다. 따라서 예외를 main() 메소드까지 미루는것은 좋지 않다.

~~~ java
package exception;

import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class ThrowsException {
    
   // FileNotFoundException, ClassNotFoundException 예외 처리 미루기
   public Class getClass(String filename, String className) throws FileNotFoundException, ClassNotFoundException {
      FileInputStream fis = new FileInputStream(filename);
      Class myClass = Class.forName(className);
      
      return myClass;
   }
   
   public static void main(String[] args) {
      ThrowsException te = new ThrowsException();
      Class c = te.getClass("myfile.txt", "myClass.class"); //에러 발생

   }

}
~~~

위 코드에서는 ThrowsException 클래스에서 두 개의 예외를 던졌다. 이후 main()메소드에서 객체를 만들어 getClass() 메소드를 호출하므로 main메소드에서 예외를 처리해야 한다. 이 때에는 두 가지 방법이 있다.

![image](https://nobbaggu.github.io/images/2018/12/no-name-41.jpg){: width="50%" height="50%"}

처리하는 방법은 3가지이다. 첫 번째는 다시 던지는 것인데 이는 JVM으로 던져버리므로 프로그램의 비정상적 종료를 불러일으킬 것이므로 아래 두 가지 방법을 선택하는 것이 좋다.

### 1) Surround with try/multi-catch

이 방법에서는 하나의 catch문에서 여러개의 예외를 처리하도록 할 수 있다.

~~~ java
try {
         Class c = te.getClass("myfile.txt", "myClass.class");
      } catch (FileNotFoundException | ClassNotFoundException e) { //두 예외 중 한 가지 발생하면 블록 실행
         e.printStackTrace();
      }
~~~

OR 조건 연산자를 사용하여 여러 예외를 하나의 블록문으로 처리할 수 있다.

&nbsp;

### 2) Surround with try/catch

~~~ java
try {
         Class c = te.getClass("myfile.txt", "myClass.class");
      } catch (FileNotFoundException e) {
         System.out.println("Can not find the file");
      } catch (ClassNotFoundException e) {
         System.out.println("Can not find the class");
      }
~~~

이 방법은 각각의 예외를 다른 방식으로 처리해야 할 경우 사용할 수 있다.

어떤 방식을 선택할지는 상황에따라, 그리고 만드는 프로그램의 특성에 따라 달리할 수 있다. 예를들어 같은 예외라도 여러곳에서 호출하면서 호출하여 사용하는 부분에 따라 다른 방식의 처리가 필요하다면 일단 throws를 통해 예외를 던진 후 호출하는 부분에서 상황에 맞게 예외를 처리하면 된다.

&nbsp;

&nbsp;

# 2. 다중 예외처리

* * *

~~~ java
try {
   Class c = te.getClass("myfile.txt", "myClass.class");
} catch (FileNotFoundException e) {
   e.printStackTrace();
} catch (ClassNotFoundException e) {
   e.printStackTrace();
} catch (Exception e) {
   e.printStackTrace();
}
~~~

어떤 예외가 발생할 지 파악할 수 없는 경우에 위와 같이 마지막에 모든 예외 클래스의 최상위 클래스인 Exception 클래스로 예외처리를 해줄 수 있다. 이 경우 어떤 예외가 들어오더라도 Exception으로 자동 형변환 되어 예외가 처리된다.

그런데 이런 상황에서 최상위 클래스인 Exception 클래스는 항상 마지막 catch문에 넣어주어야한다.

~~~ java
try {
   Class c = te.getClass("myfile.txt", "myClass.class");
} catch (Exception e) {
   e.printStackTrace();
} catch (ClassNotFoundException e) {
   e.printStackTrace();
} catch (FileNotFoundException e) {
   e.printStackTrace();
}
~~~

위와 같이 Exception클래스를 가장 위의 catch문에 놓을 경우 어떤 예외가 들어오더라도 첫 번째 catch문에 닿기 때문에 이후에 있는 예외에는 도달할 일이 없어 오류가 나게된다.

&nbsp;

&nbsp;

# 3. 사용자 정의 예외처리

* * *

어떤 사이트에서 회원가입 기능을 구현할 때 아이디를 입력받으면 적합성 체크를 한다. 아이디가 비어있거나 길이 제한을 만족하지 않을 때 예외를 발생시켜야 하는데, 이러한 클래스는 자바에서 제공하지 않으므로 직접 만들어 사용해야 한다.

예외처리를 만들어 사용할 경우는 가장 비슷한 예외 클래스를 상속받으면 되는데, 잘 모를 경우는 최상위 클래스인 Exception 클래스를 상속받아도 된다.

~~~ java
package exception;

public class IDException extends Exception {
   public IDException(String message) {
      super(message);
   }
}
~~~

IDException은 Exception 클래스를 상속받았다. IDException 객체를 생성하면서 예외 메세지를 같이 넣어주도록 만들었다. 이제 테스트를 해보자.

~~~ java
package exception;

public class IDTest {

   private String id;
      
   public String getId() {
      return id;
   }

   public void setId(String id) throws IDException {
      if(id == null) {
         throw new IDException("ID cannot be empty"); //강제 예외 발생
      }
      else if(id.length() < 8 || id.length() > 20) {
         throw new IDException("ID length can be only in 8~20"); //강제 예외 발생
      }
      
      this.id = id;
   }


   public static void main(String[] args) {
      IDTest test = new IDTest();
      
      String id = null;
      try {
         test.setId(id);
      } catch (IDException e) {
         System.out.println(e.toString());
      }
      
      String id2 = "abcd";
      try {
         test.setId(id2);
      } catch(IDException e) {
         System.out.println(e.toString());
      }
   }

}
~~~

실행결과

exception.IDException: ID cannot be empty


exception.IDException: ID length can be only in 8~20

직접 정의한 대로 예외 메세지가 출력된다.