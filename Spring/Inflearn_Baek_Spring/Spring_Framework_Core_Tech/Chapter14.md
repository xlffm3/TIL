Chapter 14 : Converter & Formatter
==================================

Converter
---------

> EventConverter.java

```java
public class StringToEventConverter implements Converter<String, Event> {
    @Override
    public Event convert(String source) {
        Event event = new Event();
        event.setId(Integer.parseInt(source));
        return event;
    }
}
```

-	S 타입을 T 타입으로 변환할 수 있는 매우 일반적인 변환기이다.<br><br>
-	상태 정보 없어 Thread-Safe하기 때문에 Bean 등록도 할 수 있다.<br><br>
-	Spring의 경우 Web 관련 Config 파일의 ConverterRegistry에 등록해서 사용한다.<br><br>

Formatter
---------

> EventFormatter.java

```java
public class EventFormatter implements Formatter<Event> {
    @Override
    public Event parse(String text, Locale locale) throws ParseException {
        Event event = new Event();
        int id = Integer.parseInt(text);
        event.setId(id);
        return event;
    }
    @Override
    public String print(Event object, Locale locale) {
        return object.getId().toString();
    }
}
```

-	PropertyEditor 대체제이며, Object와 String 간의 변환을 담당한다.<br><br>
-	문자열을 Locale에 따라 다국화하는 기능도 제공한다.<br><br>
-	상태 정보 없어 Thread-Safe하기 때문에 Bean 등록도 할 수 있다.<br><br>
-	Spring의 경우 Web 관련 Config 파일의 FormatterRegistry에 등록해서 사용한다.<br><br>

ConversionService
-----------------

![image](https://user-images.githubusercontent.com/56240505/80094846-ee274000-85a1-11ea-9d75-849e43a08e59.png)<br><br>

-	실제 변환 작업은 해당 인터페이스를 통해서 Thread-Safe하게 사용할 수 있다.<br><br>
-	Spring MVC, 빈 (value) 설정, SpEL에서 사용한다.<br><br>
-	DefaultFormattingConversionService.
	-	FormatterRegistry.
	-	ConversionService.
	-	여러 기본 Converter와 Formatter를 등록 해준다.<br><br>
-	Spring Boot는 DefaultFormattingConversionSerivce를 상속하여 만든 WebConversionService를 Bean으로 등록해주며, Formatter 및 Converter Bean을 찾아 자동으로 등록해 준다.<br><br>

---

Reference
---------

-	스프링 프레임워크 핵심 기술(백기선, Inflearn)
