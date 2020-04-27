Spring Boot MVC 설정
--------------------

-	Spring Boot의 “주관”이 적용된 자동 설정이 동작한다.
	-	JSP 보다 Thymeleaf를 선호한다.
	-	JSON을 지원한다.
	-	웰컴 페이지 및 파비콘 등 정적 리소스를 지원한다.<br><br>

Spring MVC 커스터마이징
-----------------------

> application.properties

```properties
spring.mvc.view.prefix=/WEB-INF/jsp
spring.mvc.view.suffix=.jsp
```

-	application.properties에서 여러 설정을 할 수 있다.<br><br>
-	특히 Formatter 및 Converter 등을 WebMvcConfigurer 인터페이스를 구현하여 등록할 수 있으나, Bean만 등록하면 자동으로 이용이 가능하다.<br><br>
-	@Configuration + Implements WebMvcConfigurer : Spring Boot의 MVC 자동 설정 및 추가 설정을 위한 형태이다.<br><br>
-	@Configuration + @EnableWebMvc + Imlements WebMvcConfigurer : Spring Boot의 MVC 자동 설정을 사용하지 않는다.<br><br>

Spring Boot에서 JSP 사용하기
----------------------------

> “If possible, JSPs should be avoided. There are several known limitations when using them with embedded servlet containers.”

-	JAR 프로젝트로 만들 수 없어 WAR 프로젝트로 만들어야 한다.<br><br>
-	Java -JAR로 실행할 수는 있지만 “실행가능한 JAR 파일”은 지원하지 않는다.<br><br>
-	언더토우(JBoss에서 만든 서블릿 컨테이너)는 JSP를 지원하지 않는다.<br><br>
-	Whitelabel 에러 페이지를 error.jsp로 오버라이딩 할 수 없다.<br><br>
-	`./mvnw package` 로 메이븐 빌드를 할 수 있다.<br><br>
-	WAR 파일은 웹 서버에서 배포가 가능하다.<br><br>

WAR 배포하기
------------

![image](https://user-images.githubusercontent.com/56240505/80369855-e9c49500-88c9-11ea-8c61-b90dd5e63a8f.png)<br><br> ![image](https://user-images.githubusercontent.com/56240505/80369899-fcd76500-88c9-11ea-88a9-89c8713663b1.png)<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
