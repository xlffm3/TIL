Animation
---------

-	애니메이션은 반복적인 움직임의 처리로서, 웹 UI 애니메이션은 CSS3 및 Js로 제어할 수 있다.<br><br>
-	매끄러운 애니메이션의 경우 60FPS로, 16.666ms 간격으로 frame을 생성해야 한다.<br><br>
-	간단하고 규칙적인 애니메이션은 CSS로, 세밀한 조작이 필요한 것은 Js로 제어한다.<br><br>
-	일반적으로 CSS가 Js보다 더 빠른 성능을 보장하며, 모바일 웹의 경우 CSS가 훨씬 빠르다.<br><br>

setInterval()
-------------

![image](https://user-images.githubusercontent.com/56240505/70497519-c1c0d900-1b56-11ea-98c9-19d633df3c07.png)<br>

```javascript
const interval = window.setInterval(()=> {
  console.log('현재시각은', new Date());
},1000/60);

window.clearInterval(interval);
```

-	비동기 작업이기 때문에, 스택의 작업들로 인해 이벤트 콜백이 지연되어 애니메이션이 매끄럽게 처리되지 않을 수 있다.<br><br>
-	애니메이션 구현에 잘 사용하지 않는다.<br><br>

setTimeout()
------------

```javascript
//애니메이션을 구현하려면 재귀호출을 해서 구현할 수 있습니다.

let count = 0;
function animate() {   
  setTimeout(() => {
    if(count >= 20) return;
    console.log('현재시각은', new Date());
    count++;
    animate();
  },500);
}
animate();
```

-	`setTimeout()` 또한 콜백 함수가 제때 실행되지 않을 수 있다.<br><br>
-	그러나 매 순간 timeout을 조절할 수 있는 코드 구현으로 recursive한 컨트롤이 가능하다는 점이 차이가 있다.<br><br>
-	예제의 setTimeout 콜백 함수는 `animate()` 함수를 재귀 호출하면서 콜 스택에서 비워지기 때문에, stackoverflow 현상이 발생하지 않는다.<br><br>

requestAnimationFrame()
-----------------------

```javascript
var count = 0;
var el=document.querySelector(".outside");
el.style.left = "0px";

function run() {
   if(count > 70) return;
   count = count + 1;
   el.style.left =  parseInt(el.style.left) + count + "px";
   requestAnimationFrame(run);
}

requestAnimationFrame(run);
```

-	`parseInt()`로 element의 left 값을 변화시킨 뒤 특정 조건이 만족될 때 까지 재귀 호출한다.<br><br>
-	`requestAnimationFrame()` 함수를 통해 원하는 함수를 인자로 넣어주면, 브라우저는 인자로 받은 비동기 함수가 실행될 가장 적절한 타이밍에 실행시켜준다.<br><br>
-	canvas, svg를 사용하면서 그래픽 작업을 하게 될 때 유용하다.<br><br>
-	`requestAnimationFrame()`을 여러번 호출할 경우, 등록한 순서대로 번갈아가며 반복하게 된다.<br><br>

CSS3 transition
---------------

```javascript
//CSS
.example{
    position:relative;
    transform: scale(1);
    transition: all 2s; //인자로 all로 주느냐, scale 등 특정 요소만 주느냐에 따라 결과 다름.
}

//Js
var base = document.querySelector(".example");
base.style.transform="scale(2)";
base.style.left="300px";
```

-	transition은 애니메이션을 브라우저에게 위임하는 것이다.<br><br>
-	모바일 웹에서는 transform을 활용한 element 조작을 많이 구현한다.<br><br>
-	**GPU 가속** : 애니메이션을 메인 스레드에서 처리하는 것이 아니라, 그래픽 카드에게 위임하여 처리하는 것이다.<br><br>
-	멀티 프로세스랑 병렬로 애니메이션을 처리할 수 있기 때문에, 더욱 빠른 처리가 가능하다.<br><br>
-	translateXX, rotate, opacity, scale 등 관련 속성은 reference를 참조하여 구현하자.<br><br>

---

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16754/)
