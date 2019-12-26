SLF4J
-----

---

### SLF4J<br>

-	logging 관련 라이브러리는 다양하며, SLF4J는 이러한 라이브러리들을 하나의 통일된 방식으로 사용할 수 있는 방법을 제공한다.<br><br>
-	SLF4J는 로깅 Facade이다.<br><br>
-	로깅에 대한 추상 레이어를 제공하는 것이고 interface의 모음이다.<br><br>

### Maven SLF4J & logback dependency 추가<br>

![image](https://user-images.githubusercontent.com/56240505/71477805-61f15000-282f-11ea-832e-1b83f6b641f8.png)<br><br>

-	Spring은 기본적으로 아파치 재단의 commons-logging을 사용하기 때문에, logback라이브러리를 사용하려면 제거를 해야한다.<br><br>
-	Spring라이브러리에서 commons-logging을 제거하면, Spring을 사용할 때 commons-logging 라이브러리를 찾으면서 오류가 발생한다.<br><br>
-	이러한 오류를 제거하기 위해서 jcl-over-slf4j를 추가한다.<br><br>

### logback 설정<br>

![image](https://user-images.githubusercontent.com/56240505/71477837-97963900-282f-11ea-983d-ba8f58b257ba.png)<br><br>

### Appender<br>

> ConsoleAppender<br>

```xml
<appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
        <Pattern>%d{HH:mm} %-5level %logger{36} - %msg%n</Pattern>
    </encoder>
</appender>
```

<br>

> RollingFileAppender<br>

```xml
<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>access.log</file>
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        <fileNamePattern>access-%d{yyyy-MM-dd}.log</fileNamePattern>
        <maxHistory>30</maxHistory>
    </rollingPolicy>
    <encoder>
        <Pattern>%d{HH:mm} %-5level %logger{36} - %msg%n</Pattern>
    </encoder>
</appender>
```

-	ConsoleAppender와 FileAppender는 각각 콘솔과 로그에 어떤 포맷으로 로그를 출력할지 결정한다.<br><br>
-	RollingFileAppender는 로그의 양이 많아 하나의 파일로 관리하기 어려울 경우, 하루 단위로 로그를 관리하고자 할 경우 사용된다.<br><br>

### Log Level<br>

```xml
<logger name="org.springframework" level="info"/>
<logger name="kr.or.connect" level="debug"/>
<root level="debug">
    <appender-ref ref="CONSOLE"/>
    <appender-ref ref="FILE"/>
</root>
```

-	1. trace : debug보다 세분화된 정보이다.<br><br>
-	2. debug : 디버깅하는데 유용한 세분화된 정보이다.<br><br>
-	3. info : 진행상황 같은 일반 정보이다.<br><br>
-	4. warn : 오류는 아니지만 잠재적인 오류 원인이 될 수 있는 경고성 정보이다.<br><br>
-	5. error : 요청을 처리하는 중 문제가 발생한 오류 정보이다.<br><br>

### Logger 객체 선언 및 로그 출력<br>

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
.......
private Logger logger = LoggerFactory.getLogger(this.getClass());

logger.trace("{} {} 출력", "값1", "값2");
logger.debug("{} {} 출력", "값1", "값2");
logger.info("{} {} 출력", "값1", "값2");
logger.warn("{} {} 출력", "값1", "값2");
logger.error("{} {} 출력", "값1", "값2");
```

-	로그를 남기고자 하는 클래스에 로거 객체를 필드로 선언한다.<br><br>
-	문자열 결합을 위해 '+'연산자를 사용하지 않는다.<br><br>
-	로그로 남길 변수의 수만큼 {} 를 이용한다.<br><br>
-	로그의 수준에 따라 debug(), info(), warn(), error()메소드를 이용한다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16814/)

---
