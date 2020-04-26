Spring MVC 연동
---------------

> web.xml

```xml
<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <context-param>
    <param-name>contextClass</param-name>
    <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
  </context-param>
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>com.glenn.AppConfig</param-value>
  </context-param>  
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <servlet>
    <servlet-name>app</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <init-param>
      <param-name>contextClass</param-name>
      <param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
    </init-param>
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>com.glenn.WebConfig</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>app</servlet-name>
    <url-pattern>/app/*</url-pattern>
  </servlet-mapping>
</web-app>
```

<br>

> AppConfig.java

```java
@Configuration
@ComponentScan(excludeFilters = @ComponentScan.Filter(Controller.class))
```

<br>

> WebConfig.java

```java
@Configuration
@ComponentScan(useDefaultFilters = false, includeFilters = @ComponentScan.Filter(Controller.class))
```

<br>

> HelloController.java

```java
@RestController
public class HelloController {

    @Autowired
    HelloService helloService;

    @GetMapping("/hello")
    public String hello() {
        return "mvc test hello!! " + helloService.getName();
    }
}
```

-	계층 구조를 만들 때, HelloService Bean은 ContextLoaderListener(Root ApplicationContext)에 등록하고 HelloController는 DispatcherServlet의 ApplicationContext에 등록한다.<br><br>
-	Web 관련 설정은 DispatcherServlet가 관장하고, 다른 Servlet들도 공통으로 사용할 수 있는 자원은 ContextLoaderListener(Root)가 관장하도록 한다.<br><br>
-	ContextLoaderListener가 contextClass와 contextConfigLocation을 설정했듯, DispatcherServlet 또한 이를 설정해준다.<br><br>
-	이후 @ComponentScan을 하는 Config에 각각 필터를 설정하여, ContextLoaderListener는 Controller를 제외하게 하고 DispatcherServlet은 Controller만 등록할 수 있게 한다.<br><br>
-	DispatcherServlet은 ContextLoaderListener의 WebApplicationContext를 부모로 가진다.<br><br>
-	@ComponentScan의 필터를 삭제한 뒤, ContextLoaderListener 관련 설정을 삭제하고 모든 Bean이 DispatcherServlet에 등록시켜 사용할 수 있다.<br><br>
-	최근에는 DispatcherServlet이 1개인 경우가 대다수라 계층 구조를 크게 신경쓰는 일이 적다.<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
