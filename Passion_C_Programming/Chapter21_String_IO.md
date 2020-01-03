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

### Reference<br>

-	열혈 C 프로그래밍 (윤성우 저) Chapter 21

---
