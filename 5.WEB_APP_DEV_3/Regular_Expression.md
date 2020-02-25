Regular Expression
------------------

```javascript
var result = "abc3zzz".match(/\d/)[0];
console.log(result); //숫자 1개 찾기

var result = "abc32zzz".match(/\d\d/)[0];
console.log(result); //숫자 2개 찾기
```

-	정규 표현식은 문자열의 특정 패턴을 찾을 수 있는 문법으로서, 패턴 검색을 통한 추출, 삭제, 치환 등의 문자열 조작이 가능하다.<br><br>
-	구글에서 Javascript regex cheat sheet를 참고하여 직접 구현하는 연습을 하자.<br><br>

Practice
--------

```Javascript
var result1 = "19323".match(/(\d{3}-\d{3}|\d{5})/)[0];
var result2 = "193-123".match(/(\d{3}-\d{3}|\d{5})/)[0];

"92405".match(/(\d{3}-\d{3}|[012345678]\d{4})/) //null
"92405".match(/(\d{3}-\d{3}|[0-8]\d{4})/) //null
"92405".match(/(\d{3}-\d{3}|[0-46-8]\d{4})/) //null, 5와 9는 사용 불가

var result3 = "010-9021-0011".match("/01[01789]-\d{3,4}-\d{4}/")
```

-	{} 연산자로 코드의 가독성을 높이고, or 등의 연산자로 조건을 걸 수 있다.<br><br>
-	[] 연산자는 괄호 안 숫자(알파벳)만 허용한다는 의미의 연산자이며, 줄여쓸 수 있다.<br><br>

개발자 도구에서의 함수 선택
---------------------------

```javascript
\(?function\s+[a-zA-Z_$]+

(\s?)(var)(\s+[a-zA-Z_$])
$1const$3
```

-	?는 하나가 있거나 없거나를 의미하는 연산자이며, 공백은 s로 표시한다.<br><br>
-	하나 이상일 경우, +를 표기하거나 {1, }을 표기한다.<br><br>
-	\w는 모든 알파벳과 언더바를 포함하는 것을 찾지만, $ 표기를 찾을 수 없다.<br><br>
-	정규 표현식의 하위 표현식은 $로 표시하며, 예제처럼 var을 const로 replaceAll 할 수 있다.<br><br>

replace()
---------

```javascript
var result = "011-021-0011".replace(/(\d{2})\d/, "$10");

console.log(result);
```

-	$1에 해당하는 괄호 2글자는 그대로 유지하고 세 번째 글자는 0으로 치환한다.<br><br>

greedy & lazy
-------------

```Javascript
greedy : *, +, {n,}
lazy : *?, +?, {n,}?
```

-	greedy는 가능한 모든 것을 찾으려고 하며, 적절히 lazy를 이용한다.<br><br>
-	전방 탐색 및 후방 탐색에 대해 공부하라.<br><br>

---

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16796/)
