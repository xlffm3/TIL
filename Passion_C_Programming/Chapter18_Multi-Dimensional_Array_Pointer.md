Chapter 18 : Multi-Dimensional Array & Pointer
----------------------------------------------

---

### 2차원 배열과 포인터<br>

```C
int main(void){
  int arr2d[3][3];
  printf("%d", arr2d); //4585464
  printf("%d", arr2d[0]); //4585464
  printf("%d", &arr2d[0][0]); //4585464

  printf("%d", sizeof(arr2d)); //36
  printf("%d", sizeof(arr2d[0])); //12
  printf("%d", sizeof(arr2d[1])); //12

  printf("%p", arr2d); //004BFBE0
  printf("%p", arr2d+1); //004BFBE12

  type (*ptr) [n]; //2차원 배열의 포인터 형
}
```

-	arr2d는 첫 번째 요소를 가리키면서 배열 전체를 의미한다.<br><br>
-	arr2d[0]은 첫 번째 요소를 가리키되 1행만을 의미한다.<br><br>
-	때문에 sizeof 연산의 결과가 다르며, arr2d와 arr2d[0]은 서로 다른 것이다.<br><br>
-	2차원 배열이름(포인터)을 대상으로 증가 및 감소 연산을 할 경우, 연산 결과는 각 행의 첫 번째 요소의 주소 값이 된다.<br><br>
-	2차원 배열의 포인터 형은 배열의 가로 행 길이에 따라 상이하다.<br><br>
-	ptr은 "type형 변수를 가리키면서, 포인터 연산 시 sizeof(type)\*n의 크기 단위로 값이 증가 및 감소하는 포인터 변수 ptr"을 뜻한다.<br><br>
-	1차원 배열을 가리키는 포인터 변수를 이용해서 1차원 배열의 형태로 접근이 가능하듯, 2차원 배열도 가능하다.<br><br>

### 2차원 배열을 함수의 인자로 전달<br>

```C
void ShowArr2DStyle(int (*arr)[4], int column){
  ...
}

int main(void){
  int arr1[2][4];
  ShowArr2DStyle(arr1, sizeof(arr1)/sizeof(arr1[0]));
}

/* 치환 과정
arr[2][1] = 4;
(*(arr+2))[1] = 4;
*(arr[2]+1) = 4;
*(*(arr+2)+1) = 4;
*/
```

-	`int (*arr1)[4]` 는 `int arr1[][4]` 와 동일한 매개변수 선언이다.<br><br>
-	그러나 매개변수의 선언에서만 같은 의미를 지닐 뿐, 그 이외의 영역으로 확대해서 동일한 선언으로 간주하면 안 된다.<br><br>
-	column 등 길이 계산은 함수 외부에서 수행하며, 전체 사이즈를 행 사이즈로 나눈다.<br><br>
-	2차원 배열의 원소들도 메모리 상에서 나란히 위치를 할당받기 때문에, `arr[i] == *(arr+i)` 가 성립한다.<br><br>
-	따라서 행과 열 인덱스를 모두 치환하여 계산할 수 있다.<br><br>

### Reference<br>

-	열혈 C 프로그래밍 (윤성우 저) Chapter 18

---
