Servlet
-------

-	Java 엔터프라이즈 에디션은 웹 애플리케이션 개발용 스팩과 API 제공한다.<br><br>
-	요청 당 쓰레드를 사용한다.<br><br>
-	그 중에 가장 중요한 클래스중 하나가 HttpServlet이다.<br><br>

-	Servlet 등장 이전에 사용하던 기술인 CGI(Common Gateway Interface)는 요청 당 프로세스를 만들어 사용했다.<br><br>

-	따라서 Servlet은 CGI에 비해 빠르고, 플랫폼 독립적이며 보안 및 이식성이 우수하다.<br><br>

Servlet Engine or Container
---------------------------

-	세션 관리.<br><br>
-	네트워크 서비스.<br><br>
-	MIME 기반 메시지 인코딩 디코딩.<br><br>
-	Servlet 생명주기 관리.<br><br>
-	...<br><br>

Servlet Lifecycle
-----------------

-	Servlet 컨테이너가 Servlet 인스턴스의 `init()` 메소드를 호출하여 초기화 한다.<br><br>
-	최초 요청을 받았을 때 한번 초기화 하고 나면 그 다음 요청부터는 이 과정을 생략한다.<br><br>
-	Servlet이 초기화 된 다음부터 클라이언트의 요청을 처리할 수 있다.
-	각 요청은 별도의 쓰레드로 처리하고 이때 Servlet 인스턴스의 `service()` 메소드를 호출한다.<br><br>
-	이 안에서 HTTP 요청을 받고 클라이언트로 보낼 HTTP 응답을 만든다.<br><br>
-	`service()`는 보통 HTTP Method에 따라 `doGet()`, `doPost()` 등으로 처리를 위임한다.<br><br>
-	따라서 보통 `doGet()` 또는 `doPost()`를 구현한다.
-	Servlet 컨테이너 판단에 따라 해당 Servlet을 메모리에서 내려야 할 시점에 `destroy()`를 호출한다.<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
