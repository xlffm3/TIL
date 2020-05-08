## Server Template Engine

* 템플릿 엔진이란 지정된 템플릿 양식과 데이터가 합쳐져서 HTML 문서를 출력하는 소프트웨어이다.
* 서버 템츨릿 엔진을 이용한 화면 생성은 서버에서 Java 코드로 문자열을 만든 뒤 이 문자열을 HTML로 변환하여 브라우저로 전달한다.
* 반면 Vue.js나 React.js는 브라우저 위에서 작동하며, 서버는 Json 혹은 Xml 형식의 데이터만 전달하고 클라이언트에서 조립한다.

## Mustache

> build.gradle

```java
compile('org.springframework.boot:spring-boot-starter-mustache')
```

* 문법이 다른 템플릿 엔진보다 심플하며, 로직 코드를 사용할 수 없어 View 역할과 서버의 역할을 명확하게 분리한다.
* js와 Java 등 2가지를 지원하여, 하나의 문법으로 클라이언트 및 서버 템플릿을 모두 사용 가능하다.
* src/main/resources/templates 위치에 mustache 등 html 파일을 위치시킨다.

<br>

> IndexControllerTest.java

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class IndexControllerTest extends TestCase {

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    public void loadTest() {
        String body = this.restTemplate.getForObject("/", String.class);

        assertThat(body).contains("Spring Boot로 시작하는 Web-Service");
    }
}
```

<br>

## Mustache 기본 설정

> header.mustache

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Spring Boot Web-Service</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">
</head>
<body>
```

<br>

> footer.mustache

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js"></script>

<!--index.js 추가-->
<script src="/js/app/index.js"></script>
</body>
</html>
```

<br>

> index.mustache

```html
{{>layout/header}}

<h1>Spring Boot로 시작하는 Web-Service</h1>

{{>layout/footer}}
```

* 부트스트랩 및 제이쿼리 등 프론트엔드 라이브러리는 외부 CDN을 통해 사용하거나, 직접 라이브러리를 받아 사용할 수 있다.
* CDN 서비스 제공하는 곳에서 문제가 생기면 의존 문제가 발생하기 때문에 실무에서는 지양한다.
* 공통 영역을 별도의 파일로 분리하여 필요한 곳에서 가져다 쓰는 레이아웃 방식을 이용한다.
* 페이지 로딩 속도를 높이기 위해 CSS는 header에, js는 footer에 위치시킨다.
* HTML은 위에서부터 코드가 실행되기 때문에 head가 실행되고서야 body가 실행된다.
* head가 다 불러지지 않으면 사용자 쪽에선 백지 화면만 노출되는 이슈가 있기 때문에, js 용량이 클 경우 body 하단에 두어 화면이 다 그려진 후에 호출하도록 한다.
* bootstrap.js는 제이쿼리에 의존하기 때문에 순서를 고려하여 호출 코드를 작성한다.
* {{>}} 코드는 현재 머스테치 파일을 기준으로 다른 파일을 가져온다.

<br>

## 완성 코드 : Mustache & JS

> index.mustache

```html
{{>layout/header}}

<h1>Spring Boot로 시작하는 Web-Service</h1>
<div class="col-md-12">
    <div class="row">
        <div class="col-md-6">
            <a href="/posts/save" role="button" class="btn btn-primary">
                글 등록
            </a>
        </div>
    </div>
    <br>
    <!-- 목록 출력 영역 -->
    <table class="table table-horizontal table-bordered">
        <thead class="thead-strong">
        <tr>
            <th>게시글 번호</th>
            <th>제목</th>
            <th>작성자</th>
            <th>최종 수정일</th>
        </tr>
        </thead>
        <tbody id="tbody">
        {{#posts}}
            <tr>
                <td>{{id}}</td>
                <td><a href="/posts/update/{{id}}">{{title}}</a></td>
                <td>{{author}}</td>
                <td>{{modifiedDate}}</td>
            </tr>
        {{/posts}}
        </tbody>
    </table>
</div>

{{>layout/footer}}
```

* {{posts}}를 통해 posts라는 List를 순회한다.
* {{id}} 등의 변수명을 통해 객체의 필드를 사용한다.

<br>

> posts-save.mustache

```html
{{>layout/header}}
<h1>게시글 등록</h1>

<div class="col-md-12">
    <div class="col-md-4">
        <form>
            <div class="form-group">
                <label for="title">제목</label>
                <input type="text" class="form-control" id="title" placeholder="제목을 입력하세요.">
            </div>
            <div class="form-group">
                <label for="author">작성자</label>
                <input type="text" class="form-control" id="author" placeholder="작성자를 입력하세요.">
            </div>
            <div class="form-group">
                <label for="content">내용</label>
                <input type="text" class="form-control" id="content" placeholder="내용을 입력하세요.">
            </div>
        </form>
        <a href="/" role="button" class="btn btn-secondary">취소</a>
        <button type="button" class="btn btn-primary" id="btn-save">등록</button>
    </div>
</div>
{{>layout/footer}}
```

<br>

> posts-update.mustache

```html
{{>layout/header}}
<div class="col-md-12">
    <div class="col-md-4">
        <form>
            <div class="form-group">
                <label for="title">글 번호</label>
                <input type="text" class="form-control" id="id" value="{{post.id}}" readonly>
            </div>
            <div class="form-group">
                <label for="title">제목</label>
                <input type="text" class="form-control" id="title" value="{{post.title}}">
            </div>
            <div class="form-group">
                <label for="author"> 작성자 </label>
                <input type="text" class="form-control" id="author" value="{{post.author}}" readonly>
            </div>
            <div class="form-group">
                <label for="content"> 내용 </label>
                <textarea class="form-control" id="content">{{post.content}}</textarea>
            </div>
        </form>
        <a href="/" role="button" class="btn btn-secondary">취소</a>
        <button type="button" class="btn btn-primary" id="btn-update">수정 완료</button>
        <button type="button" class="btn btn-danger" id="btn-delete">삭제</button>
    </div>
</div>
{{>layout/footer}}
```

* readonly 속성을 통해 Input 태그에 읽기 기능만 허용하도록 한다.

<br>

> index.js

```js
var main = {
    init : function () {
        var _this = this;
        $('#btn-save').on('click', function () {
            _this.save();
        });
        $('#btn-update').on('click', function () {
            _this.update();
        });
        $('#btn-delete').on('click', function () {
            _this.delete();
        })
    },

    save : function () {
        var data = {
            title : $('#title').val(),
            author : $('#author').val(),
            content : $('#content').val()
        };

        $.ajax({
            type : 'POST',
            url : '/api/v1/posts',
            dataType : 'json',
            contentType : 'application/json; charset=utf-8',
            data : JSON.stringify(data)
        }).done(function () {
            alert('글이 등록되었습니다.');
            window.location.href = '/';
        }).fail(function (error) {
            alert(JSON.stringify(error));
        });
    },

    update : function () {
        var data = {
            title: $('#title').val(),
            content: $('#content').val()
        };

        var id = $('#id').val();

        $.ajax({
            type: 'PUT',
            url: '/api/v1/posts/' + id,
            dataType: 'json',
            contentType:'application/json; charset=utf-8',
            data: JSON.stringify(data)
        }).done(function() {
            alert('글이 수정되었습니다.');
            window.location.href = '/';
        }).fail(function (error) {
            alert(JSON.stringify(error));
        });
    },

    delete : function () {
        var id = $('#id').val();

        $.ajax({
            type : 'DELETE',
            url : '/api/v1/posts/' + id,
            dataType : 'json',
            contentType : 'application/json; charset=utf-8'
        }).done(function () {
            alert('글이 삭제되었습니다.');
            window.location.href = '/'
        }).fail(function (error) {
            alert(JSON.stringify(error));
        });
    }
};

main.init();
```

<br>

> footer.mustache

```html
<script src="/js/app/index.js"></script>
```

* 브라우저의 스코프는 공용 공간으로 쓰이기 때문에 나중에 로딩된 중복 이름의 함수가 먼저 로딩된 js의 함수를 덮어쓰게 된다.
* 여러 사람이 참여하는 프로젝트에서 중복된 함수 이름은 자주 발생할 수 있다.
* 따라서 index.js만의 유효 범위를 만들기 위해 main 객체 내부에 필요한 function을 선언한다.
* index 객체 내부에서만 유효하기 때문에 다른 js와 겹칠 위험이 사라진다.
* 스프링 부트는 기본적으로 src/main/resources/static에 위치한 js와 CSS 및 이미지 등 정적 파일을 URL에서 /로 설정하며, 절대 경로 /로 바로 시작한다.

<br>

## 완성 코드 : Spring Boot

> PostsRepository.java

```java
public interface PostsRepository extends JpaRepository<Posts, Long> {

    @Query("SELECT p FROM Posts p ORDER BY p.id DESC")
    List<Posts> findAllDesc();
}
```

* Spring Data JPA에서 제공하는 기본 메소드 외에도 실제 쿼리를 작성할 수 있다.
* 규모가 있는 프로젝트의 데이터 조회는 FK 조인 및 복잡한 조건 등으로 인해 Entity 클래스만으로 처리하기가 어려워 조회용 프레임워크를 사용한다.
* 등록과 수정 및 삭제는 JPA를 통해 진행하고, 조회는 Querydsl를 추천한다.
* Querydsl 장점
  * 타입 안정성이 보장된다.
    * 단순한 문자열로 쿼리를 생성하는 것이 아니라 메소드를 기반으로 쿼리를 생성하기 때문에 IDE에서 Typo-Safe하다.
  * 국내 많은 회사가 사용하며 레퍼런스가 많다.

<br>

> PostsService.java

```java
@Transactional(readOnly = true)
public List<PostsListResponseDto> findAllDesc() {
    return postsRepository.findAllDesc().stream()
            .map(PostsListResponseDto::new)
            .collect(Collectors.toList());
}

@Transactional
public Long delete(Long id) {
    Posts posts = postsRepository.findById(id)
            .orElseThrow(() -> new IllegalArgumentException("There is no such post. id=" + id));
    postsRepository.delete(posts);
    return id;
}
```

* readOnly 옵션을 통해 트랜잭션의 범위는 유지하면서 조회 기능만 남겨두어 조회 속도가 개선된다.
* ``.map(PostsListResponseDto::new)`` 는 ``.map(posts -> new PostsListResponseDto(posts))`` 와 같다.
* postsRepository 결과로 넘어온 Posts의 Stream을 map을 통해 PostsListResponseDto로 변환하여 List로 반환하는 메소드이다.

<br>

> PostsListResponseDto.java

```java
@Getter
public class PostsListResponseDto {
    private Long id;
    private String title;
    private String author;
    private LocalDateTime modifiedDate;

    public PostsListResponseDto(Posts entity) {
        this.id = entity.getId();
        this.title = entity.getTitle();
        this.author = entity.getAuthor();
        this.modifiedDate = entity.getModifiedDate();
    }
}
```

<br>

> IndexController.java

```java
@RequiredArgsConstructor
@Controller
public class IndexController {

    private final PostsService postsService;

    @GetMapping("/")
    public String index(Model model) {
        model.addAttribute("posts", postsService.findAllDesc());
        return "index";
    }

    @GetMapping("/posts/save")
    public String postsSave() {
        return "posts-save";
    }

    @GetMapping("/posts/update/{id}")
    public String postsUpdate(@PathVariable Long id, Model model) {
        PostsResponseDto dto = postsService.findById(id);
        model.addAttribute("post", dto);
        return "posts-update";
    }
}
```

* 필요한 경우 Dto 객체를 model에 담아 view에 뿌려준다.

<br>

---

Reference
---------

-	스프링 부트와 AWS로 혼자 구현하는 웹 서비스 (이동욱 저)
