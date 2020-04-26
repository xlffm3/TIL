Servlet Listener
----------------

-	웹 애플리케이션에서 발생하는 주요 이벤트를 감지하고 각 이벤트에 특별한 작업이 필요한 경우에 사용할 수 있다.<br><br>
-	ServletContextListener 클래스를 상속받아 사용한다.<br><br>
-	서블릿 컨텍스트 수준의 이벤트.
	-	컨텍스트 라이프사이클 이벤트.
	-	컨텍스트 애트리뷰트 변경 이벤트.<br><br>
-	세션 수준의 이벤트.
	-	세션 라이프사이클 이벤트.
	-	세션 애트리뷰트 변경 이벤트.<br><br>

Servlet Filter
--------------

![image](https://user-images.githubusercontent.com/56240505/80300470-15675280-87d8-11ea-94d5-511dcae0448b.png)<br><br>

-	들어온 요청을 서블릿으로 보내고, 또 서블릿이 작성한 응답을 클라이언트로 보내기 전에 특별한 처리가 필요한 경우에 사용할 수 있다.<br><br>
-	체인 형태의 구조이다.<br><br>
-	Filter 클래스를 상속받아 사용한다.<br><br>

Servlet 관련 코드
-----------------

> web.xml

```xml
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >
<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <filter>
    <filter-name>myFilter</filter-name>
    <filter-class>com.glenn.MyFilter</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>myFilter</filter-name>
    <servlet-name>hello</servlet-name>
  </filter-mapping>
  <listener>
    <listener-class>com.glenn.MyListener</listener-class>
  </listener>
  <servlet>
    <servlet-name>hello</servlet-name>
    <servlet-class>com.glenn.HelloServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>/hello</url-pattern>
  </servlet-mapping>
</web-app>
```

<br>

> MyListener.java

```Java
public class MyListener implements ServletContextListener {

    @Override
    public void contextInitialized(ServletContextEvent servletContextEvent) {
        System.out.println("context init");
        servletContextEvent.getServletContext().setAttribute("name", "jipark");
    }

    @Override
    public void contextDestroyed(ServletContextEvent servletContextEvent) {
        System.out.println("context destoryed");
    }
}
```

<br>

> MyFilter.java

```Java
public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("filter init");
    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("filter");
        filterChain.doFilter(servletRequest, servletResponse);
    }

    @Override
    public void destroy() {
        System.out.println("filter destroyed");
    }
}
```

<br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
