ModelAttribute의 또 다른 사용법
-------------------------------

> SampleController.java

```java
@ModelAttribute
public void subjects(Model model) {
    model.addAttribute("subjects", List.of("study", "seminar", "hobby", "social"));
}

@GetMapping("/events/form/name")
@ModelAttribute //생략 가능
public Event eventFormName() {
  return new Event(); //RequestToViewNameTranslator 인터페이스가 요청 이름과 일치하는 뷰 이름으로 리턴을 해줌.
}
```

<br>

> SampleControllerTest.java

```java
@Test
public void eventTest() throws Exception {
    Event event = new Event();
    event.setName("jipark");
    event.setId(10);
    this.mockMvc.perform(get("/events/list")
    .sessionAttr("visitTime", LocalDateTime.now())
    .flashAttr("newEvent", event))
            .andDo(print())
            .andExpect(model().attributeExists("categories"))
            .andExpect(status().isOk());
}
```

-	@RequestMapping을 사용한 핸들러 메소드의 아규먼트에 사용한다.<br><br>
-	@Controller 또는 @ControllerAdvice를 사용한 클래스에서 모델 정보를 초기화 할 때 사용한다.<br><br>
-	@RequestMapping과 같이 사용하면 해당 메소드에서 리턴하는 객체를 모델에 넣어 준다.
	-	RequestToViewNameTranslator.<br><br>
-	다른 핸들러에서 공통으로 사용하는 Model을 처리하는데 유용하다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
