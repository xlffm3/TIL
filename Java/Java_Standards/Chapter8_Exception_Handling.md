Error & Exception
-----------------

-	컴파일 에러는 컴파일 시 발생하며, 런타임 에러는 실행 시에 발생한다.<br><br>
-	런타임 프로그램 오류는 에러와 예외로 구분된다.<br><br>
-	에러는 프로그램 코드에 의해 수습될 수 없는 오류이며, 예외는 수습될 수 있는 미약한 오류이다.<br><br>
-	예외는 크게 RuntimeException Class와 Exception Class로 나뉜다.<br><br>
-	RuntimeException Class는 프로그래머의 실수에 의해 발생될 수 있는 예외들로, 프로그래밍 요소들과 관계가 깊다.<br><br>
-	Exception Class는 사용자의 동작에 의해 발생하는 경우가 많다.<br><br>
-	RuntimeException Class는 unchecked Exception이며, Exception Class는 checked Exception이다.<br><br>

멀티 catch 블럭
---------------

```java
try{
  ...
}catch(ExceptionA | ExceptionB e){
  e.printStackTrace();
  e.methodA(); //컴파일 에러

  if (e instanceof ExceptionA){
    ExceptionA e1 = (ExceptionA) e;
    e1.methodA();
  }
}
```

-	코드의 중복을 줄일 수 있으나, 나열된 예외 클래스가 조상과 자손의 관계에 있다면 컴파일 에러가 발생한다.<br><br>
-	참조 변수 e는 실제로 어떤 예외인지 알 수 없기 때문에, 예외 클래스들의 공통 분모인 조상 예외 클래스에 선언된 멤버만 사용할 수 있다.<br><br>
-	필요하다면 해당 참조 변수 e가 어떤 예외인지 확인하여 사용한다.<br><br>

예외 발생시키기
---------------

```java
Exception e = new Exception("고의 발생");
throw e;
```

-	Exception 인스턴스 생성자에 문자열을 넣어주면, `getMessage()` 메서드로 에러 메시지를 얻을 수 있다.<br><br>

메서드에 예외 선언하기
----------------------

```java
void method() throws Exception1{}
```

-	메서드 내에 발생할 수 있는 예외를 적어주면 된다.<br><br>
-	자신을 호출한 메서드에게 예외를 전달하여 예외 처리를 떠맡기는 역할이다.<br><br>

finally
-------

-	try 블럭 혹은 catch 블럭에서 return이 실행되더라도, finally 블럭의 문장들이 먼저 실행된 후 메서드가 종료된다.<br><br>

try-with-resources
------------------

```java
try(FileInputStream fis = new FileInputStream("apa.txt");
DataInputStream dis = new DataInputStream(fis)){
    ...
} catch (Exception e) {
}
```

-	클래스 중에서 사용 후 꼭 닫아줘야 하는 것들은, finally 블럭에서 예외 처리가 필수이다.<br><br>
-	그러나 코드가 복잡해지며, finally 블럭에서 예외가 발생하면 try 블럭의 예외가 무시된다.<br><br>
-	try-with-resources 문법은 try 블럭을 벗어나는 순간 자동으로 `close()` 메서드가 호출되어 자원이 반환된다.<br><br>

사용자 정의 예외
----------------

```java
class MyException extends Exception{
  MyException(String msg){
    super(msg);
  }
}
```

-	알맞은 예외 클래스를 선택하며, 에러 메시지와 코드 등 생성자와 메서드를 적절히 오버라이딩한다.<br><br>
-	최근에는 RuntimeException Class를 상속받는데, unchecked Exception은 예외 처리를 강요하지 않기 때문이다.<br><br>

exception re-throwing
---------------------

-	하나의 예외에 대해 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두에서 처리해줘야 할 작업이 있을 때 사용된다.<br><br>
-	반환값이 있는 return 문의 경우, catch 블럭에도 return 문이 필요하다.<br><br>
-	그러나 catch 블럭에서 예외 던지기를 통해 호출한 메서드로 예외를 전달하는 경우, return 문이 없어도 된다.<br><br>
-	finally 블럭 내에 return 문이 존재하면, 최종적으로 finally 블럭에 명시된 값이 반환된다.<br><br>

---

Reference
---------

-	Java의 정석 (남궁성 저) Chapter 8
