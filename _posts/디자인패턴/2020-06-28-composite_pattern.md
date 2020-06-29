---
title: (디자인패턴) 11 - 컴포지트 패턴(Composite Pattern)
date: 2020-06-28T14:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 컴포지트
  - composite
  - 패턴
  - pattern
---

## 1. 컴포지트 패턴이란 ##
----

+ 개별 객체와 복합 객체(composite)를 같은 방법으로 다룰 수 있는 디자인 패턴
	+ OS에서 파일 하나와 파일의 집합인 디렉토리는 근본적으로 똑같이 취급되는걸 생각하면 된다.
	+ 객체들을 부분과 전체를 나타내는 트리 계층구조로 구성한다.
	
<br>

![composite_pattern_tree](https://nobbaggu.github.io/images/designpattern/compositepattern/composite_pattern_tree.jpg){: width="50%" height="50%"}

<br>

+ 모든 구성요소(단일 객체든 복합객체든) 하나를 component라고 한다.
	+ composite는 자식 node들을 가지는 component이다.
	+ leaf는 자식 node가 없는 단일 component이다.
	
![class_diagram](https://nobbaggu.github.io/images/designpattern/compositepattern/class_diagram.jpg){: width="50%" height="50%"}

+ 클라이언트는 composite인지 leaf인지 구별할 필요 없이 component라는 단일 인터페이스의 메소드를 호출할 수 있다.
	+ composite인지 leaf인지에 따라 메소드 구현이 달라진다.
	+ 예를들어 leaf에서 `add()` 메소드는 필요 없으므로 비워두거나 예외를 던지는 방식으로 처리한다.

<br>

## 2. 예제 ##
----

+ 디렉토리와 파일을 하나의 동일한 `Component`로 취급하는 상황을 코드로 짜본다.

~~~ java
public abstract class FileComponent {
    public void add(FileComponent fileComponent) {}
    public void remove(FileComponent fileComponent) {}
    public abstract void print();
}
~~~

~~~ java
public class Directory extends FileComponent{

    private String name;
    private List<FileComponent> childFiles;

    public Directory(String name) {
        this.name = name;
        childFiles = new ArrayList<>();
    }

    @Override
    public void add(FileComponent fileComponent) {
        childFiles.add(fileComponent);
    }

    @Override
    public void remove(FileComponent fileComponent) {
        childFiles.remove(fileComponent);
    }

    @Override
    public void print() {
        System.out.println("Directory: " + name);
        for (FileComponent component : childFiles) {
            component.print();
        }
    }
}
~~~

~~~ java
public class File extends FileComponent {
    private String name;

    public File(String name) {
        this.name = name;
    }

    @Override
    public void print() {
        System.out.println(name);
    }
}
~~~

+ `FileComponent`라는 추상클래스를 만들었다.
	+ `add()`와 `remove()`는 구현부를 비워둔다.
		+ `File`에서는 필요 없으므로 오버라이딩 하지않고 `Directory`에서는 `FileComponent`를 추가,삭제할 수 있도록 오버라이딩한다.
	+ `print()`는 추상클래스로 둔다.
		+ `File`에서는 이름을 출력해주고, `Directory`에서는 자신의 이름을 출력해준 후 자식이 있으면 각 자식에 대해 재귀적으로 `print()`를 호출해준다.

~~~ java
public class Main {
    public static void main(String[] args) {
        
		//dir1에 file1, file2 추가
		File file1 = new File("file1");
        File file2 = new File("file2");
        Directory dir1 = new Directory("dir1");
        dir1.add(file1);
        dir1.add(file2);

		//dir2에 file3 추가
        File file3 = new File("file3");
        Directory dir2 = new Directory("dir2");
        dir2.add(file3);
		
		//dir1에 dir2 추가
        dir1.add(dir2);

        System.out.println(">> dir1");
        dir1.print();

        System.out.println("-----------------");
        System.out.println(">> dir2");
        dir2.print();
    }
}
~~~


<br>

+ 예제에서 볼 수 있듯 컴포지트 패턴을 사용함으로써 클라이언트는 파일과 디렉터리를 구분하지 않고 `FileComponent`라는 하나의 인터페이스에만 의존하고 있다.
	+ 여기서 인터페이스란 실제 자바 인터페이스를 의미한다기보다는 클라이언트와 상호작용하는 상위 클래스 개념으로 이해해야한다.
	+ 클라이언트가 파일, 디렉토리인지에 따라 다른 메소드 호출을 해야한다면 신경쓸게 많아지고 일이 복잡해진다.
		+ 지저분하게 `if/else` 혹은 `instanceof` 같은 연산자들을 사용해야 한다.
		
<br>

## 3. 정리 ##
----

단일객체와 그것들이 모여있는 복합 객체(컬렉션 등)을 동일하게 다룰 수 있다. 클라이언트 입장에서는 신경쓸게 줄어들고 코드도 깨끗해진다. 이 때 복합객체와 단일객체의 메소드들을 프로그래머가 적절히 처리해놓아야 한다. 가령 단일 객체에서 `add()`, `remove()`와 같은 메소드들의 구현부를 비워놓던가 예외를 던지던가 하는 식으로 말이다.