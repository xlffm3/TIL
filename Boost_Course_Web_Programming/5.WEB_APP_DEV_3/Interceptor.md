Interceptor
-----------

---

Interceptor
-----------

![image](https://user-images.githubusercontent.com/56240505/71473715-965b1100-281b-11ea-89cf-8853a9b7f403.png)<br><br>

-	Interceptor는 Dispatcher servlet에서 Handler(Controller)로 요청을 보낼 때, Handler에서 Dispathcer servlet으로 응답을 보낼 때 동작한다.<br><br>

Interceptor 작성법
------------------

-	org.springframework.web.servlet.HandlerInterceptor 인터페이스를 구현한다.<br><br>
-	org.springframework.web.servlet.handler.HandlerInterceptorAdapter 클래스를 상속받는다.<br><br>
-	Java Config를 사용한다면, WebMvcConfigurerAdapter가 가지고 있는 `addInterceptors()` 메소드를 오버라이딩하고 등록하는 과정을 거친다.<br><br>
-	xml 설정을 사용한다면, mvc:interceptors 요소에 인터셉터를 등록한다.<br><br>

Practice
--------

> LogInterceptor.Java

```Java
public class LogInterceptor extends HandlerInterceptorAdapter{

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
            ModelAndView modelAndView) throws Exception {
        System.out.println(handler.toString() + " 가 종료되었습니다.  " + modelAndView.getViewName() + "을 view로 사용합니다.");
    }

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
            throws Exception {
        System.out.println(handler.toString() + " 를 호출했습니다.");
        return true;
    }   
}
```

<br>

> WebMvcConfigurerAdapter.java

```java
@Override
    public void addInterceptors(InterceptorRegistry registry) {
            registry.addInterceptor(new LogInterceptor());
    }
```

-	컨트롤러 메서드가 실행되기 전, 후 인터셉터에서 공통된 Logic을 적절히 수행할 수 있다.<br><br>
-	`preHandle()` 의 return 값이 false면 Controller를 수행하지 않는다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16804/)

---
