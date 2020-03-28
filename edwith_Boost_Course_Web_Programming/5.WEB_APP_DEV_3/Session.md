Session
-------

-	클라이언트 별로 서버에 저장되는 정보이다.<br><br>
-	웹 클라이언트가 서버측에 요청을 보내게 되면 서버는 클라이언트를 식별하는 session id를 생성한다.<br><br>
-	서버는 session id를 이용해서 key와 value를 이용한 저장소인 HttpSession을 생성한다.<br><br>
-	서버는 session id를 저장하고 있는 쿠키를 생성하여 클라이언트에 전송한다.<br><br>
-	클라이언트는 서버측에 요청을 보낼때 session id를 가지고 있는 쿠키를 전송한다.<br><br>
-	서버는 쿠키에 있는 session id를 이용해서 그 전 요청에서 생성한 HttpSession을 찾고 사용한다.<br><br>

javax.servlet.http.Cookie
-------------------------

![image](https://user-images.githubusercontent.com/56240505/71468653-0eb8d680-280a-11ea-9849-a1fe14a1be7b.png) ![image](https://user-images.githubusercontent.com/56240505/71468655-11b3c700-280a-11ea-9da7-4e1ec9050b8d.png)<br>

```xml
<session-config>
  <session-timeout>30</session-timeout>
</session-config>
```

-	세션은 클라이언트가 서버에 접속하는 순간 생성된다.<br><br>
-	특별히 지정하지 않으면 세션의 유지 시간은 기본 값으로 30분 설정된다.<br><br>
-	세션의 유지 시간이란 서버에 접속한 후 서버에 요청을 하지 않는 최대 시간이다.<br><br>
-	30분 이상 서버에 전혀 반응을 보이지 않으면 세션이 자동으로 끊어지며, 세션 유지 시간은 web.xml파일에서 설정 가능하다.<br><br>

Session 생성
------------

```java
HttpSession session = request.getSession();
HttpSession session = request.getSession(true);
HttpSession session = request.getSession(false);
```

-	request의 `getSession()` 메소드는 서버에 생성된 세션이 있다면 세션을 반환하고 없다면 새롭게 세션을 생성하여 반환한다.<br><br>
-	새롭게 생성된 세션 인자는 HttpSession이 가지고 있는 `isNew()` 메소드를 통해 알 수 있다.<br><br>
-	파라미터로 false를 전달시, 이미 생성된 세션이 있다면 반환하고 없으면 null을 반환한다.<br><br>

Session 값 저장 및 조회
-----------------------

```java
session.setAttribute(String name, Object value)
String value = (String) session.getAttribute("id");
```

-	반환 값은 Object 유형이므로 저장된 객체로 형 변환이 필요하다.<br><br>
-	기초 실습 내용은 Cookie와 유사하여 생략한다.<br><br>

Spring MVC에서의 Session 사용
-----------------------------

> @SessionAttributes & @ModelAttribute<br>

```java
@SessionAttributes("user")
public class LoginController {
  @ModelAttribute("user")
  public User setUpUserForm() {
    return new User();
  }
}
```

-	@SessionAttributes 파라미터로 지정된 이름과 같은 이름이 @ModelAttribute에 지정되어 있을 경우 메소드가 반환되는 값은 세션에 저장된다.<br><br>

> @ModelAttribute<br>

```java
@Controller
@SessionAttributes("user")
public class LoginController {
......
  @PostMapping("/dologin")
  public String doLogin(@ModelAttribute("user") User user, Model model) {
......
  }
}
```

-	@SessionAttributes의 파라미터와 같은 이름이 @ModelAttribute에 있을 경우 세션에 있는 객체를 가져온 후, 클라이언트로 전송받은 값을 설정한다.<br><br>

> @SessionAttribute<br>

```java
@GetMapping("/info")
public String userInfo(@SessionAttribute("user") User user) {
//...
//...
return "user";
}
```

-	메소드에 @SessionAttribute가 있을 경우 파라미터로 지정된 이름으로 등록된 세션 정보를 읽어와서 변수에 할당한다.<br><br>

> SessionStatus<br>

```java
@Controller
@SessionAttributes("user")
public class UserController {
......
    @RequestMapping(value = "/user/add", method = RequestMethod.POST)
    public String submit(@ModelAttribute("user") User user, SessionStatus sessionStatus) {
  ......
  sessionStatus.setComplete();
                                   ......
   }
 }
```

-	SessionStatus 는 컨트롤러 메소드의 파라미터로 사용할 수 있는 스프링 내장 타입이다.<br><br>
-	이 오브젝트를 이용하면 현재 컨트롤러의 @SessionAttributes에 의해 저장된 오브젝트를 제거할 수 있다.<br><br>

Spring MVC - form tag 라이브러리
--------------------------------

```html
<form:form action="login" method="post" modelAttribute="user">
Email : <form:input path="email" /><br>
Password : <form:password path="password" /><br>
<button type="submit">Login</button>
</form:form>
```

-	modelAttribute속성으로 지정된 이름의 객체를 세션에서 읽어와서 form태그로 설정된 태그에 값을 설정한다.<br><br>

Spring MVC에서의 Session Practice
---------------------------------

> GuestbookAdminController.java

```java
@Controller
public class GuestbookAdminController {

       @GetMapping(path="/loginform")
        public String loginform() {
            return "loginform";
        }

        @PostMapping(path="/login")
        public String login(@RequestParam(name="passwd", required=true) String passwd,
                HttpSession session,
                RedirectAttributes redirectAttr) {

            if("1234".equals(passwd)) {
                session.setAttribute("isAdmin", "true");
            }else {
                redirectAttr.addFlashAttribute("errorMessage","암호가 틀렸습니다.");
                return "redirect:/loginform";
            }
            return "redirect:/list";
        }

       @GetMapping(path="/logout")
        public String login(HttpSession session) {
            session.removeAttribute("isAdmin");
            return "redirect:/list";
        }
}
```

<br>

> GuestbookController.java

```java
@GetMapping(path="/delete")
public String delete(@RequestParam(name="id", required=true) Long id,
                 @SessionAttribute("isAdmin") String isAdmin,
                 HttpServletRequest request,
                 RedirectAttributes redirectAttr) {
  if(isAdmin == null || !"true".equals(isAdmin)) { // 세션값이 true가 아닐 경우
    redirectAttr.addFlashAttribute("errorMessage", "로그인을 하지 않았습니다.");
    return "redirect:loginform";
  }
  String clientIp = request.getRemoteAddr();
  guestbookService.deleteGuestbook(id, clientIp);
  return "redirect:list";       
}
```

<br>

---

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16801/)
