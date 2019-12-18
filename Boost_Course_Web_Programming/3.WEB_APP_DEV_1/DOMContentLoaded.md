DOMContentLoaded
----------------

---

### DOMContentLoaded<br>

```html
<div>a</div><script>document.querySelector("div")</script> //정상

<script>document.querySelector("div")</script><div>a</div> //null
```

-	개발자 도구의 Network Panel을 통해 확인한 결과, DOMContentLoaded와 Load의 경과 시간에 차이가 존재한다.<br><br>
-	DOM Tree 분석이 끝나면 DOMContentLoaded가 발생하며, 그 외 모든 자원이 다 받아져서 브라우저에 렌더링까지 다 끝난 시점에 Load가 발생한다.<br><br>
-	DOM Tree가 완성되면 DOM APIs를 통해서 DOM에 접근할 수 있기 때문에, 실무에서 대부분의 Js 코드는 DOMContentLoaded 이후에 동작한다.<br><br>
-	HTML 중간중간에 script를 추가해도 되지만, 순서에 유념해야 한다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/20141/)

---
