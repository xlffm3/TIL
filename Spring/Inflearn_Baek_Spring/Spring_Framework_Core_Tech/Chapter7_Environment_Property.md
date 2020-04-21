#Chapter 7 : Environment - Property

Property
--------

> application.properties

```properties
-Dapp.about=spring
```

<br>

> AppRunner.java

```java
@Component
public class AppRunner implements ApplicationRunner {

    @Autowired
    ApplicationContext ctx;

    @Autowired
    BookRepository bookRepository;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Environment evt = (Environment) ctx.getEnvironment();
        System.out.println(evt.getProperty("app.name")); //spring5
    }
}
```

-	Property란 다양한 방법으로 정의할 수 있는 설정값을 의미하며 계층적인 Key-Value이다.
-	Environment의 역할은 Property 소스 설정 및 값 가져오기다.<br><br>
-	Property(StandardServletEnvironment)의 우선순위.
	-	ServletConfig 매개변수.
	-	ServletContext 매개변수.
	-	JNDI (java:comp/env/).
	-	JVM 시스템 프로퍼티 (-Dkey=”value”).
	-	JVM 시스템 환경 변수 (운영체제 환경 변수).<br><br>

설정
----

-	@PropertySource : Environment를 통해 Property를 추가한다.<br><br>
-	Spring Boot는 기본 Property 소스(application.properties)를 지원하며, Profile까지 고려한 계층형 Property 우선 순위를 제공한다.

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
