API
---

---

### API(Application Programming Interface)<br>

-	응용 프로그램 프로그래밍 인터페이스는 응용 프로그램에서 사용할 수 있도록, 운영 체제나 프로그래밍 언어가 제공하는 기능을 제어할 수 있게 만든 인터페이스이다.<br><br>
-	주로 파일 제어, 창 제어, 화상 처리, 문자 제어 등을 위한 인터페이스를 제공한다.<br><br>

### Rest API<br>

-	REST API란 핵심 컨텐츠 및 기능을 외부 사이트에서 활용할 수 있도록 제공되는 인터페이스이다.<br><br>
-	예를 들어, 네이버에서 블로그에 글을 저장하거나, 글 목록을 읽어갈 수 있도록 외부에 기능을 제공하거나 우체국에서 우편번호를 조회할 수 있는 기능을 제공하거나, 구글에서 구글 지도를 사용할 수 있도록 제공하는 것들을 말한다.<br><br>
-	웹 브라우저 뿐만 아니라 앱 등 다양한 클라이언트가 등장하면서 그러한 클라이언트들에게 대응하기 위해 ResT API가 널리 사용되기 시작했다.<br><br>
-	서비스 업체들이 다양한 Rest API를 제공함으로써, 클라이언트는 이러한 Rest API들을 조합한 어플리케이션(Mashup)을 만들 수 있다.<br><br>

### Rest API의 Style 조건<br>

-	client-server.<br><br>
-	stateless.<br><br>
-	cache.<br><br>
-	uniform interface.<br><br>
-	layered system.<br><br>
-	code-on-demand (optional)<br><br>

### Uniform Interface의 문제<br>

-	1. 리소스가 URI로 식별되야 한다.<br><br>
-	2. 리소스를 생성,수정,추가하고자 할 때 HTTP메시지에 표현을 해서 전송해야 한다.<br><br>
-	3. 메시지는 스스로 설명할 수 있어야 한다. (Self-descriptive message)<br><br>
-	4. 애플리케이션의 상태는 Hyperlink를 이용해 전이되야 한다. (HATEOAS)<br><br>
-	3번, 4번의 경우 API에서 제공하기 어렵기 때문에, Rest의 Style 조건을 전부 지원하지 않는 API(HTTP API or WEB API)를 제작한다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16740/)<br>
-	[API](https://ko.wikipedia.org/wiki/API)

---
