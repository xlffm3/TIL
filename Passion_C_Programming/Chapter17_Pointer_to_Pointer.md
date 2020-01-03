Chapter 17 : Pointer to Pointer
-------------------------------

---

### Double Pointer Variable<br>

```c
int main(void){
  double num = 3.14;
  double * ptr = &num;
  double ** dptr = &ptr;
}
```

-	이중 포인터 또는 더블 포인터는 포인터 변수를 가리키는 또 다른 포인터 변수를 뜻한다.<br><br>
-	선언은 * 연산자를 두 개 이어서 선언한다.<br><br>
-	\*dptr은 포인터 변수 ptr을 의미하며, \**dptr은 변수 num을 의미한다.<br><br>

### 포인터 변수 대상의 Call-by-reference<br>

```c
void SwapIntPtr(int **dp1, int **dp2){
  int *temp = *dp1;
  *dp1 = *dp2;
  *dp2 = temp;
}

int main(void){
  int num1=10, num2=20;
  int *ptr1, *ptr2;
  ptr1=&num1, ptr2=&num2;
  SwapIntPtr(&ptr1, &ptr2);
  return 0;
}
```

-	싱글 포인터를 함수에 전달할 경우, 값이 swap되지 않기 때문에 더블 포인터를 전달한다.<br><br>

### 다중 포인터<br>

-	삼중 이상의 포인터를 사용할 수 있으나, 사용되는 예는 그리 많지 않다.<br><br>
-	포인터가 필요한 이유는, 함수 내에서 함수 외부에 선언된 변수에 접근하는 방법을 제시해 주기 때문이다.<br><br>

### Reference<br>

-	열혈 C 프로그래밍 (윤성우 저) Chapter 17

---
