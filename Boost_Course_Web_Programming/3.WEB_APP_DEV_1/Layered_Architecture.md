Layered Architecture
--------------------

---

Controller에서 중복되는 부분의 처리
-----------------------------------

-	별도의 객체 혹은 메서드로 분리한다.<br><br>

Controller and Service
----------------------

![image](https://user-images.githubusercontent.com/56240505/70616161-2b251280-1c51-11ea-9b0f-5bcd41600ce5.png)<br><br>

-	비즈니스 메서드를 별도의 Service 객체에 구현하도록 하고, 컨트롤러는 Service 객체를 이용한다.<br><br>

Service Object
--------------

-	서비스 객체란 비즈니스 로직을 수행하는 메서드를 지니고 있는 객체이다.<br><br>
-	보통 하나의 비즈니스 로직은 하나의 트랜잭션으로 동작한다.<br><br>

Transaction
-----------

-	하나의 논리적인 작업을 의미하며, 원자성, 일관성, 독립성, 지속성 등 4가지 특징을 가지고 있다.<br><br>

원자성(Atomicity)
-----------------

```
1. 잔액이 얼마인지 조회한다.
2. 출금하려는 금액이 잔액보다 작은지 검사한다.
3. 출금하려는 금액이 잔액보다 작다면 (잔액 - 출금액)으로 수정한다.
4. 언제, 어디서 출금했는지 정보를 기록한다.
5. 사용자에게 출금한다.
```

-	원자성은 전체가 성공하거나 전체가 실패하는 것을 의미한다.<br><br>
-	오류가 생기면 rollback을, 모든 작업을 성공했을 경우 commit을 하여 트랜잭션 처리가 완료된다.<br><br>

일관성(Consistency)
-------------------

-	일관성은 트랜잭션의 작업 처리 결과가 항상 일관성이 있어야 한다는 것이다.<br><br>
-	트랜잭션이 진행되는 동안에 데이터가 변경되더라도 업데이트된 데이터로 트랜잭션이 진행되는 것이 아니라, 처음에 트랜잭션을 진행하기 위해 참조한 데이터로 진행된다.<br><br>
-	이렇게 함으로써 각 사용자는 일관성 있는 데이터를 볼 수 있다.<br><br>

독립성(Isolation)
-----------------

-	독립성은 둘 이상의 트랜잭션이 동시에 병행 실행되고 있을 경우에 어느 하나의 트랜잭션이라도 다른 트랜잭션의 연산을 끼어들 수 없음을 뜻한다.<br><br>
-	하나의 특정 트랜잭션이 완료될 때까지, 다른 트랜잭션이 특정 트랜잭션의 결과를 참조할 수 없다.<br><br>

지속성 (Durability)
-------------------

-	트랜잭션이 성공적으로 완료됬을 경우, 결과는 영구적으로 반영된다는 특징이다.<br><br>

JDBC 프로그래밍에서 트랜잭션 처리 방법
--------------------------------------

-	DB에 연결된 후 Connection객체의 `setAutoCommit()` 메소드에 false를 파라미터로 지정한다.<br><br>
-	입력, 수정, 삭제 SQL이 실행을 한 후 모두 성공했을 경우 Connection이 가지고 있는 `commit()` 메소드를 호출한다.<br><br>
-	`setAutoCommit()` 의 default는 true이기 때문에 명령을 실행하면 일반적으로 자동 commit이 된다.<br><br>

@EnableTransactionManagement
----------------------------

-	Spring Java Config파일에서 트랜잭션을 활성화 할 때 사용하는 애노테이션이다.<br><br>
-	Java Config를 사용하게 되면 PlatformTransactionManager 구현체를 모두 찾아서 그 중에 하나를 매핑해 사용한다.<br><br>
-	특정 트랜잭션 메니저를 사용하고자 한다면 TransactionManagementConfigurer를 Java Config파일에서 구현하고 원하는 트랜잭션 메니저를 리턴하도록 한다.<br><br>
-	아니면, 특정 트랜잭션 메니저 객체를 생성시 @Primary 애노테이션을 지정한다.<br><br>

Service Object에서 중복으로 호출되는 코드의 처리
------------------------------------------------

-	서비스 객체가 지닌 비즈니스 메서드는 트랜잭션 단위로 작업이 처리가 된다.<br><br>
-	하나의 트랜잭션에는 여러 개의 DB 작업이 수행될 수 있다.<br><br>
-	비즈니스 메서드마다 중복되는 기능을 호출하는 등 서비스 객체에서의 중복은 역시 별도의 객체 혹은 메서드로 분리해서 사용한다.<br><br>

Layered Architecture
--------------------

![image](https://user-images.githubusercontent.com/56240505/70617587-46455180-1c54-11ea-9596-d4ff81c7acd2.png)<br><br>

-	Presentation Layer에서는 컨트롤러 객체가, Service Layer에서는 서비스 객체가 동작하며 Repository Layer에서는 DB에 접근하여 데이터를 가져오는 등의 작업을 수행한다.<br><br>
-	Service 객체는 해당 Repository Layer에 있는 DAO 객체를 사용한다.<br><br>

설정의 분리
-----------

-	Spring 설정 파일을 프리젠테이션 레이어쪽과 나머지를 분리할 수 있다.<br><br>
-	web.xml 파일에서 프리젠테이션 레이어에 대한 스프링 설정은 DispathcerServlet이 읽도록 하고, 그 외의 설정은 ContextLoaderListener를 통해서 읽도록 한다.<br><br>
-	DispatcherServlet을 경우에 따라서 2개 이상 설정할 수 있는데 이 경우에는 각각의 DispathcerServlet의 ApplicationContext가 각각 독립적이기 때문에 각각의 설정 파일에서 생성한 빈을 서로 사용할 수 없다.<br><br>
-	위의 경우와 같이 동시에 필요한 빈은 ContextLoaderListener를 사용함으로써 공통으로 사용하게 할 수 있다.<br><br>
-	ContextLoaderListener와 DispatcherServlet은 각각 ApplicationContext를 생성하는데, ContextLoaderListener가 생성하는 ApplicationContext가 root컨텍스트가 되고 DispatcherServlet이 생성한 인스턴스는 root컨텍스트를 부모로 하는 자식 컨텍스트가 된다.<br><br>
-	자식 컨텍스트들은 root컨텍스트의 설정 빈을 사용할 수 있다.<br><br>
-	코드의 재사용 및 유지 보수 측면에서 Layered Architecture를 지향해야 한다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16767/)

---
