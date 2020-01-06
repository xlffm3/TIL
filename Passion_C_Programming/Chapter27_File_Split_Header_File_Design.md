Chapter 27 : File Split & Header File Design
--------------------------------------------

---

### 파일의 분할

```c
extern int num;
extern void Increment(void); //extern 선언 생략 가능
```

-	용도 및 특성 별로 함수와 변수를 나눠 파일을 저장하면 소스 코드의 관리가 용이하다.<br><br>
-	파일을 분할하여 컴파일 하기 위해서는 컴파일러에게 함수 및 변수가 외부에 선언 및 정의되었다는 사실을 알려줘야 한다.<br><br>

### static

-	static 전역변수는 외부 파일에서의 접근을 허용하지 않고, 접근 범위를 파일 내부로 제한한다는 의미이다.<br><br>
-	함수 또한 동일한 선언이 가능하다.<br><br>

### \#include<br>

> header1.h

```c
{
  puts("hello world");
```

<br>

> header2.h

```c
  return 0;
}
```

<br>

> main.c

```c
#include <stdio.h>

int main(void)
#include "header1.h"
#include "header2.h"
```

-	\#include 지시자는 파일의 내용을 단순히 포함시키는 용도로 사용된다.<br><br>

### 헤더파일을 include 하는 두 가지 방법<br>

```c
#include <헤더파일 이름>
#include "헤더파일 이름"
#include "C:\Cpower\MyProject\header.h"
```

-	첫 번째 방식은 C 표준 헤더파일이 저장되어 있는 디렉토리에서 파일을 찾는다.<br><br>
-	두 번째 방식은 해당 문장을 포함하는 소스 파일이 저장된 디렉토리에서 헤더 파일을 찾는다.<br><br>
-	두 번째 방식은 프로그래머가 정의하는 헤더 파일을 포함시킬 때 사용하며, 헤더 파일의 이름뿐만 아니라 절대경로를 명시해서 지정할 수 있다.<br><br>
-	외부에 선언된 변수 및 함수를 사용하기 위한 선언을 헤더 파일에 담아 사용한다.<br><br>
-	매크로의 명령문도 파일 단위로 유효하기 때문에, 해당 매크로를 필요로 하는 소스 파일은 해당 헤더 파일을 포함시키면 된다.<br><br>

### 구조체의 선언

-	컴파일러는 다른 파일의 정보를 참조하여 컴파일을 진행하지 않기 때문에, 특정 구조체에 대한 선언 및 정의는 이를 필요로 하는 모든 파일에 존재해야 한다.<br><br>
-	동일한 구조체의 정의가 두 군데 이상 존재하면 구조체의 수정 및 확장에 불편하기 때문에, 구조체의 선언 및 정의는 헤더 파일에 삽입하는 것이 좋다.<br><br>

### 헤더 파일의 중복 삽입 문제

-	프로그램이 복잡해지면서 헤더 파일이 중복으로 삽입되는데, 동일한 구조체가 중복으로 정의된 형태가 되어 컴파일 에러가 발생한다.<br><br>
-	외부 변수 및 함수를 사용하겠다는 `extern` 선언은 단순 메시지이기 때문에, 중복이 되어도 문제가 되지 않는다.<br><br>
-	그러나 구조체 정의는 컴파일을 하는데 도움을 주는 정보가 아니라 실행 파일의 내용에 직접적인 연관이 있는 정보이기 때문에, 중복될 수 없다.<br><br>

### 조건부 컴파일을 활용한 중복 삽입 문제 해결<br>

> stdiv2.h

```c
#ifndef __STDIV2_H__
#define __STDIV2_H__

typdef struct div{
  int x, y;
} Div;

#endif
```

<br>

> intdiv4.h

```c
#ifndef __INTDIV4_H__
#define __INTDIV4_H__

#include "stdiv2.h"
Div IntDiv(int num1, int num2);

#endif
```

<br>

> intdiv4.c

```c
#include "stdiv2.h"

Div IntDiv(int num1, int num2){
  Div dval;
  ...
  return dval;
}
```

<br>

> main.c

```c
#include <stdio.h>
#include "stdiv2.h"
#include "intdiv4.h"
```

-	헤더 파일을 중복으로 포함하려고 하지만, 삽입된 매크로 지시자로 인해 중복 삽입으로 인한 문제가 발생하지 않는다.<br><br>

### Reference<br>

-	열혈 C 프로그래밍 (윤성우 저) Chapter 27

---
