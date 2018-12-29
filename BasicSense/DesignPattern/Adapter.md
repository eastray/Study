# Adapter Pattern

한 클래스의 인터페이스를 클라이언트에서 사용하고자하는 다른 인터페이스로 변환할 때 사용된다.

어뎁터 패턴을 이용하면 인터페이스 호환성 문제로 같이 쓸 수 없는 문제를 해결할 수 있다.

다른 말로 wrapper라 불린다.

![어뎁터구조](../Image/어뎁터구조2.png)

어뎁터 패턴은 객체 어뎁터와 클래스 어뎁터로 나뉘며, 클래스 어뎁터는 다중 상속이 필요하다. 자바에서는 다중 상속을 할 수 없으므로, extends와 implements 를 사용한다. 

객체 어뎁터의 경우 객제 구성(composition)을 사용하여 어뎁티 뿐만 아니라 그 하위 클래스에 대해서도 어뎁터 역할을 할 수 있다.

클래스 어뎁터의 경우 어뎁티와 타겟 클래스의 서브 클래스로 만드는 대신, 어뎁티를 오버라이드하여 어뎁티의 전체를 구성하지 않아도 된다. 하지만 상속하는 특정 어뎁티에만 해당된다.

```java
/**
 *  신형 맥북은 이더넷을 직접 연결하여 사용 할 수 없다. 이더넷 인터페이스가 맞지 않기 때문이다.
 *  맥북에서 이더넷을 사용하기 위해서는 악세사리가 필요하다.
 */
public class Macbook {

    public void connectableEthernet(){
        System.out.println("connect Connectable");
    }


}
```

```java
// 클래스 어뎁터 사용
public class ObjectAdapter extends Macbook implements Connectable {

    public void connectEthernet(){
        connectableEthernet();
    }
}

// 객체 어뎁터 사용
public class ClassAdapter implements Connectable{

    Macbook macbook = new Macbook();

    @Override
    public void connectEthernet() {
        macbook.connectableEthernet();
    }
}
```

```java
public interface Connectable {

    public abstract void connectEthernet();

}

public class main {
    public static void main(String[] args) {

        // 클래스 어뎁터 사용
        Connectable connectable = new ObjectAdapter();
        connectable.connectEthernet();

        // 객체 어뎁터 사용
        connectable = new ClassAdapter();
        connectable.connectEthernet();
    }
}
```













