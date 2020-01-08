Spring MVC Practice
-------------------

---

DispatcherServlet을 FrontController로 설정하기
----------------------------------------------

-	web.xml 파일에 설정한다.<br><br>
-	javax.servlet.ServletContainerInitializer를 사용하는 방법이 있으나, Servlet 3.0 spec 이상은 web.xml로 대신할 수 있다.<br><br>
-	org.springframework.web.WebApplicationInitializer 인터페이스를 구현해서 사용한다.<br><br>

Spring MVC 설정
---------------

![image](https://user-images.githubusercontent.com/56240505/70590475-28f19280-1c16-11ea-928e-026cc7341050.png)<br><br>

-	DispatcherServlet은 해당 설정 파일을 읽어들이고 내부적으로 Spring의 컨테이너인 ApplicationContext를 생성한다.<br><br>
-	@Configuration : 애노테이션과 Bean 애노테이션 코드를 이용하여 스프링 컨테이너에 새 로운 빈 객체를 제공할 수 있다.<br><br>
-	@EnableWebMvc
	-	DispatcherServlet의 RequestMappingHandlerMapping, RequestMappingHandlerAdapter, ExceptionHandlerExceptionResolver, MessageConverter 등 Web에 필요한 빈들을 대부분 자동으로 설정해준다.
	-	xml로 설정의 mvc:annotation-driven/ 와 동일하다.
	-	기본 설정 이외의 설정이 필요하다면 WebMvcConfigurerAdapter 를 상속받도록 Java config class를 작성한 후, 필요한 메소드를 오버라이딩 하도록 한다.
	-	DelegatingWebMvcConfiguration을 import하며, DelegatingWebMvcConfiguration는 WebMvcCOnfigurationSupport를 상속받고 있다.
	-	[관련 소스코드](https://github.com/spring-projects/spring-framework/blob/master/spring-webmvc/src/main/java/org/springframework/web/servlet/config/annotation/WebMvcConfigurationSupport.java)<br><br>
-	@ComponentScan
	-	ComponentScan애노테이션을 이용하면 Controller, Service, Repository, Component애노테이션이 붙은 클래스를 찾아 스프링 컨테이너가 관리하게 된다.
	-	DefaultAnnotationHandlerMapping과 RequestMappingHandlerMapping구현체는 다른 핸드러 매핑보다 훨씬 더 정교한 작업을 수행한다.
	-	이 두 개의 구현체는 애노테이션을 사용해 매핑 관계를 찾는 매우 강력한 기능을 가지고 있다.
	-	이들 구현체는 스프링 컨테이너 즉 애플리케이션 컨텍스트에 있는 요청 처리 빈에서 RequestMapping애노테이션을 클래스나 메소드에서 찾아 HandlerMapping객체를 생성하게 된다.
	-	HandlerMapping은 서버로 들어온 요청을 어느 핸들러로 전달할지 결정하는 역할을 수행한다.
	-	DefaultAnnotationHandlerMapping은 DispatcherServlet이 기본으로 등록하는 기본 핸들러 맵핑 객체이고, RequestMappingHandlerMapping은 더 강력하고 유연하지만 사용하려면 명시적으로 설정해야 한다.<br><br>
-	WebMvcConfigurerAdapter : @EnableWebMvc 를 이용하면 기본적인 설정이 모두 자동으로 되지만, 기본 설정 이외의 설정이 필요할 경우 해당 클래스를 상속 받은 후, 메소드를 오버라이딩 하여 구현한다<br><br>

Controller(Handler) Class 작성하기
----------------------------------

-	@Controller Annotation을 추가한다.<br><br>
-	맵핑을 위해 @RequestMapping Annotation을 클래스나 메소드에서 사용한다.<br><br>
-	@RequestMapping : Http 요청과 이를 다루기 위한 Controller 의 메소드를 연결하는 어노테이션.<br><br>
-	Http Method 와 연결하는 방법
	-	@RequestMapping(value="/users", method=RequestMethod.POST)
	-	From Spring 4.3 version (@GetMapping, @PostMapping, @PutMapping, @DeleteMapping, @PatchMapping)<br><br>
-	Http 특정 해더와 연결하는 방법
	-	@RequestMapping(method = RequestMethod.GET, headers = "content-type=application/json")<br><br>
-	Http Parameter 와 연결하는 방법
	-	@RequestMapping(method = RequestMethod.GET, params = "type=raw")<br><br>
-	Content-Type Header 와 연결하는 방법
	-	@RequestMapping(method = RequestMethod.GET, consumes = "application/json")<br><br>
-	Accept Header 와 연결하는 방법
	-	@RequestMapping(method = RequestMethod.GET, produces = "application/json")<br><br>

Practice 1 : 기본 설정
----------------------

> WebMvcContextConfiguration.java

```java
@Configuration
@EnableWebMvc
@ComponentScan(basePackages = { "kr.or.connect.mvcexam.controller" })
public class WebMvcContextConfiguration extends WebMvcConfigurerAdapter {
    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/assets/**").addResourceLocations("classpath:/META-INF/resources/webjars/").setCachePeriod(31556926);
        registry.addResourceHandler("/css/**").addResourceLocations("/css/").setCachePeriod(31556926);
        registry.addResourceHandler("/img/**").addResourceLocations("/img/").setCachePeriod(31556926);
        registry.addResourceHandler("/js/**").addResourceLocations("/js/").setCachePeriod(31556926);
    }

    // default servlet handler를 사용하게 합니다.
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }

    @Override
    public void addViewControllers(final ViewControllerRegistry registry) {
            System.out.println("addViewControllers가 호출됩니다. ");
        registry.addViewController("/").setViewName("main");
    }

    @Bean
    public InternalResourceViewResolver getInternalResourceViewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        return resolver;
    }
}
```

-	@EnableWebMvc를 이용하면 기본적인 설정이 모두 자동으로 되지만, 기본 설정 이외의 설정이 필요할 경우 WebMvcConfigurerAdapter 클래스를 상속 받은 후 메소드를 오버라이딩하여 구현한다.<br><br>
-	WebMvcContextConfiguration 클래스는 DispatcherServlet이 실행될 때 읽어들이는 설정 파일이다.<br><br>
-	web.xml의 url-pattern **"/"** 때문에 컨트롤러의 URL이 매핑되어 있는 요청뿐만 아니라 CSS, JS, Img 등의 요청까지 전부 받아들인다.<br><br>
-	`addResourceHandler()` : img, css, js 등로 시작하는 URL 요청은 어플리케이션 루트 디렉터리 밑에 있는 각각의 디렉토리에 놓고 요청이 오면 해당 경로에서 찾아 알맞게 사용한다는 설정이다.<br><br>
-	이 부분이 없다면 모두 컨트롤러가 가진 RequestMapping에서 찾으려고 하다가 오류가 발생한다.<br><br>
-	`configureDefaultServletHandling()` : 파라미터로 받은 DefaultServletHandlerConfigurer 객체의 enable 메서드를 호출함으로써 DefaultServletHandler를 사용하도록 설정한다.<br><br>
-	매핑 정보가 없는 URL 요청은 Spring의 DefaultServletHttpRequestHandler가 처리한다.<br><br>
-	또한 WAS의 DefaultServlet에게 해당 일을 넘기고, DefaultServlet이 static한 자원을 읽어서 WAS가 보여준다.<br><br>
-	`addViewControllers()` : 특정 URL에 대한 처리를 컨트롤러 클래스를 작성하지 않고 매핑할 수 있도록 해주며, 예제의 경우 요청 "/"가 들어오면 "main"이라고 하는 이름의 뷰로 보여주도록 한다.<br><br>
-	`getInternalResourceViewResolver()` : 메서드에 설정된 형태의 뷰 정보로 뷰를 사용하며, view name은 ViewResolver 객체를 이용해서 찾는다.<br><br>

String MVC가 지원하는 Controller 메서드 인수 타입
-------------------------------------------------

-	javax.servlet.ServletRequest<br>
-	**javax.servlet.http.HttpServletRequest**<br>
-	org.springframework.web.multipart.MultipartHttpServletRequest<br>
-	org.springframework.web.multipart.MultipartRequest<br>
-	javax.servlet.ServletResponse<br>
-	**javax.servlet.http.HttpServletResponse**<br>
-	**javax.servlet.http.HttpSession**<br>
-	org.springframework.web.context.request.WebRequest<br>
-	org.springframework.web.context.request.NativeWebRequest<br>
-	java.util.Locale<br>
-	java.io.InputStream<br>
-	java.io.Reader<br>
-	java.io.Writer<br>
-	java.io.OutputStream<br>
-	javax.security.Principal<br>
-	java.util.Map<br>
-	org.springframework.ui.Model<br>
-	org.springframework.ui.ModelMap<br>
-	javax.servlet.http.Part<br>
-	**org.springframework.web.multipart.MultipartFile**<br>
-	org.springframework.web.servlet.mvc.support.RedirectAttributes<br>
-	org.springframework.validation.Errors<br>
-	org.springframework.validation.BindingResult<br>
-	org.springframework.web.bind.support.SessionStatus<br>
-	org.springframework.web.util.UriComponentsBuilder<br>
-	org.springframework.http.HttpEntity<?><br>
-	Command 또는 Form 객체<br><br>

Spring MVC가 지원하는 메서드 인수 Annotation
--------------------------------------------

-	@RequestParam
	-	Mapping된 메소드의 Argument에 붙일 수 있는 어노테이션.
	-	RequestParam의 name에는 http parameter의 name과 맵핑.
	-	RequestParam의 required는 필수인지 아닌지 판단.<br><br>
-	@RequestHeader
	-	요청 정보의 헤더 정보를 읽어들 일 때 사용.
	-	@RequestHeader(name="헤더명") String 변수명.<br><br>
-	@RequestBody<br><br>
-	@RequestPart<br><br>
-	@ModelAttribute<br><br>
-	@PathVariable
	-	@RequestMapping의 path에 변수명을 입력받기 위한 place holder가 필요함.
	-	place holder의 이름과 PathVariable의 name 값과 같으면 mapping 됨.
	-	required 속성은 default true<br><br>
-	@CookieValue<br><br>

Spring MVC가 지원하는 메서드 리턴 값
------------------------------------

-	**org.springframework.web.servlet.ModelAndView**<br>
-	java.util.Map<br>
-	org.springframework.ui.Model<br>
-	org.springframework.web.servlet.View<br>
-	org.springframework.ui.ModelMap<br>
-	**java.lang.String**<br>
-	java.lang.Void<br>
-	org.springframework.http.HttpEntity<?><br>
-	org.springframework.http.ResponseEntity<?><br>
-	기타 리턴 타입<br><br>

Practice 2 : Controller 작성
----------------------------

```java
@Controller
public class PlusController {
    @GetMapping(path = "/plusform")
    public String plusform() {
        return "plusForm";
    }

    @PostMapping(path = "/plus")
    public String plus(@RequestParam(name = "value1", required = true) int value1,
            @RequestParam(name = "value2", required = true) int value2, ModelMap modelMap) {
        int result = value1 + value2;

        modelMap.addAttribute("value1", value1);
        modelMap.addAttribute("value2", value2);
        modelMap.addAttribute("result", result);
        return "plusResult";
    }
}
```

-	URL 입력은 GET 방식 요청이며, Mapping Annotation으로 받아들일 path를 설정한다.<br><br>
-	메서드로 View Name을 넘겨주는데, ModelAndView 객체 혹은 String으로 리턴한다.<br><br>
-	JSP에서 변수를 사용하기 위해 Request Scope에 넣어주는데, 종속되지 않기 위해 HttpServletRequest가 아닌 ModelMap을 사용했다.<br><br>

Practice 3 : Controller 작성
----------------------------

```java
@Controller
public class UserController {
    @RequestMapping(path="/userform", method=RequestMethod.GET)
    public String userform() {
        return "userform";
    }

    @RequestMapping(path="/regist", method=RequestMethod.POST)
    public String regist(@ModelAttribute User user) {

        System.out.println("사용자가 입력한 user 정보입니다. 해당 정보를 이용하는 코드가 와야합니다.");
        System.out.println(user);
        return "regist";
    }
}
```

-	RequestParam으로 파라미터를 하나 하나 받아서 저장하기 보다는, DTO 성격의 객체를 생성하여 정보를 저장할 수 있는 @ModelAttribute Annotation을 이용했다.<br><br>
-	이름이 동일한 변수 명 설정과 getter와 setter 및 toString 메서드의 오버라이딩이 필요하다.<br><br>

Practice 4 : Controller 작성
----------------------------

```java
@Controller
public class GoodsController {
    @GetMapping("/goods/{id}")
    public String getGoodsById(@PathVariable(name="id") int id,
                               @RequestHeader(value="User-Agent", defaultValue="myBrowser") String userAgent,
                              HttpServletRequest request,
                              ModelMap model
                              ) {

        String path = request.getServletPath();

        System.out.println("id : " + id);
        System.out.println("user_agent : " + userAgent);
        System.out.println("path : " + path);

        model.addAttribute("id", id);
        model.addAttribute("userAgent", userAgent);
        model.addAttribute("path", path);
        return "goodsById";
    }
}
```

-	URL 요청이 들어왔을 때 {id}를 path variable로 받겠다는 의미이다.<br><br>
-	request는 선택적으로 사용이 가능하다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16764/)

---
