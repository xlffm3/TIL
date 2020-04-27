Resource Handler
----------------

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("mobile/**")
                .addResourceLocations("classpath:/mobile")
                .setCacheControl(CacheControl.maxAge(10, TimeUnit.MINUTES));
    }
}
```

-	이미지, 자바스크립트, CSS 그리고 HTML 파일과 같은 정적인 리소스를 처리하는 핸들러이다.<br><br>
-	디폴트(Default) 서블릿은 서블릿 컨테이너가 기본으로 제공하는 서블릿으로 정적인 리소스를 처리할 때 사용한다.<br><br>
-	Spring은 이러한 디폴트 서블릿에 요청을 위임해서 정적인 리소스를 처리한다.<br><br>
-	Spring Boot는 기본 정적 리소스 핸들러 및 캐싱을 제공한다.<br><br>

Spring MVC 리소스 핸들러 맵핑 등록
----------------------------------

-	DefaultServletHandlerConfigurer.<br><br>
-	가장 낮은 우선 순위로 등록한다.<br><br>
-	다른 핸들러 맵핑이 “/” 이하 요청을 처리하도록 허용하고, 최종적으로 리소스 핸들러가 처리하도록 한다.<br><br>

리소스 핸들러 설정
------------------

-	어떤 요청 패턴을 지원할 것인가?<br><br>
-	어디서 리소스를 찾을 것인가?<br><br>
-	캐싱.<br><br>
-	ResourceResolver : 요청에 해당하는 리소스를 찾는 전략이다.
	-	캐싱, 인코딩(gzip, brotli), WebJar 등.<br><br>
-	ResourceTransformer : 응답으로 보낼 리소스를 수정하는 전략이다.
	-	캐싱, CSS 링크, HTML5 AppCache 등.<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
