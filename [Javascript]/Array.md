Array
-----

---

<br>

### Array<br><br>

> -	선언 방식 및 인덱스를 통한 원소 참조 등은 타 언어와 유사하다.<br><br>
> -	배열을 포함한 모든 데이터 타입이 배열 안에 들어갈 수 있으며, 선언시 별도의 길이 설정이 필요 없다.<br><br>
> -	push, pop 메서드를 통해 stack처럼 뒤에 부터 순차적으로 원소를 추가 및 삭제할 수 있다.<br><br>
> -	indexOf, join, concat, slice 등의 메서드로 추가/변경/삭제가 가능하다.<br><br>
> -	어떤 메서드는 기존의 배열 값을 변경시키고, 어떤 메서드는 신규 배열을 반환하니 예제를 꼭 확인하자.

<br><br>

### Array Search<br><br>

> -	forEach, map, filter 등이 있다.<br><br>
> -	forEach는 for보다 가독성을 높이면서 실수를 줄일 수 있으며, 인자로 함수를 받을 수 있다.<br><br>
> -	map의 경우 배열을 순환하며 명령을 수행한 후, 새로운 배열을 반환한다.<br><br>
> -	filter 메서드는 필터링 함수의 결과 값이 참인 경우의 배열 요소들만으로 새로운 배열을 생성하여 반환한다.<br><br>
> -	map은 return되는 결과 값 만으로 구성된 배열을 새로 반환하며, filter는 return 조건에 부합하는 기존 배열의 원소들로 구성된 배열을 새로 반환한다.<br><br>
> -	어떤 이유로 원본 데이터가 수정될 수 있다면, map이나 filter 내부에 원본 데이터의 내용을 담은 임시 변수로 데이터를 조작하여 immutable을 실현할 수 있다.

<br><br>

### reduce<br><br>

```
var totalPrice = data.reduce(function(prevValue, product) { return prevValue + product.price; }, 0);

console.log(totalPrice);
```

> -	reduce는 배열의 모든 요소에 대해 지정된 콜백 함수를 호출하며, 콜백 함수의 반환 값을 누적하여 반환하는 함수이다.<br><br>
> -	reduce 함수의 매개변수로 콜백 함수와 누적을 시작하기 위한 초기 값이며 초기 값은 선택사항이다.

<br><br>

### Spread Operator<br><br>

```
var origin = [1,2,3,4,5];
var changedArray = [...origin, 7, 8];
changedArray.forEach(function(v){
  console.log(v);
  });
```

> -	특정 객체 또는 배열의 값을 다른 객체, 또는 배열로 복제하거나 옮길 때 사용한다.<br><br>
> -	연산자 : `...`

<br><br>

[Reference]

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16745/)

---
