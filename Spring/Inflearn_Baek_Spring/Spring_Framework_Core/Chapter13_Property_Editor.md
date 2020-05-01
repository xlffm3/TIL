Data Binding
------------

-	org.springframework.validation.DataBinder.<br><br>
-	기술적인 관점 : Property 값을 타겟 객체에 설정하는 기능이다.<br><br>
-	사용자 관점 : 사용자 입력값을 애플리케이션 도메인 모델에 동적으로 변환해 넣어주는 기능이다.<br><br>
-	입력값은 대부분 “문자열”인데, 그 값을 객체가 가지고 있는 다양한 도메인 자료형으로 변환해서 넣어주는 기능이다.<br><br>

PropertyEditor
--------------

> EventController.java

```java
@RestController
public class EventController {

  @GetMapping("/event/{event}")
  public String getEvent(@Pathvariable Event event) {
    System.out.println(event);
    return event.getId();
  }
}
```

<br>

> EventEditor.java

```java
public class EventEditor extends PropertyEditorSupport {
    @Override
    public String getAsText() {
        Event event = (Event)getValue();
        return event.getId().toString();
    }

    @Override
    public void setAsText(String text) throws IllegalArgumentException {
        setValue(new Event(Integer.parseInt(text)));
    }
}
```

-	Spring 3.0 이전까지 DataBinder가 변환 작업 사용하던 인터페이스이다.<br><br>
-	URL로 들어온 매개변수는 String이지만, Event로 적절히 변환해주는 데이터 바인딩 기능이 없으면 Argument Resolve 시점에서 에러가 발생한다.<br><br>
-	따라서 PropertyEditorSupport를 상속받아 데이터 바인딩을 처리해주는 메소드를 오버라이딩한다.<br><br>
-	상태 정보가 있어 Thread-Safe하지 않아 Thread Scope 등 예외 상황이 아니면 Bean으로 등록하지 않고 사용한다.<br><br>
-	Object와 String 간의 변환만 할 수 있어 사용 범위가 제한적이다.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
