Window Object
-------------

---

Window Object
-------------

-	window 전역 객체에는 브라우저 개발에 필요한 메서드들이 존재한다.<br><br>
-	default 개념으기 때문에 `window.setTimeout() === setTimeout();` 처럼 표현 생략이 가능하다.<br><br>

setTimeout()
------------

```javascript
function run() {
    setTimeout(function() {
        var msg = "hello codesquad";
        console.log(msg);  //이 메시지는 즉시 실행되지 않습니다.
    }, 1000);
}

run();
```

-	일정한 시간 후에 작업을 한 번 실행한다 .<br><br>
-	보통 재귀적 호출을 사용하여 작업을 반복한다.<br><br>
-	`clearTimeout()`을 사용해 작업을 중지한다.<br><br>

setInterval()
-------------

-	일정한 시간 간격으로 작업을 수행한다.<br><br>
-	일정한 시간 간격으로 실행되는 작업이 시간 간격보다 오래 걸릴 경우 문제가 발생할 수 있다.<br><br>
-	`clearInterval()`을 사용해 작업을 중지한다.<br><br>

유의점
------

-	clear 메서드들은 실행 중인 작업을 중지시키는 것이 아니다.<br><br>
-	지정된 작업은 모두 실행된 다음, 작업 스케쥴이 중지되는 것이다.<br><br>
-	비동기(asynchronous)로 실행되기 때문에, 동기적인 다른 실행과의 관계를 고려해야 한다.<br><br>

Call Back Function
------------------

-	인자로 함수를 받으며, 보통 나중에 실행되는 함수를 콜 백 함수라고 한다.<br><br>
-	Js는 인자 및 반환 값을 함수로 설정이 가능하다.<br><br>
-	`setTimeout()`이 실행되면 `setTimeout()` 안의 익명 함수(1번째 인자)는 브라우저(Web API)가 hold하고 있다가, 주어진 시간(2번째 인자) 만큼의 시간이 지나면 event queue로 이동해 대기한다.<br><br>
-	call stack이 비워지면 `setTimeout()` 안의 익명 함수가 call stack으로 이동하여 실행된다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16698/)

---
