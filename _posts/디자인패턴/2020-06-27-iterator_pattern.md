---
title: (디자인패턴) 10 - 이터레이터 패턴(Iterator Pattern)
date: 2020-06-27T18:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 이터레이터
  - 반복자
  - iterator
  - 패턴
  - pattern
---

## 1. 반복자 ##
----

+ 문제 상황 가정
	+ 따로 운영되던 개 보호소와 고양이 보호소가 합병을 했다.
	+ 기존의 관리 시스템도 거의 비슷하다.
	+ 그러나 지금까지 고양이 보호소는 고양이 목록을 '배열'로, 개 보호소는 개 목록을 컬렉션의 '리스트'로 관리했다.
	
~~~ java
public class Cat {
    private String name;
    private int age;

    public Cat(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}

public class Dog {
    private String name;
    private int age;

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}
~~~

~~~ java
public class CatShelter {
    Cat[] cats;

    public CatShelter() {
        cats = new Cat[3];
        cats[0] = new Cat("milky", 10);
        cats[1] = new Cat("soju", 4);
        cats[2] = new Cat("beer", 9);
    }

    public Cat[] getCats() {
        return cats;
    }
}

public class DogShelter {
    private List<Dog> dogs;

    public DogShelter() {
        dogs = new ArrayList<>();
        dogs.add(new Dog("happy", 3));
        dogs.add(new Dog("marry", 4));
        dogs.add(new Dog("tom", 9));
        dogs.add(new Dog("sunny", 11));
    }

    public List<Dog> getDogs() {
        return dogs;
    }
}
~~~

~~~ java
public class Main {
    public static void main(String[] args) {
        CatShelter catShelter = new CatShelter();
        DogShelter dogShelter = new DogShelter();

        Cat[] cats = catShelter.getCats();
        for(int i = 0; i < cats.length; i++) {
            System.out.println(cats[i].getName() + ", " + cats[i].getAge());
        }

        List<Dog> dogs = dogShelter.getDogs();
        for(int i = 0; i < dogs.size(); i++) {
            System.out.println(dogs.get(i).getName() + ", " + dogs.get(i).getAge());
        }
    }
}
~~~

+ 통일이 되지 않은 상태에서 모든 동물 목록을 출력하기 위해 개와 고양이에 대해 따로 반복문을 돌려야한다.
	+ 반복문을 캡슐화 할 수 있다면 좋겠다.

<br>

## 2. 이터레이터 패턴 ##
----

+ `CatShelter`와 `DogShelter`가 `Iterator` 객체를 생성하도록 메소드를 추가해보자.

~~~ java
public class CatShelter {
    
	...

    public Iterator createIterator() {
        return new CatIterator(this);
    }
}

public class DogShelter {
    
	...

    public Iterator createIterator() {
        return dogs.iterator();
    }
}
~~~

+ `DogShelter`은 애초에 `Dog`를 `List`에 저장했다.
	+ 따라서 컬렉션이 지원하는 `iterator()`를 호출하면 된다.
	
+ `CatShelter`는 `Cat`을 배열에 저장했으므로 `CatIterator` 클래스를 따로 만들어야한다.

~~~ java
public class CatIterator implements Iterator<Cat> {

    private Cat[] cats;
    private int idx = 0;

    public CatIterator(CatShelter catShelter) {
        cats = catShelter.getCats();
    }

    @Override
    public boolean hasNext() {
        return idx < cats.length;
    }

    @Override
    public Cat next() {
        return cats[idx++];
    }
}
~~~

<br>

+ 이제 각각의 보호소 클래스의 `createIterator()`를 호출하면 통일된 `Iterator` 객체가 나온다.
	+ 아래와 같이 클라이언트 코드를 단순화시킬 수 있다.
	
~~~ java
public class Main {
    public static void main(String[] args) {
        CatShelter catShelter = new CatShelter();
        DogShelter dogShelter = new DogShelter();

        Iterator catIterator = catShelter.createIterator();
        Iterator dogIterator = dogShelter.createIterator();

        printAnimals(catIterator);
        printAnimals(dogIterator);
    }

    public static void printAnimals(Iterator iterator) {
        while(iterator.hasNext()) {
            Object obj = iterator.next();
            if(obj instanceof Cat)
                System.out.println(((Cat) obj).getName() + ", " + ((Cat) obj).getAge());
            else if(obj instanceof Dog)
                System.out.println(((Dog) obj).getName() + ", " + ((Dog) obj).getAge());
        }
    }
}
~~~

+ 반복 출력 부분을 `printAnimals()`로 분리시켰다.
	+ 클라이언트 코드에서는 집합체가 어떤 식으로 구현되어있는지(배열인지, 리스트인지) 신경쓸 필요 없다. `Iterator` 객체만 전달받으면 그만이다.
	+ 사실 `Cat`과 `Dog`를 `Animal` 인터페이스로 추상화 시키면 객체의 타입체크를 해서 경우를 나눌 필요도 없다.
	+ 코드 양이 길어지면 읽기 불편할까봐 위와 같이 `instanceof`로 타입체크를 했다. 여기서 중요한 부분은 서로 다른 집합체가 같은 `Iterator` 객체를 return 한다는 것이다.
	+ 또한 여전히 `printAnimals()`를 두 번이나 호출해야 한다는 문제가 있지만 이 파트에서 다루는 주제와는 상관없으므로 신경쓰지 말자.

<br>

![class_diagram](https://nobbaggu.github.io/images/designpattern/iteratorpattern/class_diagram.jpg){: width="50%" height="50%"}

+ 다이어그램에서는 이터레이터 생성 메소드가 `createIterator`이 아니라 `iterator()` 이다.

<br>

## 3. 이터레이터 패턴의 장점 ##
----

+ 컬렉션 구현 방법을 노출시키지 않으면서도 집합체의 모든 항목에 접근할 수 있게 해준다.

+ 다형적 코딩을 할 수 있다.
	+ 이터레이터를 지원하는 어떤 컬렉션에 대해서도 써먹을 수 있는 코드를 만들 수 있다.
	
+ **단일 역할 원칙**
	+ 클래스를 변경하는 이유는 한 가지 뿐이어야 한다.
	+ 이터레이터를 통해 반복자 객체를 분리시킴으로써 집합체 관리 클래스는 본인의 역할만 하면 된다.
	+ 이로써 단일 역할의 원칙을 지킬 수 있게 되었다.
