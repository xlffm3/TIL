@ExceptionHandler
-----------------

> EventException.java

```java
public class EventException extends RuntimeException{
}
```

<br>

> SampleController.java

```java
@ExceptionHandler({EventException.class, RuntimeException.class})
public String exceptionHandler(RuntimeException exception, Model model) {
    model.addAttribute("meesage", "event error");
    return "error";
}
```

<br>

> EventApi.java

```java
@RestController
@RequestMapping("/api/events")
public class EventApi {

    @ExceptionHandler
    public ResponseEntity errorHandler() {
        return ResponseEntity.badRequest().body("can't handler this.,");
    }
}
```

-	특정 예외가 발생한 요청을 처리하는 핸들러를 정의한다.
	-	지원하는 메소드 아규먼트(해당 예외 객체 및 핸들러 객체 등).
	-	지원하는 리턴 값.
	-	REST API의 경우 응답 본문에 에러에 대한 정보를 담아주고, 상태 코드를 설정하려면 ResponseEntity를 주로 사용한다.<br><br>
	-	가장 구체적인 핸들러가 매핑된다.<br><br>
	-	인자로 여러 에러를 받아 동시에 처리할 수 있다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
