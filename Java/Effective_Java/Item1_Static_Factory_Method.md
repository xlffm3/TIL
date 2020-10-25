## Item 1 : Static Factory Method

> Boolean.java

```java
public static Boolean valueOf(boolean b) {
  return b ? Boolean.TRUE : Boolean.FALSE;
}
```

* 클래스는 생성자와 별도로 정적 팩토리 메소드(Static Factory Method)를 제공할 수 있다.
* boolean 기본 타입 값을 받아 Boolean 객체 참조로 변환해준다.

<br>

## 정적 팩토리 메소드의 장점

* 1. 이름을 가질 수 있다.
  * 생성자에 넘기는 매개변수 및 생성자 자체만으로 반환될 객체 특성을 제대로 설명하기 어렵다.
  * 정적 팩토리는 반환 객체의 특성을 쉽게 묘사한다.
  * 하나의 시그니처로는 생성자를 하나만 만들 수 있다.
    * 파라미터 순서를 다르게 하는 생성자는 의미 파악이 어렵다.
    * 시그니처가 같은 생성자가 여러 개 필요할 때 정적 팩토리 메소드를 사용한다.

    <br>

* 2. 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다.
  * 불변 클래스는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용한다.
  * 불필요한 객체 생성을 피할 수 있다.
  * 생성 비용이 큰 객체가 자주 요청될 때 사용한다.
  * 플라이웨이트 패턴도 이와 비슷한 기법이다.
  * 인스턴스 통제 클래스는 싱글턴 혹은 인스턴스화 불가가 가능하다.
  * 불변 값 클래스에서 동치된 인스턴스가 단 하나뿐임을 보장할 수 있다.
    * ``a == b``일 때만 ``a.equals(b)``가 성립한다.
    * 플라이웨이트 패턴 및 열거 타입이 대표적이다.

    <br>

* 3. 반환 타입의 하위 타입 객체를 반환할 수 있다.
  * API를 만들 때 유연성을 응용하면 구현 클래스를 공개하지 않고도 그 객체를 반환할 수 있어 API를 작게 유지할 수 있다.
  * 인터페이스를 정적 팩토리 메소드의 반환 타입으로 사용하는 인터페이스 기반 프레임워크의 핵심 기술이다.
  * 정적 팩토리 메소드를 사용하는 클라이언트는 얻은 객체를 그 구현 클래스가 아닌 인터페이스만으로 다루게 된다.

  <br>

* 4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
  * 클라이언트는 팩토리가 건네주는 객체가 어느 클래스의 인스턴스인지 알 필요가 없다.
  * EnumSet은 원소의 개수에 따라 다른 하위 인스턴스를 반환한다.

  <br>

* 5. 정적 팩토리 메소드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.
  * JDBC 등 서비스 제공자 프레임워크를 만드는 근간이 된다.
  * 서비스 제공자 프레임워크는 3개의 핵심 컴포넌트로 구성된다.
    * 구현체의 동작을 정의하는 서비스 인터페이스.
    * 제공자가 구현체를 등록할 때 사용하는 제공자 등록 API.
    * 클라이언트가 서비스의 인스턴스를 얻을 때 사용하는 서비스 접근 API.
    * 서비스 제공자 인터페이스라는 네 번째 컴포넌트는 서비스 인터페이스의 인스턴스를 생성하는 팩토리 객체를 설명해준다.
  * 클라이언트는 서비스 접근 API를 사용할 때 원하는 구현체의 조건을 명시한다.

  <br>

## 정적 팩토리 메소드의 단점

* 1. 상속을 하려면 public이나 protected 생성자가 필요하다.
  * 정적 팩토리 메소드만 제공하면 하위 클래스를 만들 수 없다.
* 2. 프로그래머가 찾기 어렵다.
  * 생성자처럼 API 설명에 명확하게 드러나지 않아 인스턴스화 방법을 알아내야 한다.

<br>

## 흔한 명명 방식

* from
  * 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드이다.
* of
  * 여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드이다.
* valueOf
  * from과 of의 더 자세한 버전이다.
* instance 혹은 getInstance
  * 매개변수를 받는다면 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지 않는다.
* create 혹은 newInstance
  * instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
* getType
  * getInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩토리 메소드를 정의할 때 쓴다.
* newType
  * newInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩토리 메소드를 정의할 때 쓴다.
* type
  * getType과 newType의 간결한 버전이다.

<br>

---

## Reference

* Effective Java (Joshua Bloch 저) 2장 Item 1