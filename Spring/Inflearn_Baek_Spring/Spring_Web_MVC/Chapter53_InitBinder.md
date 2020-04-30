@InitBinder
-----------

> SampleController.java

```java
    @InitBinder
    public void initBinder(WebDataBinder webDataBinder) {
        webDataBinder.setDisallowedFields("id");
        webDataBinder.addCustomFormatter();
        webDataBinder.addValidators(new EventValidator());
    }
```

<br>

> EventValidator.java

```java
public class EventValidator implements Validator {
    @Override
    public boolean supports(Class<?> aClass) {
        return Event.class.isAssignableFrom(aClass);
    }

    @Override
    public void validate(Object o, Errors errors) {
        Event event = (Event)o;
        if (event.getName().equalsIgnoreCase("AAA")){
            errors.rejectValue("name", "wrongValue", "cannot AAA");
        }
    }
}
```

<br>

> SampleController.java

```java
@Autowired
EventValidator eventValidator;

@GetMapping
public void test(Event event, BindingResult bindingResult) {
  eventValidator.validate(event, bindingResult);
}
```

-	특정 컨트롤러에서 바인딩 또는 검증 설정을 커스텀하게 변경하고 싶을 때 사용한다.<br><br>
-	특정 모델 객체에만 바인딩 또는 Validator 설정을 적용하고 싶은 경우 `@InitBinder(“event”)` 와 같이 명시한다.<br><br>
-	바인딩과 포매터 및 검증 관련 설정을 할 수 있으며, Validator는 별도의 Bean으로 등록하여 원하는 시점에 핸들러 내부에서 실행하는 대안도 있다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
