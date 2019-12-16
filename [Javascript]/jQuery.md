jQuery
------

---

<br>

### jQuery<br><br>

```
//p1아이디를 가진 엘리먼트를 찾아서, color를 red 로 하고, slideup을 2초간, slideDown을 2초간 한다.
$("#p1").css("color", "red").slideUp(2000).slideDown(2000);
```

> -	jQuery는 IE6,7,8를 포함해서 다양하게 동작하는 웹브라우저의 API 간의 차이를 줄여주었다.<br><br>
> -	복잡하고 어려운 DOM APIs를 추상화해서 제공해서 쉽게 DOM 조작이 가능하다.<br><br>
> -	사고의 흐름에 맞춰 프로그래밍할 수 있다.<br><br>
> -	그러나 jQuery의 인기는 줄어드는 추세이다.<br><br>
> -	점차적으로 브라우저 호환성 이슈가 줄어들고 있으며, DOM APIs는 그대로지만, 7~8년 전부터 등장한 JS Frameworks 들이 좀 더 추상화된 방식으로 DOM 접근을 도와주고 있다.<br><br>
> -	다른 라이브러리들도 이런 방식을 지원하며, 그 외에도 ECMAScript 2015부터 편리한 함수들이 많이 제공되고 있다.

<br><br>

### Framework<br><br>

> -	Single Page Application이라는 서비스 개념이 등장하고, 웹에서 데이터 처리나 복잡한 Ajax처리, routing처리 등이 많아지면서, 이를 편리하게 해주는 Framework가 나왔다.<br><br>
> -	대표적인 예로 React, Angular, Vue, Ember와 같은 것들이 있다.<br><br>
> -	이를 사용하면 좀 더 쉽게 DOM제어를 할 수 있고, Data조작을 View에서 분리해서 관리할 수 있다.<br><br>
> -	또한 component방식으로 개발할 수 있어 재사용가 능한 코드를 만들 수 있다.<br><br>
> -	라이브러리가 유용한 함수들을 제공한다고 할 수 있다면, Framework는 큰 애플리케이션의 구조를 잡는 것을 도와주는 역할이다.<br><br>
> -	'어떤 Framework가 인기가 있는가?'에 의해서, 그 Framework이 가진 철학에 따라서 필요한 라이브러리가 의존적으로 많이 쓰이고 인기를 얻고 있다.<br><br>
> -	그렇다 보니, Framework과 관련 없이 많이 쓰일만한 라이브러리는 이제 별로 없다.<br><br>
> -	Observables을 처리해주는 RxJS나, Lodash와 같은 데이터를 쉽게 처리해주는 유틸리티 등이 인기를 얻는 정도이다.<br><br>
> -	나머지 라이브러리는 해당 Framework가 사용하는 것이 무엇인가에 따라 달라지며, React에서는 Immutable.js나 Redux 등이 인기를 얻고 있다.

<br><br>

### SPAs(Single Page Application)<br><br>

> -	페이지 갱신 요청시에 갱신에 필요한 데이터만을 전달받아 페이지를 갱신하여 주고 전체 페이지를 새로고침하지 않아서 link tag를 사용하는 기존방식보다 효율적이고 트래픽을 감소시킬수 있다.<br><br>
> -	웹보다는 앱 어플리케이션에 더 적합하다.

<br><br>

### jQuery를 사용해야 한다면?<br><br>

> -	많은 웹 서비스의 legacy code는 jQuery 및 오래된 라이브러리를 사용 중이다.<br><br>
> -	jQuery의 버전을 잘 확인하고, 버전에 적절한 메서드를 사용해야 한다.<br><br>
> -	한 페이지에 여러 가지 jQuery 버전이 존재해서는 안 된다.<br><br>
> -	가급적 대체 가능한 메서드는 표준 Js 메서드를 사용하면서 jQuery 의존도를 줄여 나간다.<br><br>
> -	직접적인 jQuery의 수정은 유지 보수 작업에서 큰 어려움을 야기할 수 있다.

<br><br>

### 결론<br><br>

> -	라이브러리나 프레임 워크는 필요한 시점에 적절히 골라 이용하면 된다.<br><br>
> -	라이브러리나 프레임 워크가 어떤 목적을 가지고 있는지 평소에 관심을 가지고 틈틈히 공부하라.

<br><br>

[Reference]

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16785/)

---
