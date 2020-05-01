Null-Safety
-----------

> Test.java

```java
public class Test {
	String id;

	@NonNull
	public String helloId(@NonNull Stirng id) {
		return "hello " + id;
	}
}
```

-	IDE의 지원을 받아 컴파일 시점에 최대한 NullPointerException을 방지하는 것이 목적이다.<br><br>
-	IDE에 부수적인 설정을 해야 IDE 상에서 Warning을 인식할 수 있다.<br><br>
-	패키지 레벨에서 @NonNull을 설정하고 Null이 발생할 수 있는 부분만 @Nullable을 설정하는 등의 코딩이 가능하다.<br><br>
-	Parameter 뿐만 아니라 메소드에도 Annotation을 붙이면 Return Type에도 Null-Safety가 적용된다.<br><br>
-	관련 Annotation.

	-	@NonNull.

	-	@Nullable.

	-	@NonNullApi(패키지 레벨 설정).

	-	@NonNullFields(패키지 레벨 설정).<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
