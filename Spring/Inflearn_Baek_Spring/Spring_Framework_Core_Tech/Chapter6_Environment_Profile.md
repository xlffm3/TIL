Chapter 6 : Environment - Profile
=================================

Environment
-----------

> AppRunner.java

```java
@Component
public class AppRunner implements ApplicationRunner {
    @Autowired
    ApplicationContext ctx;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        Environment evn = ctx.getEnvironment();
        System.out.println(Arrays.toString(environment.getActiveProfiles())); //[]
        System.out.println(Arrays.toString(environment.getDefaultProfiles())); //[default]
    }
}
```

-	Profile과 Property를 다루는 인터페이스이다.<br><br>
-	EnvironmentCapable을 상속받는 ApplicationContext의 `getEnvironment()` 메소드로 사용한다.<br><br>
-	별도의 Profile을 지정해주지 않는다면, Bean들은 기본적으로 Default Profile에 속한다.<br><br>

Profile
-------

> TestConfiguration.java

```java
@Configuration @Profile("test")
```

<br>

> TestBookRepository.java

```java
@Repository @Profile("test")
```

-	Bean들의 그룹을 의미한다.<br><br>
-	Environment의 역할은 활성화 할 Profile의 확인 및 설정이다.<br><br>
-	특정 환경에서는 사용하고 싶은 Bean이 다를 경우 이용한다.<br><br>
-	테스트 환경 등이 대표적인 유스케이스이다.<br><br>
-	클래스 및 메소드에 각각 정의할 수 있다.<br><br>
-	-Dspring.profiles.avtive=”test,A,B,...” 혹은 @ActiveProfiles Annotation을 통해 Profile을 설정한다.<br><br>

Profile 표현식
--------------

-	! (not).
-	& (and).
-	| (or).<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
