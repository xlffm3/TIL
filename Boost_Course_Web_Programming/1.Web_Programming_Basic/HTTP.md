HTTP
====

---

### Internet<br>

-	TCP/IP 기반의 네트워크가 전세계적으로 확대되어 하나로 연결된 네트워크들의 네트워크 결합체이다.<br><br>
-	하나의 물리적인 컴퓨터에는 여러 개의 서버가 동작할 수 있으며, 각각의 서버들은 포트 값으로 구분되어져 동작한다.<br><br>

### HTTP(Hypertext Transfer Protocol)<br>

-	서버와 클라이언트가 인터넷 상에서 데이터를 주고 받기 위한 통신규약이다.<br><br>
-	HTTPS는 Secure 모드로서, HTTP의 취약점을 보완하기 위한 보안이 강화된 프로토콜이다.<br><br>

### URL(Uniform Resource Locator)<br>

-	인터넷 상의 자원의 위치를 의미한다.<br><br>
-	특정 웹 서버의 특정 파일에 접근하기 위한 경로 혹은 주소이다.<br><br>

### HTTP의 작동 방식<br>

![httpimage](https://user-images.githubusercontent.com/56240505/69651201-13775700-10b3-11ea-919f-f7c23bf730e8.png)<br><br>

-	서버 / 클라이언트 모델을 따른다.<br><br>
-	장점
	-	불특정 다수를 대상으로 하는 서비스에 적합하다.
	-	클라이언트와 서버가 계속 연결된 형태가 아니기 때문에, 클라이언트와 서버 간의 최대 연결 수보다 훨씬 많은 요청과 응답을 처리할 수 있다.<br><br>
-	단점
	-	연결을 끊어버리기 때문에, 클라이언트의 이전 상황을 알 수 없다.
	-	이러한 특징을 무상태(Stateless)라고 하며, 정보를 유지하기 위해서 Cookie와 같은 기술이 등장했다.<br><br>
-	요청 메서드 : GET, PUT, POST, PUSH, OPTIONS 등의 요청 방식이 온다.

	```
	GET : 정보를 요청하기 위해서 사용한다. (SELECT)
	POST : 정보를 밀어넣기 위해서 사용한다. (INSERT)
	PUT : 정보를 업데이트하기 위해서 사용한다. (UPDATE)
	DELETE : 정보를 삭제하기 위해서 사용한다. (DELETE)
	HEAD : (HTTP)헤더 정보만 요청한다. 해당 자원이 존재하는지 혹은 서버에 문제가 없는지를 확인하기 위해서 사용한다.
	OPTIONS : 웹서버가 지원하는 메서드의 종류를 요청한다.
	TRACE : 클라이언트의 요청을 그대로 반환한다. 예컨데 echo 서비스로 서버 상태를 확인하기 위한 목적으로 주로 사용한다.
	```

-	요청 URI : 요청하는 자원의 위치를 명시한다.<br><br>

-	HTTP 프로토콜 버전 : 웹 브라우저가 사용하는 프로토콜 버전이다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16661/)

---
