Chapter 21 : String IO
----------------------

---

### Stream<br>

-	한 방향으로 흐르는 데이터의 흐름이며, 데이터의 이동 수단이다.<br><br>
-	운영체제는 외부 장치와 프로그램과의 데이터 송수신의 도구가 되는 가상의 다리인 입력 / 출력 스트림을 제공하고 있다.<br><br>
-	파일 입출력은 직접 운영체제에 스트림 형성을 요청해야 하지만, 콘솔 입 출력은 스트림 형성을 요청할 필요가 없다.<br><br>
-	콘솔 입출력을 위한 입력 스트림과 출력 스트림은 프로그램이 실행되면 자동으로 생성되고, 프로그램이 종료되면 자동으로 소멸되는 표준 스트림이다.<br><br>
-	표준 스트림(Standard Stream)
	-	표준 입력 스트림(stdin)
	-	표준 출력 스트림(stdout)
	-	표준 에러 스트림(stderr)<br><br>

### 문자 단위 입출력 함수<br>

```c
//문자 출력 함수
int putchar(int c)
int fputc(int c, FILE * stream);

//문자 입력 함수
int getchar(void);
int fgetc(FILE * stream);
```

-	`putchar()` 함수도 표준 출력 스트림을 통해 전송하지만, `fputc()` 함수는 스트림을 직접 지정할 수 있어서 파일을 대상으로 데이터를 전송할 수 있다.<br><br>
-	엔터 키도 아스키 코드 값 10인 '\n'으로 표현되는 문자이며, 입출력의 대상이 된다.<br><br>
-	출력 함수 호출 성공 시 쓰여진 문자 정보가, 실패 시 EOF가 반환된다.<br><br>
-	입력 함수의 관계는 두 출력 함수의 관계와 동일하며, 파일의 끝에 도달하거나 함수 호출 실패시 EOF를 반환한다.<br><br>

### EOF(End Of File)<br>

```c
int main(void){
  int ch;

  while(1){
    ch=getchar();
    if(ch==EOF)
      break;
    putchar(ch);
  }
}
```

-	EOF는 파일의 끝을 표현하기 위해 정의해 놓은 상수이며, 입력 함수가 호출된 결과로 EOF가 반환되었다면 파일의 끝에 도달해서 더 이상 읽을 내용이 없다는 뜻이 된다.<br><br>
-	문자 입력 함수의 호출이 실패하거나, CTRL+Z(Windows) 혹은 CTRL+D(Linux)가 입력되는 경우이다.<br><br>
-	문자 입력 함수의 반환형이 int 인 이유는, EOF가 -1로 정의된 상수이기 때문이다.<br><br>
-	반환형이 char인 경우, 일부 컴파일러들이 char의 default를 unsigned char로 처리하여 -1을 인식할 수 없어서 안전한 int를 반환형으로 채택했다.<br><br>

### 문자열 단위 입출력 함수

```c
//문자열 출력 함수
int puts(const char * s)
int fputs(const char * s, FILE * stream);

//문자열 입력 함수
char str[7]; //매개변수 char * s에 해당
char * gets(char * s);
char * gets(char * s, int n, FILE * stream);
fgets(str, sizeof(str), stdin);
```

-	`puts()` 함수는 문자열 출력 후 자동으로 개행이 이루어지지만, `fputs()` 함수는 개행이 없다.<br><br>
-	`fputs()` 함수는 출력의 대상을 직접 결정할 수 있다.<br><br>
-	문자열 출력 함수는 성공 시 음수가 아닌 값을, 실패시 EOF를 반환한다.<br><br>
-	`gets()` 함수는 미리 마련해 놓은 배열을 넘어서는 길이의 문자열이 입력되면, 할당 받지 않은 메모리 공간을 침범하여 실행 중 오류가 발생한다.<br><br>
-	가급적 `fgets()` 예제 처럼 호출하는 것이 바람직하다.<br><br>
-	`fgets()` 함수가 문자열을 입력 받고 `sizeof(str)` 길이만큼 배열 str에 저장하는데, 문자열의 끝에 자동으로 널 문자가 추가 되어 예상보다 길이가 하나 작은 문자열이 저장된다.<br><br>
-	`fgets()` 함수는 개행문자 \n을 만날 때 까지 문자열을 읽어 들이는데, \n을 제외시키거나 버리지 않고 문자열의 일부로 받아들인다.<br><br>

### 표준 입출력과 버퍼<br>

-	위에서 공부한 입출력 함수들을 표준 입출력 함수라고 한다.<br><br>

-	표준 입출력 함수를 통해 데이터를 입출력 하는 경우, 데이터들은 운영체제가 제공하는 메모리 버퍼를 중간에 통과한다.<br><br>

-	버퍼는 입력 버퍼와 출력 버퍼로 나누어진다.<br><br>

-	엔터 키가 눌리는 시점에 키보드로부터 입력된 데이터가 입력 스트림을 거쳐 입력 버퍼로 들어간다.<br><br>

-	입력 함수가 읽어 들이는 문자열은 입력 버퍼에 저장된 문자열이며, 버퍼링이 된 다음 프로그램에서 해당 문자열이 읽혀진다.<br><br>

### Buffering<br>

-	데이터를 목적지로 바로 전송하지 않고 중간에 출력 버퍼와 입력 버퍼를 둬서 전송하고자 하는 데이터를 임시 저장하는 이유는, 데이터 전송의 효율성과 관련이 있다.<br><br>
-	키보드 및 모니터와 같은 외부 장치와의 데이터 입출력은 시간이 다소 소요되는 작업이기 때문이다.<br><br>

### fflush()<br>

```c
int flush(FILE * stream);
```

-	출력 버퍼가 비워진다는 것은 출력 버퍼에 저장된 데이터가 버퍼를 떠나서 목적지로 이동됨을 뜻한다.<br><br>
-	출력 버퍼가 비워지는 시점은 시스템에 따라, 버퍼의 성격에 따라 상이하다.<br><br>
-	함수 호출 성공 시 0을, 실패 시 EOF를 반환하며 인자로 파일 스트림 정보가 전달되면 파일을 대상으로도 호출이 가능하다.<br><br>

### 입력 버퍼의 초기화<br>

```c
void ClearLineFromReadBuffer(void){
    while(getchar()!='\n');
}

int main(void){
    char perId[7];
    char name[10];

    fputs("주민번호 앞 6자리 입력 : ", stdout);
    fgets(perId, sizeof(perId), stdin);

    ClearLineFromReadBuffer();

    fputs("이름 입력 : ", stdout);
    fgets(name, sizeof(name), stdin);

    printf("주민번호 : %s \n", perId);
    printf("이름 : %s \n", name);

    return 0;
}
```

-	입력 버퍼의 비워짐은 데이터의 소멸을 의미한다.<br><br>
-	위 예제에서 `fgets()` 함수는 \n 을 만날 때 까지 읽어 들이는 함수라서, 적절한 버퍼의 초기화가 없으면 문제가 발생한다.<br><br>
-	`ClearLineFromReadBuffer()` 함수는 \n 이 읽혀질 때 까지 입력 버퍼에 저장된 문자들을 지우는 함수이다.<br><br>

### 문자열 관련 함수

```c
size_t strlen(const char*s); //문자열의 길이를 반환하되, 널 문자는 길이에 포함하지 않음

char * strcpy(char * dest, const char * src); //복사된 문자열의 주소 값 반환
char * strncpy(char * dest, const char * src, size_t n); //지정된 사이즈 만큼만 복사

char * strcat(char * dest, const char * src); //덧붙여진 문자열의 주소 값 반환
char * strncat(char * dest, const char * src, size_t n); //지정된 사이즈 만큼만 덧붙임

int strcmp(const char * s1, const char * s2); //문자열 비교
int strncmp(const char * s1, const char * s2, size_t n); //지정된 사이즈 만큼만 비교함

int atoi(const char * str); //문자열 내용을 int형으로 변환
long atol(const char * str); //문자열 내용을 long 형으로 변환
double atof(const char * str); //문자열 내용을 double 형으로 변환
```

-	복사 관련 함수는 문자열의 끝에 널 문자를 자동으로 삽입하지 않으나, 덧붙임 관련 함수는 자동으로 삽입해준다.<br><br>
-	비교 관련 함수는 사전 편찬 순서를 비교한다.<br><br>

### Reference<br>

-	열혈 C 프로그래밍 (윤성우 저) Chapter 21

---
