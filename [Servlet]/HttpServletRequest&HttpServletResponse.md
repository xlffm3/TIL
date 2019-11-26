HttpServletRequest & HttpServletResponse
----------------------------------------

---

<br>

### Request & Response<br><br>

> -	WAS는 웹 브라우저로부터 Servlet 요청을 받으면<br><br>
> -	1. 요청할 때 가지고 있는 정보를 HttpServletRequest 객체를 생성하여 저장함.<br><br>
> -	2. 웹 브라우저에게 응답을 보낼 때 사용하기 위해, HttpServletResponse 객체를 생성함.<br><br>
> -	3. 생성된 HttpServletRequest, HttpServletResponse 객체를 서블릿에 전달함.

<br><br>

### HttpServletRequest<br><br>

> -	HTTP 프로토콜의 request 정보를 서블릿에게 전달하기 위한 목적.<br><br>
> -	헤더정보, 파라미터, 쿠키, URL, URL의 Parameter 정보 등을 읽어 들이는 메소드를 가짐.<br><br>
> -	Body의 Stream을 읽어 들이는 메소드를 가짐.

<br><br>

### HttpServletResponse<br><br>

> -	WAS는 어떤 클라이언트가 요청을 보냈는지 알고 있고, 해당 클라이언트에게 응답을 보내기 위한 HttpServletResponse 객체를 생성하여 서블릿에게 전달.<br><br>
> -	서블릿은 해당 객체를 이용하여 content type, 응답코드, 응답 메시지 등을 전송함.

<br><br>

[Reference] : [Edwith](https://www.edwith.org/boostcourse-web/lecture/16689/)

---
