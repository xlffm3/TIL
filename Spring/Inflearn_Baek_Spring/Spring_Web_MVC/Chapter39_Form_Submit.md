요청 처리
---------

> SampleController.java

```java
@Controller
public class SampleController {

    @GetMapping("/events/form")
    public String eventsForm(Model model) {
        model.addAttribute("event", new Event());
        return "form";
    }

    @PostMapping("/events")
    @ResponseBody
    public Event event(@RequestParam String name, @RequestParam Integer id) {
        Event event = new Event();
        event.setName(name);
        event.setId(id);
        return event;
    }
}
```

-	GET : /events/form.<br><br>
-	뷰: form.html.<br><br>
-	모델 : “event”, new Event().<br><br>

타임리프
--------

> form.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="#" th:action="@{/events}" method="post" th:object="${event}">
    <input type="text" title="name" th:field="*{name}"/>
    <input type="text" title="id" th:field="*{id}"/>
    <input type="submit" value="create"/>
</form>
</body>
</html>
```

<br>

> SampleControllerTest.java

```java
@Test
public void eventTest() throws Exception {
    this.mockMvc.perform(get("/events/form"))
            .andDo(print())
            .andExpect(view().name("form"))
            .andExpect(model().attributeExists("events"))
            .andExpect(status().isOk());
}
```

-	@{} : URL 표현식.<br><br>
-	${} : variable 표현식.<br><br>
-	\*{} : selection 표현식으로 기본값을 보여준다.<br><br>
-	타임리프는 서블릿 컨테이너 엔진이 없어도 서버 사이드에서 렌더링이 끝나기 때문에, JSP와 다르게 View 결과 단위 테스트가 가능하다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
