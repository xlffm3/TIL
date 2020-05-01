Domain Class Converter
----------------------

> pom.xml

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
  <groupId>com.h2database</groupId>
  <artifactId>h2</artifactId>
</dependency>
```

<br>

> Person.java

```java
@Entity @Component
public class Person {
    private String name;

    @Id @GeneratedValue
    private long id;
}
```

<br>

> PersonRepository.java

```java
public interface PersonRepository extends JpaRepository<Person, Long> {
}
```

<br>

> SimpleController.java

```Java
@GetMapping("/hello")
public String hello(@RequestParam("id") Person person) {
    return "hello " + person.getName();
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

    @Autowired
    PersonRepository personRepository;

    @Test
    public void hello() throws Exception{
        Person person = new Person();
        person.setName("jipark");
        person.setId(10);
        Person savedPerson = personRepository.save(person);
        this.mockMvc.perform(get("/hello").param("id", savedPerson.getId() + ""))
                .andDo(print())
                .andExpect(content().string("hello jipark"));
    }
}
```

-	Spring Data JPA는 Spring MVC용 도메인 클래스 컨버터를 제공한다.<br><br>
-	도메인 클래스 컨버터는 Spring Data JPA가 제공하는 Repository를 사용해서 ID에 해당하는 엔티티를 읽어온다.<br><br>
-	@RequestParam을 ID로 객체 맵핑하는 경우, 직접 Formatter를 구현하는 수고스러움을 덜 수 있다.<br><br>
-	DB에서 자동 생성할 값이라면 @GeneratedValue 를 붙여준다.<br><br>
-	ID 값에서 Entity로 컨버팅할 때 JpaRepository가 필요하다.<br><br>
-	단위 테스트에서는 테스트에 만들 객체를 만들어 저장한 다음, ID String을 파라미터로 전달하면 조회가 가능하다.<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
