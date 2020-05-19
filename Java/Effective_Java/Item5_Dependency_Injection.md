## 유연하지 않고 테스트하기 어려운 예시

```java
public class SpellChecker {
  private static final Lexicon dictionary = ...;

  private SpellChecker() {}
  public static void testMethod() { ... }
  public static void testMethod2() { ... }
}

//혹은

public class SpellChecker {
  private final Lexicon dictionary = ...;

  private SpellChecker(...) {}

  public static SpellChecker INSTANCE = new SpellChecker(...);
}
```

* 정적 유틸리티나 싱글톤 방식 모두 유연하지 않고 테스트하기 어렵다.
* 두 방식은 모두 사전을 단 하나만 사용한다고 가정했다는 점이 문제다.
  * 실전에는 언어별, 어휘용, 테스트용 사전 등이 존재한다.
* SpellChecker 클래스가 여러 사전을 사용하기 위해, 필드에서 final 한정자를 제거하고 setter 메소드를 추가할 수 있다.
  * 멀티스레드 환경에서 사용할 수 없다.
  * **사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글톤 방식이 적합하지 않다.**

<br>

## 의존 객체 주입

```java
public class SpellChecker {
  private final Lexicon dictionary;

  public SpellChecker(Lexicon dictionary) {
    this.dictionary = Objects.requiredNonNull(dictionary);
  }
}
```

* 클래스가 여러 자원 인스턴스를 지원해야 하며, 클라이언트가 원하는 자원(dictionary)를 사용해야 한다.
* 인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨준다.
* dictionary라는 딱 하나의 자원을 사용하지만, 자원이 몇 개든 의존 관계가 어떻든 상관없이 잘 동작한다.
* 또한 불변을 보장하여, 같은 자원을 사용하려는 여러 클라이언트가 의존 객체들을 공유할 수 있다.
* 의존 객체 주입은 생성자, 정적 팩토리, 빌더 패턴 모두에 똑같이 응용할 수 있다.
* 유연성 및 테스트 용이성을 개선해주지만, 의존성이 수 천개나 되는 큰 프로젝트에서는 코드가 어지러워진다.
  * Spring 등 의존 객체 주입 프레임웍은 이를 해소해준다.
* 클래스가 내부적으로 하나 이상의 자원에 의존하고, 그 자원이 클래스 동작에 영향을 준다면 싱글톤과 정적 유틸 클래스 사용을 지양한다.
  * 이 자원들을 클래스가 직접 만들게 해서도 안 된다.
  * 필요한 자원 혹은 그 자원을 만들어주는 팩토리를 생성자나 정적 팩토리 및 빌더에 넣어준다.

<br>

## 자원 팩토리

```java
Mosaic create(Supplier<? extends Tile> tileFactory) { ... }
```

* 팩토리란 호출할 때마다 특정 타입의 인스턴스를 반복해서 만들어주는 객체이다.
  * 팩토리 메소드 패턴을 구현한 것이다.
  * Supplier<T> 인터페이스가 그 예이다.
* Supplier<T>를 입력으로 받는 메소드는 일반적으로 한정적 와일드 카드 타입을 사용해 팩토리의 타입 매개변수를 제한해야 한다.
* 이 방식을 사용해 클라이언트는 자신이 명시한 타입의 하위 타입이라면 무엇이든 생성할 수 있는 팩토리를 넘길 수 있다.
* 위 예제는 팩토리가 생성한 타일들로 구성된 모자이크를 만드는 메소드이다.

<br>

---

## Reference

* Effective Java (Joshua Bloch 저) 2장 Item 5
