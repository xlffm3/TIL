EL
--

---

EL(Expression Language)
-----------------------

-	값을 표현하는 데 사용되는 스크립트 언어로서, JSP의 기본 문법을 보완하는 역할을 한다.<br><br>

EL 제공 기능
------------

-	JSP의 스코프(scope)에 맞는 속성을 사용한다.<br><br>
-	집합 객체에 대한 접근 방법을 제공한다.<br><br>
-	수치 연산, 관계 연산, 논리 연산자를 제공한다.<br><br>
-	표현언어만의 기본 객체를 제공한다.<br><br>
-	자바 클래스 메소드 호출 기능을 제공한다.<br><br>

표현 방법
---------

![image](https://user-images.githubusercontent.com/56240505/69811767-1cdafd80-1232-11ea-99a2-03c1d6405554.png)<br><br>

기본 객체
---------

![image](https://user-images.githubusercontent.com/56240505/69811823-472cbb00-1232-11ea-9953-f3fac311493d.png)<br><br>

문법 유의사항
-------------

-	데이터 타입이나 연산자는 일반 언어와 크게 다를 것이 없으며, 예외 또한 "와 \는 \를 함께 사용하는 것이 동일하다.<br><br>
-	HTML 태그와 혼동이 있을 경우, div, mid, eq, ne 등 문자 연산자를 이용한다.<br><br>
-	**empty<값>** 연산자는 타입이 empty일 경우 true를 리턴한다.<br><br>
-	EL을 비활성화하려면 JSP에 `<%@ page isELIgnored = "true" %>` 를 명시한다.<br><br>
-	EL 지원 Servlet spec을 항상 확인해야 한다.<br><br>

객체 접근 규칙
--------------

-	`${<표현1>.<표현2>}` <br><br>
-	표현1이나 표현2가 null이면 null을 return한다.<br><br>
-	표현1이 Map일 경우, 표현2를 key로 한 값을 return한다.<br><br>
-	표현1이 List나 Array면 표현2가 정수일 경우, 해당 정수 index에 해당하는 값을 반환한다.<br><br>
-	표현1이 객체일 경우, 표현2에 해당하는 `getter()` 메소드에 해당하는 메소드를 호출한 결과를 반환한다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16714/)

---
