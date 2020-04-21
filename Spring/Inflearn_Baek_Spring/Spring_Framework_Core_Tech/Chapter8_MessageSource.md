Chapter 7 : MessageSource
=========================

MessageSource
-------------

> messages.properties

```properties
greeting=hello {0}
```

<br>

> messages_ko_KR.properties

```properties
greeting=안녕 {0}
```

<br>

> AppRunner.java

```java
@Component
public class AppRunner implements ApplicationRunner {

    @Autowired
    MessageSource messageSource;

    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println(messageSource.getMessage("greeting", new String[]{"jipark"}, Locale.KOREA)); //안녕 jipark
        System.out.println(messageSource.getMessage("greeting", new String[]{"jipark"}, Locale.getDefault())); //hello jipark
    }
}
```

-	국제화(i18n) 기능을 제공하는 인터페이스이다.<br><br>
-	MessageSource를 구현하는 ApplicationContext의 `getMessage(String code, Object[] args, String, default, Locale, loc)` 메소드 등을 이용한다.<br><br>
-	Spring Boot를 이용하면 별다른 설정 없이 message 관련 properties를 이용할 수 있다.<br><br>

릴로딩 기능이 있는 MessageSource
--------------------------------

> Application.java

```java
@Bean
public MessageSource messageSource() {
      var messageSource = new ReloadableResourceBundleMessageSource();
      messageSource.setBasename("classpath:/messages");
      messageSource.setDefaultEncoding("UTF-8");
      messageSource.setCacheSeconds(3);
      return messageSource;
}
```

<br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
