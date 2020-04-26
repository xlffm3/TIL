DispatcherServlet 초기화
------------------------

-	다음의 특별한 타입의 Bean들을 찾거나, 기본 전략에 해당하는 Bean들을 등록한다.<br><br>
-	HandlerMapping : 핸들러를 찾아주는 인터페이스.<br><br>
-	HandlerAdapter : 핸들러를 실행하는 인터페이스.<br><br>
-	HandlerExceptionResolver.<br><br>
-	ViewResolver.<br><br>
-	...<br><br>

DispatcherServlet 동작 순서
---------------------------

1.	(로케일, 테마, 멀티파트 등) 요청을 분석한다.<br><br>
2.	(핸들러 맵핑에게 위임하여) 요청을 처리할 핸들러를 찾는다.
	-	기본적으로 2개의 핸들러 맵핑이 등록되어 있다.<br><br>
3.	(등록되어 있는 핸들러 어댑터 중에) 해당 핸들러를 실행할 수 있는 “핸들러 어댑터”를 찾는다.<br><br>
4.	찾아낸 “핸들러 어댑터”를 사용해서 핸들러의 응답을 처리한다.
	-	핸들러의 리턴값을 보고 어떻게 처리할지 판단한다.
		-	뷰 이름에 해당하는 뷰를 찾아서 모델 데이터를 랜더링한다.
		-	@ResponseEntity가 있다면 Converter를 사용해서 응답 본문을 만든다.<br><br>
5.	(부가적으로) 예외가 발생했다면, 예외 처리 핸들러에 요청 처리를 위임한다.<br><br>
6.	최종적으로 응답을 보낸다.<br><br>

@ResponseEntity가 있는 경우
---------------------------

-	RestController는 일반적인 Controller의 Mapping 메소드에 @ResponseBody Annotation이 추가된 Controller이다.<br><br>

-	이 경우 ModelAndView는 null이다.<br><br>

-	HandlerMapping : RequestMappingHandlerMapping.<br><br>

-	HandlerAdapter : RequestMappingHandlerAdapter.<br><br>

View가 있는 경우
----------------

> SimpleController.java

```java
@org.springframework.stereotype.Controller("/simple")
public class SimpleController implements Controller {
      @Override
      public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse
      response) throws Exception {
            return new ModelAndView("/WEB-INF/simple.jsp");
      }
}
```

-	HandlerMapping : BeanNameUrlHandlerMapping.<br><br>
-	HandlerAdapter : SimpleControllerHandlerAdapter.<br><br>

Custom ViewResolver
-------------------

> WebConfig.java

```java
@Configuration
@ComponentScan
public class WebConfig {
    @Bean
    public InternalResourceViewResolver viewResolver() {
        InternalResourceViewResolver viewResolver = new InternalResourceViewResolver();
        viewResolver.setPrefix("/WEB-INF/");
        viewResolver.setSuffix(".jsp");
        return viewResolver;
    }
}
```

<br>

> SimpleController.java

```java
@org.springframework.stereotype.Controller("/simple")
public class SimpleController implements Controller {
      @Override
      public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse
      response) throws Exception {
            return new ModelAndView("simple");
      }
}
```

-	ViewResolver 및 HandlerMapping 등을 별도로 등록해놓으면 Custom한 Bean들을 사용하며, 없을 경우 기본 전략으로 설정된 기능들을 이용한다.<br><br>

-	위 예제는 ViewResolver에서 suffix와 prefix를 정의한 Custom ViewResolver이다.<br><br>

-	ViewResolver : InternalResourceViewResolver.<br><br>

-	InternalResourceViewResolver : Prefix & Suffix.<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
