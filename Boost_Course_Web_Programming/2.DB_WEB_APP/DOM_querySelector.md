DOM & querySelector
-------------------

---

### DOM & querySelector<br>

![image](https://user-images.githubusercontent.com/56240505/71048569-fb12cd80-2182-11ea-91b5-a9bea298b522.png)<br><br>

-	브라우저에서는 HTML Element를 DOM(Document Object Model)이라는 객체 형태의 모델로 저장한다.<br><br>
-	그렇게 저장된 정보를 DOM Tree라고 하며, HTML Element는 Tree 형태로 저장된다.<br><br>
-	브라우저는 다양한 DOM API를 제공하며, API에는 DOM Tree를 찾고 조작하는 여러 메서드가 포함되어 있다.

	```javascript
	getElementById() : ID 정보를 확인할 수 있다.
	Element.querySelector() : 노드의 하위 트리에서 첫 번째 일치하는 Element 노드를 반환한다. 결과가 없으면 null을 반환한다.
	Element.querySelectorAll : 노드의 하위 트리 안에서 일치하는 모든 Element를 포함한 NodeList를 반환한다. 결과가 없으면 빈 NodeList를 반환한다.
	removeChild(child) : 삭제
	replaceChild(newChild, oldChild) : 대체
	appendChild(child) : 노드의 마지막 자식으로 주어진 엘리먼트를 추가한다.
	document.createElement(tagname) : tagname 엘리먼트 노드를 추가한다.
	document.createTextNode(data) :텍스트 노드를 추가한다.
	```

	<br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16699/)

---
