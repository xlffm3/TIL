Function Pointer
----------------

```c
int SoSimple(int num1, int num2){
  ...
  return 0;
}

int main(void){
  int (*fptr)(int, int);
  fptr = SoSimple;
  int result = fptr(3, 4);
}
```

-	배열의 이름이 배열의 시작 주소 값을 의미하듯, 함수의 이름도 함수가 저장된 메모리 공간의 주소 값을 의미한다.<br><br>
-	함수의 주소 값 저장을 위한 포인터 변수를 함수 포인터라고 한다.<br><br>
-	함수 포인터 변수에는 함수의 반환형 정보와 매개변수 선언의 정보가 모두 표현되어 있어야 한다.<br><br>
-	fptr이 함수 포인터이며, 이를 통해 함수를 호출할 수 있다.<br><br>
-	매개변수에도 마찬가지로 함수 포인터가 전달될 수 있다.<br><br>

Void Pointer
------------

```C
int main(void){
  int num=10;
  void * ptr = &num;
  *ptr = 20; //컴파일 에러
  ptr++; //컴파일 에러
}
```

-	보이드 포인터는 변수 및 함수 등 모든 것을 담을 수 있는 포인터 변수이다.<br><br>
-	그러나 가리키는 대상에 대한 어떠한 형 정보도 담겨있지 않아, 값의 변경이나 참조가 불가능하다.<br><br>
-	따라서 주소 값에만 의미를 두고 포인터의 형은 나중에 결정하는 등의 상황에서 쓰인다.<br><br>

main 함수로의 인자 전달
-----------------------

```c
int mian(int argc, char * argv[]){
  char * str[3] = {"c pro", "go pro", "java pro"};
  int i=0;
  printf("전달된 문자열의 수 : %d", argc);
  for(i=0; i<argc; i++){
    printf("%d번째 문자열 : %s", i+1, argv[i]);
  }
  return 0;
}
```

-	`void SimpleFunc(Type * arr)` 혹은 `void SimpleFunc(Type arr[])` 는 Type 형 1차원 배열의 이름(주소 값)을 인자로 전달받을 수 있는 매개변수의 선언이다.<br><br>
-	이를 `Type` 을 `char*` 로 바꾸면, `(char ** arr)` 혹은 `(char * arr[])` 이 된다.<br><br>
-	즉, main 함수의 매개변수 argv는 char 형 더블 포인트 변수이며, 이는 char 형 포인터 변수로 이뤄진 1차원 배열의 이름을 전달받을 수 있는 매개변수이다.<br><br>
-	str의 요소들이 `char*` 이기 때문에, 배열 이름 str의 형은 `char**` 가 된다.<br><br>
-	main 함수로 인자를 전달하는 경우 공백을 기준으로 문자열이 나뉘며, 각각의 문자열 주소 값은 포인터 배열 argv의 원소가 된다.<br><br>
-	cmd 창에서 입력 시, 큰 따옴표로 묶어서 입력하면 공백을 포함하는 문자열을 생성해서 인자로 전달하는 것이 가능하다.<br><br>

---

Reference
---------

-	열혈 C 프로그래밍 (윤성우 저) Chapter 19
