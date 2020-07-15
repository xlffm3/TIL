## Spring Boot

* Spring 프레임 워크를 이용하는데 필요한 설정을 Convention에 따라 자동으로 제공해주는 등 편리한 개발을 지원해준다.
* 톰캣 WAS 등 서블릿 컨테이너에 웹 애플리케이션을 등록하던 기존 Spring과 다르게, 웹 애플리케이션이 내장 WAS를 만들고 DispatcherServlet을 등록한다.

<br>

## 시작하기

> pom.xml

```java
<!-- Inherit defaults from Spring Boot -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.3.RELEASE</version>
</parent>

<!-- Add typical dependencies for a web application -->
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>

<!-- Package as an executable jar -->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

<br>

> Application.java

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

* Spring Boot Initializer를 사용하면 이러한 설정이 필요없다.
* 프로젝트 경로에 생성된 jar 파일을 실행해도, IDE에서 run한 것과 같은 효과를 낸다.
* @SpringBootApplication 어노테이션이 위치한 패키지 위치부터 Component를 Scan하기 때문에 패키지 최상위에 위치시킨다.
* parent pom 파일을 계층적으로 올라가다 보면, 필요한 dependency와 여러 설정들을 자동으로 management 해준다.
  * 특정 버전을 명시하거나, 사용하는 parent project를 명시할 수 있다.
  * Spring Boot가 의존성 관리하지 않는 의존성들은 버전을 명시해야 한다.

<br>

> pom.xml

```java
<properties>
  <spring.version>5.0.6.RELEASE</spring.version>
</properites>
```

* Spring Boot가 관리하는 의존성의 버전을 변경하고 싶다면, 계층 pom 파일을 따라 가서 설정된 값들을 오버라이드해주면 된다.

<br>

---

Reference
---------

-	스프링 부트 개념과 활용(백기선, Inflearn)
