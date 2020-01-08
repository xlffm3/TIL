Object
------

---

Object
------

```javascript
const myFriend = { key : "value", key2 : "value" };
console.log(myFriend);

// 객체 속성 추가
myFriend["name"] = "crong";
console.log(myFriend["name"]);

myFriend.age = 34;
console.log(myFriend.age);

for(value in myFriend){
  console.log(myFriend[value]);
}

Object.keys(myFriend).forEach(function(key){
  console.log(myFriend[key]);
});

// 객체 속성 삭제
delete myFriend.key;
delete myFriend["key2"];
console.log(myFriend);
```

-	key, value를 지닌 자료 구조이다.<br><br>
-	표현 형식은 {}이며, 서버와 클라이언트 간에 데이터를 교환할 때 Object 포맷과 비슷한 방식으로 교환한다.<br><br>
-	순서가 필요한 것에는 배열을, 어떠한 key 값을 기반으로 동작하는 것에는 객체를 이용한다.<br><br>
-	forEach, for-in 구문으로 탐색하며, 배열의 인덱스 참조 혹은 dot notation으로 읽고 제거할 수 있다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16746/)

---
