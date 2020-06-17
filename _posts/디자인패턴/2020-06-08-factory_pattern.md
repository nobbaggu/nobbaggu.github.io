---
title: (디자인패턴) 4 - 팩토리 패턴(Factory Pattern)
date: 2020-06-08T12:37:19+09:00
author: nobbaggu
layout: post
categories: 디자인패턴
tags:
  - 디자인패턴
  - 팩토리
  - 패턴
  - 인터페이스
  - 추상클래스
  - 상속
  - 구현
  - 분리
  - new
  - 의존성
---

## 1. 개요 ##
----

+ 인터페이스/추상클래스에 맞춰 코딩을 하면 다형성에 의해 여러 객체를 갈아끼울 수 있다.
	+ ConcreteClass 타입으로 변수를 선언하면 그 타입이 아닌 객체는 변수에 대입할 수 없다.
	+ 즉, 변화가 일어날 때 마다 코드를 고쳐야 한다는 말이다.
	
+ 인터페이스/추상클래스에 맞춘 코딩을 하면 끝인가?
	+ 새로운 객체로 바꾸거나 추가할 때 `new` 연산자를 통해 객체를 변경/추가해야하므로 코드가 변경되어야한다.
	+ 아래 피자가게(`PizzaStore`) 예시를 보자
	+ 모든 피자 객체는 `Pizza` 추상 클래스를 상속받는다.
	
~~~ java
public class PizzaStore {
    public Pizza orderPizza(String type) {
        //추상클래스 Pizza 타입
		Pizza pizza;

        if(type.equals("cheese")) {
            pizza = new CheesePizza();
        } else if(type.equals("greek")) {
            pizza = new GreekPizza();
        } else if(type.equals("pepperoni")) {
            pizza = new PepperoniPizza();
        }

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }
}
~~~

<br>

+ `GreekPizza` 메뉴가 제외되고 `ClamPizza`와 `VeggiePizza` 두 개의 신메뉴가 추가된다면?

~~~ java
public class PizzaStore {
    public Pizza orderPizza(String type) {
        Pizza pizza;

        //변하는 부분
        if(type.equals("cheese")) {
            pizza = new CheesePizza();
        } else if(type.equals("pepperoni")) {
            pizza = new PepperoniPizza();
        } else if(type.equals("clam")) {
            pizza = new ClamPizza();
        } else if(type.equals("veggie")) {
            pizza = new VeggiePizza();
        }

        //변하지 않는부분
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }
}
~~~

<br> 

+ `Pizza` 인터페이스 타입으로 변수를 선언하고 다형성을 통해 상황에 따른 피자 객체를 생성한다.
	+ 하지만 신메뉴가 추가되거나 기존 메뉴가 사라질 경우 코드가 수정되어야 한다.
	+ 변하는 부분을 분리시킨다는 객체지향 디자인 원칙에 따르면?
		+ 객체 생성 부분이 변하므로 분리 시켜야한다.
		
+ **팩토리 패턴은 객체를 생성하는 부분을 분리해 객체 생성만 담당하게 하는 디자인 방법**이다.

<br>

## 2. 팩토리란? ##
----

+ 위에서 든 예시 코드에서 피자객체 생성 부분만 따로 떼어내 캡슐화해본다.

~~~ java
public class SimplePizzaFactory {
    public Pizza createPizza(String type) {
        Pizza pizza = null;

        if(type.equals("cheese")) {
            pizza = new CheesePizza();
        } else if(type.equals("pepperoni")) {
            pizza = new PepperoniPizza();
        } else if(type.equals("clam")) {
            pizza = new ClamPizza();
        } else if(type.equals("veggie")) {
            pizza = new VeggiePizza();
        }

        return pizza;
    }
}
~~~

<br>

+ 이제 객체 생성 부분을 팩토리가 대신하게 하는 `PizzaStore` 클래스를 다시 작성해보자

~~~ java
public class PizzaStore {

    SimplePizzaFactory factory;
    
    public PizzaStore(SimplePizzaFactory factory) {
        this.factory = factory;
    }
    
    public Pizza orderPizza(String type) {

		//팩토리에 메뉴만 전달하면 해당 피자를 만들어준다
		Pizza pizza = factory.createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }
}
~~~

+ 아래 그림은 `PizzaStore`가 `SimplePizzaFactory`를 통해 피자 객체를 생성하는 다이어그램을 보여준다.

![simple_factory](https://nobbaggu.github.io/images/designpattern/factorypattern/simple_factory.jpg){: width="50%" height="50%"}

+ 간단한 팩토리를 만들었지만 이것이 **팩토리 패턴**은 아니다.
	+ 흔히 말하는 **팩토리 패턴**은 **팩토리 메소드 패턴**이다.
	+ 지금 만든 것은 팩토리 패턴에서 객체 생성부를 따로 캡슐화한 **추상 팩토리**이다.
	+ 아직 팩토리 패턴에 대해서는 알아보지 않았다.
	+ 팩토리 메소드 패턴은 말그대로 객체 생성부를 따로 분리시키지만 하나의 메소드로 뺄 뿐 캡슐화까지 가지는 않는다.
	
<br>

## 3. 팩토리 메소드 패턴 ##
----

+ 피자가게 예시를 팩토리 메소드 패턴으로 바꿔보자

~~~ java
public abstract class PizzaStore {

    public Pizza orderPizza(String type) {
        Pizza pizza;

		//createPizza()를 통해 피자를 생성할 때 어떤 피자가 올지 전혀 알지 못한다.
		//즉, 분리되어있다.
        pizza = createPizza(type);

        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();

        return pizza;
    }

    //피자 생성 부분을 온전히 담당한다.
	//구상 클래스에서 피자 종류가 확정된다.
    abstract Pizza createPizza(String type);
}
~~~

<br>

+ `PizzaStore`를 구현한 두 클래스를 만들어보자.

~~~ java
public class NYPizzaStore extends PizzaStore {
    Pizza createPizza(String type) {
        if(type.equals("cheese")) {
            return new NYStyleCheesePizza();
        } else if(type.equals("pepperoni")) {
            return new NYStylePepperoniPizza();
        } else if(type.equals("clam")) {
            return new NYStyleClamPizza();
        } else if(type.equals("veggie")) {
            return new NYStyleVeggiePizza();
        }

        return pizza;
    }
}
~~~

~~~ java
public class ChicagoPizzaStore extends PizzaStore {
    Pizza createPizza(String type) {
        if(type.equals("cheese")) {
            return new ChicagoStyleCheesePizza();
        } else if(type.equals("pepperoni")) {
            return new ChicagoStylePepperoniPizza();
        } else if(type.equals("clam")) {
            return new ChicagoStyleClamPizza();
        } else if(type.equals("veggie")) {
            return new ChicagoStyleVeggiePizza();
        }

        return pizza;
    }
}
~~~

+ `orderPizza()`에서 피자객체 생성 부분이 `createPizza()` 메소드로 분리되었다.
	+ `PizzaStore`를 확장하는 구상클래스(concrete class)들은 `createPizza()`를 오버라이딩 해야한다.
	+ `NYPizzaStore`, `ChicagoPizzaStore`과 같은 구상 클래스에서 자신들 스타일대로 `createPizza()`를 구현해야한다.
	+ **'객체 생성을 서브클래스에 위임'**한다는 사실이 중요하다.
	+ 서브클래스에 위임하는 메소드를 **팩토리 메소드**라고 한다.

<br>

+ `Pizza` 클래스도 만들어보자

~~~ java
public abstract class Pizza {
    String name;
    String dough;
    String sauce;
    ArrayList<String> toppings = new ArrayList<>();

    void prepare() {
        System.out.println(name + " 준비중...");
        System.out.println("도우 만드는중...");
        System.out.println("소스 뿌리는중...");
        System.out.println("토핑 얹는중...");
        for(int i = 0; i < toppings.size(); i++) {
            System.out.println("   >" + toppings.get(i));
        }
    }

    void bake() {
        System.out.println("350도에서 25분동안 굽습니다.");
    }

    void cut() {
        System.out.println("피자를 대각선으로 자릅니다.");
    }

    void box() {
        System.out.println("피자를 박스에 담습니다.");
    }

    public String getName() {
        return name;
    }
}
~~~

<br>

+ 아래는 `Pizza`를 구현한 구상 클래스들이다.
	+ 귀찮으니까 각 Style별로 1개씩만 해보자.

~~~ java
public class NYStyleCheesePizza extends Pizza {
    public NYStyleCheesePizza() {
        name = "뉴욕 스타일 치즈피자";
        dough = "Thin Crust Dough";
        sauce = "Marinara Sauce";

        toppings.add("Grated Reggiano Cheese");
    }
}
~~~

~~~ java
public class ChicagoStyleCheesePizza extends Pizza {
    public ChicagoStyleCheesePizza() {
        name = "시카고 스타일 치즈피자";
        dough = "Extra Thick Crust Dough";
        sauce = "Plum Tomato Sauce";

        toppings.add("Shredded Mozzarella Cheese");
    }

    void cut() {
        System.out.println("Cutting the pizza into square slices");
    }
}
~~~

<br>

+ 메인 클래스를 작성하고 피자를 주문해보자.

~~~ java
public class Main {
    public static void main(String[] args) {
        PizzaStore nyPizzaStore = new NYPizzaStore();
        PizzaStore chicagoPizzaStore = new ChicagoPizzaStore();

        nyPizzaStore.orderPizza("cheese");
        System.out.println();
        chicagoPizzaStore.orderPizza("cheese");
    }

}
~~~

실행결과

~~~ console
뉴욕 스타일 치즈피자 준비중...
도우 만드는중...
소스 뿌리는중...
토핑 얹는중...
   >Grated Reggiano Cheese
350도에서 25분동안 굽습니다.
피자를 대각선으로 자릅니다.
피자를 박스에 담습니다.

시카고 스타일 치즈피자 준비중...
도우 만드는중...
소스 뿌리는중...
토핑 얹는중...
   >Shredded Mozzarella Cheese
350도에서 25분동안 굽습니다.
피자를 사각형으로 자릅니다.
피자를 박스에 담습니다.
~~~

<br>

+ 팩토리 메소드 패턴에서 클래스간의 관계를 정리하면 아래 그림과 같다.

![class_diagram](https://nobbaggu.github.io/images/designpattern/factorypattern/class_diagram.jpg){: width="50%" height="50%"}

<br>

+ 일반적으로 객체를 생성하는 추상클래스를 `Creator`라 부른다.
	+ 구상 클래스는 `ConcreteCreator`라 부른다.
	+ 일반적인 메소드들은 `Creator`에서 구현되어있다.
	+ `ConcreteCreator`는 실제 객체 생성만 담당한다.
	
+ 일반적으로 생성되는 객체 인터페이스/추상클래스를 `Product`라고 부른다.
	+ 이를 구현한 실제 객체를 `ConcreteProduct`라고 부른다.

## 4. 특징 ##
----

+ `Creator`와 `Product`가 느슨한 결합을 하고있다.
	+ 새로운 제품의 출시, 변경이 있을 경우 `Creator`를 건드릴 필요가 없다.
	+ 위에서 말한 경우에는 `ConcreteCreator`의 객체 생성 부분만 바꾸면 된다.
	+ 중복되는 코드는 추상클래스에서 한 번만 작성하면 되고 이후에는 (거의) 변하지 않는다. 따라서 실제 구상 객체를 만들 때 필요한 인터페이스 코드(피자객체를 만드는 코드)에 집중할 수가 있다.
	+ 결론적으로 **구상 클래스에 대한 의존성이 줄어든다.**
		+ `Creator`가 구상클래스이고 여기서 모든 스타일, 모든 종류의 피자 객체 생성을 직접 구현했다면 신메뉴 추가, 변경 및 새로운 지역 가게의 오픈 및 점포 운영 중단 등이 발생할 때마다 클래스 코드를 허구한날 고쳐야한다. 즉, 구상 클래스에 '의존적'이다.
		+ 그러나 팩토리 메소드 패턴을 통해 변하는 부분을 분리시켰기 때문에 구상 클래스에 대한 의존성을 줄였다.**(의존성 뒤집기 원칙 - Dependency Inversion Principle, DIP)**
	
+ **DIP(의존성 뒤집기 원칙)**
	+ 추상화된 것에 의존하도록 만들어라.
	+ 구상클래스에 의존하지 않도록 만들어라.
	+ 즉, 고수준 구성요소가 저수준 구성요소에 의존하지 않게 해야한다는 객체지향 디자인 원칙
	+ (ex) `PizzaStore`의 동작은 클래스에 내포된 `Pizza`에 의해 정의된다. 따라서 상위요소인 `PizzaStroe`가 `Pizza` 클래스에 의존하지 않도록 해야한다.
	
+ 추상 팩토리 vs 팩토리 메소드 패턴
	+ 추상 팩토리는 객체생성을 담당하는 클래스를 따로 캡슐화했다.
	+ 뭔가 더 깔끔하고 고급져 보일 수 있다.
	+ 그러나 팩토리 메소드 패턴에 비해 유연성이 부족하다.
	+ 팩토리 메소드 패턴에서는 추상클래스를 확장하여 실제 클래스를 만들 때 원하는 피자 종류를 마음대로 제공할 수 있어 강력한 유연성을 발휘한다.

<br>

+ `PizzaStore` 추상화시키지 않았다면?(아래 그림처럼)
	
![dependent_pizzastore](https://nobbaggu.github.io/images/designpattern/factorypattern/dependent_pizzastore.jpg){: width="50%" height="50%"}

+ `PizzaStore`에서 모든 피자 객체를 직접 생성하기때문에 모든 종류의 피자에 의존한다.
	+ 피자의 조리법 등이 바뀌면 `PizzaStore`의 코드를 수정해야한다.

+ 의존성 뒤집기로 인해 의존성이 아래와 같이 역전(inversion)된다.
	+ 화살표 방향에 따라 저수준 구성요소(피자 구상 클래스)와 고수준 구성요소(PizzaStore 클래스) 모두 하나의 추상 클래스(Pizza)에만 의존하게 된다.
	+ 이제 `PizzaStore`는 아무것도 신경쓰지 않고 `createPizza()`만 호출하면 구상 클래스의 팩토리 메소드 코드에 따라 자동으로 피자가 튀어나온다.

![independent_pizzastore](https://nobbaggu.github.io/images/designpattern/factorypattern/independent_pizzastore.jpg){: width="50%" height="50%"}

<br>

## 5. 추상 팩토리 ##
----

+ **2**에서 피자 객체 생성부를 따로 Factory 클래스로 분리했던 것이 추상 팩토리이다.

+ 현재는 치즈피자를 만들 때 뉴욕, 시카고 지역별로 다른 클래스로 작성했다.
	+ 지역별로 다른 것은 올라가는 재료 뿐이다. 즉, 변하는 것은 재료이므로 분리 가능하다.
	+ 재료 공장을 추상팩토리로 만들어 지역별로 클래스를 두 번 작성할 필요 없이 통일 시킬 수 있다.
	
<br>
	
~~~ java
public interface IngredientFactory {
    Dough createDough();
    Sauce createSauce();
    Cheese createCheese();
    Veggies[] createViggies();
    Pepperoni createPepperoni();
    Clams createClam();
}
~~~

<br>
	
~~~ java
public class NYPizzaIngredientFactory implements IngredientFactory {
    @Override
    public Dough createDough() {
        return new ThinCrustDough();
    }

    @Override
    public Sauce createSauce() {
        return new MarinaraSauce();
    }

    @Override
    public Cheese createCheese() {
        return new ReggianoCheese();
    }

    @Override
    public Veggies[] createViggies() {
        Veggies[]  veggies = {new Garlic(), new Onion(), new Mushroom(), new RedPepper()};
        return veggies;
    }

    @Override
    public Pepperoni createPepperoni() {
        return new SlicedPepperoni();
    }

    @Override
    public Clams createClam() {
        return new FreshClam();
    }
}
~~~

<br>
	
~~~ java
public class ChicagoPizzaIngredientFactory implements IngredientFactory {
    @Override
    public Dough createDough() {
        return new ThickCrustDough();
    }

    @Override
    public Sauce createSauce() {
        return new TomatoSauce();
    }

    @Override
    public Cheese createCheese() {
        return new MozzarellaCheese();
    }

    @Override
    public Veggies[] createViggies() {
        Veggies[]  veggies = {new Orregano(), new BlackOlive(), new Spinach(), new EggPlant()};
        return veggies;
    }

    @Override
    public Pepperoni createPepperoni() {
        return new SlicedPepperoni();
    }

    @Override
    public Clams createClam() {
        return new FrozenClam();
    }
}
~~~

<br>
	
~~~ java
public class NYPizzaStore extends PizzaStore {

    Pizza pizza = null;
    IngredientFactory ingredientFactory = new NYPizzaIngredientFactory();

    Pizza createPizza(String type) {
        if(type.equals("cheese")) {
            pizza = new CheesePizza(ingredientFactory);
            pizza.setName("New York Style Cheese Pizza");
        } else if(type.equals("veggie")) {
            pizza = new VeggiePizza(ingredientFactory);
            pizza.setName("New York Style Veggie Pizza");
        } else if(type.equals("clam")) {
            pizza = new ClamsPizza(ingredientFactory);
            pizza.setName("New York Style Clams Pizza");
        } else if(type.equals("pepperoni")) {
            pizza = new PepperoniPizza(ingredientFactory);
            pizza.setName("New York Style Pepperoni Pizza");
        }

        return pizza;
    }
}

public class ChicagoPizzaStore extends PizzaStore {
    Pizza pizza = null;
    IngredientFactory ingredientFactory = new ChicagoPizzaIngredientFactory();

    Pizza createPizza(String type) {
        if(type.equals("cheese")) {
            pizza = new CheesePizza(ingredientFactory);
            pizza.setName("New York Style Cheese Pizza");
        } else if(type.equals("veggie")) {
            pizza = new VeggiePizza(ingredientFactory);
            pizza.setName("New York Style Veggie Pizza");
        } else if(type.equals("clam")) {
            pizza = new ClamsPizza(ingredientFactory);
            pizza.setName("New York Style Clams Pizza");
        } else if(type.equals("pepperoni")) {
            pizza = new PepperoniPizza(ingredientFactory);
            pizza.setName("New York Style Pepperoni Pizza");
        }

        return pizza;
    }
}
~~~

+ `NYPizzaStore`와 `ChicagoPizzaStore`가 각각 `IngredientFactory` 인터페이스를 구상한 `NYPizzaIngredientFactory`와 `ChicagoPizzaIngredientFactory` 객체를 받는다.
	+ 전달받은 `IngredientFactory` 객체에 따라 지역에 맞는 재료 생성이 자동으로 된다.

<br>
	
~~~ java
public abstract class Pizza {
    IngredientFactory ingredientFactory;

    String name;

    Dough dough;
    Sauce sauce;
    Cheese cheese;

    Veggies veggies[];
    Pepperoni pepperoni;
    Clams clam;

    abstract void prepare();

    void bake() {
        System.out.println("baking...");
    }

    void cut() {
        System.out.println("cutting...");
    }

    void box() {
        System.out.println("boxing...");
    }

    void setName(String name) {
        this.name = name;
    }

    String getName() {
        return name;
    }

}

public class CheesePizza extends Pizza {

    public CheesePizza(IngredientFactory ingredientFactory) {
        this.ingredientFactory = ingredientFactory;
    }

    //전달받은 재료 공장에 의해 재료가 알아서 준비되므로 신경쓸 필요 없어졌다.
    @Override
    void prepare() {
        System.out.println("preparing " + name);
        dough = ingredientFactory.createDough();
        sauce = ingredientFactory.createSauce();
        cheese = ingredientFactory.createCheese();
    }
}
~~~

+ 각각의 `Pizza` 구상 클래스들은 객체 생성 시 `IngredientFactory` 객체를 전달받는다.
	+ 지역별로 나누지 않고 `CheesePizza` 하나로 클래스가 줄어든다.
	+ 나머지 피자 종류들도 `CheesePizza`와 같다.
	
<br>

## 6. 정리 및 생각 ##
----

+ 팩토리 메소드 패턴과 추상 팩토리 패턴의 차이

둘 모두 객체 생성 부분을 분리하여 클라이언트 코드에서 인터페이스에 의한 프로그래밍을 할 수 있도록 지원해준다는 점은 동일하다. 그러나 팩토리 메소드 패턴은 객체 생성 부분을 클래스에서 구현한다. 즉, 클래스를 확장할 때 팩토리 메소드를 오버라이딩 하여 객체를 만든다. 반면 추상 팩토리 메소드는 객체 구성(composition)을 통해 객체를 만든다. 쉽게 말하자면 상속과는 다르게 특정 기능을 메소드를 통해 구현하는게 아니라 아예 따로 캡슐화해서 해당 기능을 하는 객체를 전달받아서 사용하는 방식이다.

<br>

+ 팩토리 메소드 패턴은 서브클래스를 구현할 때 기능이 정해진다.

+ 추상 팩토리 패턴에서는 연관된 제품군, 행동양식을 만들 때 활용하면 좋다.

+ 두 가지 팩토리 패턴의 목적은 **구상 클래스에 대한 의존성을 줄여 객체간의 느슨한 결합을 유지하는 것이 목표이다. 다른말로는 구상보다는 추상에 의존하게 도와준다.(추상화)**