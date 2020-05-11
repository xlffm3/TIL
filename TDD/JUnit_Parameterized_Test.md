## @ParameterizedTest

```java
testCompile("org.junit.jupiter:junit-jupiter-params:5.4.2")
```

* 단일 테스트 메소드를 여러 파라미터로 여러번 수행하는 기능이다.
* junit-jupiter-params 의존성을 주입받는다.

<br>

## 사용법

```java
public class Numbers {
    public static boolean isOdd(int number) {
        return number % 2 != 0;
    }
}
```

* 위와 같은 메소드에 대한 테스트를 진행하고자 한다.

<br>

```java
@ParameterizedTest
@ValueSource(ints = {1, 3, 5, -3, 15, Integer.MAX_VALUE})
void isOdd_ShouldReturnTrueForOddNumbers(int number) {
    assertTrue(Numbers.isOdd(number));
}
```

* @ValueSource 배열에 포함된 값들을 매 테스트마다 파라미터에 넣어 총 6번의 테스트를 수행한다.

<br>

## Null and Empty Values

```java
@ParameterizedTest
@NullSource
void isBlank_ShouldReturnTrueForNullInputs(String input) {
    assertTrue(Strings.isBlank(input));
}
```

* @NullSource를 통해 single null value를 파라미터로 보낼 수 있다.
* 원시형 자료형은 null 값을 허용하지 않기 때문에 사용이 불가능하다.

<br>

```java
@ParameterizedTest
@EmptySource
void isBlank_ShouldReturnTrueForEmptyStrings(String input) {
    assertTrue(Strings.isBlank(input));
}
```

* @EmptySource는 빈 argument를 파라미터로 보낸다.
* 해당 파라미터 소스는 컬렉션 타입 및 배열 타입에도 빈 argument를 보낼 수 있다.

<br>

```java
@ParameterizedTest
@NullAndEmptySource
void isBlank_ShouldReturnTrueForNullAndEmptyStrings(String input) {
    assertTrue(Strings.isBlank(input));
}
```

<br>

```java
@ParameterizedTest
@NullAndEmptySource
@ValueSource(strings = {"  ", "\t", "\n"})
void isBlank_ShouldReturnTrueForAllTypesOfBlankStrings(String input) {
    assertTrue(Strings.isBlank(input));
}
```

* 둘을 동시에 사용하는 어노테이션도 제공한다.

<br>

## Enum

```java
@ParameterizedTest
@EnumSource(Month.class) // passing all 12 months
void getValueForAMonth_IsAlwaysBetweenOneAndTwelve(Month month) {
    int monthNumber = month.getValue();
    assertTrue(monthNumber >= 1 && monthNumber <= 12);
}
```

* Enum에서 각각 다른 값들을 사용해 테스트를 진행할 때 @EnumSource 어노테이션을 사용한다.

<br>

```java
@ParameterizedTest
@EnumSource(value = Month.class, names = {"APRIL", "JUNE", "SEPTEMBER", "NOVEMBER"})
void someMonths_Are30DaysLong(Month month) {
    final boolean isALeapYear = false;
    assertEquals(30, month.length(isALeapYear));
}
```

* 혹은 직접 값을 명시함으로써 일부 Enum들을 필터링할 수 있다.

<br>

```java
@ParameterizedTest
@EnumSource(
  value = Month.class,
  names = {"APRIL", "JUNE", "SEPTEMBER", "NOVEMBER", "FEBRUARY"},
  mode = EnumSource.Mode.EXCLUDE)
void exceptFourMonths_OthersAre31DaysLong(Month month) {
    final boolean isALeapYear = false;
    assertEquals(31, month.length(isALeapYear));
}
```

* names들이 enum 값들과 매치되는 기능이 기본값이지만, mode를 EXCLUDE로 조정하여 해제할 수 있다.

<br>

```java
@ParameterizedTest
@EnumSource(value = Month.class, names = ".+BER", mode = EnumSource.Mode.MATCH_ANY)
void fourMonths_AreEndingWithBer(Month month) {
    EnumSet<Month> months =
      EnumSet.of(Month.SEPTEMBER, Month.OCTOBER, Month.NOVEMBER, Month.DECEMBER);
    assertTrue(months.contains(month));
}
```

* names 값에 정규식을 응용할 수 있다.

<br>

## CSV Literals

```java
@ParameterizedTest
@CsvSource({"test,TEST", "tEst,TEST", "Java,JAVA"})
void toUpperCase_ShouldGenerateTheExpectedUppercaseValue(String input, String expected) {
    String actualValue = input.toUpperCase();
    assertEquals(expected, actualValue);
}
```

* 테스트를 할 때 입력값과 기대값을 동시에 기재하여 사용하는 방법이다.
* 파라미터로 오는 값이 항상 문자열일 필요는 없다.

<br>

```java
@ParameterizedTest
@CsvSource(value = {"test:test", "tEst:test", "Java:java"}, delimiter = ':')
void toLowerCase_ShouldGenerateTheExpectedLowercaseValue(String input, String expected) {
    String actualValue = input.toLowerCase();
    assertEquals(expected, actualValue);
}
```

* delim 기능 또한 예제처럼 제공한다.

<br>

## Method

```java
private static Stream<Arguments> provideStringsForIsBlank() {
    return Stream.of(
      Arguments.of(null, true),
      Arguments.of("", true),
      Arguments.of("  ", true),
      Arguments.of("not blank", false)
    );
}

@ParameterizedTest
@MethodSource("provideStringsForIsBlank")
void isBlank_ShouldReturnTrueForNullOrBlankStrings(String input, boolean expected) {
    assertEquals(expected, Strings.isBlank(input));
}
```


* 복잡한 객체를 테스트할 때는 일반적인 방법으로는 다소 제약이 많다.
* @Method 어노테이션을 이용하여 메소드 명을 기입하고, 리턴되는 스트림을 input으로 받아 expected 값과 비교한다.
* 리스트와 같은 다른 컬렉션 인터페이스도 리턴할 수 있다.

<br>

```java
@ParameterizedTest
@MethodSource // hmm, no method name ...
void isBlank_ShouldReturnTrueForNullOrBlankStringsOneArgument(String input) {
    assertTrue(Strings.isBlank(input));
}

private static Stream<String> isBlank_ShouldReturnTrueForNullOrBlankStringsOneArgument() {
    return Stream.of(null, "", "  ");
}
```

* 메소드 명을 기입하지 않으면 JUnit이 테스트 메소드 이름과 일치하는 메소드를 탐색해 사용한다.

<br>

---


Reference
---------

-	[Guide to JUnit 5 Parameterized Tests](https://www.baeldung.com/parameterized-tests-junit-5)
