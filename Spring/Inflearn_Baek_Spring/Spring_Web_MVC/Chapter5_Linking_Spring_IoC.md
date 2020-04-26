Spring IoC 컨테이너 연동
------------------------

> pom.xml

```xml
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
  <version>5.1.3.RELEASE</version>
</dependency>
```

<br>

> web.xml

```xml
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
```

<br>

> AppConfig.java

```java
@Configuration
@ComponentScan
public class AppConfig {
}
```

<br>

> HelloService.java

```java
@Service
public class HelloService {

    public String getName() {
        return "glenn";
    }
}
```

<br>

> HelloServlet.java

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    ApplicationContext context = (ApplicationContext)getServletContext().getAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE);
    HelloService helloService = (HelloService)context.getBean(HelloService.class);
    System.out.println("do get");
    resp.getWriter().println("<html><body><h1>hi" + helloService.getName() + "</h1></body></html>");
}
```

-	ContextLoaderListener.
	-	서블릿 리스너 구현체이다.
	-	ApplicationContext를 만들어 준다.
	-	ApplicationContext를 서블릿 컨텍스트 라이프사이클에 따라 등록하고 소멸시켜준다.
	-	서블릿에서 IoC 컨테이너를 ServletContext를 통해 꺼내 사용할 수 있다.<br><br>
-	이를 통해 Servlet들이 ApplicationContext(Spring IoC 컨테이너)와 등록된 Bean을 사용할 수 있다.<br><br>
-	Listener가 사용하는 여러 파라미터들을 XML에 지정해주어야 한다.<br><br>
-	Servlet이 추가될 때 마다 xml 설정이 늘어나는데, 디자인 패턴 중 Front Controller로 이를 해결한다.<br><br>

DispatcherServlet
-----------------

![image](https://user-images.githubusercontent.com/56240505/80301210-bb698b80-87dd-11ea-9809-f4d9f2dd3fb9.png)<br><br>

-	Spring MVC의 핵심이며, Front Controller의 역할을 한다.<br><br>
-	xml 설정으로 등록된 Listener가 Root WebApplicationContext를 만들더라도, DispatcherServlet은 Root ApplicationContext를 상속받는 WebApplicationContext를 새로 만든다.<br><br>
-	Root WebApplicationContext는 다른 Servlet도 공용으로 사용하고, DispatcherServlet이 생성한 WebApplicationContext은 해당 DispatcherServlet만 사용한다.<br><br>
-	Scope이 다르기 때문에 이러한 상속 구조가 생겨났다.<br><br>
-	Root WebApplicationContext는 다른 DispatcherServlet도 사용하기 때문에 Web과 관련된 Bean은 없고 공용 자원들이 존재한다.<br><br>
-	Servlet WebApplicationContext에는 해당 DispatcherServlet에 한정적인 Web 관련 자원이 존재한다.<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
