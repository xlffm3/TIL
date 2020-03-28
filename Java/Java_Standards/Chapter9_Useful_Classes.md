Object Class
------------

-	`equals()` 메서드는 두 객체의 같고 다름을 참조 변수의 값으로 판단한다.<br><br>
-	`hashCode()` 메서드는 데이터가 저장된 위치를 알려주는 해시 코드를 반환한다.<br><br>
-	상황에 따라서 이러한 메서드를 알맞게 오버라이딩해야 원하는 결과를 얻을 수 있다.<br><br>

공변 반환 타입
--------------

-	오버라이딩할 때 조상 메서드의 반환 타입을 자손 클래스의 타입으로 변경을 허용하는 것이다.<br><br>
-	번거로운 형 변환을 줄일 수 있다.<br><br>

얕은 복사와 깊은 복사
---------------------

-	`clone()` 메서드는 단순히 객체에 저장된 값을 복사할 뿐, 객체가 참조하고 있는 객체까지 복사하지 않는다.<br><br>
-	따라서 원본을 변경하면 복사본도 영향 받는다.<br><br>
-	깊은 복사는 원본이 참조하고 있는 객체까지 복사한다.<br><br>

getClass()
----------

```java
Class obj = new Card().getClass();
Class obj = Card.class;
Class obj = Class.forName("Card");
```

-	해당 메서드는 자신이 속한 클래스의 Class 객체를 반환하는 메서드다.<br><br>
-	Class 객체는 클래스의 모든 정보를 담고 있으며, 클래스 당 1개만 존재한다.<br><br>
-	클래스 파일이 클래스 로더에 의해서 메모리에 올라갈 때 Class 객체가 생성된다.<br><br>
-	File 형태로 저장된 클래스를 읽어 Class 클래스에 정의된 형식으로 변환하는 것이다.<br><br>
-	Class 객체를 이용해 객체를 생성하거나 메서드를 호출하는 등 동적인 코드 작성이 가능하다.<br><br>

String Class
------------

```java
String str1 = "abc";
String str2 = new String("abc");
```

-	String class는 문자열을 내부적으로 문자형 배열에 저장한다.<br><br>
-	문자열에 연산이 발생하면, 새로운 String 인스턴스를 반환한다.<br><br>
-	new 키워드가 아닌 방법으로 String 클래스를 생성하면, 문자열 리터럴은 컴파일 되면서 JVM 내 constant pool에 한 번만 저장된다.<br><br>
-	이 후, constant pool에 저장된 문자열 리터럴을 공유하여 참조한다.<br><br>

빈 문자열
---------

-	Java는 길이가 0인 배열을 허용하기 때문에, 빈 문자열이 사용 가능하다.<br><br>

StringBuffer & StringBuilder
----------------------------

-	StringBuffer는 멀티 쓰레드 환경에 안전하도록 동기화되었으며, 멀티 쓰레드프로그램이 아니면 성능이 낮아진다.<br><br>
-	따라서 쓰레드의 동기화를 뺀 StringBuilder가 추가되었으며, 사용법은 두 메서드 모두 동일하다.<br><br>
-	문자열을 저장하고 편집하기 위한 공간인 버퍼를 생성 및 사용하기 때문에, 문자열 연산이 발생해도 String과 달리 내용이 변경된다.<br><br>

Wrapper Class
-------------

-	Autoboxing과 Unboxing은 기본형과 참조형 간의 형변환과 연산에 필요한 구문을 컴파일러가 자동으로 추가해주는 것을 의미한다.<br><br>

Objects Class
-------------

-	Object Class와 다르게 `compare()` 메서드로 등가비교가 가능하다.<br><br>
-	Objects Class의 `equals()` 메서드는 null check가 필요없다.<br><br>
-	`deepEquals()` 메서드는 다차원 배열의 비교가 가능하다.<br><br>

---

Reference
---------

-	Java의 정석 (남궁성 저) Chapter 9
