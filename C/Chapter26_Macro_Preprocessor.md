선행 처리
---------

-	소스 파일은 선행 처리기에 의해 선행 처리된 뒤, 컴파일러에 의해 컴파일된다.<br><br>
-	바이너리 데이터로 이루어진 오브젝트 파일은 링커에 의해 링크된다.<br><br>
-	선행 처리된 파일도 같은 소스 파일 형태이지만, 삽입해 놓은 선행처리 명령문이 소스코드 일부를 치환한다.<br><br>

define : Object-like macro
--------------------------

```c
#define PI 3.1415
//지시자, 매크로, 매크로 몸체
```

-	매크로 상수 PI를 전부 3.1415로 치환한다.<br><br>

define : Function-like macro
----------------------------

```c
#define SQUARE(X) X*X
```

-	SQUARE(X)와 동일한 패턴을 만나면 X*X로 치환한다.<br><br>
-	매크로 몸체에 괄호를 제대로 정의하지 않으면 연산 결과가 기대값과 다를 수 있다.<br><br>

매크로 특징
-----------

```c
#define SQUARE(X) \
        X * X
#define DIV(X) X/SQUARE(X)
```

-	매크로는 기본적으로 한 줄에 정의되어야 한다.<br><br>

-	두 줄에 걸쳐 정의할 경우, \ 문자를 활용해 개행을 명시한다.<br><br>

-	매크로 정의 시, 먼저 정의된 매크로를 사용할 수 있다.<br><br>

매크로 함수의 장단점
--------------------

-	매크로 함수는 일반 함수에 비해 실행 속도가 빠르며, 자료형에 따라 별도로 함수를 정의하지 않아도 된다.<br><br>
-	정의하기가 까다로우며, 디버깅하기가 쉽지 않다.<br><br>
-	따라서 작은 크기의 함수 및 호출의 빈도수가 높은 함수를 매크로로 정의한다.<br><br>

조건부 컴파일을 위한 매크로
---------------------------

```c
#if... #endif //참이라면
#ifdef... #endif //정의되었다면
#ifndef... #endif //정의되지 않았다면
#else //모든 조건부 매크로들에 삽입 가능
#elif //if에만 해당
```

-	특정 조건에 따라 소스 코드의 일부를 삽입하거나 삭제할 수 있도록 디자인 된 지시자들이다.<br><br>
-	일반적인 조건문과 유사하여 사용에 어렵지 않다.<br><br>

문자열 내에서 매크로의 매개변수 치환
------------------------------------

```c
#define STRING_JOB(A, B) "A의 직업은 B입니다." //치환 되지 않음
#define STR(ABC) #ABC
#define STRING_JOB(A, B) #A "의 직업은 " #B "입니다."

char * str = "ABC" "DEF"; //char * str = "ABCDEF" 와 동일
char * str = STR(12) STR(34); //char * str = "1234" 와 동일
```

-	문자열 안에서는 매크로의 매개변수 치환이 발생하지 않는다.<br><br>
-	\# 연산자를 이용해서 매개변수에 전달되는 인자를 문자열로 치환한다.<br><br>
-	문자열은 나란히 선언할 경우, 하나의 문자열로 간주된다.<br><br>

매크로 \#\# 연산자
------------------

```c
#define CON(UPP, LOW) UPP ## 00 ## LOW
int num = CON(22, 77); //220077
```

-	해당 연산자는 매크로 함수의 전달 인자를 다른 대상과 이어줄 때 사용한다.<br><br>

---

Reference
---------

-	열혈 C 프로그래밍 (윤성우 저) Chapter 26
