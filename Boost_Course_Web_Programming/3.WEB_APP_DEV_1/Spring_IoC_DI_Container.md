Spring IOC/DI Container
-----------------------

---

Container
---------

-	Container는 인스턴스의 생명 주기를 관리한다.<br><br>
-	생성된 인스턴스들에게 추가적인 기능을 제공한다.<br><br>
-	예를 들어, Servlet 클래스를 정의하더라도 Tomcat(WAS)가 Servlet Container를 가지고 있기 때문에 개발자가 실제 인스턴스화는 직접 진행하지 않는다.<br><br>

IoC(Inversion of Control)
-------------------------

-	제어의 역전으로서, 개발자가 프로그램의 흐름을 제어하는 코드를 작성하면 다른 프로그램이 그 흐름을 제어하는 것이다.<br><br>

DI(Dependency Injection)
------------------------

-	의존성 주입으로, 클래스 사이의 의존 관계를 빈(Bean) 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것을 말한다.<br><br>

참고 코드
---------

```java
//DI 미적용
class 엔진 {

}

class 자동차 {
     엔진 v5 = new 엔진();
}

//DI 적용
@Component
class 엔진 {

}

@Component
class 자동차 {
     @Autowired
     엔진 v5;
}
```

-	DI가 미적용되면 개발자가 직접 인스턴스를 생성하여 사용해야 한다.<br><br>
-	DI가 적용되면 컨테이너가 엔진 type의 v5 변수에 인스턴스를 할당해준다.<br><br>

Spring에서 제공하는 IOC/DI Container
------------------------------------

-	BeanFactory : IoC/DI에 대한 기본 기능을 가지고 있다.<br><br>
-	ApplicationContext : BeanFactory의 모든 기능을 포함하며, 일반적으로 BeanFactory보다 추천된다.<br><br>
-	트랜잭션처리, AOP등에 대한 처리를 할 수 있으며, BeanPostProcessor, BeanFactoryPostProcessor등을 자동으로 등록하고, 국제화 처리, 어플리케이션 이벤트 등을 처리할 수 있다.<br><br>
-	BeanPostProcessor : 컨테이너의 기본로직을 오버라이딩하여 인스턴스화 와 의존성 처리 로직 등을 개발자가 원하는 대로 구현 할 수 있도록 한다.<br><br>
-	BeanFactoryPostProcessor : 설정된 메타 데이터를 커스터마이징 할 수 있다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/20656/)

---
