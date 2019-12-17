Clean Code
----------

---

<br>

### Clean Code<br><br>

> -	가독성이 있는 좋은 코드를 의미한다.

<br><br>

### Coding Convention<br><br>

> -	함수는 목적에 맞게 이름이 지어져 있는가?<br><br>
> -	함수 안의 내용은 이름에 어울리게 하나의 로직을 담고 있는가?<br><br>
> -	함수는 동사 + 명사이며 함수의 의도를 충분히 반영하고 있는가?<br><br>
> -	함수는 카멜표기법 또는 _를 중간에 사용했는가?<br><br>
> -	변수는 명사이며 의미 있는 이름을 지었는가?

<br><br>

### 의도가 드러난 구현 패턴<br><br>

> -	예를 들어, var a = value * 19.2; 이라는 코드가 있다고 보자.<br><br>
> -	19.2가 무엇을 의미하는지 알 수가 없다.<br><br>
> -	대신 변수로 저장하고, 변수에 적절한 이름을 쓰면 더 좋다.

<br><br>

### 불필요한 전역 변수의 최소화<br><br>

```
var a = 'hello';

function print() {
   return data;
}

function exec() {
   var data = "world";
   a = a + " ";
   print(a + data)
}
```

> -	함수 내부에서만 사용이 필요한 것은 지역 변수로 만들고 매개 변수를 통해 다른 함수로 전달하는 식으로 사용하라.<br><br>
> -	전역 공간이 복잡해지면 디버깅이 어려울 수 있다.

<br><br>

### if문 중첩, else 삭제<br><br>

```
function foo(pobi,crong) {
  if(pobi) {
    if(crong) {
      return true;
    }
  }
}

function foo(pobi,crong) {
  if(!pobi) return;
  if(crong) {
    return true;
  }
}
```

> -	return을 통해 if문의 중첩과 else를 제거하여 가독성을 높인다.

<br><br>

### 정적 분석 도구

> -	eslint와 같은 도구는 코드를 읽어서 잠재적인 문제와 anit-pattern을 알려주며, 개발 도구에서 plugin을 연동해 활용한다.

<br><br>

[Reference]

-	[edwith](https://www.edwith.org/boostcourse-web/lecture/16786/)

---
