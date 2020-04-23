Chapter 12 : Validation Abstraction
===================================

Validation Abstraction
----------------------

-	org.springframework.validation.Validator.<br><br>
-	애플리케이션에서 사용하는 객체 검증용 인터페이스이다.<br><br>
-	어떤한 계층과도 관계가 없어 웹과 서비스 및 데이터 등 모든 계층에서 사용해도 좋다.<br><br>
-	구현체 중 하나로, JSR-303(Bean Validation 1.0)과 JSR-349(Bean Validation 1.1)을 지원한다.(LocalValidatorFactoryBean)<br><br>
-	DataBinder에 들어가 바인딩 할 때 같이 사용되기도 한다.<br><br>
-	`boolean supports(Class clazz)` : 어떤 타입의 객체를 검증할 때 사용할 것인지 결정한다.<br><br>
-	`void validate(Object obj, Errors e)` : 실제 검증 로직을 이 안에서 구현하며, 구현할 때 ValidationUtils 사용하며 편리하다.<br><br>

스프링 부트 2.0.5 이상 버전
---------------------------

-	LocalValidatorFactoryBean 빈으로 자동 등록되어 Validator 인스턴스를 Autowired할 수 있다.<br><br>
-	JSR-380(Bean Validation 2.0.1) 구현체로 hibernate-validator를 사용한다.<br><br>
-	@NotEmpty, @Email, @Size 등 다양한 Annotation으로 유효성을 검사할 수 있다.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
