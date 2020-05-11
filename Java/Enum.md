## Enum

```java
enum Color {
  RED, GREEN, BLUE
}

public class EnumExample {
    public static final String RED = "RED";
    public static final String BLUE = "BLUE";

    public static void main (String[] args) {
        String color1;

        color1 = EnumExampl.RED;
        color1 = EnumExample.BLUE;
        color1 = "BLACK";
        //상수 값이 아닌 것을 할당받아도 에러가 나지 않는 이슈        

        Color color2;
        color2 = Color.RED;
        color2 = Color.BLUE;
        color2 = "BLACK";
        //컴파일 에러 발생         
    }     
}
```

* Enum은 열거형으로서 서로 연관있는 상수의 그룹을 나타낼 때 사용한다.
* 기존에 상수를 정의하는 방법 중 하나가 ``final static`` 을 붙이는 것인데, 가독성 및 상수 중복으로 인한 에러 등의 이슈가 존재한다.
* Enum의 장점
  * 코드가 단순해지며 가독성이 좋아진다.
  * 인스턴스 생성과 상속을 방지하여 상수값의 타입 안정성이 보장된다.
  * enum class를 사용해 새로운 상수들의 타입을 정의함으로써 정의한 타입이외의 타입을 가진 데이터값을 컴파일시 체크한다.
  * 구현 의도가 enum을 통해 열거임을 명시할 수 있다.

<br>

## Enum의 중요점

```java
class Color
{
     public static final Color RED = new Color();
     public static final Color BLUE = new Color();
     public static final Color GREEN = new Color();
}
```

* Color라는 enum은 내부적으로 위와 같이 정의된다.
* 모든 enum 상수들은 객체 타입의 enum이다.
* 내부적으로 public static final로 정의되기 때문에 자식 enum을 가질 수 없다.
* 생성자는 private 접근 범위를 가진다.
* 모든 enum들은 내부적으로 java.lang.Enum 클래스에 의해 상속된다.
* 다른 클래스를 상속받을 수는 없지만, 인터페이스는 구현이 가능하다.

<br>

```java
enum Type{ // abstract class
    ADD,    // public static final class ADD extends Type{}
    SUB;    // public static final class SUB extends Type{}
}
```

* enum은 추상 클래스이며 각 열거형 변수는 실제로는 변수가 아니라 enum 타입을 상속받은 하위 클래스이다.
* 따라서 enum 클래스에 static 메소드, abstract 메소드, 일반 메소드를 구현할 수 있다.

<br>

```java
public enum Operator implements OperatorService {
    PlUS("+") {
        @Override
        public int operate(int firstNumber, int nextNumber) { return firstNumber + nextNumber; }
    },
    MINUS("-") {
        @Override
        public int operate(int firstNumber, int nextNumber) {
            return firstNumber - nextNumber;
        }
    },
    MULTIPLE("*") {
        @Override
        public int operate(int firstNumber, int nextNumber) { return firstNumber * nextNumber; }
    },
    DIVIDE("/") {
        @Override
        public int operate(int firstNumber, int nextNumber) {
            if (nextNumber == 0)
                throw new ArithmeticException();
            return firstNumber / nextNumber;
        }
    };
    private final String value;

    private Operator(String value) {
        this.value = value;
    }

    String getValue() { return this.value; }

    static Operator getOperator(String inputOperator) {
    return (Operator)Arrays.stream(Operator.values())
            .filter(target -> target.getValue().equals(inputOperator))
            .findFirst()
            .orElseThrow(IllegalArgumentException::new);
    }
}
```

* 사칙연산과 관련된 enum을 정의한 예제이다.
* 인터페이스를 통해 각 하위 열거형 변수들이 사칙연산 메소드를 구현했다.
* enum에서 생성자를 구현한다면, 각 열거형 변수의 옆에 괄호를 추가해 생성자를 표기해준다.

<br>

## 관련 메소드

```java
enum Color {
    RED, GREEN, BLUE;
}

public class Test
{
    public static void main(String[] args)
    {
        Color arr[] = Color.values();

        for (Color col : arr) {
            System.out.println(col + " at index " + col.ordinal());
        }

        System.out.println(Color.valueOf("RED"));
    }
}
/*
RED at index 0
GREEN at index 1
BLUE at index 2
RED
*/
```

* ``values()`` 메소드는 enum안에 존재하는 모든 값들을 배열로 반환한다.
* ``ordinal()`` 메소드를 사용하여 배열 인덱스처럼 각 enum 상수 인덱스를 찾을 수 있다.
* ``valueOf()`` 메소드는 존재한다면 특정 enum 상수의 스트링 값을 반환한다.

<br>

---


Reference
---------

*	[자바의 enum](https://velog.io/@pop8682/Enum-27k067ns4a)
* [Java: enum의 뿌리를 찾아서...](http://www.nextree.co.kr/p11686/)
* [[Java] enum 이란?](https://limkydev.tistory.com/50)
