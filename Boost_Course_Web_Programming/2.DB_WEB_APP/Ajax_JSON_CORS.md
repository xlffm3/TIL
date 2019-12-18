Ajax
----

---

### Ajax(Asynchronous JavaScript and XML)<br>

-	JavaScript와 XML을 이용한 비동기적 정보 교환 기법이다.<br><br>
-	웹에 데이터를 갱신할 때, 브라우저 새로고침 없이 서버로부터 데이터를 받아 변경된 부분만 수정해서 표한한다.<br><br>
-	필요한 시점에 동적으로 콘텐츠를 받아 표현하는데 활용한다.<br><br>
-	**XML(eXtensible Markup Language)** : 다른 특수한 목적을 갖는 마크업 언어를 만드는데 사용하도록 권장하는 다목적 마크업 언어이다.<br><br>

### Ajax의 작동 방식<br>

-	1. 브라우저가 사이트에 접속하면 서버는 사이트의 기본 얼개를 담은 '템플릿'을 전달한다.<br><br>
-	2. 브라우저는 수신받은 템플릿 HTML과 CSS를 해석해 화면의 기본 모양을 그린다.<br><br>
-	3. 계속해서 서버는 데이터의 요청 방식과 수신받은 데이터를 어떻게 가공해야 하는지를 기술한 자바스크립트 파일을 전달한다.<br><br>
-	4. 브라우저는 자바스크립트 파일을 해석해서 파일에 기술된 방식대로 서버에 추가 데이터를 요청한다.<br><br>
-	5. 서버는 순수 데이터를 응답으로 되돌려준다.<br><br>
-	6. 브라우저는 수신한 데이터를 해석하여 템플릿의 적절한 위치에 삽입한다. 데이터의 가공 방식에 따라 삽입 외의 작업(변경, 삭제)을 할 수도 있다.<br><br>

### Ajax 실행 코드<br>

```javascript
function ajax(data) {
 var oReq = new XMLHttpRequest();
 oReq.addEventListener("load", function() {
   console.log(this.responseText);
 });    
 oReq.open("GET", "http://www.example.org/getData?data=data");//parameter를 붙여서 보낼수있음.
 oReq.send();
}
```

-	XMLHttpRequest 객체를 생성해서, open 메서드로 요청을 준비하고, send 메서드로 서버로 보낸다.<br><br>
-	요청 처리가 완료되어 서버에서 응답이 오면, load 이벤트가 발생하고 콜백 함수가 실행된다.<br><br>
-	콜백 함수가 실행될 때, 이미 `ajax()` 함수는 반환하고 콜 스택에서 사라진 상태이다.<br><br>
-	이는 `setTimeout()` 콜백 함수의 실행과 유사하게 동작하는 '비동기' 로직이다.<br><br>
-	GET 방식 외 POST 방식도 있다.<br><br>

### Ajax 응답 처리<br>

-	서버로부터 받아온 JSON 데이터는 문자열 형태이기때문에 브라우저에서 바로 실행이 불가능하다.<br><br>
-	문자열을 일일이 파싱하는 불편함을 감수하는 대신, JSON 객체를 통해 Js 객체로 사용이 가능하다.<br><br>
-	`var obj = JSON.parse("this.responseText");`<br><br>

### JSON(JavaScript Object Notation)<br>

```javascript
{
    "회사": [
        {
           "이름": "Apple",
           "운영체제": [
               "macOS",
               "iOS"
           ]
        },
        {
           "이름": "Microsoft",
           "운영체제": [
               "MS-DOS",
               "Windows"
           ]
        }
    ]
}
```

-	서버에서 클라이언트로 데이터를 보낼 때 사용하는 양식이다.<br><br>
-	클라이언트가 사용하는 언어에 관계 없이 통일된 데이터를 주고 받을 수 있도록, 일정한 패턴을 지닌 문자열을 생성해 내보내면 클라이언트는 그를 해석해 데이터를 자기만의 방식으로 온전히 저장, 표시할 수 있다.<br><br>
-	Ajax 통신에서 자주 쓰이는 포맷이다.<br><br>
-	객체 표기 기법이라 웹 브라우저 레벨에서 쉽게 해석이 가능하며, XML보다 용량이 적고 편하다.<br><br>

### CORS(Cross Origin Resource Sharing)<br>

-	교차 출처 리소스 공유로서, Cross-Site Http Requset를 가능하게 하는 표준 규약이다.<br><br>
-	다른 도메인으로부터 리소스가 필요할 경우, Cross-Site Http Request가 필요하다.<br><br>
-	보안 상의 이유로, 웹 브라우저들이 Ajax로 외부 host를 접속하는 것을 막기 시작했다.<br><br>
-	XMLHttpRequest는 자신과 동일한 도메인으로만 HTTP 요청을 보내도록 제한되었다.<br><br>
-	지속적으로 웹 어플리케이션을 개선하고 쉽게 개발하기 위해, XMLHttpRequest가 Cross-Domain을 요청할 수 있는 방법이 필요했으며 CORS가 개발되었다.<br><br>
-	BE 코드에서 헤더 설정을 통해 CORS를 사용하며, 이 외에도 비표준이지만 사실상 표준으로 사용하는 JSONP를 통해 CROSS DOMAIN 문제를 해결한다.<br><br>

###Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16701/)<br>
-	[Using XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest)<br>
-	[[CORS] Cross Origin Resource Sharing](https://zamezzz.tistory.com/137)<br>
-	[CORS (Cross-origin resource sharing) 간략한 설명](https://webfortj.blogspot.com/2014/05/cors-cross-origin-resource-sharing.html)<br>
-	[JSON](https://namu.wiki/w/JSON)

---
