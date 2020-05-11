Calendar & Date
---------------

```java
Calendar cal = Calendar.getInstance();
Date date = new Date();
date = new Date(cal.getTimeInMillis());
cal.setTime(date);
```

-	Date는 Claendar의 등장으로 대부분의 메서드가 deprecated 되었다.<br><br>
-	Calendar Class는 추상 클래스이기 때문에 메서드를 통해 인스턴스를 생성한다.<br><br>
-	두 객체간의 형 변환이 요구되는 메서드들이 많다.<br><br>

DecimalFormat
-------------

```java
double number = 1234567.89;
DecimalFormat df = new DecimalFormat("#.#E0");
String result = df.format(number);
```

-	숫자를 형식화 하는데 사용하며, 패턴은 Java API 문서에 정리되어 있다.<br><br>

SimpleDateFormat
----------------

```java
SimpleDateFormat sd = new SimpleDateFormat("yyyy-mm-dd");
Date today = new Date();
String result = sd.format(today);
```

-	Date나 Calendar 객체를 원하는 형식으로 출력하는데 자주 사용된다.<br><br>

ChoiceFormat
------------

```java
double[] limits = {60, 70, 80, 90, 100};
String[] grades = {"A", "B", "C", "D", "E"};
int[] scores = {...};
ChoiceFormat form = new ChoiceFormat(limits, grades);
form.format(scores[i]);
```

-	ChoiceFormat은 특정 범위에 속하는 값을 문자열로 변환해준다.<br><br>
-	패턴 구분자를 통해 경계값을 포함하거나 배제할 수 있다.<br><br>

MessageFormat
-------------

```java
String msg = "Name : {0}, Tel : {1}";
Object[] arguments = {"이지바", "010-4444-4444"};
String result = MessageFormat.format(msg, arguments);
```

-	데이터를 정해진 양식에 맞게 출력한다.<br><br>

java.time 패키지의 핵심 클래스
------------------------------

```java
LocalDate date = LocalDate.now();
date = LocalDate.of(2015, 11, 23);
```

-	멀티 쓰레드 환경에서는 동시에 여러 쓰레드가 같은 객체에 접근할 수 있기 때문에, 변경 가능한 객체는 데이터가 잘못될 가능성이 있다.<br><br>
-	따라서 날짜나 시간과 관련된 클래스는 String처럼 불변성을 갖고, 기존의 객체를 변경하기 보다는 변경된 새로운 객체를 반환한다.<br><br>
-	날짜를 표현할 때는 LocalDate, 시간을 표현할 때는 LocalTime, 날짜와 시간을 동시에 표현할 때는 LocalDateTime 클래스를 사용한다.<br><br>
-	여기에 시간대까지 다뤄야 한다면, ZonedDateTime 클래스를 이용한다.<br><br>
-	`of()` 혹은 `now()` 연산자로 객체를 생성한다.<br><br>
-	두 날짜간의 차이는 Period로, 시간의 차이는 Duration으로 표현한다.<br><br>

DateTimeFormatter
-----------------

-	날짜와 시간을 원하는 형식으로 파싱하는데 사용하는 클래스로서, 직접 형식을 지정하거나 상수로 정의된 형식을 사용할 수 있다.<br><br>

---

Reference
---------

-	Java의 정석 (남궁성 저) Chapter 10
