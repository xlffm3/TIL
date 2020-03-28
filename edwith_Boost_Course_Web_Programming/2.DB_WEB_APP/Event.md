Event
-----

-	브라우저가 클릭 및 스크롤 등 이벤트를 발생시킬 때 특정 작업을 지시할 수 있다.<br><br>
-	HTML Element를 찾아 특정 이벤트에 특정 작업을 수행하도록 등록시킬 수 있다.<br><br>

addEventListener()
------------------

```javascript
var el = document.getElementById("outside");
el.addEventListener("click", function(evt){
 console.log(evt.target);
 console.log(evt.target.nodeName);
}, false);
```

-	`addEventListener()`의 두 번째 인자는 이벤트가 발생할 때 실행되는 함수로서, Event Hanlder or Event Listener라고 부른다.<br><br>
-	콜백 함수는 이벤트가 발생할 때 실행된다.<br><br>
-	`function(evt)`를 통해 이벤트의 정보를 parameter로 받을 수 있다.<br><br>
-	브라우저는 이벤트 리스너를 호출할 때, 사용자로부터 어떤 이벤트가 발생했는지에 대한 정보를 담은 이벤트 객체를 생성해 리스너 함수에 전달한다.<br><br>
-	`evt.target`은 이벤트가 발생한 element로서 객체이다.<br><br>
-	이벤트 리스너 안에서 이벤트 객체를 활용하여 nodeName, classname같은 속성이 사용 가능하다.<br><br>

---

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16760/)
