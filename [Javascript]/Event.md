Event
-----

---

<br>

### Event<br><br>

> -	브라우저가 클릭 및 스크롤 등 이벤트를 발생시킬 때 특정 작업을 지시할 수 있다.<br><br>
> -	HTML Element를 찾아 특정 이벤트에 특정 작업을 수행하도록 등록시킬 수 있다.

<br><br>

### addEventListener<br><br>

```
var el = document.querySelector(".outside");
el.addEventListener("click", function(){
//do something..
}, false);

var el = document.getElementById("outside");
el.addEventListener("click", function(evt){
 console.log(evt.target);
 console.log(evt.target.nodeName);
}, false);
```

> -	addEventListener의 두 번째 인자는 이벤트가 발생할 때 실행되는 함수로서, Event Hanlder or Event Listener라고 부른다.<br><br>
> -	콜백 함수는 이벤트가 발생할 때 실행된다.<br><br>
> -	function(evt)를 통해 이벤트의 정보를 parameter로 받을 수 있다.<br><br>
> -	브라우저는 이벤트 리스너를 호출할 때, 사용자로부터 어떤 이벤트가 발생했는지에 대한 정보를 담은 이벤트 객체를 생성해 리스너 함수에 전달한다.<br><br>
> -	evt.target은 이벤트가 발생한 element로서 객체이다.<br><br>
> -	이벤트 리스너 안에서 이벤트 객체를 활용하여 nodeName, classname같은 속성이 사용 가능하다.

<br><br>

### Event Bubbling & Capturing<br><br>

```
ul.addEventListener("click",function(evt) {
    console.log(evt.currentTarget, evt.target);
});
```

> -	반복문으로 event를 효율적으로 등록할 수 있지만, 브라우저가 기억해야 할 EventListener의 수가 많아질 수록 비효율적이다.<br><br>
> -	클릭한 지점이 하위 엘리먼트라고 하여도, 그것을 감싸고 있는 상위 엘리먼트까지 올라가면서 EventListener가 있는지 찾는 과정을 Event Bubbling이라고 한다.<br><br>
> -	즉, 많은 li의 이벤트를 추가하기 보다 그 상위 ul의 이벤트를 추가 하면 더욱 효율적이다.<br><br>
> -	Capturing은 반대로 이벤트가 발생하는 것이며, EventListener는 기본적으로 Bubbling 순서로 이벤트를 발생시킨다.<br><br>
> -	Capturing 단계에서 이벤트를 발생시키고 싶다면, addEventListener 메서드의 3번째 인자에 true 값을 준다.<br><br>
> -	EventListener의 evt 객체는 evt.target과 evt.currentTarget 두 가지로 나뉜다.<br><br>
> -	target은 Event Bubbling의 마지막에 위치한 최하위 요소를 반환하며, 클릭된 요소를 기준으로 사용하는 경우에 사용한다.<br><br>
> -	currentTarget은 이벤트가 바인딩 된 요소에 해당하는 요소를 반환한다.

<br><br>

![image](https://user-images.githubusercontent.com/56240505/70501788-698fd400-1b62-11ea-9cea-af53982696c7.png)

<br><br>

### Event Delegation<br><br>

> -	하위 태그에서 발생해야 할 이벤트를, 상위 부모에게 위임하는 것이다.<br><br>
> -	Event Bubbling을 응용한 개념으로, 하위에 추가 이벤트 등록 없이 효율적으로 이벤트 등록이 가능하다.

<br><br>

[Reference]

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16760/)<br>
-	[Introduction to events](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#Event_handler_properties)<br>
-	[Event reference](https://developer.mozilla.org/en-US/docs/Web/Events)<br>
-	[Bubbling and Capturing](https://javascript.info/bubbling-and-capturing)

---
