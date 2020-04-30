@(Rest)ControllerAdvice
-----------------------

> BaseController.java

```java
@ControllerAdvice
@ControllerAdvice(assignableTypes = {SampleController.class, EventApi.class})
public class BaseController {
    @InitBinder
    //중략
    @ExceptionHandler
    //중략
    @ModelAttribute
    //중략
}
```

-	예외 처리, 바인딩 설정, 모델 객체를 모든 컨트롤러 전반에 걸쳐 적용하고 싶은 경우에 사용한다.
	-	@ExceptionHandler.
	-	@InitBinder.
	-	@ModelAttributes.<br><br>
-	적용할 범위를 지정할 수도 있다.
	-	특정 애노테이션을 가지고 있는 컨트롤러에만 적용하기.
	-	특정 패키지 이하의 컨트롤러에만 적용하기.
	-	특정 클래스 타입에만 적용하기.<br><br>

TODO
----

-	@JSONView.<br><br>
-	비동기 요청 처리.<br><br>
-	CORS 설정.<br><br>
-	HTTP/2.<br><br>
-	웹 소켓.<br><br>
-	웹 플럭스.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
