JUNIT Test
----------

> SimpleController.java

```Java
@RestController
public class SampleController {

    @GetMapping("/hello/{name}")
    public String hello(@PathVariable("name") Person person) {
        return "hello " + person.getName();
    }
}
```

<br>

> SimpleControllerTest.java

```Java
@RunWith(SpringRunner.class)
@WebMvcTest
class SampleControllerTest {

    @Autowired
    MockMvc mockMvc;

    @Test
    public void hello() throws Exception{
        this.mockMvc.perform(get("/hello/jipark"))
                .andDo(print())
                .andExpect(content().string("hello jipark"));
    }
}
```

-	Ctrl + Shift + T 를 통해 JUNIT 테스트 Shortcut를 사용하여 테스트 환경을 구축한다.<br><br>
-	문자열을 객체로 받으려고 하는데 Spring이 방법을 모르기 때문에 Formatter를 구현해야 한다.<br><br>

Formatter
---------

> PersonFormatter.java

```Java
public class PersonFormatter implements Formatter<Person> {

    @Override
    public Person parse(String s, Locale locale) throws ParseException {
        Person person = new Person();
        person.setName(s);
        return person;
    }
}
```

<br>

> WebConfig.java

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addFormatter(new PersonFormatter());
    }
}
```

-	Printer : 해당 객체를 (Locale 정보를 참고하여) 문자열로 어떻게 출력할 것인가를 다룬다.<br><br>

-	Parser : 어떤 문자열을 (Locale 정보를 참고하여) 객체로 어떻게 변환할 것인가를 다룬다.<br><br>

-	Spring Boot를 사용하는 경우 WebMvcConfigurer의 메서드를 오버라이드 할 필요 없이, Formatter를 Bean으로 등록하기만 하면 된다.<br><br>

RequestParam
------------

> SimpleController.java

```java
@RestController
public class SampleController {

    @GetMapping("/hello")
    public String hello(@RequestParam("name") Person person) {
        return "hello " + person.getName();
    }
}
```

<br>

> SimpleControllerTest.java

```java
@Test
public void hello() throws Exception{
    this.mockMvc.perform(get("/hello").param("jipark") )
            .andDo(print())
            .andExpect(content().string("hello jipark"));
}
```

-	localhost:8080/hello?name=jipark 으로 접근한다.<br><br>

WebMvcTest
----------

> SimpleControllerTest.java

```java
@RunWith(SpringRunner.class)
@SpringBootTest
@AutoConfigureMockMvc
```

-	@WebMvcTest는 슬라이싱 테스트로서 Web과 관련된 Bean만 등록하기 때문에, Formatter를 @Controller가 아닌 @Component로 설정한다면 단위 테스트에서 실패한다.<br><br>
-	따라서 명시적으로 해당 Formatter를 테스트에서 등록해주거나, 통합 테스트로 변경해준다.<br><br>
-	이 때, MockMvc가 자동 주입되지 않기 때문에 @AutoConfigureMockMvc를 붙어주어야 한다.<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
