TDD(Test-Driven Development)
----------------------------

-	테스트가 주도하는 개발로서 테스트 코드를 먼저 작성하는 것 부터 시작한다.
	-	항상 실패하는 테스트를 먼저 작성하고(Red),
	-	테스트가 통과하는 프로덕션 코드를 작성하고(Green),
	-	테스트가 통과하면 프로덕션 코드를 리팩토링한다(Refactor).
-	Unit Test는 TDD의 첫 번째 단계인 기능 단위의 테스트 코드를 작성하는 것이다.

<br>

Unit Test의 장점
----------------

-	개발 단계 초기에 문제를 발견할 수 있다.
-	개발자가 나중에 코드를 리팩토링하거나 라이브러리 업그레이드 등에서 기존 기능이 올바르게 작동하는지 확인할 수 있다.
-	기능에 대한 불확실성을 감소시킬 수 있다.
-	시스템에 대한 실제 문서를 제공한다.

<br>

Test Code 작성
--------------

> Application.class

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

-	@SpringBootApplication으로 인해 Bean 읽기 및 생성 등 스프링 부트의 다양한 기능들이 자동 설정된다.
-	해당 어노테이션의 위치부터 설정을 읽어가기 때문에, 이 클래스는 항상 프로젝트 최상단에 위치해야 한다.
-	`SpringApplication.run` 메소드로 인해 내장 WAS가 실행된다.
-	내장 WAS로 언제 어디서나 같은 환경에서 스프링 부트를 배포할 수 있다.

<br>

> HelloController.java

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```

<br>

> HelloControllerTest.java

```java
@RunWith(SpringRunner.class)
@WebMvcTest(controllers = HelloController.class)
public class HelloControllerTest extends TestCase {

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void testHello() throws Exception {
        String hello = "hello";

        this.mockMvc.perform(get("/hello"))
                .andExpect(status().isOk())
                .andExpect(content().string(hello));
    }
}
```

-	Inflearn에서 다룬 내용이기 때문에 자세한 설명은 생략한다.
-	@RestController는 컨트롤러를 JSON을 반환하는 컨트롤러로 만들어준다.
-	@RunWith은 SpringRunner라는 실행자를 실행시켜 Spring Boot Test와 JUnit 사이에서 연결자 역할을 한다.

<br>

Lombok
------

> build.gradle

```java
compile('org.projectlombok:lombok')
```

<br>

> HelloResponseDto.java

```java
@Getter
@RequiredArgsConstructor
public class HelloResponseDto {

    private final String name;
    private final int amount;
}
```

-	@RequiredArgsConstructor는 final이 포함된 필드들을 생성자에 포함시켜 생성자를 생성한다.

<br>

> HelloResponseDtoTest.java

```java
public class HelloResponseDtoTest extends TestCase {

    @Test
    public void testLombok() {
        String name = "test";
        int amount = 1000;

        HelloResponseDto dto = new HelloResponseDto(name, amount);
        assertThat(dto.getName()).isEqualTo(name);
        assertThat(dto.getAmount()).isEqualTo(amount);
    }
}
```

-	Junit과 비교했을 때 AssertJ는 CoreMatchers와 달리 추가 라이브러리가 필요하지 않다.
-	또한 자동 완성이 좀 더 확실하게 지원된다.

<br>

---

Reference
---------

-	스프링 부트와 AWS로 혼자 구현하는 웹 서비스 (이동욱 저)
