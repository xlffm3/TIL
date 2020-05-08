## Spring Security

* 스프링 시큐리티는 막강한 인증과 인가 기능을 가진 프레임워크다.
* 스프링 기반의 애플리케이션에서 보안을 위한 표준이며, 인터셉터 및 필터 기반의 보안 기능 구현보다 권장된다.

<br>

## OAuth 2.0

* 직접 로그인을 구현하는 것은 보안 등 부수적인 기능 개발로 이어지기 때문에 소셜 로그인을 사용하면 간편하다.
* 스프링 부트 1.5와 2.0의 OAuth 연동 방법이 크게 달라졌으나, spring-security-oauth2-autoconfigure 라이브러리 덕분에 설정 방법은 큰 차이가 없다.
  * 스프링 팀에서 1.5에서 사용하던 oauth 프로젝트는 신규 기능을 추가하지 않는 방향으로 결정했기 때문에 oauth2 라이브러리를 이용한다.
  * 기존 방식은 확장 포인트가 적절하게 오픈되어 있지 않아 불편하다.
* application.properties나 application.yml 정보를 통해 1.5인지 2.0인지 확인한다.
* 1.5 방식은 url 주소를 명시했으나, 2.0 방식은 client 인증 정보만 입력하며 직접 입력했던 값들은 enum으로 대체되었다.
* CommonOAuth2Provider라는 enum에서 구글, 깃허브, 페이스북 등의 기본 설정값을 제공한다.
  * 다른 소셜 로그인을 추가한다면 해당 enum에 추가해준다.

<br>

## 구글 서비스 등록

* 구글 클라우드 플랫폼에서 인증 정보를 발급받는다.
* 승인된 리디렉션 URI
  * 서비스에서 파라미터로 인증 정보를 주었을 때 인증이 성공하면 구글에서 리다이렉트할 URL이다.
  * 스프링 부트 2 버전의 시큐리티에서는 기본적으로 {도메인}/login/oauth2/code/{소셜서비스코드}로 지원한다.
  * 별도로 리다이렉트 URL을 지원하는 Controller가 필요없다.
  * AWS 서버에 배포하게 되면 localhost 외에 추가로 주소를 추가한다.

<br>

> application-oauth.properties

```java
spring.security.oauth2.client.registration.google.client-id=클라이언트id
spring.security.oauth2.client.registration.google.client-secret=클라이언트 보안 비밀
spring.security.oauth2.client.registration.google.scope=profile,email
```

<br>

> application.properties

```java
spring.profiles.include=oauth
```

* scope의 기본 값은 openid,profile,email이지만, openid가 있으면 OpenId Provider로 인식한다.
  * 이렇게 되면 OpenId Provider인 서비스와 그렇지 않은 서비스를 나눠서 각각 OAuth2Service를 만들어야 한다.
  * 하나의 OAuth2Service를 사용하기 위해 강제로 scope를 등록한다.
* 스프링 부트에서 application-xxx.properties를 생성하면 xxx라는 profile이 생성되어 이를 통해 관리할 수 있다.
  * profile=xxx 라는 식으로 호출하면 해당 properties의 설정들을 가져올 수 있다.
  * 방법은 다양하며 해당 예제는 application.properties에서 application-oauth.properties를 포함하도록 구성한다.

<br>

> .gitignore

```git
application-oauth.properties
```

* 중요 정보들이 공개되지 않도록 .gitignore 파일을 변경한다.

<br>

## 구글 로그인 연동

> User.java

```java
@Getter
@NoArgsConstructor
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false)
    private String email;

    @Column
    private String picture;

    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private Role role;

    @Builder
    public User(String name, String email, String picture, Role role) {
        this.name = name;
        this.email = email;
        this.picture = picture;
        this.role = role;
    }

    public User update(String name, String picture) {
        this.name = name;
        this.picture = picture;
        return this;
    }

    public String getRoleKey() {
        return this.role.getKey();
    }
}
```

* 사용자 정보를 담당할 도메인인 User 클래스이다.
* @Enumerated(EnumType.STRING)
  * JPA로 db에 저장할 때 Enum값을 어떤 형태로 저장할지 결정한다.
  * 기본적으로 int 숫자를 저장하지만, DB에서 그 의미를 파악하기 어려워 String으로 선언한다.

<br>

> Role.java

```java
@Getter
@RequiredArgsConstructor
public enum Role {
    GUEST("ROLE_GUEST", "손님"),
    USER("ROLE_USER", "일반 사용자");

    private final String key;
    private final String title;
}
```

* 각 사용자의 권한을 관리할 Enum클래스이다.
* 스프링 시큐리티에서는 권한 코드에 항상 ROLE_이 앞에 있어야 한다.

<br>

> UserRepository.java

```java
public interface UserRepository extends JpaRepository<User, Long> {

    Optional<User> findByEmail(String email);
}
```

* User의 CRUD를 담당하며, 소셜 로그인으로 반환되는 값 중 email을 통해 이미 생성된 사용자인지 처음 가입하는 사용자인지 판단하는 메소드를 추가한다.

<br>

## Spring Security 설정

> build.gradle

```java
compile('org.springframework.boot:spring-boot-starter-oauth2-client')
```

* 소셜 로그인 등 클라이언트 입장에서 소셜 기능 구현시 필요한 의존성이다.
* oauth2-client와 oauth2-jose를 기본적으로 관리해준다.

<br>

> SecurityConfig.java

```java
@RequiredArgsConstructor
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    private final CustomOAuth2UserService customOAuth2UserService;

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
                .csrf().disable()
                .headers().frameOptions().disable()
                .and()
                    .authorizeRequests()
                    .antMatchers("/", "/css/**",
                            "images/**", "/js/**", "/h2-console/**").permitAll()
                    .antMatchers("/api/v1/**").hasRole(Role.USER.name())
                    .anyRequest().authenticated()
                .and()
                    .logout()
                        .logoutSuccessUrl("/")
                .and()
                    .oauth2Login()
                        .userInfoEndpoint()
                            .userService(customOAuth2UserService);
    }
}
```

* OAuth 라이브러리를 이용한 소셜 로그인 설정 코드이다.
* @EnableWebSecurity
  * Spring Security 설정을 활성화시킨다.
* ``csrf().disable().headers().frameOptions().disable()``
  * h2-console 화면을 사용하기 위해 해당 옵션들을 disable한다.
* ``authorizeRequests()``
  * URL별 권한 관리를 설정하는 옵션의 시작점이다.
  * authorizeRequests가 선언되어야만 antMatchers 옵션을 사용할 수 있다.
* ``antMatchers()``
  * 권환 관리 대상을 지정하는 옵션이다.
  * URL, HTTP 메소드별로 관리가 가능하다.
  * "/" 등 지정된 URL들은 전체 열람 권한을 준다.
  * "/api/v1/\*\*" 주소를 가진 API는 USER 권한을 가진 사람만 가능하도록 한다.
* ``anyRequest()``
  * 설정된 값들 이외 나머지 URL들을 나타낸다.
  * ``authenticated()`` 를 추가해 나머지 URL들은 인증된(로그인) 사용자들만 허용시킨다.
* ``logout().logoutSuccessUrl("/")``
  * 로그아웃 기능에 대한 여러 설정의 진입점이다.
  * 로그아웃 성공 시 해당 주소로 이동한다.
* ``oauth2Login()``
  * 로그인 기능에 대한 여러 설정의 진입점이다.
* ``userInfoEndpoint()``
  * 로그인 성공 이후 사용자 정보를 가져올 때의 설정을 담당한다.
* ``userService()``
  * 소셜 로그인 성공 시 후속 조치를 진행할 UserService 인터페이스의 구현체를 등록한다.
  * 리소스 서버(소셜 서비스들)에서 사용자 정보를 가져온 상태에서 추가로 진행하고자 하는 기능을 명시할 수 있다.

<br>

> CustomOAuth2UserService.java

```java
@RequiredArgsConstructor
@Service
public class CustomOAuth2UserService implements OAuth2UserService<OAuth2UserRequest, OAuth2User> {

    private final UserRepository userRepository;
    private final HttpSession httpSession;

    @Override
    public OAuth2User loadUser(OAuth2UserRequest userRequest) throws OAuth2AuthenticationException {
        OAuth2UserService delegate = new DefaultOAuth2UserService();
        OAuth2User oAuth2User = delegate.loadUser(userRequest);

        String registrationId = userRequest.
                getClientRegistration().getRegistrationId();
        String userNameAttributeName = userRequest.
                getClientRegistration().getProviderDetails()
                .getUserInfoEndpoint().getUserNameAttributeName();

        OAuthAttributes attributes = OAuthAttributes
                .of(registrationId, userNameAttributeName,
                        oAuth2User.getAttributes());
        User user = saveOrUpdate(attributes);

        httpSession.setAttribute("user", new SessionUser(user));

        return new DefaultOAuth2User(
                Collections.singleton(new
                        SimpleGrantedAuthority(user.getRoleKey())),
                        attributes.getAttributes(),
                        attributes.getNameAttributeKey());
    }

    private User saveOrUpdate(OAuthAttributes attributes) {
        User user = userRepository.findByEmail(attributes.getEmail())
                .map(entity -> entity.update(attributes.getName(),
                        attributes.getPicture()))
                .orElse(attributes.toEntity());
        return userRepository.save(user);
    }
}
```

* 구글 로그인 이후 가져온 사용자의 정보들을 기반으로 가입 및 정보 수정, 세션 저장 등의 기능을 지원한다.
* registrationId
  * 현재 로그인 진행 중인 서비스를 구분하는 코드이다.
  * 지금은 구글만 사용하는 불필요한 값이지만, 이후 네이버 등 다른 소셜 로그인이 추가되면 구분하기 위해 사용한다.
* userNameAttributeName
  * OAuth2 로그인 진행 시 키가 되는 필드값을 이야기하며, PK 의미이다.
  * 구글의 경우 기본적으로 코드를 지원하나, 네이버 및 카카오 등은 지원하지 않는다.
  * 구글의 기본 코드는 "sub"이며, 이후 네이버 로그인과 구글 로그인을 동시 지원할 때 사용한다.
* OAuthAttributes
  * OAuth2UserService를 통해 가져온 OAuth2User의 attribute를 담을 클래스이다.
  * 네이버 등 다른 소셜 로그인들도 이 클래스를 사용한다.
* SessionUser
  * 세션에 사용자 정보를 저장하기 위한 Dto 클래스이다.
* 구글 사용자 정보가 업데이트 되었을 때를 대비해 update 기능도 구현하며, 이는 변경시 User 엔티티에 반영된다.

<br>

> OAuthAttributes.java

```java
@Getter
public class OAuthAttributes {
    private Map<String, Object> attributes;
    private String nameAttributeKey;
    private String name;
    private String email;
    private String picture;

    @Builder
    public OAuthAttributes(Map<String, Object> attributes,
                           String nameAttributeKey,
                           String name,
                           String email,
                           String picture) {
        this.attributes = attributes;
        this.nameAttributeKey = nameAttributeKey;
        this.name = name;
        this.email = email;
        this.picture = picture;
    }

    public static OAuthAttributes of(String registrationId,
                                     String userNameAttributeName,
                                     Map<String, Object> attributes) {
        return ofGoogle(userNameAttributeName, attributes);
    }

    private static OAuthAttributes ofGoogle(String userNameAttributeName,
                                           Map<String, Object> attributes) {
        return OAuthAttributes.builder()
                .name((String)attributes.get("name"))
                .email((String)attributes.get("email"))
                .picture((String)attributes.get("picture"))
                .attributes(attributes)
                .nameAttributeKey(userNameAttributeName)
                .build();
    }

    public User toEntity() {
        return User.builder()
                .name(this.name)
                .email(this.email)
                .picture(this.picture)
                .role(Role.GUEST)
                .build();
    }
}
```

* Dto로 간주한다.
* ``of()``
  * OAuth2User에서 반환하는 사용자 정보는 Map이기 때문에 각 값을 변환해야 한다.
* ``toEntity()``
  * User 엔티티를 생성한다.
  * OAuthAttributes에서 엔티티를 생성하는 시점은 처음 가입할 때이다.
  * 가입할 때 기본 권한을 Guest로 주기 위해 role 빌더값에 Role.GUEST를 이용한다.

<br>

> SessionUser.java

```java
@Getter
public class SessionUser implements Serializable {
    private String name;
    private String email;
    private String picture;

    public SessionUser(User user) {
        this.name = user.getName();
        this.email = user.getEmail();
        this.picture = user.getPicture();
    }
}
```

* SessionUser에는 인증된 사용자 정보만 필요로 한다.
* User 클래스를 그대로 사용한다면 세션에 저장할 때 직렬화를 구현하지 않아 에러가 발생한다.
* 그러나 User 클래스에 직렬화 코드를 넣기에는 리스크가 크다.
  * User 클래스는 엔티티이며, 엔티티 클래스는 언제 다른 엔티티와 관계를 형성할지 모른다.
  * @OneToMany, @ManyToMany 등 자식 엔티티를 갖고 있다면 직렬화 대상에 자식들까지 포함된다.
    * 이는 성능 이슈 및 부수 효과를 발생시킬 확률이 높다.
  * 따라서 직렬화 기능을 가진 세션 Dto를 추가하는 것이 운영 및 유지 보수에 도움된다.

<br>

## 로그인 테스트

> index.mustache

```html
<!-- 로그인 기능 영역 -->
<div class="row">
    <div class="col-md-6">
        <a href="/posts/save" role="button" class="btn btn-primary">
            글 등록
        </a>
        {{#userName}}
            Logged in as: <span id="user">{{userName}}</span>
            <a href="/logout" class="btn btn-info active" role="button">Logout</a>
        {{/userName}}
        {{^userName}}
            <a href="/oauth2/authorization/google" class="btn btn-success active" role="button">
                Google Login
            </a>
        {{/userName}}
    </div>
</div>
```

* {{#userName}}
  * 머스테치는 if 조건문을 제공하지 않고, true 및 false만 판별한다.
  * 머스테치는 항상 최종값을 넘겨줘야 하며, userName이 있으면 이를 노출시키도록 구성한다.
* a href="/logout"
  * 스프링 시큐리티에서 기본적으로 제공하는 로그아웃 URL이다.
  * 개발자가 별도로 해당 URL에 해당하는 컨트롤러를 만들 필요는 없다.
  * SecurityConfig 클래스에서 URL 변경이 가능하다.
* {{^userName}}
  * 해당 값이 존재하지 않는 경우 ^를 사용해 로그인 버튼을 노출시키도록 구성한다.
* a href="/oauth2/authorization/google"
  * 스프링 시큐리티에서 기본적으로 제공하는 로그인 URL이다.

<br>

> IndexController.java

```java
private final PostsService postsService;
private final HttpSession httpSession;

@GetMapping("/")
public String index(Model model) {
    model.addAttribute("posts", postsService.findAllDesc());
    SessionUser user = (SessionUser) httpSession.getAttribute("user");
    if (user != null) {
        model.addAttribute("userName", user.getName());
    }
    return "index";
}
```

* 앞서 작성된 CustomOAuth2UserService에서 로그인 성공 시 세션에 SessionUser를 저장하도록 구성해두었다.
* h2 console로 확인하면 GUEST 권한이며, posts 기능을 사용할 수 없다.

<br>

## Annotation 기반으로 개선하기

> IndexController.java

```java
SessionUser user = (SessionUser)httpSession.getAttribute("user");
```

* 같은 코드를 복사 붙여넣기로 반복한다면 이후 수정이 필요할 때 유지 보수가 어렵다.
* 다른 메소드에서 세션값이 필요하다면 그 때마다 직접 세션에서 값을 가져와야 하는데, 이를 메소드 인자로 세션값을 바로 받을 수 있도록 변경한다.

<br>

> LoginUser.java

```java
@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface LoginUser {
}
```

* 메소드의 파라미터로 선언된 객체에서만 사용할 수 있다.

<br>

> LoginUserArgumentResolver.java

```java
@RequiredArgsConstructor
@Component
public class LoginUserArgumentResolver implements HandlerMethodArgumentResolver {

    private final HttpSession httpSession;

    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        boolean isLoginUserAnnotation = parameter.getParameterAnnotation(LoginUser.class) != null;
        boolean isUserClass = SessionUser.class
                .equals(parameter.getParameterType());
        return isLoginUserAnnotation && isUserClass;

    }

    @Override
    public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
        return httpSession.getAttribute("user");
    }
}
```

* HandlerMethodArgumentResolver 인터페이스는 조건에 맞는 경우 메소드가 있다면, 해당 메소드의 파라미터에 구현체가 지정한 값을 보낼 수 있다.
* ``supportsParameter()``
  * 컨트롤러 메서드의 특정 파라미터를 지원하는지 판단한다.
  * @LoginUser 어노테이션이 있으며, SessionUser.class인 경우만 허용한다.

<br>

> WebConfig.java

```java
@RequiredArgsConstructor
@Configuration
public class WebConfig implements WebMvcConfigurer {
    private final LoginUserArgumentResolver loginUserArgumentResolver;

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(loginUserArgumentResolver);
    }
}
```

* Inflearn에서 배운 내용이며, WebMvcConfigurer를 이용하여 Spring에서 인식될 수 있게끔 추가한다.
* 이 방법이 한계가 있다면 직접 해당 기능을 @Bean으로 등록하여 구현한다.

<br>

> IndexController.java

```java
@GetMapping("/")
public String index(Model model, @LoginUser SessionUser user) {
    model.addAttribute("posts", postsService.findAllDesc());
    if (user != null) {
        model.addAttribute("userName", user.getName());
    }
    return "index";
}
```

<br>

## Spring Boot의 Session 문제

* 현재 애플리케이션은 재실행하면 로그인이 풀리는데, 이는 세션이 내장 톰캣의 메모리에 저장되기 때문이다.
  * 기본적으로 세션은 실행되는 WAS의 메모리에 저장되고 호출된다.
  * 배포할 때마다 톰캣이 재시작되어 초기화된다.
* 2대 이상의 서버에서 서비스하고 있다면 톰캣마다 세션 동기화를 설정해야 한다.

<br>

## 현업 해결 방법

* (1) 톰캣 세션을 사용한다.
  * 별다른 설정을 하지 않을 때 기본 선택 방식이다.
  * 이렇게 될 경우 톰캣(WAS)에 세션이 저장되기 때문에 2대 이상의 WAS가 구동되는 환경에서는 톰캣들 간의 세션 공유 설정이 필요하다.
* (2) MySQL과 같은 DB를 세션 저장소로 사용한다.
  * 여러 WAS 간의 공용 세션을 사용할 수 있는 쉬운 방법이다.
  * 로그인 요청마다 DB IO가 발생하여 성능상 이슈가 발생할 수 있다.
  * 로그인 요청이 많이 없는 백오피스 혹은 사내 시스템 용도에서 사용한다.
* (3) Redis, Memcached와 같은 메모리 DB를 세션 저장소로 사용한다.
  * B2C 서비스에서 많이 사용하는 방식이다.
  * 실제 서비스로 사용하기 위해서는 Embedded Redis와 같은 방식이 아닌 외부 메모리 서버가 필요하다.

<br>

## 설정

> build.gradle

```java
compile('org.springframework.session:spring-session-jdbc')
```

<br>

> application.properties

```java
spring.session.store-type=jdbc
```

* h2-console에 접근하면 세션을 위한 테이블 2개가 생성된다.
* Spring을 재시작하면 세션이 풀리는데 이는 H2가 재시작되기 때문이다.
* 추후 AWS로 배포하게 되면 AWS의 DB인 RDS를 사용하면서 세션이 풀리지 않는다.

<br>

## 네이버 API 등록

> application-oauth.properties

```java
# NAVER REGISTRATION

spring.security.oauth2.client.registration.naver.client-id=id
spring.security.oauth2.client.registration.naver.client-secret=secret
spring.security.oauth2.client.registration.naver.redirect-uri={baseUrl}/{action}/oauth2/code/{registrationId}
spring.security.oauth2.client.registration.naver.authorization-grant-type=authorization_code
spring.security.oauth2.client.registration.naver.scope=name,email,profile_image
spring.security.oauth2.client.registration.naver.client-name=Naver

# NAVER Provider

spring.security.oauth2.client.provider.naver.authorization-uri=https://nid.naver.com/oauth2.0/authorize
spring.security.oauth2.client.provider.naver.token-uri=https://nid.naver.com/oauth2.0/token
spring.security.oauth2.client.provider.naver.user-info-uri=https://openapi.naver.com/v1/nid/me
spring.security.oauth2.client.provider.naver.user-name-attribute=response
```

* https://developers.naver.com/ 에서 등록한다.
* 네이버에서는 스프링 시큐리티를 공식 지원하지 않기 때문에 CommonOAuth2Provider에서 해주던 값들을 수동 입력한다.
* 기준이 되는 user_name의 이름을 네이버에서는 response로 한다.
  * 네이버의 회원 조회 시 반환되는 JSON 형태 때문이다.
* 스프링 시큐리티에서는 하위 필드를 명시할 수 없으며, 최상위 필드만 user_name으로 지정 가능하다.
  * 네이버의 응답값 최상위 필드는 resultCode, message, response이다.
  * 이 세 필드 중 한 가지를 선택해야 하며, 이 경우 Java 코드로 response의 id를 user_name으로 지정한다.

## Spring Security 설정 등록

> OAuthAttributes.java

```java
public static OAuthAttributes of(String registrationId,
                                 String userNameAttributeName,
                                 Map<String, Object> attributes) {
    if ("naver".equals(registrationId))
        return ofNaver("id", attributes);
    return ofGoogle(userNameAttributeName, attributes);
}

private static OAuthAttributes ofNaver(String userNameAttributeName,
                                       Map<String, Object> attributes) {
    Map<String, Object> response = (Map<String, Object>)attributes.get("response");
    return OAuthAttributes.builder()
            .name((String)attributes.get("name"))
            .email((String)attributes.get("email"))
            .picture((String)attributes.get("profile_image"))
            .attributes(response)
            .nameAttributeKey(userNameAttributeName)
            .build();
}
```

<br>

> index.mustache

```html
{{^userName}}
    <a href="/oauth2/authorization/google" class="btn btn-success active" role="button">
        Google Login
    </a>
    <a href="/oauth2/authorization/naver" class="btn btn-secondary active" role="button">
        Naver Login
    </a>
{{/userName}}
```

* application-oauth.properties에 등록한 redirect-uri 값에 맞춰 자동 등록된다.
* /oauth2/authorization/ 까지 고정이며 마지막 Path만 소셜 로그인 코드를 사용하면 된다.

<br>

## 기존 테스트에 Security 적용하기

* 시큐리티 옵션이 활성화되면 인증된 사용자만 API를 호출할 수 있다.
* 기존의 API 테스트 코드들이 인증에 대한 권한을 받지 못하였으므로, 이를 수정한다.

<br>

## 1. CustomOAuth2UserService를 찾을 수 없음

> application.properties

```java
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
spring.h2.console.enabled=true
spring.session.store-type=jdbc

# TEST OAuth
spring.security.oauth2.client.registration.google.client-id=test
spring.security.oauth2.client.registration.google.client-secret=test
spring.security.oauth2.client.registration.google.scope=profile,email
```

* 단순한 "hello" GetMapping 관련 테스트가 fail이 뜬다.
* CustomOAuth2UserService를 생성하는데 필요한 소셜 로그인 관련 설정값들이 없기 때문에 발생한다.
* main과 test는 본인만의 환경 구성을 가진다.
* test에 application.properties가 없으면 main의 설정을 가져오지만, 자동으로 가져오는 옵션의 범위는 application.properties 파일까지다.
* 따라서 application-oauth.properties를 인식하지 못하기 때문에, 가짜 설정값을 등록한다.

<br>

## 2. 302 Status Code

* 스프링 시큐리티 설정 때문에 인증되지 않은 사용자의 요청은 이동 시킨다.
* 따라서 save 및 update 등의 API 요청은 임의로 인증된 사용자를 추가하여 API만 테스트할 수 있다.

<br>

> build.gradle

```java
testCompile('org.springframework.security:spring-security-test')
```

<br>

> PostsApiControllerTest.java

```java
@Test
@WithMockUser(roles = "USER")
public void saveTest() throws Exception {
  ...
}

@Test
@WithMockUser(roles = "USER")
public void updateTest() throws Exception {
  ...
}
```

* 인증된 모의 사용자를 만들어 사용하며, 권한을 추가할 수 있다.
* 해당 어노테이션을 통해 ROLE_USER 권한을 가진 사용자가 API를 요청하는 것과 동일한 효과를 가진다.

<br>

> PostsApiControllerTest.java

```java
@Autowired
private WebApplicationContext context;

private MockMvc mvc;

@Before
public void setup() {
    mvc = MockMvcBuilders
            .webAppContextSetup(context)
            .apply(springSecurity())
            .build();
}
@After
public void tearDown() throws Exception {
    postsRepository.deleteAll();
}

//saveTest
mvc.perform(post(url)
            .contentType(MediaType.APPLICATION_JSON_UTF8)
            .content(new ObjectMapper().writeValueAsString(requestDto)))
            .andExpect(status().isOk());

//updateTest
mvc.perform(put(url)
            .contentType(MediaType.APPLICATION_JSON_UTF8)
            .content(new ObjectMapper().writeValueAsString(requestDto)))
            .andExpect(status().isOk());
```

* @WithMockUser는 MockMvc에서만 작동하기 때문에, MockMvc를 테스트 전에 생성한다.
* ObjectMapper로 본문 요청에 해당하는 Dto를 JSON 문자열로 변환하여 요청을 날린다.

<br>

## 3. @WebMvcTest에서 CustomOAuth2UserService를 찾을 수 없음

> HelloControllerTest.java

```java
@WebMvcTest(controllers = HelloControllerTest.class,
excludeFilters = {
  @ComponentScan.Filter(type = FilterType.ASSIGNABLE_
  TYPE, classes = SecurityConfig.class)
})


//각 테스트 메소드 앞에
@WithMockUser(rolers = "USER")
```

<br>


* @WebMvcTest는 Controller 등 웹 관련 설정만 스캔한다.
  * 따라서 @WebMvcTest가 아닌 스프링 부트 통합 테스트 환경은 테스트가 정상 수행된다.
* SecurityConfig는 읽지만, CustomOAuth2UserService를 스캔하지 않는다.
* 이를 해결하기 위해 스캔 대상에서 SecurityConfig를 제외한다.

<br>

> JpaConfig.java

```java
@EnableJpaAuditing
@Configuration
public class JpaConfig {
}
```

* 테스트를 돌리면 @EnableJpaAuditing으로 에러가 발생한다.
  * @EnableJpaAuditing를 사용하기 위해서는 최소한 하나 이상의 @Entity 클래스가 필요하다.
  * @EnableJpaAuditing이 @SpringBootApplication과 함께 있어서 스캔이 됬지만, @WebMvcTest는 사용할 @Entity 클래스가 없어 발생한 문제이다.
* 따라서 @EnableJpaAuditing과 @SpringBootApplication을 분리한다.

<br>

---

Reference
---------

-	스프링 부트와 AWS로 혼자 구현하는 웹 서비스 (이동욱 저)
