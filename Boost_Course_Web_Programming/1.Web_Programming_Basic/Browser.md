Browser
-------

---

### Browser<br>

-	월드와이드웹(WWW)에서 정보를 탐색하고 표현하기 위한 소프트웨어이다.<br><br>
-	특정 정보로 이동할 수 있는 주소 입력창 및 서버와 HTTP로 정보를 주고 받을 수 있는 네트워크 모듈을 포함한다.<br><br>
-	서버에서 받은 문서(HTML, CSS, Javascript)를 해석하고 실행하여 화면에 표현하기 위한 해석기(Parser)들을 가지고 있으며, 브라우저마다 렌더링 엔진이 상이하다.<br><br>
-	브라우저가 사용자 소스를 렌더링할 때, HTML의 TAG를 DOM Tree 형태로 변환하여 Parsing한 후, CSS 및 Javascript도 순차적으로 Parsing한다.<br><br>
-	HTML Parsing 내용이 확인되기 전에 JS 등의 내용이 있다면, 흐름을 방해할 수 있다.<br><br>
-	따라서 CSS는 head 영역 안, JS는 body tag의 하단에 위치하는게 일반적이다.<br><br>

![browser1](https://user-images.githubusercontent.com/56240505/69651200-12dec080-10b3-11ea-9c16-03309f84d114.png)<br><br>

### Browser의 작동 방식<br>

-	HTML을 해석해서 DOM Tree를, CSS를 해석해서 CSS Tree(CSS Object Model)을 생성한다.<br><br>
-	이 과정에서 토큰 단위로 해석되는 Parsing 절차가 필요하다.<br><br>
-	DOM Tree와 CSS Tree는 다시 Render Tree로 조합되며, 조합된 결과는 화면에 어떻게 배치할지 등 크기 및 위치 정보를 포함한다.<br><br>
-	Painting 과정을 통해 화면에 정보가 표현이 된다.<br><br>
-	브라우저는 변경에 대해 가능한 최소한의 동작으로 반응하기 때문에, 화면의 변화에 대해 항상 새로운 Rendering 작업을 수행하는 것은 아니다.<br><br>

### Reference<br>

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16663/)

---
