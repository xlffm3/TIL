Object Literal & This
---------------------

---

Object Literal & this
---------------------

```javascript
var healthObj = {
  name : "달리기",
  lastTime : "PM10:12",
  showHealth : function() {
    console.log(this.name + "님, 오늘은 " + this.lastTime + "에 운동을 하셨네요");
  }
}

const obj = {
   getName() {
     return this.name;
     },
  setName(name) {
      this.name = name;
    }
}

obj.setName("crong");
const result = obj.getName();

function get() {
    return this;
}

get(); //window. 함수가 실행될때의 컨텍스트는 window를 참조한다.
new get(); //object. new키워드를 쓰면 새로운 object context가 생성된다.
```

-	Object Literal을 만든 뒤, this라는 키워드를 사용할 수 있다.<br><br>
-	객체 안에서의 this는 그 객체 자신을 가르킨다.<br><br>
-	ES6에서는 객체에서 메서드를 사용할 때 function을 생략할 수 있다.<br><br>
-	Js에서는 전역 스크립트나 함수가 실행될 때 실행 문맥(Execution Context)이 생성된다.<br><br>
-	모든 context에는 참조하고 있는 객체(thisBinding)가 존재하며, 현재 context가 참조하고 있는 객체를 알기 위해 this를 사용한다.<br><br>

this의 비동기
-------------

```javascript
var healthObj = {
  name : "달리기",
  lastTime : "PM10:12",
  showHealth : function() {
    setTimeout(function() {
        console.log(this.name + "님, 오늘은 " + this.lastTime + "에 운동을 하셨네요");      
    }, 1000)
  }
}
healthObj.showHealth();
```

-	예제와 같은 비동기 call-back function의 경우, this가 참조하는 객체가 healthObj가 아닌 window가 되버린다.<br><br>
-	this의 경우 실행 타이밍이 결정되는 것으로, `showHealth()` 메서드는 이미 스택에서 반환된 뒤 이벤트 큐에 저장되어 있던 `setTimeout()`이 실행되기 때문에 this가 window를 객체로 본다.<br><br>
-	ES6의 Arrow 함수의 경우 결과가 다르다.<br><br>

bind
----

```javascript
var healthObj = {
  name : "달리기",
  lastTime : "PM10:12",
  showHealth : function() {
    setTimeout(function() {
        console.log(this.name + "님, 오늘은 " + this.lastTime + "에 운동을 하셨네요");      
    }.bind(this), 1000)
  }
}
healthObj.showHealth();
```

-	bind 함수의 첫번째 인자로 this를 주어, 그 시점의 this를 기억하고 있는 새로운(바인드된) 함수가 반환된다.<br><br>
-	함수 뒤에 .을 붙일 수 있는 경우, 함수도 .으로 불리는 순간 객체가 된다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16779/)

---
