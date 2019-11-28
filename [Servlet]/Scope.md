Scope
-----

---

<br>

### Scope<br><br>

> -	변수가 어떤 범위 내에서 사용될 수 있는 지를 결정하는 단위.<br><br>
> -	Page : 페이지 내에서 지역 변수처럼 사용된다.<br><br>
> -	Request : HTTP 요청을 WAS가 받아서 웹 브라우저에게 응답할 때 까지 변수가 유지되는 경우 사용한다.
> 	-	Forward의 경우, Servlet은 서로 Page가 다르지만 공유하는 것을 생각해보면 쉽다.<br><Br>
> -	Session : 웹 브라우저 별로 변수가 관리되는 경우 사용한다.
> 	-	세션 객체가 생성되서 소멸할 때 까지 유지되기 때문에, 요청이 하나가 아니라 여러 개여도 계속 남아있는 scope이다.<br><br>
> -	Application : 웹 어플리케이션이 시작되고 종료될 때 까지 변수가 유지되는 경우 사용한다.<br><br>
> -	값을 저장할 때는 request 객체의 setAttribute()메소드를 , 값을 읽어 들일 때는 request 객체의 getAttribute()메소드를 사용한다.

<br><br>

![image](https://user-images.githubusercontent.com/56240505/69784645-9dc9d300-11f9-11ea-8b7a-99b94a75780a.png)

<br><br>

### Page Scopre<br><br>

> -	PageContext 추상 클래스를 사용한다.<br><br>
> -	forward가 될 경우 해당 Page scope에 지정된 변수는 사용할 수 없다.<br><br>
> -	지역 변수처럼 해당 JSP나 서블릿이 실행되는 동안에만 정보를 유지하고자 할 때 사용된다.<br><br>
> -	JSP에서 pageScope에 값을 저장한 후 해당 값을 EL표기법 등에서 사용할 때 사용된다.<br><br>

<br><br>

### Request Scope<br><br>

> -	http 요청을 WAS가 받아서 웹 브라우저에게 응답할 때까지(예 : Forward) 변수값을 유지하고자 할 경우 사용한다.<br><br>
> -	Servlet에서는 HttpServletRequest 객체를 사용한다.<br><br>
> -	JSP에서는 request 내장 변수를 사용한다.<br><br>

<br><br>

### Session Scope<br><br>

> -	웹 브라우저별로 변수를 관리하고자 할 경우 사용한다.<br><br>
> -	클라이언트(웹 브라우저)마다 하나의 객체를 만들어 관리하는 것이 세션이다.<br><br>
> -	웹 브라우저간의 탭 간에는 세션정보가 공유되기 때문에, 각각의 탭에서는 같은 세션정보를 사용할 수 있다.<br><br>
> -	HttpSession 인터페이스를 구현한 객체를 사용한다.<br><br>
> -	JSP에서는 session 내장 변수를 사용한다.<br><br>
> -	서블릿에서는 HttpServletRequest의 getSession()메소드를 이용하여 session 객체를 얻는다.<br><br>
> -	장바구니처럼 사용자별로 유지가 되어야 할 정보가 있을 때 등 상태 유지에 사용한다.

<br><br>

### Application Scope<br><br>

> -	하나의 서버에는 여러 개의 웹 어플리케이션이 존재할 수 있다.<br><br>
> -	웹 어플리케이션이 시작되고 종료될 때까지 변수를 사용할 수 있다.<br><br>
> -	ServletContext 인터페이스를 구현한 객체를 사용한다.<br><br>
> -	JSP에서는 application 내장 객체를 이용한다.<br><br>
> -	서블릿의 경우는 getServletContext()메소드를 이용하여 application객체를 이용한다.<br><br>
> -	웹 어플리케이션 하나당 하나의 application객체가 사용된다.<br><br>
> -	모든 클라이언트가 공통으로 사용해야 할 값들이 있을 때 사용한다.

<br><br>

[Reference]

-	[APPLICATION, REQUEST, SESSION AND PAGE SCOPES IN SERVLETS AND JSPS](https://www.javajee.com/application-request-session-and-page-scopes-in-servlets-and-jsps)

---
