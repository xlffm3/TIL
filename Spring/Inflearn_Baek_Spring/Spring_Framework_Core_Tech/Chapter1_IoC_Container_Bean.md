Chapter 1 : Spring IoC Container &Bean
======================================

IoC(Inversion of Control)
-------------------------

-	의존 관계 주입(Dependency Injection)이라고 한다.<br><br>
-	어떤 객체가 사용하는 의존 객체를 직접 만들어서 사용하는 것이 아니라, 주입 받아 사용하는 방식을 의미한다.<br><br>

Spring IoC Container
--------------------

-	Bean 설정 소스로부터 빈 정의를 읽어들인 뒤, Bean을 구성하여 제공한다.<br><br>
-	IoC 기능 및 Bean을 담고 있는 'Container' 역할을 한다.<br><br>
-	어플리케이션 컴포넌트의 중앙 저장소이며, 최상위 인터페이스는 'BeanFactory'이다.<br><br>
-	ApplicationContext : BeanFactory를 상속받은 인터페이스이다.
	-	메시지 소스 처리 기능(i18n).
	-	이벤트 발행 기능.
	-	리소스 로딩 기능.
	-	...<br><br>

Bean
----

-	Spring IoC Container가 관리하는 객체이다.<br><br>
-	장점.
	-	의존성 관리.
	-	Life Cycle Interface : Bean 생성 이후 Callback 작업 등이 편리하다.
	-	Scope.
		-	Singleton : 하나의 객체만 사용하는 것을 의미한다.
		-	Prototype : 매번 다른 객체를 사용하는 것을 의미한다.
		-	Singleton의 경우, 런타임 성능 및 메모리 측면에서 효율적이다.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
