Argument Resolver
-----------------

-	컨트롤러의 메소드의 인자로 사용자가 임의의 값을 전달하는 방법을 제공하고자 할 때 사용된다.<br><br>
-	예를 들어, 세션에 저장되어 있는 값 중 특정 이름의 값을 메소드 인자로 전달한다.<br><br>

Argument Resolver 작성 방법
---------------------------

```xml
<mvc:annotation-driven>
     <mvc:argument-resolvers>
         <bean class="아규먼트리졸버클래스"></bean>      
     </mvc:argument-resolvers>
 </mvc:annotation-driven>
```

-	org.springframework.web.method.support.HandlerMethodArgumentResolver를 구현한 클래스를 작성한다.<br><br>
-	`supportsParameter()` 메소드를 오버라이딩하고 원하는 타입의 인자가 있는지 검사한 후 있을 경우 true가 리턴되도록 한다.<br><br>
-	`resolveArgument()` 메소드를 오버라이딩 하고 메소드의 인자로 전달할 값을 리턴한다.<br><br>
-	Java Config 설정을 사용하는 경우, WebMvcConfigurerAdapter를 상속받은 Java Config 파일에서 addArgumentResolvers 메소드를 오버라이딩 한 후 원하는 아규먼트 리졸버 클래스 객체를 등록한다.<br><br>
-	XML 설정의 경우, 위 예제처럼 등록한다.<br><br>

Spring MVC의 기본 Argument Resolver
-----------------------------------

-	`getDefaultArgumentResolvers()` 메소드를 보면 기본으로 설정되는 아규먼트 리졸버에 어떤 것이 있는지 알 수 있다.<br><br>
-	Map객체나 Map을 상속받은 객체는 Spring에서 이미 선언한 아규먼트 리졸버가 처리하기 때문에 전달 할 수 없다.<br><br>
-	Map객체를 전달하려면 Map을 필드로 가지고 있는 별도의 객체를 선언한 후 사용해야 한다.<br><br>
-	[소스코드 바로가기](https://github.com/spring-projects/spring-framework/blob/v5.0.0.RELEASE/spring-webmvc/src/main/java/org/springframework/web/servlet/mvc/method/annotation/RequestMappingHandlerAdapter.java)<br><br>

Map 객체 전달 Practice
----------------------

> HeaderInfo.java<br>

```Java
public class HeaderInfo {
    private Map<String, String> map;

    public HeaderInfo() {
        map = new HashMap<>();
    }

    public void put(String name, String value) {
        map.put(name,  value);
    }

    public String get(String name) {
        return map.get(name);
    }
}
```

-	Map은 Spring이 내부적으로 사용하는 Argument Resolver가 선처리하기 때문에, 직접 사용하는 것은 불가능하며 Map을 필드로 가진 객체를 이용한다.<br><br>

> HeaderMapArgumentResolver.java<br>

```Java
public class HeaderMapArgumentResolver implements HandlerMethodArgumentResolver {

    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        return parameter.getParameterType() == HeaderInfo.class;
    }

    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer,
            NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {

        HeaderInfo headerInfo = new HeaderInfo();

        Iterator<String> headerNames = webRequest.getHeaderNames();
        while(headerNames.hasNext()) {
            String headerName = headerNames.next();
            String headerValue = webRequest.getHeader(headerName);
//          System.out.println(headerName + " , " + headerValue);
            headerInfo.put(headerName, headerValue);
        }

        return headerInfo;
    }
}
```

-	HandlerMethodArgumentResolver 인터페이스를 구현한다.<br><br>
-	Controller 메서드 인자가 4개일 경우, `supportsParameter()` 메서드가 4번 호출된다.<br><br>
-	`supportsParameter()` 가 인자의 정보를 파라미터로 전달하고, 해당 파라미터에 원하는 정보가 있으면 true를 반환한다.<br><br>
-	`supportsParameter()` 가 true일 경우, `resolveArgument()` 가 호출되며, `resolveArgument()` 가 return한 값이 Controller 메서드의 인자로 전달된다.<br><br>

> WebMvcConfigurerAdapter.java<br>

```Java
@Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
            System.out.println("아규먼트 리졸버 등록..");
        argumentResolvers.add(new HeaderMapArgumentResolver());
    }
```

<br>

> GuestbookController.java<br>

```java
@GetMapping(path="/list")
    public String list(@RequestParam(name="start", required=false, defaultValue="0") int start,
                       ModelMap model, @CookieValue(value="count", defaultValue="1", required=true) String value,
                       HttpServletResponse response,
                       HeaderInfo headerInfo) {
        System.out.println(headerInfo.get("user-agent"));       
```

<br>

---

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16806/)
