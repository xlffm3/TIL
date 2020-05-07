SQL Mapper
----------

```java
//직관적인 코드
User user = findUser();
Group group = user.getGroup();

//RDB의 문제
User user = userDao.findUser();
Group group = groupDao.findGroup(user.getGroupId());
```

-	MyBatis 등 SQL Mapper는 객체 모델링보다는 테이블 모델링에만 집중하고, 객체를 단순히 테이블에 맞추어 데이터 전달 역할만 하는 기현상이 발생한다.
-	RDB는 어떻게 데이터를 저장할지에 초점이 맞춰진 반면, OOP 언어는 기능과 속성을 한 곳에서 관리하는 기술이다.
-	패러다임 불일치로 인해 상속 및 1:N 등 다양한 객체 모델링을 DB로 구현하는데 문제가 발생한다.
-	위 예제처럼, User와 Group이 어떤 관계인지 알기 모호해진다.

<br>

JPA
---

-	JPA는 ORM(Object Relational Mapping)의 약어이며, 인터페이스로서 Java 표준 명세서이다.
-	개발자가 OOP를 하면, jpa가 이를 관계형 DB에 맞게 SQL을 대신 생성새허 실행해준다.
-	OOP와 RDB 중간에서 패러다임을 일치시켜주며, SQL에 종속적인 개발을 탈피하게 도와준다.
-	JPA 인터페이스의 구현체로는 Hibernate 및 Eclipse Link 등이 있으나, Spring은 이러한 구현체를 추상화시킨 Spring Data JPA 모듈을 이용한다.
-	Hibernate 등의 구현체를 직접 사용하지 않고 한 단계 더 감싸놓은 이유는, 구현체 및 저장소 교체가 용이해지기 때문이다.
	-	Spring Data JPA 내부에서 구현체 매핑을 지원해주기 때문에, Hibernate 외에 다른 구현체로 쉽게 교체가 가능하다.
	-	RDB에서 MongDB 등으로 교체할 때 Spring Data JPA에서 의존성만 교체하면 된다.
	-	Spring Data의 하위 프로젝트들은 기본적으로 CRUD 인터페이스가 같아 기능 변경이 필요없다.

<br>

Spring Data JPA 적용하기
------------------------

> build.gradle

```java
compile('org.springframework.boot:spring-boot-starter-data-jpa')
compile('com.h2database:h2')
```

-	Spring Data Jpa 추상화 라이브러리는 스프링 부트 버전에 맞춰 자동으로 JPA 관련 라이브러리들의 버전을 관리해준다.
-	h2는 인메모리 관계형 DB이다.
	-	별도의 설치가 필요 없이 프로젝트 의존성만으로 관리할 수 있다.
	-	메모리에서 실행되기 때문에 어플리케이션을 재시작할 때마다 초기화되며, 테스트에 많이 이용된다.

<br>

> Posts.java

```java
@Getter
@NoArgsConstructor
@Entity
public class Posts {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 500, nullable = false)
    private String title;

    @Column(columnDefinition = "TEXT", nullable = false)
    private String content;

    private String author;

    @Builder
    public Posts(String title, String content, String author) {
        this.title = title;
        this.content = content;
        this.author = author;
    }
}
```

-	Domain 패키지는 게시글, 댓글, 회원, 정산, 결제 등 소프트웨어에 대한 요구사항 혹은 문제 영역 등의 도메인을 담을 패키지이다.
-	기존 MyBatis와 같은 쿼리 매퍼가 xml에 쿼리를 담고, 클래스는 오로지 쿼리의 결과만 담던 dao 패키지의 일들이 모두 도메인 클래스에서 해결된다.
-	주요 어노테이션을 클래스에 가깝게 두어, 새 언어 전환으로 부수적인 어노테이션을 쉽게 삭제할 수 있다.
-	Posts 클래스는 실제 DB의 테이블과 매치욀 클래스이며, Entity 클래스라고 한다.
-	@Entity
	-	테이블과 링크될 클래스임을 의미하며, 기본값으로 클래스의 카멜케이스 이름을 언더스코어 네이밍으로 테이블 이름을 매칭한다.
-	@Id
	-	해당 테이블의 PK 필드를 나타낸다.
-	@GeneratedValue
	-	PK의 생성 규칙을 나타내며, 옵션을 통해 auto_increment를 적용했다.
-	@Column
	-	테이블의 칼럼을 나타내며 기본값 이외에 추가로 변경이 필요한 옵션이 있을 때 사용한다.
-	Entity의 PK는 Long 타입의 auto_increment를 권장한다.
	-	복합키로 PK를 맺을 경우, FK를 맺을 때 다른 테이블에서 복합키 전부를 갖고 있거나 중간 테이블을 하나 더 둬야 하는 상황이 발생한다.
	-	인덱스에 좋지 못하다.
	-	유니크한 조건이 변경될 경우, PK 전체를 수정해야 하는 일이 발생한다.
	-	주민등록번호, 복합키 등은 유니크 키로 별도로 추가하는 것이 낫다.
-	@Builder
	-	해당 클래스의 빌더 패턴 클래스를 생성하며, 생성자 상단에 선언 시 생성자에 포함된 필드만 빌더에 포함한다.

<br>

> SetterAndBuilderExample.java

```java
//잘못된 사용 예
public class Order {
  public void setStatus(boolean status) {
    this.status = status;
  }
}

public void cancle() {
  order.setStatus(false);
}

//올바른 사용 예
public class Order {
  public void cancleOrder() {
    this.status = status;
  }
}

public void cancle() {
  order.cancleOrder();
}

//builder의 예시
Example.builder().a(a).b(b).build();
```

-	Entity 클래스에서는 절대 Setter 메소드를 만들지 않는다.
-	해당 필드의 값 변경이 필요하면 명확히 그 목적과 의도를 나타낼 수 있는 메소드를 추가한다.
-	해당 클래스의 인스턴스 값들이 언제 어디서 변해야 하는지 코드상으로 명확하게 구분이 어려우면, 차후 기능 변경 시 복잡해진다.
-	기본적으로 생성자를 통해 최종값을 채운 후 DB에 삽입하며, 값 변경이 필요한 경우 해당 이벤트에 맞는 메소드를 호출하여 변경하는 것이 전제다.
-	빌더를 사용하면 어느 필드에 어떤 값을 채워야 할지 명확하게 구분할 수 있다.

<br>

> PostsRepository.java

```java
public interface PostsRepository extends JpaRepository<Posts, Long> {
}
```

-	JPA Repository는 SQL Mapper 등에서 Dao라고 불리는 DB Layer 접근자이다.
-	@Repository 어노테이션은 필요 없으나, Entity 클래스와 함께 위치해야 한다.

<br>

Spring Data JPA 테스트 코드
---------------------------

> PostsRepositoryTest.java

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class PostsRepositoryTest extends TestCase {

    @Autowired
    PostsRepository postsRepository;

    @After
    public void cleanup() {
        postsRepository.deleteAll();;
    }

    @Test
    public void findTest() {
        String title = "title";
        String content = "content";
        String author = "author";

        postsRepository.save(Posts.builder()
				.title(title)
				.content(content)
				.author(author)
				.build());
        List<Posts> list = postsRepository.findAll();
        Posts posts = list.get(0);
        assertThat(posts.getTitle()).isEqualTo(title);
        assertThat(posts.getContent()).isEqualTo(content);
    }
}
```

<br>

> application.properties

```java
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=
org.hibernate.dialect.MySQL5InnoDBDialect
```

-	여러 테스트가 동시에 수행되면 테스트용 DB인 H2에 데이터가 남아 있을 수 있기 때문에, 단위 테스트가 끝날 때 마다 수행하는 메소드를 지정한다.
-	save() 메소드는 테이블에 id 값이 있다면 update가, 없다면 insert 쿼리가 실행된다.
-	SpringBootTest는 자동으로 H2 DB를 실행해준다.
-	콘솔 로그에서 H2 문법이 아닌 MySQL 버전의 쿼리를 보기 위해 별도의 설정을 한다.

<br>

Spring 웹 계층
--------------

-	Web Layer
	-	컨트롤러 및 뷰 템플릿 영역이다.
	-	필터와 인터셉터 및 컨트롤러 어드바이스 등 외부 요청과 응답에 대한 전반적인 영역을 이야기한다.
-	Service Layer
	-	@Service에 사용되는 영역이며, Controller와 Dao 중간 영역에서 사용된다.
	-	@Transactional이 사용되는 곳이며, 트랜잭션과 도메인 기능간 순서 보장의 역할을 한다.
-	Repository Layer
	-	Dao 등 Database와 같은 DB에 접근하는 영역이다.
-	Dtos
	-	Dto는 계층 간에 데이터 교환을 위한 객체이며, Dtos는 이들의 영역이다.
	-	뷰 템플릿 엔진에서 사용될 객체 혹은 Repository Layer에서 결과로 넘겨준 객체 등을 의미한다.
-	Domain Model
	-	Domain 개발 대상을 모든 사람이 동일한 관점에서 이해할 수 있고 공유할 수 있도록 단순화 시킨 것이다.
	-	무조건 DB와 관계가 있어야 하는 것은 아니며, VO처럼 값 객체들도 이 영역에 속한다.
	-	비즈니스 로직 처리를 담당하는 곳이다.
-	과거 트랜잭션 스크립트는 모든 로직이 서비스 클래스 내부에서 처리되며, 서비스 계층이 무의미해지고 객체가 데이터 덩어리로 전락하는 부작용이 있었다.
-	도메인 모델은 각 도메인들이 본인의 이벤트를 처리하며, 서비스 메소드는 트랜잭션과 도메인 간의 순서만 보장해준다.

<br>

등록 구현
---------

> PostsSaveRequestDto.java

```java
@Getter
@NoArgsConstructor
public class PostsSaveRequestDto {
    private String title;
    private String content;
    private String author;

    @Builder
    public PostsSaveRequestDto(String title, String content, String author) {
        this.title = title;
        this.content = content;
        this.author = author;
    }

    public Posts toEntity() {
        return Posts.builder()
				.title(this.title)
				.content(this.content)
				.author(this.author)
				.build();
    }
}
```

-	Entity와 거의 유사한 형태임에도 Dto 클래스를 추가로 생성한다.
-	Entity 클래스는 DB와 맞닿은 핵심 클래스이기 때문에, 절대 Request/Response 클래스로 사용하지 않는다.
-	Entity 클래스를 기준으로 테이블이 생성되고 스키마가 변경되며, 화면 변경을 위해 Entity를 변경하면 리스크가 크다.
-	Entity는 여러 클래스에 영향을 끼치지만, Request 및 Response 용 Dto는 View를 위한 클래스라 자주 변경이 필요하다.
-	때문에 View Layer(Controller에서 쓸 Dto)와 DB Layer(Entity)의 역할 분리가 철저해야 한다.

<br>

> PostsService.java

```java
@RequiredArgsConstructor
@Service
public class PostsService {

    private final PostsRepository postsRepository;

    public Long save(PostsSaveRequestDto requestDto) {
        return postsRepository.save(requestDto.toEntity()).getId();
    }
}
```

<br>

> PostsApiController.java

```java
@RequiredArgsConstructor
@RestController
public class PostsApiController {

    private final PostsService postsService;

    @PostMapping("/api/v1/posts")
    public Long save(@RequestBody PostsSaveRequestDto requestDto) {
        return postsService.save(requestDto);
    }
}
```

-	Bean을 주입받을 때, @Autowired는 지양하고 생성자로 주입받는 방식을 이용한다.
-	@RequiredArgsConstructor 롬복 코드가 final 필드들을 인자로 하는 생성자를 생성한다.
-	롬복을 사용하면 클래스의 의존성 관계가 변경될 때마다 생성자 코드를 수정하지 않는 장점이 있다.

<br>

> PostsApiControllerTest.java

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class PostsApiControllerTest extends TestCase {

    @LocalServerPort
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    @Autowired
    PostsRepository postsRepository;

    @After
    public void tearDown() throws Exception {
        postsRepository.deleteAll();
    }

    @Test
    public void saveTest() throws Exception {
        String title = "title";
        String content = "content";
        String author = "author";
        PostsSaveRequestDto requestDto =
				 PostsSaveRequestDto.builder()
				 .title(title)
				 .content(content)
				 .author(author)
				 .build();
        String url = "http://localhost:" + port + "/api/v1/posts";

        ResponseEntity<Long> responseEntity =
				restTemplate.postForEntity
				(url, requestDto, Long.class);
        assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(responseEntity.getBody()).isGreaterThan(0L);
        List<Posts> list = postsRepository.findAll();
        Posts posts = list.get(0);
        assertThat(posts.getTitle()).isEqualTo(title);
        assertThat(posts.getContent()).isEqualTo(content);
    }
}
```

-	@WebMvcTest는 JPA 기능이 작동하지 않기 때문에 통합 테스트 환경에서 TestRestTemplate을 이용한다.

<br>

수정 및 조회 구현
-----------------

> PostsApiController.java

```java
@PutMapping("/api/v1/posts/{id}")
public Long update(@PathVariable Long id, @RequestBody PostsUpdateRequestDto requestDto) {
    return postsService.update(id, requestDto);
}

@GetMapping("/api/v1/posts/{id}")
public PostsResponseDto findById(@PathVariable Long id) {
    return postsService.findById(id);
}
```

<br>

> PostsResponseDto.java

```java
@Getter
public class PostsResponseDto {

    private Long id;
    private String title;
    private String content;
    private String author;

    public PostsResponseDto(Posts entity) {
        this.id = entity.getId();
        this.title = entity.getTitle();
        this.content = entity.getContent();
        this.author = entity.getAuthor();
    }
}
```

-	ResponseDto는 Entity의 일부 필드만 이용하기 때문에 생성자로 Entity를 받는다.

<br>

> PostsUpdateRequestDto.java

```java
@Getter
@NoArgsConstructor
public class PostsUpdateRequestDto {
    private String title;
    private String content;

    @Builder
    public PostsUpdateRequestDto(String title, String content) {
        this.title = title;
        this.content = content;
    }
}
```

<br>

> Posts.java

```java
public void update(String title, String content) {
    this.title = title;
    this.content = content;
}
```

<br>

> PostsService.java

```java
@Transactional
public Long update(Long id, PostsUpdateRequestDto requestDto) {
    Posts posts = postsRepository.findById(id)
            .orElseThrow(() -> new IllegalArgumentException("There is no such post. id=" + id));
    posts.update(requestDto.getTitle(), requestDto.getContent());
    return id;
}

public PostsResponseDto findById(Long id) {
    Posts entity = postsRepository.findById(id)
            .orElseThrow(() -> new IllegalArgumentException("There is no such post. id=" + id));
    return new PostsResponseDto(entity);
}
```

-	update 기능은 데이터베이스에 쿼리를 날리는 부분이 없는데, 이는 JPA의 영속성 컨텍스트 때문이다.
-	영속성 컨텍스트란 엔티티를 영구 저장하는 환경으로서, JPA의 핵심은 엔티티가 영속성 컨텍스트에 포함되어 있냐 아니냐로 갈린다.
-	JPA의 엔티티 매니저가 활성환 상태에서(Spring Data JPA 기본 옵션) 트랜잭션 안에서 DB의 데이터를 가져온다면 이 데이터는 영속성 컨텍스트가 유지된 상태이다.
-	이 상태에서 해당 데이터의 값을 변경하면 트랜잭션이 끝나는 시점에서 해당 테이블에 변경분을 반영한다.
-	Entity 객체의 값만 변경할 뿐 별도의 Update 쿼리가 필요없으며, 이를 더티 체킹(Dirty-Checking)이라고 한다.

<br>

> PostsApiControllerTest.java

```java
@Test
public void updateTest() throws Exception {
    String title = "title";
    String content = "content";
    String author = "author";
    Posts savedPosts = postsRepository.save(Posts
		.builder()
		.title(title)
		.content(content)
		.author(author)
		.build());

    Long id = savedPosts.getId();
    String expectedTitle = "title2";
    String expectedContent = "content2";

    PostsUpdateRequestDto requestDto = PostsUpdateRequestDto
		.builder()
		.title(expectedTitle)
		.content(expectedContent)
		.build();
    String url = "http://localhost:" + port + "/api/v1/posts/" + id;
    HttpEntity<PostsUpdateRequestDto> requestEntity =
		new HttpEntity<>(requestDto);

    ResponseEntity<Long> responseEntity =
		restTemplate.exchange(url, HttpMethod.PUT, requestEntity,
		Long.class);

    assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
    assertThat(responseEntity.getBody()).isGreaterThan(0L);
    List<Posts> list = postsRepository.findAll();
    Posts posts = list.get(0);
    assertThat(posts.getTitle()).isEqualTo(expectedTitle);
    assertThat(posts.getContent()).isEqualTo(expectedContent);
}
```

<br>

> application.properties

```java
spring.h2.console.enabled=true
```

-	메모리에서 실행되는 H2 DB에 직접 접근하기 위해 웹 콘솔 관련 설정을 해주며, localhost의 h2-console page로 접근한다.
-	JDBC URL을 jdbc:h2:mem:testdb로 변경해 확인한다.

<br>

JPA Auditing
------------

> BaseTimeEntity.java

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public class BaseTimeEntity {

    @CreatedDate
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
```

-	JPA Auditing으로 생성 시간 및 수정 시간을 자동화할 수 있다.
-	LocalDateTime은 Date 및 Calendar 클래스와 다르게 쓰레드 세이프하다.
-	@MappedSuperclass
	-	JPA Entity 클래스들이 BaseTimeEntity를 상속할 경우 필드들을 칼럼으로 인식하도록 한다.
-	@EntityListeners
	-	BaseTimeEntity 클래스에 Auditing 기능을 포함시킨다.
-	어노테이션을 기반으로 Entity가 생성되거나 수정될 때 시간이 자동 저장된다.

<br>

> Application.java

```java
@EnableJpaAuditing
```

-	Posts 클래스가 BaseTimeEntity를 상속받고 해당 어노테이션을 Application 클래스에 명시하면 Auditing이 활성화된다.

<br>

> PostsRepositoryTest.java

```java
@Test
public void dateTest() {
    LocalDateTime now = LocalDateTime.of(2019, 6, 4, 0, 0, 0);
    postsRepository.save(Posts
		.builder()
		.title("title")
		.content("content")
		.author("author")
		.build());

    List<Posts> list = postsRepository.findAll();
    Posts posts = list.get(0);
    assertThat(posts.getCreatedDate()).isAfter(now);
    assertThat(posts.getModifiedDate()).isAfter(now);
}
```

<br>

---

Reference
---------

-	스프링 부트와 AWS로 혼자 구현하는 웹 서비스 (이동욱 저)
