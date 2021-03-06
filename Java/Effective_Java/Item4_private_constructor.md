## 인스턴스화를 막기 위한 private 생성자

```java
public class Utility {
  private Utility() {
    // 인스턴스화 방지용
    throw new AssertionError();
  }
}
```

* 유틸리티 클래스는 OOP와 방향이 같지는 않지만 유용하다.
  * Arrays나 Math 및 Collections 등의 클래스는 유용한 정적 메소드를 모아놓았다.
  * Java 8부터는 인터페이스에도 정적 메소드를 담을 수 있다.
  * final 클래스는 상속할 수 없기 때문에, 이와 관련한 메소드들을 모아놓을 때 사용한다.
* 정적 메소드 및 정적 필드만을 담은 클래스를 사용할 때 private 생성자를 만든다.
  * 인스턴스로 만들어 쓰려고 설계한 게 아니기 때문이다.
  * 컴파일러는 생성자를 명시하지 않으면 public 기본 생성자를 추가한다.
* 추상 클래스로 만드는 것으로는 인스턴스화를 막을 수 없다.
  * 하위 클래스를 만들어 인스턴스화할 수 있기 때문이다.
  * 상속해서 쓰라는 뜻으로 오해할 수 있다.
* 위의 예제는 클래스 안에서 실수로 생성자를 호출하지 않도록 에러를 던진다.
  * 코드가 직관적이지 않으니 주석을 달도록 한다.
* 해당 방식은 상속을 불가능하게 하는 효교도 있다.

<br>

---

## Reference

* Effective Java (Joshua Bloch 저) 2장 Item 4
