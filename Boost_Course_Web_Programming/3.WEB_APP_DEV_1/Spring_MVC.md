Spring MVC
----------

---

### MVC(Model-View-Controller)<br>

-	원래는 제록스 연구소에서 일하던 트뤼그베 린즈커그가 처음으로 소개한 개념으로, 데스트톱 어플리케이션용으로 고안되었다.<br><br>
-	**Model** : 모델은 뷰가 렌더링하는데 필요한 데이터이며, 예를 들어 사용자가 요청한 상품 목록이나 주문 내역이 이에 해당한다.<br><br>
-	**View** : 웹 애플리케이션에서 뷰(View)는 실제로 보이는 부분이며, 모델을 사용해 렌더링을 하며, 뷰는 JSP, JSF, PDF, XML등으로 결과를 표현한다.<br><br>
-	**Controller** : 컨트롤러는 사용자의 액션에 응답하는 컴포넌트이며, 컨트롤러는 모델을 업데이트하고, 다른 액션을 수행한다.<br><br>

### MVC Model 1 Architecture<br>

![image](https://user-images.githubusercontent.com/56240505/70532258-04f06b80-1b9a-11ea-9add-280ec58a252d.png)<br><br>

-	JSP 자체에 JAVA 코드와 HTML 코드가 섞여 유지 보수가 어렵다.<br><br>

### MVC Model 2 Architecture<br>

![image](https://user-images.githubusercontent.com/56240505/70532518-8ea03900-1b9a-11ea-8850-a78ebf61660f.png)<br><br> ![image](https://user-images.githubusercontent.com/56240505/70532646-d1faa780-1b9a-11ea-8c5e-2e4fc37ac5ce.png)<br><br>

-	로직과 뷰를 분리한다.<br><br>
-	클라이언트의 요청은 Front Controller Servlet이 받으며, Front Controller는 단 1개만 존재한다.<br><br>
-	Front Controller는 Controller Class(a.k.a Handler Class)에 요청을 위임한다.<br><br>
-	Servlet은 관련 요청을 처리하기에 구조가 다소 불편하기 때문에, 실제 처리는 위임함으로써 관련된 URL을 하나의 클래스에서 다 처리할 수 있도록 한다.<br><br>
-	만들어진 결과를 모델에 담아 Front Controller에 보내면, 알맞은 뷰에 이를 전달하여 결과를 출력한다.<br><br>

### Spring Web Module<br>

![image](https://user-images.githubusercontent.com/56240505/70533016-8694c900-1b9b-11ea-91e9-d2615f27875f.png)<br><br>

-	Model 2의 발전된 형태가 Spring Framework의 Web Module에 구현되어 있으며, 이를 보통 Spring MVC라고 한다.<br><br>

### Front Controller로 얻는 이점<br>

-	하나의 웹 어플리케이션은 많은 뷰와 컨트롤러가 존재하는데, 각각의 뷰와 컨트롤러가 독립적으로 연결되면 서버 입장에서는 웹 어플리케이션 실행에 대해 일괄적으로 처리하기 어렵다.<br><br>
-	**Front Controller Design Pattern** : Front Controller를 두고 뷰에서 들어오는 모든 요청을 담당하여 웹 어플리케이션을 실행하는 모든 요청을 일괄적으로 처리하는 구조이다.<br><br>

### Spring MVC Flowchart<br>

![image](https://user-images.githubusercontent.com/56240505/70580578-a8bc3480-1bf7-11ea-8766-15f426a0df12.png)<br><br>

-	XML 혹은 Annotation으로 설정한 컨트롤러 및 메서드 정보를 Handler Mapping 객체가 생성되어 관리한다. <br><br>
-	Handler Adapter가 실행한 뒤 그 결과를 Model에 받아 다시 Dispatcher Servlet에게 전달한다.<br><br>

### Dispatcher Servlet<br>

-	Front Controller로서, 클라이언트의 모든 요청을 받은 후 이를 처리할 핸들러에게 넘기고 핸들러가 처리한 결과를 받아 사용자에게 응답 결과를 보여준다.<br><br>
-	여러 컴포넌트를 이용해 작업을 처리한다.<br><br>

### Dispatcher Servlet Flowchart<br>

![image](https://user-images.githubusercontent.com/56240505/70581656-45340600-1bfb-11ea-9076-3cdcdc53b8e9.png)<br><br>

### Dispatcher Servlet Flowchart : 요청 선처리 작업<br>

![image](https://user-images.githubusercontent.com/56240505/70581704-73194a80-1bfb-11ea-9b81-1f5a601d702c.png)<br><br>

-	org.springframework.web.servlet.LocaleResolver
	-	지역 정보를 결정해주는 전략 오브젝트이다.
	-	디폴트인 AcceptHeaderLocalResolver는 HTTP 헤더의 정보를 보고 지역정보를 설정하는 지역화 작업을 수행한다.<br><br>
-	org.springframework.web.servlet.FlashMapManager
	-	FlashMap객체를 조회(retrieve) & 저장을 위한 인터페이스이다.
	-	RedirectAttributes의 addFlashAttribute메소드를 이용해서 저장한다.
	-	리다이렉트 시 ? 및 파라미터로 정보를 전달하는데 URL의 복잡성 증가 및 길이 제한 등이 있기 때문에, FlashMap 객체로 값을 한 번 유지시킬 수 있게 해준다.
	-	리다이렉트 후 조회를 하면 바로 정보는 삭제된다.<br><br>
-	org.springframework.web.context.request.RequestContextHolder
	-	일반 빈에서 HttpServletRequest, HttpServletResponse, HttpSession 등을 사용할 수 있도록 한다.
	-	해당 객체를 일반 빈에서 사용하게 되면, Web에 종속적이 될 수 있다.<br><br>
-	org.springframework.web.multipart.MultipartResolver
	-	멀티파트 파일 업로드를 처리하는 전략<br><br>

### Dispatcher Servlet Flowchart : 요청 전달<br>

![image](https://user-images.githubusercontent.com/56240505/70582442-ea4fde00-1bfd-11ea-83d4-45ff2bb999dc.png)<br><br>

-	org.springframework.web.servlet.HandlerMapping
	-	HandlerMapping구현체는 어떤 핸들러가 요청을 처리할지에 대한 정보를 알고 있다.
	-	디폴트로 설정되는 있는 핸들러매핑은 BeanNameHandlerMapping과 DefaultAnnotationHandlerMapping 2가지가 설정되어 있다.<br><br>
-	org.springframework.web.servlet.HandlerExecutionChain
	-	HandlerExecutionChain구현체는 실제로 호출된 핸들러에 대한 참조를 가지고 있다.
	-	즉, 무엇이 실행되어야 될지 알고 있는 객체라고 말할 수 있으며, 핸들러 실행전과 실행후에 수행될 HandlerInterceptor도 참조하고 있다.<br><br>
-	org.springframework.web.servlet.HandlerAdapter
	-	실제 핸들러를 실행하는 역할을 담당한다.
	-	핸들러 어댑터는 선택된 핸들러를 실행하는 방법과 응답을 ModelAndView로 변화하는 방법에 대해 알고 있다.
	-	디폴트로 설정되어 있는 핸들러어댑터는 HttpRequestHandlerAdapter, SimpleControllerHandlerAdapter, AnnotationMethodHanlderAdapter 3가지이다.
	-	@RequestMapping과 @Controller 애노테이션을 통해 정의되는 컨트롤러의 경우 DefaultAnnotationHandlerMapping에 의해 핸들러가 결정되고, 그에 대응되는 AnnotationMethodHandlerAdapter에 의해 호출이 일어난다.<br><br>

### Dispatcher Servlet Flowchart : 요청 처리<br>

![image](https://user-images.githubusercontent.com/56240505/70582711-aad5c180-1bfe-11ea-8838-43f75ddb5ae6.png)<br><br>

-	org.springframework.web.servlet.ModelAndView
	-	ModelAndView는 Controller의 처리 결과를 보여줄 view와 view에서 사용할 값을 전달하는 클래스이다.
	-	request 객체 등을 이용하면 Spring에 종속되니, ModelAndView와 같은 컴포넌트를 이용하는 것이 바람직하다. <br><br>
-	org.springframework.web.servlet.RequestToViewNameTranslator
	-	컨트롤러에서 뷰 이름이나 뷰 오브젝트를 제공해주지 않았을 경우 URL과 같은 요청정보를 참고해서 자동으로 뷰 이름을 생성해주는 전략 오브젝트이다.
	-	디폴트는 DefaultRequestToViewNameTranslator이다.<br><br>

### Dispatcher Servlet Flowchart : 예외 처리<br>

![image](https://user-images.githubusercontent.com/56240505/70582917-79a9c100-1bff-11ea-9c0d-3c78735e8e52.png)<br><br>

-	org.springframework.web.servlet.handlerexceptionresolver
	-	기본적으로 DispatcherServlet이 DefaultHandlerExceptionResolver를 등록한다.
	-	HandlerExceptionResolver는 예외가 던져졌을 때 어떤 핸들러를 실행할 것인지에 대한 정보를 제공한다.<br><br>

### Dispatcher Servlet Flowchart : 뷰 렌더링<br>

![image](https://user-images.githubusercontent.com/56240505/70583000-b970a880-1bff-11ea-8f00-3214a9bce6db.png)<br><br>

-	org.springframework.web.servlet.ViewResolver
	-	컨트롤러가 리턴한 뷰 이름을 참고해서 적절한 뷰 오브젝트를 찾아주는 로직을 가진 전략 오프젝트이다.
	-	뷰의 종류에 따라 적절한 뷰 리졸버를 추가로 설정해줄 수 있다.<br><br>

### Dispatcher Servlet Flowchart : 요청 처리 종료<br>

![image](https://user-images.githubusercontent.com/56240505/70583198-34d25a00-1c00-11ea-8b62-29281403dd6d.png)<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16762/)<br>
-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16763/)

---
