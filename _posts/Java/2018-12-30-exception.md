---
title: (Java) 50. 예외처리
date: 2018-12-30T16:16:20+09:00
author: nobbaggu
layout: post
categories: Java
image: /images/2018/12/Java-thumbnail.jpg
tags:
  - autocloseable
  - catch
  - close
  - exception
  - Java
  - try
  - 예외
  - 오류
  - 자바
  - 처리
---
프로그램을 사용하다 보면 언제나 오류가 생길 가능성이 있다. 컴파일 이전 코드 작성단계에서 발견되는 에러는 큰 문제가 되지않고 수정하면 된다.

그러나 컴파일과정에서 잡지 못한 에러가 있을 수 있다. 이것은 프로그램 실행 중에 문제를 일으킬 소지가 있으며, 따라서 실행오류(Runtime Error)라고 부른다.

&nbsp;

# 1. 실행오류

* * *

실행오류는 크게 2가지로 나뉜다.

### 1) 오류(Error)

말 그대로 오류이다. 사용 가능한 스택 메모리의 부족, 배열의 잘못된 index 접근 등이 있다. 이것은 하드웨어적인 문제이므로 우리가 어떻게 제어할 방법이 없다.

Throwable 클래스를 상속받는 Error클래스는 시스템 오류를 다루는 클래스이다. 그러나 프로그램에서 제어하지 않는다.

&nbsp;

### 2) 예외(Exception)

예외는 우리가 충분히 제어 가능한 경우이다. 네트워크 연결이 끊어진 경우, 파일을 불러오려는데 파일이 비어있는 경우 등이 있다.

Throwable 클래스를 상속받는 Exception클래스와 그 하위클래스들은 예외처리를 할 때 사용한다.

&nbsp;

# 

![image](https://nobbaggu.github.io/images/2018/12/no-name-40.jpg){: width="50%" height="50%"}

모든 예외처리 클래스의 최상위 클래스는 Exception클래스이다. 위 그림에 표시되지 않은 클래스 이외에도 많은 클래스가 있지만 위 클래스들은 사용 빈도가 높은 클래스들이다.

보통 예외가 발생하면 컴파일러가 알려주어 예외처리를 유도하기는 한다. 이 때에는 try-catch문을 사용하여 예외처리를 하면된다. 그런데 RuntimeException 경우는 예외처리를 하지 않아도 컴파일 과정에서 오류가 나지 않는다. 예를 들면 나누기 식에서 분모에 0이 입력되는 경우인데, 실제로 사용자로부터 입력을 받기 전까지는 0이 입력될 지 알 수 없기 때문이다.

&nbsp;

&nbsp;

# 2. 예외 처리하기

* * *

~~~ java
package exception;

public class ArrayException {
   public static void main(String[] args) {
      int[] arr = {1,2,3,4,5};
      
      //arr는 index가 0~4까지 인데 5를 대입하면 ArrayIndexOutOfBoundsException 발생
      for(int i=0; i<=5; i++) {
         System.out.println(arr[i]);
      }
   }
}
~~~

위와 같은 예외는 컴파일 과정에서 잡아내지 않는다. 따라서 이후 프로그램 사용중에 런타임 에러에 의해 프로그램이 비정상 종료될 수도 있으므로 프로그래머가 예외처리를 해주어야한다.

예외처리는 try-catch문으로 한다. 위 코드를 예외처리를 하도록 변경하면 아래와 같이 된다.

~~~ java
package exception;

public class ArrayException {
   public static void main(String[] args) {
      int[] arr = {1,2,3,4,5};
      
      
      try {//예외가 일어날 수 있는 코드
         for(int i=0; i<=5; i++) {
            System.out.println(arr[i]);
         }
      }catch(ArrayIndexOutOfBoundsException e) { //예외가 일어났을 경우 실행할 코드. 매개변수로는 처리할 예외의 종류
         System.out.println(e);
         System.out.println("배열 인덱스 범위 오류 예외처리");
      }
      
   }
}
~~~

try문 안에는 예외가 발생할 수 있는 코드부분을 넣는다. catch문에는 예외가 일어났을 경우 실행할 코드를 넣어주면 된다. 그리고 catch문의 매개변수로는 처리할 예외를 넣어주면 된다.

위 예제는 컴파일러가 예외를 체크하지 않는다. 그러나 자바에서는 많은 예외들을 컴파일러가 잡아내주어 이에 대해 예외처리를 해주지 않으면 컴파일리 되지 않는다. 파일 입출력이 이와 같은 경우이다.

~~~ java
package exception;

import java.io.FileInputStream; //파일을 읽고 쓰기위한 스트림

public class ArrayException {
   public static void main(String[] args) {
      FileInputStream fis = new FileInputStream("myfile.txt"); // FileNotFoundException 예외 발생
      
   }
}
~~~

이 예제에서는 읽으려는 파일이 없는경우에 해당하는 FileNotFoundException 예외가 발생하였다. 이 경우도 이 전의 예제와 같이 try-catch문으로 예외처리를 해주면 된다. 빨간줄이 나타나는 부분에 마우스를 가져다 대면 1) 예외를 던지거나 2) try-catch문으로 감싸거나 둘 중 하나를 선택할 수 있다. try-catch문으로 감싸는 것을 선택하면 다음과 같이 코드가 수정된다.

~~~ java
package exception;

import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class ArrayException {
   public static void main(String[] args) {
      
      try {
         FileInputStream fis = new FileInputStream("myfile.txt");
      } catch (FileNotFoundException e) {
         System.out.println(e); //e.toString() 메소드 호출 -> 예외 메세지 출력
      }

      System.out.println("Program is still running");
      
   }
}
~~~

실행결과

java.io.FileNotFoundException: myfile.txt (지정된 파일을 찾을 수 없습니다)


Program is still running


 

예외 자료형의 참조변수 e를 출력한다는 것은 e.toString()을 출력하는 것이다. 그리고 이 메소드는 해당하는 예외 메세지를 출력하도록 재정의 되어있다.

아무튼 try-catch문으로 예외를 처리해주면 런타임 오류시에도 프로그램의 비정상적인 종료가 일어나지 않는다. 뒷부분에 이를 확인하기 위해 넣어둔 "Program is still running" 구문이 출력되는 것으로 확인할 수 있다.

&nbsp;

&nbsp;

# 3. finally문

* * *

위에서 파일 입출력을 위하여 FileInputStream을 생성하였다. 스트림을 생성한다는 것은 메모리 자원을 사용하는 것이다. 파일 입출력이 끝났다면 이 스트림을 해제하여 해당 자원을 다시 회수하는 것이 올바르다. 위에서는 fis라는 참조변수에 스트림 객체를 저장하였으므로 fis.close(); 메소드 호출을 통하여 스트림을 해제할 수 있다.



~~~ java
package exception;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class ArrayException {
   public static void main(String[] args) {
      
      try {
         FileInputStream fis = new FileInputStream("myfile.txt");
         
         ///////////////////////////
         if(fis != null) {
            try {
               fis.close();
            }catch(IOException e) {    //스트림 종료
               e.printStackTrace();
            }
         }
         ///////////////////////////
         
      } catch (FileNotFoundException e) {
         System.out.println(e);
      }
      
      System.out.println("Program is still running");
      
   }
}
~~~

위 코드는 스트림 종료 메소드 호출을 추가한 것이다. 그런데 지금은 try-catch문이 단순하다. 만약 예외가 발생할 수 있는 부분이 10개라면 10개 모두에 대한 try-catch문에서 모두 스트림 종류 메소드를 호출해 주어야하고 이는 코드를 매우 복잡하게 만들것이다.

이럴 때 사용할 수 있는것이 finally 구문이다. **finally구문은 일단 try문이 실행되었다면 어떤 상황에서도 실행이 된다.**

~~~ java
package exception;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class ArrayException {
   public static void main(String[] args) {
      
      FileInputStream fis = null;
      
      try {
         fis = new FileInputStream("myfile.txt");
      } catch (FileNotFoundException e) {
         System.out.println(e);
         return;
      } finally {
         if(fis != null) {
            try {
               fis.close();
            } catch (IOException e) {
               e.printStackTrace();
            }
         }
         
         System.out.println("This finally block is always executed");
      }
      
   }
}
~~~

실행결과

java.io.FileNotFoundException: myfile.txt (지정된 파일을 찾을 수 없습니다)


This finally block is always executed


 예외가 발생하여 catch로 넘어가고 return문을 통해 프록램이 종료되도록 하더라도 finally문이 실행 된 다음에 프로그램이 종료된다.

&nbsp;

&nbsp;

# 4. AutoCloseable 인터페이스

* * *

사실 Java7 부터는 FileInputStream이 명시적으로 close() 메소드 호출을 통해 닫아주지 않더라도 자동으로 닫히도록 구현되어있다.

AutoCloseable 인터페이스는 close() 추상메소드를 가지고 있고, 이를 구현한 클래스들은 close()메소드를 재정의 하여야한다.

~~~ java
package exception;

public class AutoCloseTest implements AutoCloseable {

   @Override
   public void close() throws Exception {
      System.out.println("close() is called automatically"); //호출되면 해당 문장 출력    
   }
   
   public static void main(String[] args) {
      try(AutoCloseTest obj = new AutoCloseTest()){
         //No Code
      }catch(Exception e) {
         System.out.println(e);
      }
   }

}


~~~

실행결과

close() is called automatically

AutoCloseTest 클래스는 AutoCloseable 인터페이스를 구현하였고 close() 메소드를 재정의하였다. 이후 try문에서 AutoCloseTest로 객체를 하나 만들어 넘겨주었고 close() 메소드가 자동으로 호출되는 것을 확인할 수 있다.

catch문에서도 close 메소드가 호출되는지 확인해야한다. 위 코드에서 //No Code 부분에 throw new Exception(); 이라고 치면 강제로 예외를 만들어내어 catch문을 실행시킬 수 있다.

~~~ java
package exception;

public class AutoCloseTest implements AutoCloseable {

   @Override
   public void close() throws Exception {
      System.out.println("close() is called automatically"); //호출되면 해당 문장 출력    
   }
   
   public static void main(String[] args) {
      try(AutoCloseTest obj = new AutoCloseTest()){
         throw new Exception(); //강제로 예외 발생시켜 catch문 실행
      }catch(Exception e) {
         System.out.println("This is exception");
      }
   }

}
~~~

실행결과

close() is called automatically


This is exception


