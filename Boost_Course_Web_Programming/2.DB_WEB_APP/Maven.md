Maven
-----

---

### Maven<br>

-	어플리케이션을 개발하기 위해 반복적으로 진행하는 작업을 지원하는 도구이다.<br><br>
-	빌드, 패키징, 문서화, 테스트, 테스트 리포팅, git, 의존성 관리, svn 등과 같은 형상 관리 서버와 연동(SCMs), 배포 등의 작업을 손 쉽게 할 수 있다.<br><br>
-	CoC(Convention over Configuration)에 대해 알아가는 것과 동일하다.<br><br>
-	의존성 라이브러리 관리, 일관된 프로젝트 빌드 가이드, 플러그인을 통한 자동화 등을 설정 파일에 적어줌으로써 실현할 수 있다.<br><br>

### Maven 기본 작성<br>

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>kr.or.connect</groupId>
    <artifactId>examples</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>mysample</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

-	Archetype을 이용하여 Maven 기반 프로젝트를 생성할 경우, 생성된 프로젝트 하위에 `pom.xml` 파일이 생성된다.<br><br>
-	project : `pom.xml` 파일의 최상위 루트 엘리먼트(Root Element)이다.<br><br>
-	modelVersion : POM model의 버전이다. <br><br>
-	groupId : 프로젝트를 생성하는 조직의 고유 아이디를 결정하며, 일반적으로 도메인 이름을 거꾸로 적는다.<br><br>
-	artifactId : 해당 프로젝트에 의하여 생성되는 artifact의 고유 아이디를 결정하며, 예제 파일을 빌드할 경우 examples-1.0-SNAPSHOT.jar 파일이 생성된다.<br><br>
-	packaging : 해당 프로젝트를 어떤 형태로 packaging 할 것인지 결정하며, jar, war, ear 등이 해당된다.<br><br>
-	version : 프로젝트의 현재 버전이며, 프로젝트가 개발 중일 때는 SNAPSHOT을 접미사로 사용한다.<br><br>
-	name : 프로젝트의 이름이다.<br><br>
-	url : 프로젝트 사이트가 있다면 사이트 URL을 등록하는 것이 가능하다.<br><br>
-	**Dependency Management** : `</dependency>` 엘리멘트 안에서 필요한 라이브러리를 지정한다.<br><br>

### Scope<br>

-	**compile** : 컴파일 할 때 필요. 테스트 및 런타임에도 클래스 패스에 포함된다. scope 을 설정하지 않는 경우 기본값이다..<br><br>
-	**runtime** : 런타임에 필요. JDBC 드라이버 등이 예가 된다. 컴파일 시에는 필요하지 않지만, 실행 시에 필요한 경우이다.<br><br>
-	**provided** : 컴파일 시에 필요하지만, 실제 런타임 때에는 컨테이너 같은 것에서 제공되는 모듈. servlet, jsp api 등이 이에 해당. 배포 시 제외된다. <br><br>
-	**test** : 테스트 코드를 컴파일 할 때 필요. 테스트 시 클래스 패스에 포함되며, 배포 시 제외된다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16724/)

---
