Layered Architecture Practice
-----------------------------

---

<br>

### DBConfig.java<br><br>

```
@Configuration
@EnableTransactionManagement
public class DBConfig implements TransactionManagementConfigurer {
    private String driverClassName = "com.mysql.jdbc.Driver";

    private String url = "jdbc:mysql://localhost:3306/connectdb?useUnicode=true&characterEncoding=utf8";

    private String username = "connectuser";

    private String password = "connect123!@#";

    @Bean
    public DataSource dataSource() {
        BasicDataSource dataSource = new BasicDataSource();
        dataSource.setDriverClassName(driverClassName);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }

    @Override
    public PlatformTransactionManager annotationDrivenTransactionManager() {
        return transactionManger();
    }

    @Bean
    public PlatformTransactionManager transactionManger() {
        return new DataSourceTransactionManager(dataSource());
    }
}
```

> -	WebMvcContextConfiguration 등 기존에 실습해본 코드는 생략했다.<br><br>
> -	@EnableTransactionManagement은 트랜잭션과 관련된 설정을 자동으로 구현해주지만, 사용자 간의 트랜잭션 처리를 위한 PlatformTransactionManager를 구현하기 위해서는 TransactionManagementConfigurer를 별도로 구현해야 한다.<br><br>
> -	오버라이딩한 annotationDrivenTransactionManager 메서드가 PlatformTransactionManager 객체를 반환한다.

<br><br>

### web.xml<br><br>

> -	프레젠테이션 레이어 부분은 DispatcherServlet이, 그 외의 부분은 ContextLoadedListener가 처리하도록 config 이름 등을 설정한다.<br><br>
> -	filter의 경우, 문자 인코딩에 관련된 내용으로 url-pattern에 대해 필터 적용 범위를 전부(/*)로 지정할 수 있다.

<br><br>

### LogDao.java<br><br>

```
@Repository
public class LogDao {
    private NamedParameterJdbcTemplate jdbc;
    private SimpleJdbcInsert insertAction;

    public LogDao(DataSource dataSource) {
        this.jdbc = new NamedParameterJdbcTemplate(dataSource);
        this.insertAction = new SimpleJdbcInsert(dataSource)
                .withTableName("log")
                .usingGeneratedKeyColumns("id");
    }

    public Long insert(Log log) {
        SqlParameterSource params = new BeanPropertySqlParameterSource(log);
        return insertAction.executeAndReturnKey(params).longValue();
    }
}
```

> -	usingGeneratedKeyColumns : ID가 자동으로 입력되게끔 사용한다.<br><br>
> -	insertAction.executeAndReturnKey : insert 명령을 내부적으로 생성해서 실행하고 자동으로 생성된 id 값을 리턴한다.

<br><br>

### GuestbookDaoSqls.java<br><br>

```
public static final String SELECT_PAGING = "SELECT id, name, content, regdate FROM guestbook ORDER BY id DESC limit :start, :limit";
public static final String DELETE_BY_ID = "DELETE FROM guestbook WHERE id = :id";
public static final String SELECT_COUNT = "SELECT count(*) FROM guestbook";
```

> -	query문 중 limit는 시작 값, 끝날 때의 값을 설정해서 특정 부분만 select할 수 있다.

<br><br>

### GuestbookService.java<br><br>

```
public interface GuestbookService {
    public static final Integer LIMIT = 5;
    public List<Guestbook> getGuestbooks(Integer start);
    public int deleteGuestbook(Long id, String ip);
    public Guestbook addGuestbook(Guestbook guestbook, String ip);
    public int getCount();
}
```

> -	서비스의 비즈니스 로직에 대해 인터페이스를 작성한다.

<br><br>

### GuestbookServiceImpl.java<br><br>

```
@Service
public class GuestbookServiceImpl implements GuestbookService{
    @Autowired
    GuestbookDao guestbookDao;

    @Autowired
    LogDao logDao;

    @Override
    @Transactional
    public List<Guestbook> getGuestbooks(Integer start) {
        List<Guestbook> list = guestbookDao.selectAll(start, GuestbookService.LIMIT);
        return list;
    }

    @Override
    @Transactional(readOnly=false)
    public int deleteGuestbook(Long id, String ip) {
        int deleteCount = guestbookDao.deleteById(id);
        Log log = new Log();
        log.setIp(ip);
        log.setMethod("delete");
        log.setRegdate(new Date());
        logDao.insert(log);
        return deleteCount;
    }

    @Override
    @Transactional(readOnly=false)
    public Guestbook addGuestbook(Guestbook guestbook, String ip) {
        guestbook.setRegdate(new Date());
        Long id = guestbookDao.insert(guestbook);
        guestbook.setId(id);

//      if(1 == 1)
//          throw new RuntimeException("test exception");
//          
        Log log = new Log();
        log.setIp(ip);
        log.setMethod("insert");
        log.setRegdate(new Date());
        logDao.insert(log);


        return guestbook;
    }

    @Override
    public int getCount() {
        return guestbookDao.selectCount();
    }   
}
```

> -	Service Layer에 해당한다는 것을 명시하기 위해 @Service Annotation을 작성한다.<br><br>
> -	Autowired로 Bean을 자동 등록한다.<br><br>
> -	읽기만 하는 메서드는 트랜잭션을 처리할 때 @Transactional Annotation을 작성하면 되며, 내부적으로 readOnly를 수행한다.<br><br>
> -	readOnly가 아닐 경우 명시적으로 false를 표기한다.<br><br>
> -	Transactional이 명시된 경우, 메서드 수행 중 예외가 발생했을 때 Rollback이 일어나는 원자성을 확인할 수 있다.

<br><br>

### GuestbookController.java<br><br>

```
@Controller
public class GuestbookController {
    @Autowired
    GuestbookService guestbookService;

    @GetMapping(path="/list")
    public String list(@RequestParam(name="start", required=false, defaultValue="0") int start, ModelMap model) {
        List<Guestbook> list = guestbookService.getGuestbooks(start);

        int cnt = guestbookService.getCount();
        int pageCnt = cnt / GuestbookService.LIMIT;

        if (cnt%GuestbookService.LIMIT > 0) {
            pageCnt++;
        }

        List<Integer> pageStartList = new ArrayList<>();
        for(int i=0; i<pageCnt; i++) {
            pageStartList.add(i*GuestbookService.LIMIT);
        }

        model.addAttribute("list", list);
        model.addAttribute("count", cnt);
        model.addAttribute("pageStartList", pageStartList);

        return "list";
    }

    @PostMapping(path="/write")
    public String write(@ModelAttribute Guestbook guestbook,
                        HttpServletRequest request) {
        String clientIp = request.getRemoteAddr();
        System.out.println("clientIp : " + clientIp);
        guestbookService.addGuestbook(guestbook, clientIp);
        return "redirect:list";
    }   
}
```

<br><br>

### list.jsp<br><br>

```
<body>

    <h1>방명록</h1>
    <br> 방명록 전체 수 : ${count }
    <br>
    <br>

    <c:forEach items="${list}" var="guestbook">

${guestbook.id }<br>
${guestbook.name }<br>
${guestbook.content }<br>
${guestbook.regdate }<br>

    </c:forEach>
    <br>

    <c:forEach items="${pageStartList}" var="pageIndex" varStatus="status">
        <a href="list?start=${pageIndex}">${status.index +1 }</a>&nbsp; &nbsp;
</c:forEach>

    <br>
    <br>
    <form method="post" action="write">
        name : <input type="text" name="name"><br>
        <textarea name="content" cols="60" rows="6"></textarea>
        <br> <input type="submit" value="등록">
    </form>
</body>
```

> -	Controller와 JSP를 연동시키면 간단한 페이징 작업이 완료된다.

<br><br>

[Reference]

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16772/)

---
