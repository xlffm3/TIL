Servlet
-------

---

### Java Web Application<br>

-	WAS에 설치(deploy)되어 동작하는 어플리케이션이다.<br><br>
-	JWA에는 HTML, CSS, Image, Java class(including Servlet, package, Interface, and so on) 및 각종 설정 파일 등이 포함된다.<br><br>

![servlet1](https://user-images.githubusercontent.com/56240505/69651203-13775700-10b3-11ea-8140-b1bc22adc0a8.png)<br><br>

### Servlet<br>

-	JWA의 구성 요소 중 동적인 처리를 하는 프로그램의 역할로서, WAS에 동작하는 Java class이다.<br><br>
-	HttpServlet class를 상속받아야 한다.<br><br>
-	3.0 spec 미만인 경우 web.xml 파일에 등록해야 하며, 그 이상 spec의 경우는 web.xml을 사용하지 않고 Java annotation을 활용한다.<br><br>

![servlet2](https://user-images.githubusercontent.com/56240505/69651204-13775700-10b3-11ea-8889-313f7c5e419c.png)<br><br>

### Servlet Life Cycle<br>

-	1. WAS는 서블릿 요청을 받으면, 해당 서블릿이 메모리에 있는지 확인한다.
	-	(메모리에 없다면) 해당 서블릿 클래스를 메모리에 올리고 `init()` 메소드를 실행한다.<br><br>
-	2. `service()` 메소드를 실행한다.<br><br>
-	3. WAS가 종료되거나, 웹 어플리케이션이 새롭게 갱신(서블릿 수정 등)될 경우 `destroy()` 메소드를 실행한다.<br><br>

### service(request, response) method<br>

-	HttpServlet의 `service()` 메소드는 템플릿 메소드 패턴으로 구현한다.<br><br>
-	Client의 요청이 GET일 경우에는 자신이 가지고 있는 `doGet(request, response)` 호출한다.<br><br>
-	Client의 요청이 Post일 경우에는 자신이 가지고 있는 `doPost(request, response)`를 호출한다.<br><br>
-	서블릿에 URL 주소를 직접 입력하거나, 링크를 클릭하는 것은 GET 방식으로 서버에게 요청하는 것이다.<br><br>
-	같은 URL 매핑이어도 GET or POST 방식 차이에 따라 다른 메서드가 실행된다.<br><br>

### Request & Response<br>

-	WAS가 웹 브라우저로부터 Servlet 요청을 받으면?
	-	1. 요청할 때 가지고 있는 정보를 HttpServletRequest 객체를 생성하여 저장한다.
	-	2. 웹 브라우저에게 응답을 보낼 때 사용하기 위해, HttpServletResponse 객체를 생성한다.
	-	3. 생성된 HttpServletRequest, HttpServletResponse 객체를 서블릿에 전달한다.<br><br>

### HttpServletRequest<br>

-	HTTP 프로토콜의 request 정보를 서블릿에게 전달하기 위한 목적이다.<br><br>
-	헤더정보, 파라미터, 쿠키, URL, URL의 Parameter 정보 등을 읽어 들이는 메소드를 가진다.<br><br>
-	Body의 Stream을 읽어 들이는 메소드를 지녔다.<br><br>

### HttpServletResponse<br>

-	WAS는 어떤 클라이언트가 요청을 보냈는지 알고 있고, 해당 클라이언트에게 응답을 보내기 위한 HttpServletResponse 객체를 생성하여 서블릿에게 전달한다.<br><br>
-	서블릿은 해당 객체를 이용하여 content type, 응답코드, 응답 메시지 등을 전송한다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16688/)

---
