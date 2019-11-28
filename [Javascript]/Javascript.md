Javascript
----------

---

<br>

### Javascript<br><br>

> -	Javascript의 버전은 ECMAScript(ES)의 버전에 따라 결정되며, 이를 JS 실행 엔진이 반영한다.<br><br>
> -	ES6 문법이 현재 표준이며, ES5 문법을 포함하고 있기 때문에 호환성 문제는 없다.<br><br>
> -	feature 별로 지원하지 않는 브라우저가 있을 수 있다.

<br><br>

### Variable<br><br>

> -	var
> 	-	ES6 이전의 변수 선언 방식.
> 	-	function 단위의 scope를 지닌다.
> 	-	변수의 중복 선언을 허용한다.<br><br>
> -	const
> 	-	재정의가 불가능한 상수.
> 	-	{} 단위의 scope를 지닌다.<br><br>
> -	let
> 	-	재정의가 가능한 변수.
> 	-	{} 단위의 scope를 지닌다.
> 	-	변수의 중복 선언은 불가능하다.

<br><br>

### Basic Grammar<br><br>

> -	조건, 반복, 논리 연산자는 다른 언어와 유사하다.<br><br>
> -	비교 연산자의 경우 정확한 데이터 타입까지의 비교를 위해 ===를 사용한다.<br><br>
> -	Type : undefined, null, boolean, number, string, object, function, array, date 등.<br><br>
> -	함수 안의 파라미터나 변수는 선언 시점이 아닌 실행 시점에 그 타입이 결정된다.<br><br>
> -	typeof 키워드 혹은 toString.call 함수를 이용해 결과를 매칭하여 타입을 체크한다.<br><br>
> -	배열의 경우 isArray 함수가 있다.<br><br>
> -	for-each, for-of, for-in 탐색도 사용이 가능하지만, 브라우저의 지원 범위 차이가 있으므로 확인이 필요하다.<br><br>
> -	JS는 문자가 별도로 없으며, Single Quote도 문자열이 가능하다.<br><br>

<br><br>

### Function<br><br>

> -	함수는 여러 개의 인자를 받아서 그 결과를 출력하며, 파라미터의 개수와 인자의 개수가 일치하지 않아도 오류가 나지 않는다.<br><br>
> -	파라미터가 1개일 때 인자의 개수가 0개라면, 파라미터(매개변수)는 할당을 받지 못해 undefined이 된다.<br><br>
> -	반드시 return 값이 존재해야 하며, 없을 때는 기본 반환 값인 undefined가 반환된다.<br><br>
> -	false 조건 : null, undefined, 공백.

<br><br>

### Function Expression<br><br>

> -	익명 클래스 형식으로 변수에 함수를 할당할 수 있다.<br><br>
> -	그러나 function 내부에 익명 클래스 형식으로 함수를 표현할 경우, 순서에 유의해서 선언해야 한다.<br><br>
> -	익명 클래스 형식이 아닌 내부 클래스 형식으로 함수 내부에 함수를 정의할 경우, 선언 순서 상관 없이 정상 실행된다.

<br><br>

### Hoisting<br><br>

> -	JS는 함수가 실행될 때 함수 내부를 훑고, 필요한 변수 값들을 미리 다 모아서 선언한다.<br><br>
> -	***Hoisting*** : 함수 내부에 위치한 변수 및 함수들을 모두 끌어올려서 선언하고 기억하는 것.<br><br>
> -	내부 클래스 형식으로 함수 내부에 함수를 정의할 경우, 호이스팅 되어 내부 함수를 기억하기 때문에 선언 순서와 상관 없이 정상 실행된다.<br><br>
> -	그러나 익명 클래스 형식으로 함수 내부의 변수에 함수를 할당했을 때, 순서가 올바르지 않으면?<br><br>
> -	변수만 호이스팅되어 JS가 기억할 뿐, 함수 자체는 할당되기 전이라 undefined 값이 할당된다.

<br><br>

### Arguments 속성<br><br>

> -	JS 함수 속에는 arguments라는 특별한 지역 변수가 존재한다.<br><br>
> -	JS 함수는 선언한 파라미터보다 더 많은 인자를 보낼 수 있다.<br><br>
> -	이 때, 넘어온 인자를 arguments 배열의 형태로 반복문을 통해 하나씩 접근이 가능하다.<br><br>
> -	객체이긴 하지만 배열은 아니기 때문에, 배열 메서드는 사용이 불가능하다.<br><br>
> -	가변 인자를 활용하는 함수 생성에 유용하다.

<br><br>

### Arrow Function<br><br>

> -	Java의 람다 정규식과 유사함.

<br><br>

### Call Stack<br><br>

> -	First-In Last-Out 구조.

<br><br>

[Reference]

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16695/)

---
