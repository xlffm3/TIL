Event Delegation
----------------

---

### Event Bubbling & Capturing<br>

![image](https://user-images.githubusercontent.com/56240505/70501788-698fd400-1b62-11ea-9cea-af53982696c7.png)<br>

```javascript
ul.addEventListener("click",function(evt) {
    console.log(evt.currentTarget, evt.target);
});
```

-	반복문으로 event를 효율적으로 등록할 수 있지만, 브라우저가 기억해야 할 EventListener의 수가 많아질 수록 비효율적이다.<br><br>
-	클릭한 지점이 하위 엘리먼트라고 하여도, 그것을 감싸고 있는 상위 엘리먼트까지 올라가면서 EventListener가 있는지 찾는 과정을 Event Bubbling이라고 한다.<br><br>
-	즉, 많은 li의 이벤트를 추가하기 보다 그 상위 ul의 이벤트를 추가 하면 더욱 효율적이다.<br><br>
-	Capturing은 반대로 이벤트가 발생하는 것이며, EventListener는 기본적으로 Bubbling 순서로 이벤트를 발생시킨다.<br><br>
-	Capturing 단계에서 이벤트를 발생시키고 싶다면, `addEventListener` 메서드의 3번째 인자에 true 값을 준다.<br><br>
-	EventListener의 evt 객체는 `evt.target`과 `evt.currentTarget` 두 가지로 나뉜다.<br><br>
-	`target`은 Event Bubbling의 마지막에 위치한 최하위 요소를 반환하며, 클릭된 요소를 기준으로 사용하는 경우에 사용한다.<br><br>
-	`currentTarget`은 이벤트가 바인딩 된 요소에 해당하는 요소를 반환한다.<br><br>

### Event Delegation<br>

```javascript
var ul = document.querySelector("ul");
ul.addEventListener("click",function(evt) {
  debugger;
    if(evt.target.tagName === "IMG") {
      log.innerHTML = "clicked" + evt.target.src;
    } else if (evt.target.tagName === "LI") {
      log.innerHTML = "clicked" + evt.target.firstChild.src;
    }
});
```

-	하위 태그에서 발생해야 할 이벤트를, 상위 부모에게 위임하는 것이다.<br><br>
-	Event Bubbling을 응용한 개념으로, 하위에 추가 이벤트 등록 없이 효율적으로 이벤트 등록이 가능하다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16760/)

---
