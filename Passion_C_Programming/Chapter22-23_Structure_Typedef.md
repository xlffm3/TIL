Chapter 22 - 23 : Structure & Typedef
-------------------------------------

---

Structure
---------

```c
struct person{
  char name[20];
  char phoneNum[20];
  int age;
}

int main(void){
  struct person man;
  strcpy(man.name, "안성준");
  strcpy(man.phoneNum, "010-0000-0000");
  man.age = 20;

  struct person man = {"이승기", "010-1111-1111", 21};
  return 0;
}
```

-	구조체는 포인터 변수 및 배열 등을 포함한 하나 이상의 변수를 묶어 새로운 자료형을 정의하는 도구이다.<br><br>
-	문자열 저장을 위해 `strcpy()` 함수를 이용했다.<br><br>
-	구조체의 멤버가 배열 혹은 포인터 변수면, 그에 맞는 접근 방식을 취한다.<br><br>
-	구조체를 선언과 동시에 초기화하면 번거로운 함수를 사용하지 않아도 된다.<br><br>

구조체 변수와 포인터
--------------------

```c
struct point{
  int x, y;
}

int main(void){
  struct point pos = {11, 22};
  struct point * pptr = &pos;
  (*pptr).x = 15;
  pptr -> y = 10;

  return 0;
}
```

-	포인터의 사용은 일반적인 방법과 유사하며, -> 연산자를 사용할 수 있다.<br><br>
-	TYPE 형 구조체 변수의 멤버로 TYPE 형 포인터 변수를 둘 수 있다.<br><br>
-	구조체 변수의 주소 값은 구조체 변수의 첫 번째 멤버의 주소 값과 동일하다.<br><br>

typedef 선언
------------

```c
typedef int INT; //int의 또 다른 이름 INT 부여
typedef name1 name2 name3;

typedef struct point{
  int x, y;
} Point;

typedef struct {
  int x, y;
} Point'
```

-	기존에 존재하는 자료형의 이름에 새 이름을 부여하는 목적이다.<br><br>
-	새로운 이름의 부여는 가장 마지막에 등장하는 단어를 중심으로 이뤄지며, name3가 name1 및 name2에 부여된 새로운 이름이 된다.<br><br>
-	예제처럼 구조체의 정의와 typedef의 선언을 동시에 수행할 수 있다.<br><br>
-	예제처럼 구조체의 이름을 생략할 수 있지만, struct 선언이 불가능해진다.<br><br>

Union Type<br>
--------------

-	구조체의 크기는 모든 멤버 크기의 합이지만, 공용체의 크기는 가장 크기가 큰 멤버의 크기 1개만 계산된다.<br><br>
-	공용체의 멤버들이 메모리 공간을 공유하고 있기 때문에, 하나의 메모리 공간을 둘 이상의 다양한 방식으로 접근할 수 있다는 점에서 유용하다.<br><br>

Enumerated Type
---------------

```c
typedef enum syllable{
  Do = 1; Re = 2; Mi = 3; Fa = 4; So = 5; La = 6; Ti = 7;
} Syllable

int main(void){
  Syllable tone;

  for(tone=Do; tone<=Ti; tone+=1){
    ...
  }
}
```

-	syllable 열거형 변수에는 저장이 가능한 정수 값들을 결정한다는 의미이다.<br><br>
-	키워드들을 특정 값을 의미하는 상수로 정의하며, 예제는 열거형 상수를 통해 반복문을 사용한다.<br><br>
-	열거형 상수의 값을 정의하지 않으면, 상수의 값은 0부터 1씩 증가하는 형태로 결정이 된다.<br><br>
-	중간마다 값이 선언되어 있지 않으면, 앞서 선언된 상수보다 1이 증가한 값이 할당된다.<br><br>
-	열거형은 둘 이상의 연관이 있는 이름을 상수로 선언함으로써 프로그램의 가독성을 높이는데 의의가 있다.<br><br>

Reference
---------

-	열혈 C 프로그래밍 (윤성우 저) Chapter 22 - 23

---
