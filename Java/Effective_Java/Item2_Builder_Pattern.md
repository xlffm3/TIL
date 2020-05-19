## 기존 객체 생성 방식의 문제점

* 정적 팩토리 또한 선택적 매개변수가 많을 때 적절히 대응하기 어렵다.
* 생성자 매개변수가 많은데, 일부는 필수이고 일부는 선택적인 경우 : 영양 정보 클래스.
  * 점층적 생성자 패턴을 주로 사용했다.
  * 원하는 매개변수를 모두 포함한 생성자 중 가장 짧은 것을 골라 호출하면, 생성자 내부에서 ``this()`` 생성자를 통해 다른 생성자를 점층적으로 호출하는 방식이다.
  * 설정하기 원치 않은 매개변수까지 포함되거나, 코드 자체가 복잡해진다.
* 그렇다면 Java Beans 패턴은?
  * 매개변수가 없는 생성자로 객체를 만든 후, setter 메소드를 호출하는 방식이다.
  * 객체 하나를 만드는데 여러 메소드를 호출해야 하며, 완전히 생성되기 전까지는 일관성이 무너진 상태가 된다.
  * 디버깅이 어려우며, 불변 클래스로 만들 수 없어 스레드 안정성을 얻으려면 추가 작업이 필요하다.

<br>

## Builder Pattern

```java
public class Information {
  private final int a;
  private final int b;
  private final int c;
  private final int d;

  public static class Builder {
    //필수 매개변수
    private final int a;

    //선택 매개변수
    private int b = 0;
    private int c = 0;

    public Builder(int a) {
      this.a = a;
    }

    public Builder b(int val) {
      b = val;
      return this;
    }

    public Builder c(int val) {
      c = val;
      return this;
    }

    public Information build() {
      return new Information(this);
    }
  }

  private Information(Builder builder) {
    a = builder.a;
    b = builder.b;
    c = builder.c;
  }
}

Information Information = new Information.Builder(15).b(98).c(333).build();
```

* 필수 매개변수만으로 생성자 혹은 정적 팩토리를 호출해 빌더 객체를 얻는다.
* 빌더 객체가 제공하는 일종의 setter 메소드로 선택 매개변수들을 설정한다.
* 마지막으로 매개변수가 없는 ``build()`` 메소드를 호출해 보통은 불변인 객체를 얻는다.
* 빌더는 생성할 클래스 안에 정적 멤버 클래스로 만들어둔다.
* Information 클래스는 불변이며 모든 매개변수의 기본값들을 한 곳에 모아 두었다.
* 메소드 체이닝을 사용하며, 파이썬 및 스칼라 등의 **명명된 선택적 매개변수** 를 흉내낸 것이다.
* 생성자에서 여러 매개변수에 걸친 불변식을 검사한다.
  * 매개변수가 잘못된 경우 IllegalArgumentException을 던지면 된다.
  * 불변은 어떠한 변경도 허용하지 않는다.
  * 불변식은 프로그램이 실행되는 동안, 혹은 정해진 기간 동안 반드시 만족해야 하는 조건이다.
  * 가변 객체에도 불변식이 존재할 수 있으며, 불변은 불변식의 극단적인 예이다.

<br>

## ETC

* 계층적으로 설계된 클래스와 함께 쓰기 좋다.
  * P.19 셀프 타입 관용구를 응용한 계층적 빌더 예시를 참조할 것.
* 빌더를 이용하면 가변인수 매개변수를 여러 개 사용할 수 있다.
* 빌더 하나로 여러 객체를 순회하면서 만들 수 있고, 넘기는 매개변수에 따라 다른 객체를 만들 수 있다.
* 객체마다 부여되는 일련번호와 같은 특정 필드는 빌더가 알아서 채우도록 할 수 있다.
* 그러나 빌더를 만드는 비용이나 코드의 장황함 등 단점도 존재한다.
  * 매개변수가 4개 이상은 되어야 값어치를 한다.
  * API는 시간이 지날수록 매개변수가 많아지는 경향이 있다.
  * 매개변수 중 다수가 필수가 아니거나 같은 타입이면 빌더 패턴이 더 낫다.
  * 점층적 생성자보다 코드가 읽고 쓰기 간결하며, 자바 빈즈보다 훨씬 안전하다.
* Lombok!

<br>

---

## Reference

* Effective Java (Joshua Bloch 저) 2장 Item 2
