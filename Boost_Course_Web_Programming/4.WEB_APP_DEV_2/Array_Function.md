Array Function
--------------

---

Map & Filter
------------

```javascript
var filteredData = data.filter(function(v) {
  return v.price > 5000;
}).map(function(v) {
  v.price = (''+v.price).replace(/^(\d+)(\d{3})$/, "$1,$2원");
  return v;
}); //원본 데이터가 변경됨

var filteredData = data.filter(function(v) {
    return v.price > 5000;
}).map(function(v) {
  var obj = {};
  obj.title = v.title;
  obj.content = v.content;
  obj.price = (''+v.price).replace(/^(\d+)(\d{3})$/, "$1,$2원");
  return obj;
}); //immutable
```

-	map은 return되는 결과 값만으로 구성된 배열을 새로 반환하며, filter는 return 조건에 부합하는 기존 배열의 원소들로 구성된 배열을 새로 반환한다.<br><br>
-	어떤 이유로 원본 데이터가 수정될 수 있다면, map이나 filter 내부에 원본 데이터의 내용을 담은 임시 변수로 데이터를 조작하여 immutable을 실현할 수 있다.<br><br>

reduce()
--------

```javascript
var totalPrice = data.reduce(function(prevValue, product) {
   return prevValue + product.price;
 }, 0);
```

-	reduce는 배열의 모든 요소에 대해 지정된 콜백 함수를 호출하며, 콜백 함수의 반환 값을 누적하여 반환하는 함수이다.<br><br>
-	reduce 함수의 매개변수로 콜백 함수와 누적을 시작하기 위한 초기 값이며 초기 값은 선택사항이다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16778/)

---
