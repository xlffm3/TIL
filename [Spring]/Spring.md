Spring
------

---

<br>

### Spring<br><br>

> -	**Framework** : 어떤 프로그램을 만들기 위한 기본 토대 및 환경이며, 중요하고 어렵고 복잡한 부분은 이미 구현되어 있기 때문에 이러한 반제품을 사용하여 제품을 쉽게 제작할 수 있다.<br><br>
> -	라이브러리는 어떤 로직이나 원하는 연산 결과를 얻을 수 있도록 제공하는 함수 또는 그런 기능으로, Framework가 반제품이라면 라이브러리는 반제품을 완제품으로 만들기 위한 도구이다.<br><br>
> -	Spring Framework는 엔터프라이즈 급 어플리케이션을 구축할 수 있는 가벼운 솔루션이자, 모든 과정을 한꺼번에 해결하는 상점인 One-Stop-Shop이다.<br><br>
> -	원하는 부분만 가져다 사용할 수 있도록 모듈화가 잘 되어 있다.<br><br>
> -	IoC 컨테이너이다.<br><br>
> -	선언적으로 트랜잭션을 관리할 수 있다.<br><br>
> -	완전한 기능을 갖춘 MVC Framework를 제공한다.<br><br>
> -	AOP를 지원한다.<br><br>
> -	스프링은 도메인 논리 코드와 쉽게 분리될 수 있는 구조를 가지고 있다.

<br><br>

![image](https://user-images.githubusercontent.com/56240505/70505511-8ed51000-1b6b-11ea-8f38-d4e8bad305cc.png)

<br><br>

### AOP(Aspect Oriented Programming)와 Instrumentation<br><br>

> -	관점 지향 프로그래밍은 어떤 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하는 것이다.<br><br>
> -	Spring-AOP : AOP Alliance와 호환되는 방법으로 AOP를 지원한다.<br><br>
> -	Spring-Aspects : AspectJ와의 통합을 제공한다.<br><br>
> -	Spring-Instrument : 인스트루멘테이션을 지원하는 클래스와 특정 WAS에서 사용하는 클래스로더 구현체를 제공한다.<br><br>
> -	참고로 BCI(Byte Code Instrumentations)은 런타임이나 로드(Load) 때 클래스의 바이트 코드에 변경을 가하는 방법을 말한다.

<br><br>

### Messaging<br><br>

> -	Spring-Messaging : 스프링 프레임워크 4는 메시지 기반 어플리케이션을 작성할 수 있는 Message, MessageChannel, MessageHandler 등을 제공한다.<br><br>
> -	또한, 해당 모듈에는 메소드에 메시지를 맵핑하기 위한 어노테이션도 포함되어 있으며, Spring MVC 어노테이션과 유사하다.

<br><br>

### Data Access & Integration<br><br>

> -	데이터 엑세스/통합 계층은 JDBC, ORM, OXM, JMS 및 트랜잭션 모듈로 구성되어 있다.<br><br>
> -	Spring-Jdbc : 자바 JDBC프로그래밍을 쉽게 할 수 있도록 기능을 제공한다.<br><br>
> -	Spring-Tx : 선언적 트랜잭션 관리를 할 수 있는 기능을 제공한다.<br><br>
> -	Spring-Orm : JPA, JDO및 Hibernate를 포함한 ORM API를 위한 통합 레이어를 제공공한다.<br><br>
> -	Spring-Jms : 메시지 생성(producing) 및 사용(consuming)을 위한 기능을 제공, Spring Framework 4.1부터 spring-messaging모듈과의 통합을 제공한다.<br><br>
> -	Spring-Oxm : JAXB, Castor, XMLBeans, JiBX 및 XStream과 같은 Object/XML 맵핑을 지원한다.

<br><br>

### Web<br><br>

> -	Spring-Web : 멀티 파트 파일 업로드, 서블릿 리스너 등 웹 지향 통합 기능을 제공하며, HTTP클라이언트와 Spring의 원격 지원을 위한 웹 관련 부분을 제공한다.<br><br>
> -	Spring-Webmvc : Web-Servlet 모듈이라고도 불리며, Spring MVC 및 REST 웹 서비스 구현을 포함한다.<br><br>
> -	Spring-Websocket : 웹 소켓을 지원한다.<br><br>
> -	Spring-Webmvc-Portlet : 포틀릿 환경에서 사용할 MVC 구현을 제공한다.

<br><br>

[Reference]

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/20655/)

---
