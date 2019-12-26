Cookie
------

---

### Cookie<br>

-	클라이언트 단에 저장되는 작은 정보의 단위이다.<br><br>
-	클라이언트에서 생성하고 저장될 수 있고, 서버 단에서 전송한 쿠키가 클라이언트에 저장될 수 있다.<br><br>
-	서버에서 클라이언트의 브라우저로 전송되어 사용자의 컴퓨터에 저장한다.<br><br>
-	저장된 쿠키는 다시 해당하는 웹 페이지에 접속할 때, 브라우저에서 서버로 쿠키를 전송한다.<br><br>
-	쿠키는 이름(name)과 값(value) 쌍으로 정보를 저장한다.
	-	이름-값 쌍 외에도 도메인(Domain), 경로(Path), 유효기간(Max-Age, Expires), 보안(Secure), HttpOnly 속성을 저장할 수 있다.<br><br>
-	쿠키의 수와 그 크기는 브라우저별로 상이한 제한 값을 가지고 있다.<br><br>

### javax.servlet.http.Cookie<br>

![image](https://user-images.githubusercontent.com/56240505/71465621-808c2280-2800-11ea-9ee4-c4f9707977fe.png)<br>

```java
Cookie cookie = new Cookie(이름, 값);
response.addCookie(cookie);

Cookie[] cookies = request.getCookies();

Cookie cookie = new Cookie("이름", null);
cookie.setMaxAge(0);
response.addCookie(cookie);
```

-	서버에서 쿠키를 생성하고, Reponse의 addCookie메소드를 이용해 클라이언트에게 전송한다.<br><br>
-	쿠키는 (이름, 값)의 쌍 정보를 입력하여 생성한다.<br><br>
-	쿠키의 이름은 일반적으로 알파벳과 숫자, 언더바로 구성하며 한글의 경우 인코딩과 디코딩의 과정이 포함된다.<br><br>
-	클라이언트가 보낸 쿠키 정보를 읽을 때 배열이 반환되며, 없을 경우 null이 return된다.<br><br>
-	Cookie가 가지고 있는 getName()과 getValue()메소드를 이용해서 원하는 쿠키정보를 찾아 사용한다.<br><br>
-	쿠키를 삭제하는 명령은 없고, maxAge가 0인 같은 이름의 쿠키를 전송한다.<br><br>
-	같은 이름의 쿠키는 가질 수 없기 때문에 새로 들어온 쿠키를 클라이언트가 지니게 되는데, 유효시간이 0이면 삭제되는 원리이다.<br><br>
-	메소드 `setMaxAge()`
	-	인자는 유효기간을 나타내는 초 단위의 정수형이다.
	-	만일 유효기간을 0으로 지정하면 쿠키를 삭제한다.
	-	음수를 지정하면 브라우저가 종료될 때 쿠키가 삭제된다.<br><br>
-	유효기간을 10분으로 지정하려면
	-	cookie.setMaxAge(10 * 60); //초 단위 : 10분
	-	1주일로 지정하려면 (7*24*60*60)로 설정합니다.<br><br>

### Spring MVC의 Cookie<br>

-	@CookieValue Annotation
	-	컨트롤러 메소드의 파라미터에서 CookieValue애노테이션을 사용함으로써 원하는 쿠키정보를 파라미터 변수에 담아 사용할 수 있다.<br><br>
-	컨트롤러메소드(@CookieValue(value="쿠키이름", required=false, defaultValue="기본값") String 변수명)<br><br>

### Practice<br>

> 기본 예제<br>

```java
@GetMapping(path="/list")
public String list(@RequestParam(name="start", required=false, defaultValue="0") int start,
           ModelMap model,
                     HttpServletRequest request,
           HttpServletResponse response) {


  String value = null;
  boolean find = false;
  Cookie[] cookies = request.getCookies();
  if(cookies != null) {
    for(Cookie cookie : cookies) {
      if("count".equals(cookie.getName())) {
        find = true;
        value = cookie.getValue();
      }
    }
  }  

  if(!find) {
    value = "1";
  }else { // 쿠키를 찾았다면.
    try {
      int i = Integer.parseInt(value);
      value = Integer.toString(++i);
    }catch(Exception ex) {
      value = "1";
    }
  }  

  Cookie cookie = new Cookie("count", value);
  cookie.setMaxAge(60 * 60 * 24 * 365); // 1년 동안 유지.
  cookie.setPath("/"); // / 경로 이하에 모두 쿠키 적용.
  response.addCookie(cookie);
  model.addAttribute("cookieCount", value); // jsp에게 전달하기
```

<br>

> Spring MVC의 @CookieValue Annotation 사용<br>

```java
@GetMapping(path="/list")
    public String list(@RequestParam(name="start", required=false, defaultValue="0") int start,
                       ModelMap model, @CookieValue(value="count", defaultValue="1", required=true) String value,
                       HttpServletResponse response) {

        // 쿠키 값을 1증가 시킨다.
        try {
            int i = Integer.parseInt(value);
            value = Integer.toString(++i);
        }catch(Exception ex){
            value = "1";
        }

        // 쿠키를 전송한다.
        Cookie cookie = new Cookie("count", value);
        cookie.setMaxAge(60 * 60 * 24 * 365); // 1년 동안 유지.
        cookie.setPath("/"); // / 경로 이하에 모두 쿠키 적용.
        response.addCookie(cookie);     
```

<br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16799/)

---
