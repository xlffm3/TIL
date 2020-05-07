Gradle Project를 Spring Boot Project로 변경하기
-----------------------------------------------

> build.gradle

```java
buildscript {
    ext {
        springBootVersion = '2.1.7.RELEASE'
    }
    repositories {
        mavenCentral()
        jcenter()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'

group 'com.glenn.book'
version '1.0-SNAPSHOT'
sourceCompatibility = 1.8

repositories {
    mavenCentral()
    jcenter()
}

dependencies {
    compile('org.springframework.boot:spring-boot-starter-web')
    testCompile('org.springframework.boot:spring-boot-starter-test')
}
```

-	`ext` 키워드를 통해 springBootVersion 전역 변수를 생성하고, 그 값을 지정한다.
-	repositories는 각종 의존성(라이브러리)들을 어떤 원격 저장소에서 받을지를 정한다.
-	기본적으로 mavenCentral을 많이 사용하지만, 업로드 난이도 때문에 jcenter도 많이 사용한다.
-	의존성 코드에 특정 버전을 명시하지 않아야, 맨 위에 작성한 spring-boot-gradle-plugin의 버전을 따라갈 수 있다.
-	이렇게 관리함으로써 각 라이브러리의 버전 관리가 한 곳에 집중되게 할 수 있다.

<br>

---

Reference
---------

-	스프링 부트와 AWS로 혼자 구현하는 웹 서비스 (이동욱 저)
