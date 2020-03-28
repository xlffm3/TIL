Web의 상태 유지 기술
--------------------

-	HTTP 프로토콜은 상태 유지가 안되는 프로토콜이다.<br><br>
-	이전에 무엇을 했고, 지금 무엇을 했는지에 대한 정보를 갖고 있지 않다.<br><br>
-	웹 브라우저(클라이언트)의 요청에 대한 응답을 하고 나면 해당 클라이언트와의 연결을 지속하지 않는다.<br><br>
-	상태 유지를 위해 Cookie와 Session기술이 등장했다.<br><br>

Cookie & Session
----------------

-	Cookie
	-	사용자 컴퓨터에 저장된다.
	-	저장된 정보를 다른 사람 또는 시스템이 볼 수 있는 단점이 있다.
	-	유효시간이 지나면 사라진다.<br><br>
-	Session
	-	서버에 저장된다.
	-	서버가 종료되거나 유효시간이 지나면 사라진다.<br><br>

Cookie의 작동 방식
------------------

![image](https://user-images.githubusercontent.com/56240505/71463566-26885e80-27fa-11ea-8d94-651094872726.png) ![image](https://user-images.githubusercontent.com/56240505/71463568-2a1be580-27fa-11ea-9e71-4a6213625051.png)<br><br>

Session의 작동 방식
-------------------

![image](https://user-images.githubusercontent.com/56240505/71463572-2daf6c80-27fa-11ea-9bac-164a50ef231c.png) ![image](https://user-images.githubusercontent.com/56240505/71463578-30aa5d00-27fa-11ea-9d8c-c4a53d3c6f20.png)<br><br>

---

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16798/)
