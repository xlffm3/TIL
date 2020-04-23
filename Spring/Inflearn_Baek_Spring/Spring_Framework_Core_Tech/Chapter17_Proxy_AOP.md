Chapter 17 : AOP(Aspect-Oriented Programming)
=============================================

Spring AOP
----------

-	Proxy 기반의 AOP 구현체이며, Bean에만 AOP를 적용할 수 있다.<br><br>
-	모든 AOP 기능을 제공하는 것이 목적이 아니라, Spring IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제에 대한 해결책을 제공하는 것이 목적이다.<br><br>

Proxy Pattern
-------------

![image](https://user-images.githubusercontent.com/56240505/80108193-fdfd4f00-85b6-11ea-863a-c54984ba5e15.png)<br><br>

-	기존 코드 변경 없이 접근 제어 또는 부가 기능 등을 추가할 수 있다.<br><br>
-	그러나 프록시 클래스 작성 비용 및 객체 관리 등 단점이 존재한다.<br><br>
-	Spring IoC 컨테이너가 제공하는 기반 시설과 Dynamic 프록시를 사용하여 여러 이러한 이슈를 해결한다.<br><br>

-	동적 프록시 : 동적으로 프록시 객체 생성하는 방법이며, 자바가 제공하는 방법은 인터페이스 기반의 프록시 생성이다.<br><br>

-	CGlib은 클래스 기반 프록시도 지원한다.<br><br>

-	Spring IoC : 기존 Bean을 대체하는 동적 프록시 Bean을 만들어 등록 시켜주며, 클라이언트 코드 변경이 없다.<br><br>

-	AbstractAutoProxyCreator implements BeanPostProcessor.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
