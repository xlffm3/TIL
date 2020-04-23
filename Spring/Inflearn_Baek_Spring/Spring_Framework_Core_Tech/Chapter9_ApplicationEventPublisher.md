Chapter 9 : ApplicationEventPublisher
=====================================

ApplicationEventPublisher
-------------------------

-	옵저버 패턴의 구현체로서, 이벤트 프로그래밍에 필요한 인터페이스를 제공한다.<br><br>
-	ApplicationEventPublisher를 상속받는 ApplicationContext의 `publishEvent(ApplicationEvent event)` 를 사용한다.<br><br>
-	이벤트를 발생시키는데 ApplicationEvent 클래스를 상속받았으나, Spring 4.2 부터는 상속받지 않아도 사용이 가능하다.<br><br>

Spring 4.2 이전의 이벤트 발생 및 처리
-------------------------------------

> MyEvent.java

```java
public class MyEvent extends ApplicationEvent {

    private int data;

    public MyEvent(Object source, int data) {
        super(source);
        this.data = data;
    }

    public int getData() {
        return data;
    }
}
```

<br>

> AppRunner.java

```java
@Component
public class AppRunner implements ApplicationRunner {

    @Autowired
    ApplicationEventPublisher publishEvent;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        publishEvent.publishEvent(new MyEvent(this, 100)); //이벤트 발생
    }
}
```

<br>

> MyEventHandler.java

```java
@Component
public class MyEventHandler implements ApplicationListener<MyEvent> {

    @Override
    public void onApplicationEvent(MyEvent myEvent) {
        System.out.println("event accpeted!" + myEvent.getData());
    }
}
```

-	ApplicationListener<T>를구현한 클래스 만들어서 빈으로 등록한다.<br><br>
-	이벤트가 발생하면 등록되어 있는 Bean 중 ApplicationListener를 구현한 클래스가 이를 처리한다.<br><br>

Spring 4.2 이후의 이벤트 발생 및 처리
-------------------------------------

> MyEvent.java

```java
public class MyEvent {

    private Object source;
    private int data;

    public MyEvent(Object source, int data) {
        this.source = source;
        this.data = data;
    }

    public Object getSource() {
        return source;
    }

    public int getData() {
        return data;
    }
}
```

<br>

> MyEventHandler.java

```java
@Component
public class MyEventHandler {

    @EventListener
    @Order(Ordered.HIGHEST_PRECEDENCE)
    public void onApplicationEvent(MyEvent myEvent) {
        System.out.println("event accpeted!" + myEvent.getData());
    }
}
```

-	특정 클래스를 상속받지 않고 @EventListener를 이용하여 간단하게 처리한다.<br><br>
-	Framework의 코드가 나의 코드에 침입하지 않아 유지보수 및 테스트가 용이해진다.<br><br>
-	또 다른 EventListener가 있다면 기본적으로 Synchronized하게 실행된다.<br><br>
-	순서를 설정하려면 @Order Annotation을 이용한다.<br><br>
-	비동기적으로 설정하려면 EventListener에 @Async 및 Application 클래스에 @EnableAysnc Annotation을 각각 추가해준다.<br><br>

Spring이 제공하는 기본 이벤트
-----------------------------

-	ContextRefreshedEvent : ApplicationContext를 초기화 및 리프레시 했을 때 발생한다.<br><br>
-	ContextStartedEvent : ApplicationContext를 start()하여 라이프사이클 Bean들이 시작 신호를 받은 시점에 발생한다.<br><br>
-	ContextStoppedEvent : ApplicationContext를 stop()하여 라이프사이클 Bean들이 정지 신호를 받은 시점에 발생한다.<br><br>
-	ContextClosedEvent : ApplicationContext를 close()하여 싱글톤 Bean이 소멸되는 시점에 발생한다.<br><br>
-	RequestHandledEvent : HTTP 요청을 처리했을 때 발생한다.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
