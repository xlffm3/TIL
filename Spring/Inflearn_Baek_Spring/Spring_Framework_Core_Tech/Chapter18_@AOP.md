Chapter 17 : @AOP
=================

Dependency 추가
---------------

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

<br>

Aspect와 Advice 및 Pointcut 정의
--------------------------------

```java
@Aspect
@Component
public class PerfAspect {

    @Around("execution(* com.example..*.EventService.*(..))")
    @Around("@annotation(PerLogging)")
    @Bean("bean(SimpleEventService)")
    public Object logPeer(ProceedingJoinPoint pip) throws Throwable {
        long begin = System.currentTimeMillis();
        System.out.println();
        Object retval = pip.proceed();
        System.out.println(System.currentTimeMillis() - begin);
        return retval;
    }
}
```

-	Pointcut은 @Pointcut(표현식)도 가능하며 &&와 || 및 ! 등의 식을 사용할 수 있다.<br><br>
-	특정 Annotation 지시를 통해 일부 메서드의 AOP 지정을 누락시킬 수 있다.<br><br>
-	Advice는 @Around 외에도 @Before 와 @AfterReturning 및 @AfterThrowing 등을 통해 Joint Point 시점을 조정할 수 있다.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
