Chapter 4 : @Component & @ComponentScan
=======================================

@ComponentScan 주요 기능
------------------------

-	스캔의 위치를 설정한다.<br><br>
-	특정 Annotation을 스캔할지 안 할지를 결정하는 필터 기능이 있다.<br><br>

@Component
----------

-	@Repository.
-	@Service.
-	@Controller.
-	@Configuration.<br><br>

동작 원리
---------

-	@ComponentScan은 스캔할 패키지와 Annotation에 대한 정보를 제공한다.<br><br>
-	실제 스캐닝은 ConfigurationClassPostProcessor라는 BeanFactoryPostProcessor에 의해 처리 된다.<br><br>
-	@Bean 및 Function 등의 방식을 이용한 Bean보다 더 우선적으로 스캔되어 생성된다.<br><br>

Function을 이용한 Bean 등록
---------------------------

> Application.java

```java
public static void main(String[] args) {
        new SpringApplicationBuilder()
        .sources(Demospring51Application.class)
        .initializers((ApplicationContextInitializer<GenericApplicationContext>)
  applicationContext -> {
                            applicationContext.registerBean(MyBean.class);
      })
      .run(args);
}
```

-	등록해야할 Bean이 많다면 @ComponentScan 방식은 처음 구동 시간이 오래 걸릴 수 있다.<br><br>
-	Function을 이용하여 Bean을 등록하면 이러한 구동 시간 이슈를 해결할 수 있으며, 조금 더 유연하게 코드를 짤 수 있다.<br><br>
-	설정이 복잡해진다는 단점이 존재한다.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
