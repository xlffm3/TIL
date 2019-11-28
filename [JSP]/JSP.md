JSP
---

---

<br>

### JSP(Java Server Page)<br><br>

> -	Java 언어를 기반으로 하는 Server Side Script Language.<br><br>
> -	HTML 코드에 Java 코드를 넣어, 동적인 웹 페이지를 생성하는 웹 어플리케이션 도구.<br><br>
> -	JSP를 통해, 정적인 HTML과 동적으로 생성된 content(HTTP 요청 파라미터)를 혼합하여 사용이 가능하다.

<br><br>

### JSP의 작동 방식<br><br>

> -	1. 브라우저가 WAS에 JSP에 대한 요청 정보를 전달한다.<br><br>
> -	2. (브라우저가 요청한 JSP가 최초로 요청했을 경우에만) JSP가 실행되면, WAS는 내부적으로 JSP 파일을 Java Servlet으로 변환한다.<br><br>
> -	3. Servlet 코드를 컴파일해서 실행 가능한 bytecode로 변환한다. (class 파일 생성)<br><br>
> -	4. Servlet class를 로딩하고 인스턴스를 생성한다.<br><br>
> -	5. 생성 데이터를 웹 페이지와 함께 클라이언트로 응답한다.

<br><br>

### JSP Life Cylce<br><br>

> -	Servlet Life Cycle과 동일하며, 메서드 명만 _jspInit(), _jspService(), _jspDestroy()로 다르다.

<br><br>

![image](https://user-images.githubusercontent.com/56240505/69778544-07d97c80-11e8-11ea-9118-10d0130fcfd2.png)

### JSP Grammar<br><br>

> -	스크립트릿 내부의 Java 코드는 Servlet의 service 메서드 내부의 지역 변수 / 지역 메서드가 된다.<br><br>
> -	선언문의 Java 코드는 service 메서드의 내부가 아닌 외부에 위치하며, 전역 변수 / 전역 메서드가 된다.<br><br>
> -	표현식은 Servlet의 out.print와 같다.<br><br>
> -	주석
> -	- HTML 주석 : 개발자 도구의 소스 보기를 통해 주석 확인이 가능하다.
> -	- JSP 주석
> -	- Java 주석

<br><br>

![image](https://user-images.githubusercontent.com/56240505/69778876-6d7a3880-11e9-11ea-9085-fcd42ac7c466.png)

<br><br>

### JSP Implicit Objects<br><br>

> -	JSP에 입력한 대부분의 코드는 생성되는 Servlet 소스의 _jspService() 메서드 안에 삽입되는 코드로 생성된다.<br><br>
> -	_jspService()에 삽입된 코드의 윗 부분에 미리 선언된 내장 객체들이 있으며, 이를 JSP에 사용 가능하다.

<br><br>

[Reference]

-	[JavaServer Pages(TM) Specification 2.0 Final Release](https://download.oracle.com/otndocs/jcp/jsp-2.0-fr-oth-JSpec/)<br>
-	[Java Server Pages (JSP) Life Cycle](https://beginnersbook.com/2013/05/jsp-tutorial-life-cycle/)<br>
-	[JSP Implicit Objects](https://www.javatpoint.com/jsp-implicit-objects)

---
