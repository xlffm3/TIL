@ModelAttribute
---------------

> Event.java

```java
public class Event {
  @Min(0)
  Integer id;
}
```

<br>

> SampleController.java

```java
@PostMapping("/events")
@ResponseBody
public Event event(@Valid @ModelAttribute Event event, BindingResult bindingResult) {
  if (bindingResult.hasError()) {
    //생략
  }
    return event;
}
```

<br>

> SampleControllerTest.java

```java
@Test
public void eventTest() throws Exception {
    this.mockMvc.perform(post("/events")
            .param("name", "jipark")
            .param("id", "10"))
            .andDo(print())
            .andExpect(status().isOk())
            .andExpect(jsonPath("name").value("jipark"))
            .andExpect(jsonPath("id").value("10"));
}
```

-	여러 곳에 있는 단순 타입 데이터를 복합 타입 객체로 받아오거나 해당 객체를 새로 만들 때 사용할 수 있다.<br><br>
-	여러 곳: URI 패스, 요청 매개변수, 세션 등을 의미한다.<br><br>
-	생략이 가능하다.<br><br>

값을 바인딩 할 수 없는 경우
---------------------------

-	BindException 및 400 에러가 발생한다.<br><br>
-	바인딩 에러를 직접 다루고 싶은 경우, BindingResult 타입의 아규먼트를 바로 오른쪽에 추가한다.<br><br>
-	바인딩 이후에 검증 작업을 추가로 하고 싶은 경우, @Valid 또는 @Validated 애노테이션을 사용한다.<br><br>
-	Id가 음수값인 경우 맵핑되어도 @Valid에서 걸린다.<br><br>

@Validated
----------

> Event.java

```java
public class Event {

  interface ValidateName {}
  interface ValidateId {}

  @Min(value = 0, groups = ValidateId.class)
  Integer id;
}
```

<br>

> SampleController.java

```java
@PostMapping("/events")
@ResponseBody
public Event event(@Validated(Event.ValidateId.class) @ModelAttribute Event event, BindingResult bindingResult) {
  if (bindingResult.hasError()) {
    //생략
  }
    return event;
}
```

-	스프링 MVC 핸들러 메소드 아규먼트에 사용할 수 있으며, validation group이라는 힌트를 사용할 수 있다.<br><br>
-	@Valid 애노테이션에는 그룹을 지정할 방법이 없다.<br><br>
-	@Validated는 스프링이 제공하는 애노테이션으로 그룹 클래스를 설정할 수 있다.<br><br>

Form Submit
-----------

> form.html

```html
<p th:if="${#fields.hasErrors('limit')}" th:errors="*{limit}">Incorrect date</p>
```

<br>

> list.html

```html
<a th:href="@{/form}">Create New Event</a>
<div th:unless="${#lists.isEmpty(eventList)}">
    <ul th:each="event: ${eventList}">
        <p th:text="${event.name}">Event Name</p>
    </ul>
</div>
```

<br>

> SampleController.java

```java
@PostMapping("/events")
public String event(@Validated @ModelAttribute Event event, BindingResult bindingResult, Model model) {
    if (bindingResult.hasErrors()) {
        return "form";
    }
    //DB 저장하는 구간
    return "redirect:list";
}

@GetMapping("/list")
public String getEvent(Model model) {
    Event event = new Event();//DB에서 읽는 구간
    event.setName("spring");
    event.setId(10);
    List<Event> eventList = new ArrayList<>();
    eventList.add(event);
    model.addAttribute("eventList", eventList);
    return "list";
}
```

-	바인딩 에러가 발생하면 Model에는 Event 및 BindingResult.event 정보가 담긴다.<br><br>
-	Post 이후에 브라우저를 리프래시 하더라도 폼 서브밋이 발생하지 않도록 Post / Redirect / Get 패턴을 이용한다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
