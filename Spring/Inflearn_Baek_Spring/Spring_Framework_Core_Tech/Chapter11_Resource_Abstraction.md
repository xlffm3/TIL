Chapter 11 : Resource Abstraction
=================================

Resource Abstraction
--------------------

-	org.springframework.core.io.Resource.<br><br>
-	java.net.URL을 추상화 한 것이며, 스프링 내부에서 많이 사용하는 인터페이스이다.<br><br>
-	추상화 이유.
	-	클래스패스 기준으로 리소스 읽어오는 기능이 없다.
	-	ServletContext를 기준으로 상대 경로로 읽어오는 기능이 없다.
	-	새로운 핸들러를 등록하여 특별한 URL 접미사를 만들어 사용할 수는 있지만 구현이 복잡하고 편의성 메소드가 부족하다.<br><br>

구현체
------

-	UrlResource : 기본으로 지원하는 프로토콜 http, https, ftp, file, jar 등이 있다.<br><br>
-	ClassPathResource : 지원하는 접두어는 classpath:이다.<br><br>
-	ServletContextResource : 웹 애플리케이션 루트에서 상대 경로로 리소스 찾는다.<br><br>
-	FileSystemResource 등.<br><br>

Resource 읽어오기
-----------------

-	Resource의 타입은 locaion 문자열과 ApplicationContext의 타입에 따라 결정 된다.<br><br>

-	ClassPathXmlApplicationContext -> ClassPathResource.<br><br>

-	FileSystemXmlApplicationContext -> FileSystemResource.<br><br>

-	WebApplicationContext -> ServletContextResource.<br><br>

-	ApplicationContext의 타입에 상관없이 리소스 타입을 강제하려면 java.net.URL 접두어(+ classpath:) 중 하나를 사용할 수 있다.<br><br>

-	classpath:me/whiteship/config.xml -> ClassPathResource.<br><br>

-	file:///some/resource/path/config.xml -> FileSystemResource.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
