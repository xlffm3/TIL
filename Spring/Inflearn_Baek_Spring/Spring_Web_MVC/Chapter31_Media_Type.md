특정한 타입의 데이터를 담고 있는 요청만 처리하는 핸들러
-------------------------------------------------------

> SampleController.java

```java
@Controller
public class SampleController {

    @GetMapping(value = "/hello", consumes = APPLICATION_JSON_UTF8_VALUE)
    @ResponseBody
    public String hello() {
        return "hello";
    }
}
```

<br>

> SimpleControllerTest.java

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
class SampleControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    public void hello() throws Exception {
        this.mockMvc.perform(get("/hello")
                    .contentType(MediaType.APPLICATION_JSON_UTF8))
                    .andDo(print())
                    .andExpect(status().isOk())
                    .andExpect(content().string("hello"));
    }
}
```

-	@RequestMapping(consumes=MediaType.APPLICATION_JSON_UTF8_VALUE).<br><br>
-	Content-Type 헤더로 필터링한다.<br><br>
-	매치 되는 않는 경우에 415 Unsupported Media Type이 응답된다.<br><br>

특정한 타입의 응답을 만드는 핸들러
----------------------------------

> SampleController.java

```java
@Controller
public class SampleController {

    @GetMapping(value = "/hello", consumes = APPLICATION_JSON_UTF8_VALUE, produces = MediaType.APPLICATION_JSON_UTF8_VALUE)
    @ResponseBody
    public String hello() {
        return "hello";
    }
}
```

<br>

> SimpleControllerTest.java

```code
@Test
public void hello() throws Exception {
    this.mockMvc.perform(get("/hello")
                .contentType(MediaType.APPLICATION_JSON_UTF8)
                .accept(MediaType.APPLICATION_JSON))
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(content().string("hello"));
}
```

-	@RequestMapping(produces=”application/json”).<br><br>
-	Accept 헤더로 필터링한다..<br><br>
-	매치 되지 않는 경우에 406 Not Acceptable 응답한다.<br><br>

주의사항
--------

-	문자열을 입력하는 대신 MediaType을 사용하면 상수를 자동 완성으로 사용할 수 있다.<br><br>

-	GetMapping 등 Mapping 관련 Annotation은 내부 인자들을 배열로 받을 수 있다.<br><br>

-	클래스에 선언한 @RequestMapping에 사용한 것과 조합이 되지 않고 메소드에 사용한 @RequestMapping의 설정으로 오버라이딩된다.<br><br>

-	Not (!)을 사용해서 특정 미디어 타입이 아닌 경우로 맵핑 할 수도 있다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn)
