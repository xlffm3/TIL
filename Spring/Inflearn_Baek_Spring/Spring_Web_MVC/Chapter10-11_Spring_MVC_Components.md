Spring MVC Components
---------------------

![image](https://user-images.githubusercontent.com/56240505/80305103-c29c9380-87f5-11ea-8a60-1ae157c2fe84.png)

DispatcherSerlvet의 기본 전략
-----------------------------

-	DispatcherServlet.properties.<br><br>

MultipartResolver
-----------------

-	파일 업로드 요청 처리에 필요한 인터페이스이다.<br><br>
-	HttpServletRequest를 MultipartHttpServletRequest로 변환해주어 요청이 담고 있는 File을 꺼낼 수 있는 API 제공한다.<br><br>

LocaleResolver
--------------

-	클라이언트의 위치(Locale) 정보를 파악하는 인터페이스이다.<br><br>
-	기본 전략은 요청의 accept-language를 보고 판단한다.<br><br>

ThemeResolver
-------------

-	애플리케이션에 설정된 테마를 파악하고 변경할 수 있는 인터페이스이다.<br><br>

HandlerMapping
--------------

-	요청을 처리할 핸들러를 찾는 인터페이스이다.<br><br>

HandlerAdapter
--------------

-	HandlerMapping이 찾아낸 “핸들러”를 처리하는 인터페이스이다.<br><br>
-	스프링 MVC 확장력의 핵심이다.<br><br>

HandlerExceptionResolver
------------------------

-	요청 처리 중에 발생한 에러 처리하는 인터페이스이다.<br><br>

RequestToViewNameTranslator
---------------------------

-	핸들러에서 뷰 이름을 명시적으로 리턴하지 않은 경우, 요청을 기반으로 뷰 이름을 판단하는 인터페이스이다.<br><br>

ViewResolver
------------

-	뷰 이름(string)에 해당하는 뷰를 찾아내는 인터페이스이다.<br><br>

FlashMapManager
---------------

-	FlashMap 인스턴스를 가져오고 저장하는 인터페이스이다.<br><br>
-	FlashMap은 주로 리다이렉션을 사용할 때 요청 매개변수를 사용하지 않고 데이터를 전달하고 정리할 때 사용한다.<br><br>
-	redirect:/events..<br><br>

---

Reference
---------

-	스프링 웹 MVC(백기선, Inflearn)
