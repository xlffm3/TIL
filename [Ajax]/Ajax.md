Ajax
----

---

<br>

### Ajax(Asynchronous JavaScript and XML)<br><br>

> -	JavaScript와 XML을 이용한 비동기적 정보 교환 기법.<br><br>
> -	웹에 데이터를 갱신할 때, 브라우저 새로고침 없이 서버로부터 데이터를 받아 변경된 부분만 수정해서 표한한다.<br><br>
> -	필요한 시점에 동적으로 콘텐츠를 받아 표현하는데 활용한다.<br><br>
> -	**XML(eXtensible Markup Language)** : 다른 특수한 목적을 갖는 마크업 언어를 만드는데 사용하도록 권장하는 다목적 마크업 언어.

<br><br>

### Ajax의 작동 방식<br><br>

> -	1. 브라우저가 사이트에 접속하면 서버는 사이트의 기본 얼개를 담은 '템플릿'을 전달한다.<br><br>
> -	2. 브라우저는 수신받은 템플릿 HTML과 CSS를 해석해 화면의 기본 모양을 그린다.<br><br>
> -	3. 계속해서 서버는 데이터의 요청 방식과 수신받은 데이터를 어떻게 가공해야 하는지를 기술한 자바스크립트 파일을 전달한다.<br><br>
> -	4. 브라우저는 자바스크립트 파일을 해석해서 파일에 기술된 방식대로 서버에 추가 데이터를 요청한다.<br><br>
> -	5. 서버는 순수 데이터를 응답으로 되돌려준다.<br><br>
> -	6. 브라우저는 수신한 데이터를 해석하여 템플릿의 적절한 위치에 삽입한다. 데이터의 가공 방식에 따라 삽입 외의 작업(변경, 삭제)을 할 수도 있다.

<br><br>

### Ajax 실행 코드<br><br>

```
function ajax(data) {
 var oReq = new XMLHttpRequest();
 oReq.addEventListener("load", function() {
   console.log(this.responseText);
 });    
 oReq.open("GET", "http://www.example.org/getData?data=data");//parameter를 붙여서 보낼수있음.
 oReq.send();
}
```

> -	XMLHttpRequest 객체를 생성해서, open 메서드로 요청을 준비하고, send 메서드로 서버로 보낸다.<br><br>
> -	요청 처리가 완료되어 서버에서 응답이 오면, load 이벤트가 발생하고 콜백 함수가 실행된다.<br><br>
> -	콜백 함수가 실행될 때, 이미 ajax 함수는 반환하고 콜 스택에서 사라진 상태이다.<br><br>
> -	이는 setTimeout 함수의 콜백 함수의 실행과 유사하게 동작하는 '비동기' 로직이다.<br><br>
> -	GET 방식 외 POST 방식도 있다.

<br><br>

[Reference]

-	[Using XMLHttpRequest](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/Using_XMLHttpRequest)

---
