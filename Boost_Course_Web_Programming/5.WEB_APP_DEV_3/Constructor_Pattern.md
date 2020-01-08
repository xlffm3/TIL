Constructor Pattern
-------------------

---

Object Literal
--------------

```javascript
var healthObj = {
  name : "달리기",
  lastTime : "PM10:12",
  showHealth : function() {
    console.log(this.name + "님, 오늘은 " + this.lastTime + "에 운동을 하셨네요");
  }
}

healthObj.showHealth();
```

-	healthObj 형태를 가진 여러 개의 객체가 필요하다면, 위와 같은 방식은 코드 중복으로 유지 보수 및 메모리 관리 측면에서 효율적이지 않다.<br><br>

Constructor
-----------

```javascript
function Health(name, lastTime) {
  this.name = name;
  this.lastTime = lastTime;
    this.showHealth = function(){...}
}
const h = new Health("달리기", "10:12");
const h2 = new Health("걷기", "20:11");
```

-	`typeof()` 를 사용하면, h와 h2가 object임을 확인할 수 있다.<br><br>
-	`new` 키워드는 객체를 생성하는 함수로서, 생성자라고 한다.<br><br>
-	그러나 `showHealth()` 메서드가 중복되어 메모리 비효율을 유발하고 있다.<br><br>

Prototype을 활용한 메서드 생성
------------------------------

![image](https://user-images.githubusercontent.com/56240505/71458880-5169b700-27e8-11ea-8b72-26112b9eb772.png)<br>

```javascript
function Health(name, lastTime) {
  this.name = name;
  this.lastTime = lastTime;

}

Health.prototype.showHealth = function() {
    console.log(this.name + "," + this.lastTime);
}

const h = new Health("달리기", "10:12");
console.log(h);  
/**
[object Object] {
  lastTime: "10:12",
  name: "달리기",
  showHealth: function() {
    window.runnerWindow.proxyConsole.log(this.name + "," + this.lastTime);
}
}
**/
h.showHealth();

const h = new Health("달리기", "10:12");
const h2 = new Health("걷기", "14:20");
console.log(h.showHealth === h2.showHealth); //true
```

-	상속과 비슷한 개념으로, 각 인스턴스들이 prototype이라는 같은 참조 객체를 바라보게 한다.<br><br>

-	prototype의 어떤 속성이 변하면 모든 인스턴스들에게 공유된다.<br><br>

-	`_proto_` 속성으로 내부 prototype 정보를 확인할 수 있다.<br><br>

-	ES6의 Class도 prototype을 활용하여 클래스 구조를 생성하며, 많은 framework들이 이런 패턴으로 컴포넌트를 만든다.<br><br>

-	`Object.create` 를 사용하여 Class와 같은 코드를 만들 수 있다.<br><br>

Tab UI 실습
-----------

```javascript
function Tab(tabElement) {
    this.tabmenu = tabElement;
    this.registerEvents();
}

Tab.prototype = {
    registerEvents : function() {
        this.tabmenu.addEventListener("click", function (evt) {
            this.sendAjax("./json.txt", evt.target.innerText);
        }.bind(this));
    },
    sendAjax : function(url, clickedName) {
        var oReq = new XMLHttpRequest();
        oReq.addEventListener("load", function () {
            var data = JSON.parse(oReq.responseText);
            this.makeTemplate(data, clickedName);
        }.bind(this));
        oReq.open("GET", url);
        oReq.send();
    },
    makeTemplate : function(data, clickedName) {
        var html = document.getElementById("tabcontent").innerHTML;
        var resultHTML = "";
        for (var i = 0; i < data.length; i++) {
            if (data[i].name === clickedName) {
                resultHTML = html.replace("{name}", data[i].name)
                    .replace("{favorites}", data[i].favorites.join(" "));
                break;
            }
        }
        document.querySelector(".content").innerHTML = resultHTML
    }
}

var tabmenu = document.querySelector(".tabmenu");
var o = new Tab(tabmenu);
```

-	prototype 기반 코드는 하나의 클래스(모듈)로 만드는 것이며, ES6에서 Class와 extend 개념으로도 만들 수 있다.<br><br>
-	Object Literal 안에서는 Arrow Function을 가급적 지양한다.<br><br>

Reference
---------

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16794/)

---
