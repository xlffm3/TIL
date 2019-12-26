RestController
--------------

---

### @RestController<br>

-	Spring 4 version에서 Rest API 또는 Web API 개발을 위해 등장한 Annotation이다.<br><br>
-	이전 버전의 @Controller와 @ResponseBody를 포함한다.<br><br>

### MessageConvertor<br>

-	자바 객체와 HTTP 요청 / 응답 바디를 변환하는 역할이다.<br><br>
-	외부에서 전달받은 JSON 메서드를 내부에서 사용할 수 있는 객체로 변환하거나, 컨트롤러를 리턴한 객체가 클라이언트에게 JSON으로 변환해서 전달하는 역할 등이 그 예다.<br><br>
-	@ResponseBody, @RequestBody.<br><br>
-	@EnableWebMvc 로 인한 기본 설정이 된다.<br><br>
-	WebMvcConfigurationSupport 를 사용하여 Spring MVC 구현한다.<br><br>
-	Default MessageConvertor 를 제공한다.<br><br>
-	링크 바로가기의 `addDefaultHttpMessageConverters` 메소드 항목을 참고한다.<br><br>

### MessageConvertor 종류<br>

![image](https://user-images.githubusercontent.com/56240505/70691659-f1a9e100-1cfc-11ea-9f26-0f9e8d62c868.png)<br><br>

### JSON 응답하기<br>

-	컨트롤러의 메소드에서는 JSON으로 변환될 객체를 반환한다.<br><br>
-	jackson라이브러리를 추가할 경우 객체를 JSON으로 변환하는 메시지 컨버터가 사용되도록 @EnableWebMvc에서 기본으로 설정되어 있다.<br><br>
-	jackson라이브러리를 추가하지 않으면 JSON메시지로 변환할 수 없어 500오류가 발생한다.<br><br>
-	사용자가 임의의 메시지 컨버터(MessageConverter)를 사용하도록 하려면 WebMvcConfigurerAdapter의 configureMessageConverters메소드를 오버라이딩 한다.<br><br>

### Practice<br>

```java
@RestController
@RequestMapping(path="/guestbooks")
public class GuestbookApiController {
    @Autowired
    GuestbookService guestbookService;

    @GetMapping
    public Map<String, Object> list(@RequestParam(name="start", required=false, defaultValue="0") int start) {

        List<Guestbook> list = guestbookService.getGuestbooks(start);

        int count = guestbookService.getCount();
        int pageCount = count / GuestbookService.LIMIT;
        if(count % GuestbookService.LIMIT > 0)
            pageCount++;

        List<Integer> pageStartList = new ArrayList<>();
        for(int i = 0; i < pageCount; i++) {
            pageStartList.add(i * GuestbookService.LIMIT);
        }

        Map<String, Object> map = new HashMap<>();
        map.put("list", list);
        map.put("count", count);
        map.put("pageStartList", pageStartList);

        return map;
    }

    @PostMapping
    public Guestbook write(@RequestBody Guestbook guestbook,
                        HttpServletRequest request) {
        String clientIp = request.getRemoteAddr();
        // id가 입력된 guestbook이 반환된다.
        Guestbook resultGuestbook = guestbookService.addGuestbook(guestbook, clientIp);
        return resultGuestbook;
    }

    @DeleteMapping("/{id}")
    public Map<String, String> delete(@PathVariable(name="id") Long id,
            HttpServletRequest request) {
        String clientIp = request.getRemoteAddr();

        int deleteCount = guestbookService.deleteGuestbook(id, clientIp);
        return Collections.singletonMap("success", deleteCount > 0 ? "true" : "false");
    }
}
```

-	해당 API 클래스 내부에서는 같은 매핑으로 여러 개를 수행하기 때문에, 클래스 자체에다가 RequestMapping을 선언하면 클래스 내부에 있는 것들을 공통으로 사용할 수 있다.<br><br>
-	Class 자체에 Mapping을 했기 때문에, 메서드에는 별도의 Mapping path 설정이 필요 없다.<br><br>
-	GetMapping 메서드 : application/json 요청이기 때문에 DispatcherServlet은 jsonMessageConverter를 내부적으로 사용해서 해당 Map 객체를 json으로 변환해서 전송한다.<br><br>
-	PostMapping 메서드 역시 클라이언트에 응답이 갈 때는 json 메서드로 변환된다.<br><br>
-	Chrome의 Restlet Client 플러그인으로 테스트를 할 수 있다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16773/)

---
