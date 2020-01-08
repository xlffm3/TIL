Form Data
---------

---

Form Tag
--------

```html
<form action="/join" method="post">
    <div class="inputWrap">
        <div class="email">
            <span> Email </span> <input type="text" name="email"><br/>
        </div>
        <div class="password">
            <span> Password </span> <input type="password" name="password"><br/>
        </div>
    </div>
    <input class="sendbtn" type="submit">
</form>
```

-	input(or button) type이 submit이고 클릭할 경우, 입력한 정보가 기술된 method 방식에 의해 action url로 전송된다.<br><br>
-	서버에서는 요청을 적절하게 routing 처리한다.<br><br>
-	`/join` 으로 request url이 탐지되면, 이제 클라이언트에서 보낸 데이터를 획득하고(request 객체에 담겨서 온 값) 그 값이 올바른지 확인하거나 아니면 DB에 그 값을 추가하는 등의 작업을 한다.<br><br>
-	이후에는 다시 클라이언트에 어떤 결과 페이지(html)를 만들어서 응답한다.<br><br>
-	HTTPS가 아닌 경우, 전송되는 데이터가 노출될 수 있다.<br><br>
-	form 데이터를 ajax로 처리할 수 있다.<br><br>

유효성 검증
-----------

```javascript
<script>
var btn = document.querySelector(".sendbtn");
var result = document.querySelector(".result");
btn.addEventListener("click", function(evt) {
  evt.preventDefault();
  var emailValue = document.querySelector("[name='email']").value;
  var bValid = (/^[\w+_]\w+@\w+\.\w+/).test(emailValue);
  if(!bValid)  {
      result.innerHTML = "올바르지 않은 이메일입니다";
  } else {
      result.innerHTML = "이메일정보가 좋아요~";
      document.querySelector("#myform").submit();
  }
});
</script>
```

-	유효성 검증을 서버에서 처리한다면, 입력 값이 틀렸다는 메시지가 뒤 늦게 나오며 입력한 정보가 전부 삭제되는 등 사용자 친화적이지 않은 UX가 되버린다.<br><br>
-	더 나은 UX를 제공하기 위해 에러 메시지를 빨리 노출해주어야 하며, 정규 표현식을 통해 해결할 수 있다.<br><br>
-	`preventDefault()` 메서드는 브라우저의 default 행동을 막는다.<br><br>
-	`nameElememnt.addEventListener("change", function(evt) {...});` <br><br>
-	form의 input data 값이 변경되면 발생하는 이벤트 타입인 change를 통해 더 빨리 form의 유효성 테스트가 가능하다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16796/)

---
