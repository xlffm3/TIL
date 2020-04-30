RedirectAttributes
------------------

> SampleController.java

```java
@PostMapping("/events/form/id")
public String eventId(@Validated @ModelAttribute Event event, BindingResult bindingResult, SessionStatus sessionStatus,
                      RedirectAttributes redirectAttributes) {
    if (bindingResult.hasErrors()) {
        return "form-id";
    }
    sessionStatus.setComplete();
    redirectAttributes.addAttribute("name", event.getName());
    redirectAttributes.addAttribute("id", event.getId());
    return "redirect:/events/list";
}

@GetMapping("/events/list")
public String getList(Model model, @RequestParam String name, @RequestParam Integer id, @SessionAttribute LocalDateTime visitTime) {
    System.out.println(visitTime);
    Event event = new Event();
    List<Event> eventList = new ArrayList<>();
    event.setName("spring");
    eventList.add(event);
    model.addAttribute(eventList);
    return "list";
}
```

-	리다이렉트 할 때 기본적으로 Model에 들어있는 primitive type 데이터는 URI 쿼리 매개변수에 추가된다.<br><br>
-	Spring Boot에서는 이 기능이 기본적으로 비활성화 되어 있다.<br><br>
-	Ignore-default-model-on-redirect 프로퍼티를 사용해서 활성화 할 수 있다.<br><br>
-	원하는 값만 리다이렉트 할 때 전달하고 싶다면 RedirectAttributes에 명시적으로 추가할 수 있다.<br><br>
-	리다이렉트 요청을 처리하는 곳에서 쿼리 매개변수를 @RequestParam 또는 @ModelAttribute로 받을 수 있다.<br><br>
-	@SessionAttributes에 event가 명시되어 있고 이미 소멸되었기 때문에, @ModelAttribute로 바인딩할 시 ("newEvent")와 같이 value를 별도로 지정해야 에러가 나지 않는다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
