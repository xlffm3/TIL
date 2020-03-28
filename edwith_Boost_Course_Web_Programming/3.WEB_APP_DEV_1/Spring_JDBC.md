Spring JDBC
-----------

![image](https://user-images.githubusercontent.com/56240505/70523682-ecc32100-1b86-11ea-9261-f6ccbfef1452.png)<br><br>

-	JDBC 프로그래밍 중 반복되는 저수준 개발 요소들은 Spring Framework이 대신 처리해준다.<br><br>

Spring JDBC Packages
--------------------

-	org.springframework.jdbc.core : JdbcTemplate 및 관련 Helper 객체를 제공한다.<br><br>
-	org.springframework.jdbc.datasource : DataSource를 쉽게 접근하기 위한 유틸 클래스, 트랜젝션매니져 및 다양한 DataSource 구현을 제공한다.<br><br>
-	org.springframework.jdbc.object : RDBMS 조회, 갱신, 저장등을 안전하고 재사용 가능한 객제를 제공한다.<br><br>
-	org.springframework.jdbc.support : jdbc.core 및 jdbc.object를 사용하는 JDBC 프레임워크를 지원한다.<br><br>

JDBC Template
-------------

-	리소스 생성, 해지를 처리해서 연결을 닫는 것을 잊어 발생하는 문제 등을 피할 수 있도록 한다.<br><br>
-	스테이먼트(Statement)의 생성과 실행을 처리한다.<br><br>
-	SQL 조회, 업데이트, 저장 프로시저 호출, ResultSet 반복호출 등을 실행한다.<br><br>
-	JDBC 예외가 발생할 경우 org.springframework.dao패키지에 정의되어 있는 일반적인 예외로 변환시킨다.<br><br>

그 외의 접근 방식
-----------------

-	NamedParameterJdbcTemplate : JdbcTemplate에서 JDBC statement 인자를 ?를 사용하는 대신 파라미터명을 사용하여 작성하는 것을 지원한다.<br><br>
-	SimpleJdbcInsert : 테이블에 쉽게 데이터 insert 기능을 제공한다.<br><br>
-	MyBatis 및 JPA 등 JDBC 프로그래밍을 더욱 쉽게 하는 기술이 존재한다.<br><br>

DTO(Data Transfer Object)
-------------------------

-	계층간 데이터 교환을 위한 자바 빈즈이다.<br><br>
-	여기서의 계층이란 컨트롤러 뷰, 비지니스 계층, 퍼시스턴스 계층을 의미한다.<br><br>
-	일반적으로 DTO는 로직을 가지고 있지 않고, 순수한 데이터 객체이다.<br><br>
-	필드와 getter, setter를 가지며, 추가적으로 toString(), equals(), hashCode()등의 Object 메소드를 오버라이딩 할 수 있다.<br><br>

DAO(Data Access Object)
-----------------------

-	데이터를 조회하거나 조작하는 기능을 전담하도록 만든 객체이다.<br><br>

Connection Pool
---------------

![image](https://user-images.githubusercontent.com/56240505/70525054-cf438680-1b89-11ea-9339-22bac7beaddf.png)<br><br>

-	DB 연결은 시간 및 리소스 등 비용이 많이 들기 때문에, 커넥션 이 미리 커넥션을 여러 개 맺어둔다.<br><br>
-	커넥션이 필요할 경우, 커넥션 풀에게 빌려서 사용한 뒤 반납한다.<br><br>

DataSource
----------

-	커넥션 풀은 경우에 따라서 여러 개 생성될 수 있으며, 그러한 커넥션 풀을 관리하는 목적으로 사용되는 것이 DataSource이다.<br><br>
-	커넥션을 얻어오고 반납하는 등의 작업을 수행한다.<br><br>
-	DataSource로 얻은 Connection 객체는 close 메서드로 반납을 수행한다.<br><br>

Practice
--------

![image](https://user-images.githubusercontent.com/56240505/70525422-a8d21b00-1b8a-11ea-8d11-b812fa8c1496.png)<br><br>

> ApplicationConfig.java<br>

```java
@Configuration
@ComponentScan(basePackages = { "kr.or.connect.daoexam.dao" }) //basePackages를 이용하면 패키지 나열이 가능
@Import({DBConfig.class}) //DB 연결 설정 부분만 분리하여 import
public class ApplicationConfig {
}
```

<br>

> DBConfig.java <br>

```java
@Configuration
@EnableTransactionManagement //트랜젝션 제어를 위한 Annotation
public class DBConfig {
    private String driverClassName = "com.mysql.jdbc.Driver";
    private String url = "jdbc:mysql://localhost:3306/connectdb?useUnicode=true&characterEncoding=utf8";

    private String username = "connectuser";
    private String password = "connect123!@#";

    @Bean
    public DataSource dataSource() { //커넥션을 관리하는 목적
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }
}
```

<br>

> RoleDaoSqls.java<br>

```java
public class RoleDaoSqls {
    public static final String SELECT_ALL = "SELECT role_id, description FROM role order by role_id";
    public static final String UPDATE = "UPDATE role SET description = :description WHERE ROLE_ID = :roleId";
    // :를 통해 binding 할 부분을 명시함
    public static final String SELECT_BY_ROLE_ID = "SELECT role_id, description FROM role where role_id = :roleId";
    public static final String DELETE_BY_ROLE_ID = "DELETE FROM role WHERE role_id = :roleId";
}
```

<br>

> RoleDao.java<br>

```java
import static kr.or.connect.daoexam.dao.RoleDaoSqls.*; //static import
@Repository //DAO는 저장소의 역할을 한다는 의미로 붙임
public class RoleDao {
    private NamedParameterJdbcTemplate jdbc; //이름을 이용해서 바인딩하거나 결과값을 가져오는 등 더 편한 기능을 사용하기 위해서 선언함.
    private SimpleJdbcInsert insertAction;
    private RowMapper<Role> rowMapper = BeanPropertyRowMapper.newInstance(Role.class);

    public RoleDao(DataSource dataSource) {
        this.jdbc = new NamedParameterJdbcTemplate(dataSource);
        this.insertAction = new SimpleJdbcInsert(dataSource)
                .withTableName("role"); //어떤 테이블인지 명시
    }

    public List<Role> selectAll(){
        return jdbc.query(SELECT_ALL, Collections.emptyMap(), rowMapper);
    }

    public int insert(Role role) {
        SqlParameterSource params = new BeanPropertySqlParameterSource(role);
        return insertAction.execute(params);
    }

    public int update(Role role) {
        SqlParameterSource params = new BeanPropertySqlParameterSource(role);
        return jdbc.update(UPDATE, params);
    }

    public int deleteById(Integer id) {
      Map<String, ?> params = Collections.singletonMap("roleId", id);
      return jdbc.update(DELETE_BY_ROLE_ID, params);
    }

    public Role selectById(Integer id) {
      try {
        Map<String, ?> params = Collections.singletonMap("roleId", id);
        return jdbc.queryForObject(SELECT_BY_ROLE_ID, params, rowMapper);       
      }catch(EmptyResultDataAccessException e) {
        return null;
      }
    }
}
```

-	Spring version 4.3부터는 @ComponentScan으로 객체를 찾을 때, 기본 생성자가 없다면 자동으로 객체를 주입해준다.<br><br>
-	DBConfig에서 bean으로 등록한 dataSource가 RoleDao 생성자로 전달되어 `NamedParameterJdbcTemplate` 객체가 생성된다.<br><br>
-	RoleDaoSqls의 query를 가져오기 위해 static import를 사용한다.<br><br>
-	selectAll의 emptyMap은 sql query에 바인딩 할 값이 존재한다면 바인딩 할 값을 전달할 목적으로 사용하는 객체이다.<br><br>
-	rowMapper는 select 한 건의 결과를 DTO에 저장하는 목적이며, `BeanPropertyRowMapper` 객체를 이용해서 column의 값을 DTO에 자동으로 저장해준다.<br><br>
-	query 메서드는 결과가 여러 건일 때, 내부적으로 반복하여 DTO를 생성하고 List에 담아 전달한다.<br><br>
-	`BeanPropertyRowMapper`는 DBMS와 Java의 명명 규칙을 통일해주는 기능을 제공한다.<br><br>
-	Insert 메서드의 경우 Primary Key를 자동으로 생성하는 경우가 존재하며, 생성된 PK를 다시 읽어야 하며 SimpleJdbcInsert 객체가 이를 수행한다.<br><br>
-	Insert는 Role 객체의 값을 Map으로 변환하며 Java DTO 변수명을 DB의 column 명으로 변경하여 Map 객체를 생성한다.<br><br>
-	Update 메서드의 2번째 파라미터 값은 query의 바인딩 요소를 매핑시켜주는 객체이며, `SqlParameterSource`가 role을 Map 객체로 변환시킨다.<br><br>
-	1건만 조회 및 삭제하는 명령의 경우, 값이 여러 개가 들어가지 않고 1개만 넣어 사용하기 때문에 `Collections.singletonMap`을 사용한다.<br><br>
-	1건만 조회하는 경우, `queryForObject` 메서드를 사용한다.<br><br>

---

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/20660/)<br>
-	[edwith](https://www.edwith.org/boostcourse-web/lecture/20661/)
