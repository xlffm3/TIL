WAS
---

---

<br>

### WAS(Web Application Server)<br><br>

> -	클라이언트 쪽에 비즈니스 로직이 많을 경우, 클라이언트 관리(업데이트시 신규 배포 등)로 인해 비용이 많이 발생한다.<br><br>
> -	비즈니스 로직을 클라이언트와 DBMS 사이의 MiddleWare 서버에서 동작하도록 함으로써, 클라이언트는 입력 및 출력만 담당하게 한다.<br><br>
> -	**WAS** : 일종의 MiddleWare로서 웹 클라이언트(보통 웹 브라우저)의 요청 중 웹 애플리케이션이 동작하도록 지원하는 목적이다.

<br><br>

![was1](https://user-images.githubusercontent.com/56240505/69651205-140fed80-10b3-11ea-8b1e-c36c67c902de.png)

<br><br>

### WAS vs Web Server<br><br>

> -	Web Server는 정적 컨텐츠, WAS는 동적 컨텐츠를 주로 관장한다.<br><br>
> -	Web의 발전으로 동적 컨텐츠가 증가함에 따라, 자원의 효율적인 활용을 위해 WAS의 필요성이 증가했다.<br><br>
> -	WAS도 보통 자체적으로 Web Server 기능을 내장하고 있으며, 정적 컨텐츠를 처리하는데 무리가 없다.<br><br>
> -	그러나 자원 이용의 효율성과 장애 극복 및 배포와 유지 보수의 편의성을 위해, 서비스의 규모가 커질수록 Web Server와 WAS를 대체로 분리한다.

<br><br>

[Reference]

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16666/)

---
