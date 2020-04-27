기본적인 Spring MVC 설정
------------------------

-	DispatcherServlet의 기본 전략에는 다소 한계가 있다.<br><br>
-	@Configuration을 사용한 Java 설정 파일에 Spring 구성 요소를 직접 @Bean으로 사용해서 등록한다.<br><br>

@EnableWebMvc
-------------

> WebConfig.java

```Java
@Configuration
@ComponentScan
@EnableWebMvc
public class WebConfig {

}
```

<br>

> WebApplication.java

```Java
public class WebApplication implements WebApplicationInitializer {
    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
        AnnotationConfigWebApplicationContext context = new AnnotationConfigWebApplicationContext();
        context.setServletContext(servletContext);
        context.register(WebConfig.class);
        context.refresh();
        DispatcherServlet dispatcherServlet = new DispatcherServlet(context);
        ServletRegistration.Dynamic app = servletContext.addServlet("app", dispatcherServlet);
        app.addMapping("/app/*");
    }
}
```

-	Annotation 기반 Spring MVC를 사용할 때 편리한 웹 MVC 기본 설정이다.<br><br>
-	DelegatingWebMvcConfiguration.class를 Import하는데, 상속 관계를 따라 올라가다 보면 실질적으로 사용할 Bean들이 등록되어 있다.<br><br>
-	`context.setServletContext(servletContext);` 이 필요하다.<br><br>

WebMvcConfigurer
----------------

> WebConfig.java

```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
        @Override
        public void configureViewResolvers(ViewResolverRegistry registry) {
                  registry.jsp("/WEB-INF/", ".jsp");
        }
}
```

-	@EnableWebMvc가 제공하는 Bean을 커스터마이징할 수 있는 기능을 제공하는 인터페이스이다.<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
