Converter
---------

> EventConverter.java

```java
public class EventConverter {
  public class StringToEventConverter implements Converter<String, Event> {
      @Override
      public Event convert(String source) {
          Event event = new Event();
          event.setId(Integer.parseInt(source));
          return event;
      }
  }
}
```

<br>

> WebConfig.java

```java
@Configuration
public class WebConfig implements WebConfigurer {

  @Override
  public void addFormatters(FormatterRegistry registry) {
    registry.addConverter(new EventConverter.StringToEventConverter());
  }
}
```

-	S 타입을 T 타입으로 변환할 수 있는 매우 일반적인 변환기이다.<br><br>
-	상태 정보 없어 Thread-Safe하기 때문에 Bean 등록도 할 수 있다.<br><br>
-	Spring Boot 없이 사용하는 경우 Web 관련 Config 파일의 ConverterRegistry에 등록해서 사용한다.<br><br>

Formatter
---------

> EventFormatter.java

```java
public class EventFormatter implements Formatter<Event> {

    @Override
    @Component
    public Event parse(String text, Locale locale) throws ParseException {
        Event event = new Event();
        int id = Integer.parseInt(text);
        event.setId(id);
        return event;
    }
    @Override
    @Component
    public String print(Event object, Locale locale) {
        return object.getId().toString();
    }
}
```

<br>

> WebConfig.java

```java
@Configuration
public class WebConfig implements WebConfigurer {

  @Override
  public void addFormatters(FormatterRegistry registry) {
    registry.addFormatters(new EventFormatter());
  }
}
```

-	PropertyEditor 대체제이며, Object와 String 간의 변환을 담당한다.<br><br>
-	문자열을 Locale에 따라 다국화하는 기능도 제공한다.<br><br>
-	상태 정보 없어 Thread-Safe하기 때문에 Bean 등록도 할 수 있다.<br><br>
-	다른 Bean을 주입받아 MessageSource를 응용할 수 있다.<br><br>
-	Spring의 경우 Web 관련 Config 파일의 FormatterRegistry에 등록해서 사용한다.<br><br>

ConversionService
-----------------

![image](https://user-images.githubusercontent.com/56240505/80094846-ee274000-85a1-11ea-9d75-849e43a08e59.png)<br><br>

-	실제 변환 작업은 해당 인터페이스를 통해서 Thread-Safe하게 사용할 수 있다.<br><br>
-	Spring MVC, Bean(value) 설정, SpEL에서 사용한다.<br><br>
-	DefaultFormattingConversionService.
	-	FormatterRegistry.
	-	ConversionService.
	-	여러 기본 Converter 및 Formatter를 등록 해준다.<br><br>
-	Spring Boot는 DefaultFormattingConversionSerivce를 상속하여 만든 WebConversionService를 Bean으로 등록해주며, Formatter 및 Converter Bean을 찾아 자동으로 등록해 준다.<br><br>
-	자주 쓰이는 자료형에 대해 Spring이 기본적으로 제공하는 Formatter가 있다.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
