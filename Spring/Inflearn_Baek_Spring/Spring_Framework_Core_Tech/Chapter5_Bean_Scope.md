Scope
=====

> Single.java

```java
@Component
public class Single {

    @Autowired
    private Proto proto;

    public Proto getProto() {
        return proto;
    }
}
```

<br>

> Proto.java

```java
@Component @Scope("prototype")
```

<br>

> AppRunner.java

```java
@Component
public class AppRunner implements ApplicationRunner {
    @Autowired
    ApplicationContext ctx;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println(ctx.getBean(Single.class).getProto());
        System.out.println(ctx.getBean(Single.class).getProto());
        System.out.println(ctx.getBean(Single.class).getProto());
    }
}
```

-	Singleton : 해당 빈의 인스턴스가 어플리케이션에서 단 1개인 것을 의미한다.<br><br>
-	Prototype : 해당 빈의 인스턴스가 어플리케이션에서 여러 개인 것을 의미한다.<br><br>
-	Singleton 인스턴스는 ApplicationContext 초기 구동시 생성되며, Property가 공유되기 때문에 Thread-Safe 하지 않다.<br><br>
-	Bean Scope가 Prototype인 경우 참조될 때 마다 다른 인스턴스가 생성되어 다른 주소값이 출력된다.<br><br>
-	Prototype이 Singleton을 참조하는 경우 문제가 없지만, Singleton이 Prototype을 참조하는 경우 프로퍼티인 Prototype Bean이 변경되지 않는 문제가 발생한다.<br><br>
-	위 예제는 Singleton Bean이 생성된 시점에 지닌 Proto의 동일한 주소 3개를 출력한다.<br><br>
-	이러한 문제는 `@Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)` 와 같이 Proxy를 통해 해결할 수 있다.<br><br>

Proxy
-----

![image](https://user-images.githubusercontent.com/56240505/79845356-e035bb80-83f7-11ea-9b2c-3ca91197b8dc.png)<br><br>

-	Class 기반의 Proxy로 Proto Bean을 감싸고, 다른 Bean이 Proto Bean을 사용할 때 이를 감싼 Proxy Bean을 사용하라는 의미이다.<br><br>
-	다른 Bean이 Proto Bean을 직접 참조할 경우, 새로운 Proto 인스턴스를 만들 수 없기 때문이다.<br><br>
-	실제로는 Proxy Bean이 생성되며, Proto를 상속받기 때문에 Proto 자료형의 객체에 @Autowired로 주입될 수 있다.<br><br>
-	생명주기가 긴 객체가 생명주기가 짧은 객체를 참조할 때 이러한 문제에 유념한다.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
