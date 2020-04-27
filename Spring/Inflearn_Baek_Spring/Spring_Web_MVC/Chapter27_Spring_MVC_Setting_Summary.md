Spring MVC 설정
---------------

-	Spring MVC 설정이란 DispatcherServlet이 사용할 여러 Bean의 설정을 의미한다.<br><br>

-	HandlerMapper 및 HandlerAdapter 등을 일일히 등록하려니 너무 많고, 해당 빈들이 참조하는 또 다른 객체들까지 설정하려면 비용이 많이 든다.<br><br>

@EnableWebMvc
-------------

-	애노테이션 기반의 Spring MVC 설정을 간편화한다.<br><br>

-	WebMvcConfigurer가 제공하는 메소드를 구현하여 커스터마이징할 수 있다.<br><br>

Spring Boot
-----------

-	Spring Boot 자동 설정을 통해 다양한 Spring MVC 기능을 아무런 설정 파일을 만들지 않아도 제공한다.<br><br>
-	WebMvcConfigurer가 제공하는 메소드를 구현하여 커스터마이징할 수 있다.<br><br>
-	@EnableWebMvc를 사용하면 Spring 부트 자동 설정을 사용하지 못한다.<br><br>

Spring MVC 설정 방법
--------------------

-	Spring 부트를 사용하는 경우에는 application.properties 부터 시작한다.<br><br>
-	한계가 있다면 WebMvcConfigurer로 커스터마이징한다.<br><br>
-	한계가 있다면 @Bean으로 MVC 구성 요소를 직접 등록한다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn)
