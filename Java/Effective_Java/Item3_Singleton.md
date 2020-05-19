## Singleton

* 싱글톤이란 인스턴스를 오직 하나만 생성할 수 있는 클래스이다.
  * 무상태 객체나 설계상 유일해야 하는 시스템 컴포넌트 등이 대표적이다.
* 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트의 테스트가 어렵다.
* 타입을 인터페이스로 정의한 다음, 그 인터페이스를 구현해서 만든 싱글턴이 아니라면 mock 구현이 힘들다.

<br>

## public static final 필드 방식

```java
public class Elvis {
  public static final Elvis INSTANCE = new Elvis();

  private Elvis() {...}
}
```

* private 생성자는 Elvis.INSTANCE를 초기화할 때 딱 한 번만 호출된다.
* public 및 protected 생성자가 없으므로 해당 인스턴스가 전체 시스템에서 하나뿐임이 보장된다.
  * 리플렉션 API인 AccessibleObject.setAccessible을 사용해 private 생성자를 호출할 수 있다.
  * 공격을 방어하려면 생성자를 수정하여 두 번째 객체가 생성되려 할 때 예외를 던지게 한다.
* public static 필드가 final이니 클래스가 싱글턴임을 API에 명백히 드러낼 수 있는 코드이다.

<br>

## 정적 팩토리 방식

```java
public class Elvis {
  private static final Elvis INSTANCE = new Elvis();

  private Elvis() {...}
  public static Elvis getInstance() { return INSTANCE; }
}
```

* 항상 같은 객체의 참조를 반환하므로 제 2의 Elvis 인스턴스란 만들어지지 않는다.
  * 리플렉션 예외는 적용된다.
* API를 바꾸지 않고도 싱글톤이 아니게 변경할 수 있다.
  * 팩토리 메소드가 호출하는 스레드별로 다른 인스턴스를 반환하게 만들 수 있다.
* 정적 팩토리를 제너릭 싱글톤 팩터리로 만들 수 있다.
  * 정적 팩토리의 메서드 참조를 공급자(supplier)로 사용할 수 있다.
  * ``Elvis::getInstance`` 를 ``Supplier<Elvis>`` 로 사용하는 방식이다.

<br>

## 직렬화 문제

```java
private Object readResolve() {
  return INSTANCE;
}
```

* Serializable을 구현하는 것으로는 직렬화가 부족하다.
* 모든 인스턴스 필드를 일시적이라고 선언하고, ``readResolve()`` 메소드를 제공해야 한다.
  * 이렇게 하지 않으면 역직렬화할 때마다 새로운 인스턴스가 생성된다.
* 예시 메소드를 통해 가짜 Elvis는 가비지 컬렉터에 맡기고 진짜 Elvis만 반환한다.

<br>

## Enum 방식

```java
public enum Elvis {
  INSTANCE;
}
```

* public 필드 방식과 비슷하지만 더 간결하게 직렬화가 가능하다.
* 복잡한 직렬화 및 리플렉션 공격에서도 제 2의 인스턴스가 생기는 일을 막아준다.
* **대부분 상황에서 원소가 하나 뿐인 열거 타입이 싱글턴을 만드는 가장 좋은 방법이다.**
  * 단, 만드려는 싱글턴이 Enum 외의 클래스를 상속해야 한다면 이를 사용할 수 없다.
  * 인터페이스를 구현하도록 선언할 수는 있다.

<br>

---

## Reference

* Effective Java (Joshua Bloch 저) 2장 Item 3
