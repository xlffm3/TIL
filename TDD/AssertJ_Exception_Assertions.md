## AssertJ Exception Assertions

* 예외가 발생해야 테스트가 가능한 경우, AssertJ의 기능을 활용한다.

<br>

## assertThatThrownBy()

```java
assertThatThrownBy(() -> {
    List<String> list = Arrays.asList("String one", "String two");
    list.get(2);
}).isInstanceOf(IndexOutOfBoundsException.class)
  .hasMessageContaining("Index: 2, Size: 2");

//Method
.hasMessage("Index: %s, Size: %s", 2, 2)
.hasMessageStartingWith("Index: 2")
.hasMessageContaining("2")
.hasMessageEndingWith("Size: 2")
.hasMessageMatching("Index: \\d+, Size: \\d+")
.hasCauseInstanceOf(IOException.class)
.hasStackTraceContaining("java.io.IOException");
```

* 예외를 유발하는 부분들이 람다식 표현에 포함된다.
* 메소드 체이닝이 가능하다.

<br>

## assertThatExceptionOfType

```java
assertThatExceptionOfType(IndexOutOfBoundsException.class)
  .isThrownBy(() -> {
      // ...
}).hasMessageMatching("Index: \\d+, Size: \\d+");
```

* 위 방법과 유사하지만, 예외 클래스가 두괄식으로 제시되어 더 직관적으로 예외 확인이 가능하다.

<br>

## assertThatIOException and Other Common Types

```java
assertThatIOException().isThrownBy(() -> {
    // ...
});
```

* AssertJ는 자주 발생하는 Exception의 Wrapper 클래스를 제공한다.
  * assertThatIllegalArgumentException()
  * assertThatIllegalStateException()
  * assertThatIOException()
  * assertThatNullPointerException()

<br>

## Exception과 Assertion의 분리

```java
Throwable thrown = catchThrowable(() -> {
    // ...
});

// then
assertThat(thrown)
  .isInstanceOf(ArithmeticException.class)
  .hasMessageContaining("/ by zero");
```

* When과 Then 로직을 나누어서 구현하는 대안도 있다.

<br>

---


Reference
---------

*	[AssertJ Exception Assertions](https://www.baeldung.com/assertj-exception-assertion)
* [Introduction to AssertJj](https://www.baeldung.com/introduction-to-assertj)
