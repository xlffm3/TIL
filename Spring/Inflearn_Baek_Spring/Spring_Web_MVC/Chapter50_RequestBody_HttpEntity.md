RequestBody
-----------

> EventApi.java

```java
@RestController
@RequestMapping("/api/events")
public class EventApi {

    @PostMapping
    public Event createEvent(@RequestBody Event event) {
        //save Event
        return event;
    }

    @PostMapping
    public Event createEvent(HttpEntity<Event> request) {
      MediaType contentType = request.getHeaders().getContentType();
      return request.getBody();
    }

}
```

<br>

> SampleControllerTest.java

```java
@Autowired
ObjectMapper objectMapper;

@Test
public void createEvent() throws Exception {
    Event event = new Event();
    event.setName("jipark");
    event.setId(10);
    String jsonMessage = objectMapper.writeValueAsString(event);
    this.mockMvc.perform(post("/api/events")
    .contentType(MediaType.APPLICATION_JSON_UTF8)
    .content(jsonMessage))
    .andDo(print())
    .andExpect(status().isOk())
    .andExpect(jsonPath("name").value("jipark"))
    .andExpect(jsonPath("id").value(10));
}
```

-	요청 본문(body)에 들어있는 데이터를 핸들러 어댑터가 HttpMessageConveter를 통해 변환한 객체로 받아올 수 있다.<br><br>
-	@Valid 또는 @Validated를 사용해서 값을 검증 할 수 있다.<br><br>
-	BindingResult 아규먼트를 사용해 코드로 바인딩 또는 검증 에러를 확인할 수 있다.<br><br>
-	HttpEntity는 @RequestBody와 비슷하지만 추가적으로 요청 헤더 정보를 사용할 수 있다.<br><br>
-	ObjectMapper를 통해 객체를 JSON 문자열로, JSON 문자열을 객체로 만들 수 있다.<br><br>

HttpMessageConverter
--------------------

-	Spring MVC 설정(WebMvcConfigurer)에서 설정할 수 있다.<br><br>
-	configureMessageConverters : 기본 메시지 컨버터의 대체재이다.<br><br>
-	extendMessageConverters : 메시지 컨버터에 추가한다.<br><br>
-	기본 컨버터 : WebMvcConfigurationSupport.addDefaultHttpMessageConverters.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
