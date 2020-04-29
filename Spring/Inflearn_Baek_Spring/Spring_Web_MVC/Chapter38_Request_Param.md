@RequestParam
-------------

> SampleController.java

```java
@Controller
public class SampleController {

    @GetMapping("/events")
    @ResponseBody
    public Event event(@RequestParam Map<String, String> map) {
        Event event = new Event();
        event.setName(map.get("name"));
        return event;
    }
}
```

<br>

> SampleControllerTest.java

```java
@Test
public void eventTest() throws Exception {
    this.mockMvc.perform(get("/events").param("name", "jipark"))
            .andDo(print())
            .andExpect(status().isOk());
}
```

-	요청 매개변수에 들어있는 단순 타입 데이터를 메소드 아규먼트로 받아올 수 있다.<br><br>
-	값이 반드시 있어야 한다.
	-	required=false 또는 Optional을 사용해서 부가적인 값으로 설정할 수도 있다.<br><br>
-	String이 아닌 값들은 타입 컨버전을 지원한다.<br><br>
-	Map<String, String> 또는 MultiValueMap<String, String>에 사용해서 모든 요청 매개변수를 받아 올 수도 있다.<br><br>
-	애노테이션 생략이 가능하다.<br><br>

요청 매개변수
-------------

-	쿼리 매개변수.
-	폼 데이터.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
