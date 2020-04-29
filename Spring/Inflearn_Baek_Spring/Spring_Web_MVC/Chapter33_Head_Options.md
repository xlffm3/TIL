Spring Web MVC에서 자동으로 처리하는 HTTP Method
------------------------------------------------

> SimpleControllerTest.java

```java
@Test
public void hello() throws Exception {
    this.mockMvc.perform(options("/hello"))
                .andDo(print())
                .andExpect(status().isOk())
                .andExpect(content().string("hello"))
                .andExpect(header().stringValues(HttpHeaders.ALLOW, "GET", "POST", "HEAD", "OPTIONS"));
}
```

-	HEAD.<br><br>
-	OPTIONS.<br><br>

HEAD
----

-	GET 요청과 동일하지만 응답 본문을 받아오지 않고 응답 헤더만 받아온다.<br><br>

OPTIONS
-------

-	사용할 수 있는 HTTP Method 목록을 제공한다.
-	서버 또는 특정 리소스가 제공하는 기능을 확인할 수 있다.<br><br>
-	서버는 Allow 응답 헤더에 사용할 수 있는 HTTP Method 목록을 제공해야 한다.<br><br>

---

Reference
---------

-	Spring 웹 MVC(백기선, Inflearn).<br><br>
