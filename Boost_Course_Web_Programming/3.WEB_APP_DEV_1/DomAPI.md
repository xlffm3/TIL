DOM API
-------

---

### DOM API<br>

-	라이브러리보다 더 빠르지만 그 차이는 크지 않으며, w3school에 `document.` 및 `element.`를 통해 사용할 수 있는 API가 정리되어 있다.<br><br>

### Search<br>

-	tagName : 엘리먼트의 태그명 반환한다.<br><br>
-	textContent : 노드의 텍스트 내용을 설정하거나 반환한다.<br><br>
-	nodeType : 노드의 노드 유형을 숫자로 반환 한다.<br><br>
-	childNodes : 엘리먼트의 자식 노드의 노드 리스트(텍스트 노드, 주석 노드 포함)를 반환한다.<br><br>
-	firstChild :엘리먼트의 첫 번째 자식 노드를 반환한다.<br><br>
-	firstElementChild : 첫 번째 자식 엘리먼트를 반환한다.<br><br>
-	parentElement : 엘리먼트의 부모 엘리먼트를 반환한다.<br><br>
-	nextSibling : 동일한 노드 트리 레벨에서 다음 노드를 반환한다.<br><br>
-	nextElementSibling : 동일한 노드 트리 레벨에서 다음 엘리먼트를 반환한다.<br><br>

### Control<br>

-	`removeChild`, `appendChild`, `insertBefore`, `replaceChilde`, `cloneNode` 등이 있다.<br><br>
-	`closet`은 상위로 올라가면서 가장 가까운 엘리먼트를 반환하며, `insertBefore`는 기존 node를 move시킬 수 있다.<br><br>

### HTML을 문자열로 처리해주는 DOM<br>

-	innerText : 지정된 노드와 모든 자손의 텍스트 내용을 설정하거나 반환한다.<br><br>
-	innerHTML : 지정된 element 내부의 html을 설정하거나 반환한다.<br><br>
-	insertAdjacentHTML : HTML로 텍스트를 지정된 위치에 삽입한다.<br><br>

### Polyfill<br>

-	낡은 브라우저에서 modern code를 사용하게 해주는 기법이다.<br><br>
-	DOM querySelector를 통해 찾은 노드 리스트는 배열이 아니기 때문에, `Array.from` 메서드를 통해 배열로 변경시켜서 forEach를 활용한다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/37422/)

---
