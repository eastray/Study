# Decorator

데코레이터 패턴은 서브 클래스 구현을 통해 기능을 유연하게 확장 할 수 있으며, 객체에 추가적인 요건을 동적으로 관리할 수 있다. 

![데코레이터구조](../Image/데코레이터구조.png)

- Decorator는 자신이 구현할 구성요소(Component)와 같은 인터페이스 또는 추상 클래스를 구현한다.
- ConcreteDecoratorA, ConcreteDecoratorB에는 데코레이터가 감싸고 있는 Component 객체를 위한 인스턴스 변수가 있고, 데코레이터는 해당 요소의 상태를 확장할 수 있다.

```
디자인 원칙
- OCP(Open Close Principle)
- 클래스는 확장에 대해 열려있고, 코드 변경에 대해 닫혀 있어야 한다.
```

데코레이션을 통해 기존 코드를 건들지 않으면서, 확장을 통해 새로운 행동을 추가할 수 있다.



```Java
public abstract class Beverage {

    String description="";

    public String getDescription() {
        return this.description;
    }

    public abstract int cost();
}

public abstract class Decorator extends Beverage{

    public abstract String getDescription();

}

public class Espresso extends Beverage{

    public Espresso() {
        this.description = "Espresso";
    }

    @Override
    public int cost() {
        return 300;
    }
}

```

```Java
public class Milk extends Decorator{

    Beverage beverage;

    public Milk(Beverage beverage) {
        this.beverage = beverage;
    }

    @Override
    public String getDescription() {
        return beverage.getDescription() + " Latte";
    }

    @Override
    public int cost() {
        return 300 + beverage.cost();
    }
}
```

```java
public class main {

    public static void main(String[] args) {

        Beverage espresso = new Espresso();
        System.out.println(espresso.getDescription());
        System.out.println("cost: " + espresso.cost());
        System.out.println();

        Beverage latte = new Espresso();
        latte = new Milk(espresso);
        System.out.println(latte.getDescription());
        System.out.println("cost: "+latte.cost());
    }
}
```

