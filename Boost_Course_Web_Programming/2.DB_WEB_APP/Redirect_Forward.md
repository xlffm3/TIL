Redirect & Forward
------------------

---

Redirect
--------

![image](https://user-images.githubusercontent.com/56240505/69779270-c39bab80-11ea-11ea-8f71-abb70936bd79.png)<br><br>

-	HTTP 프로토콜로 정해진 규칙이다.<br><br>
-	서버는 클라이언트로부터 요청을 받은 후, 클라이언트에게 특정 URL로 이동하라고 요청한다.<br><br>
-	서버에서 클라이언트에게 응답으로 상태 코드 302와 함께 이동할 URL 정보를 location 헤더에 담아 전송한다.<br><br>
-	클라이언트는 서버로부터 받은 상태 코드 값이 302면 location 헤더 값으로 재요청을 보내고, 브라우저의 주소창이 해당 전송 url로 변경된다.<br><br>
-	Redirect는 HttpServletResponse가 가진 `sendRedirect()` 메소드를 사용한다.<br><br>
-	Redirect는 서버에게 요청을 2번하기 때문에, 각 요청마다 서로 다른 request, response 객체를 이용한다.<br><br>

Forward
-------

![image](https://user-images.githubusercontent.com/56240505/69779332-ffcf0c00-11ea-11ea-802f-6270b399c912.png)<br><br>

-	클라이언트가 Servlet1에게 요청을 보낸다.<br><br>
-	Servlet1은 요청 처리 후, 결과를 request 객체에 저장한다.<br><br>
-	Servlet1은 request, response를 Servlet2에게 위임(forward)한다.<br><br>
-	Servlet2는 1에게 받은 객체를 활용하여 요청을 처리하고, 웹 브라우저에게 결과를 전송한다.<br><br>
-	서블릿이 일을 일부만 처리한 뒤 일부는 위임하는 것을 Forward라고 하며, 요청이 1번 밖에 일어나지 않는 점에서 Redirect와 차이가 있다.<br><br>
-	Forward 경로는 반드시 **" / "** 로 시작하며, 같은 웹 어플리케이션에서만 가능하다.<br><br>

Servlet & JSP 연동
------------------

-	Servlet은 자바이기 때문에 IDE 지원 등 프로그램 로직 수행에 있어서 유리하다.<br><br>
-	JSP는 HTML을 단순히 입력해도 되는 등, 결과 출력에 있어서 유리하다.<br><br>
-	Servlet에서 프로그램 로직을 수행하고, 그 결과를 JSP로 Forward하여 결과를 출력하는 연동 방법이 있다.<br><br>

Redirect 참고 코드
------------------

> redirect01.jsp<br>

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%
    response.sendRedirect("redirect02.jsp");
%>    
```

<br>

> redirect02.jsp<br>

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
redirect된 페이지 입니다.
</body>
</html>
```

<br>

Forward 참고 코드
-----------------

> FrontServlet.java<br>

```java
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

            int diceValue = (int)(Math.random() * 6) + 1;
            request.setAttribute("dice", diceValue);

            RequestDispatcher requestDispatehcer = request.getRequestDispatcher("/next");
            requestDispatehcer.forward(request, response);
    }
```

<br>

> NextServlet.java<br>

```java
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       response.setContentType("text/html");
       PrintWriter out = response.getWriter();
       out.println("<html>");
       out.println("<head><title>form</title></head>");
       out.println("<body>");

       int dice = (Integer)request.getAttribute("dice");
       out.println("dice : " + dice);
       for(int i = 0; i < dice; i++) {
           out.print("<br>hello");
       }
       out.println("</body>");
       out.println("</html>");
   }
```

<br>

Servlet & JSP 연동 참고 코드
----------------------------

> LogicServlet.java<br>

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        int v1 = (int)(Math.random() * 100) + 1;
        int v2 = (int)(Math.random() * 100) + 1;
        int result = v1 + v2;

        request.setAttribute("v1", v1);
        request.setAttribute("v2", v2);
        request.setAttribute("result", result);

        RequestDispatcher requestDispatcher = request.getRequestDispatcher("/result.jsp");
        requestDispatcher.forward(request, response);
    }
```

<br>

> result.jsp<br>

```javascript
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
EL표기법으로 출력합니다.<br>
${v1} + ${v2} = ${result} <br><br>

스클립틀릿과 표현식을 이용해 출력합니다.<br>
<%
    int v1 = (int)request.getAttribute("v1");
    int v2 = (int)request.getAttribute("v2");
    int result = (int)request.getAttribute("result");
%>

<%=v1%> + <%=v2 %> = <%=result %>
</body>
</html>
```

<br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16706/)

---
