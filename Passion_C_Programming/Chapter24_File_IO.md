Chapter 24 : File IO
--------------------

---

### fopen<br>

```c
FILE * fopen(const char * filename, const char * mode);
//성공 시 해당 파일의 FILE 구조체 변수의 주소 값, 실패 시 NULL 포인터 반환
```

-	해당 함수를 통해 프로그램상에서 파일과의 스트림을 형성할 수 있다.<br><br>
-	FILE 구조체 변수에는 파일에 대한 정보가 담기며, 포인터는 사실상 파일을 가리키는 지시자의 역할을 한다.<br><br>

### fclose<br>

```c
int main(void){
  FILE * fp = fopen("data.txt", "wt");
  if (fp==NULL){
    puts("failure");
    return -1;
  }
  fputc('A', fp);
  fclose(fp); //스트림의 종료
  return 0;
}
```

-	실제로 스트림을 형성하는 주체는 운영체제이다.<br><br>
-	해당 함수는 개방되었던 파일을 닫아주는데, 이는 운영체제가 할당한 자원을 반환하고 버퍼링 되었던 데이터를 출력하기 위함이다.<br><br>

### fflush<br>

```c
int flush(FILE * stream);
//성공 시 0, 실패 시 EOF 반환
```

-	출력 버퍼를 비우는 함수이며, 입력 버퍼를 비우는 함수는 필요하지 않다.<br><br>

### 파일의 개방 모드<br>

![image](https://user-images.githubusercontent.com/56240505/71781082-fa48bb00-300d-11ea-9440-9100a4894182.png)<br><br>

-	읽기와 쓰기 및 텍스트 파일과 바이너리 파일 등의 스트림 기준을 통해 적합한 개방 모드를 선택하면 된다.<br><br>
-	+가 붙은 키워드는 읽기, 쓰기가 모두 가능한 스트림이지만 메모리 버퍼를 비우는 등 여러 불편함과 리스크가 존재하여 지양한다.<br><br>
-	운영체제마다 인식하는 개행의 표현이 상이하기 때문에, 텍스트 모드로 파일을 개방하면 치환이 발생한다.<br><br>
-	반면 바이너리 모드는 치환이 발생하지 않고, 개방 모드에 t도 b도 붙이지 않으면 파일은 기본적으로 텍스트 모드로 개방된다.<br><br>
-	문자열이 파일에 저장될 때에는 문자열의 끝을 의미하는 널 문자는 저장되지 않고, 파일은 개행을 기준으로 문자열을 구분한다.<br><br>

### feof<br>

```c
int feof(FILE * stream);
//파일의 끝에 도달한 경우 0이 아닌 값을 반환

int main(void){
  FILE * src = fopen("src.txt", "rt");
  FILE * des = fopen("des.txt", "wt");
  char str[20];

  while(fgets(str, sizeof(str), src) != NULL)
    fputs(str, des);

  if (feof(src) != 0)
    puts("파일 복사 완료");
  else
    puts("파일복사 실패");

    fclose(src);
    fclose(des);
    return 0;
}
```

-	파일 복사 프로그램과 같이 파일의 끝을 확인해야 하는 경우 유용하게 사용된다.<br><br>

### 바이너리 데이터의 입출력 : fread, fwrite<br>

```c
size_t fread(void * buffer, size_t size, size_t count, FILE * stream);
//성공 시 전달인자 count, 실패 또는 파일의 끝 도달 시 count보다 작은 값 반환
size_t fwrite(const void * buffer, size_t size, size_t count, FILE * stream);
//성공 시 전달인자 count, 실패 시 count보다 작은 값 반환

int main(void){
  FILE * src = fopen("src.bin", "rb");
  FILE * des = fopen("dst.bin", "wb");
  char buf[20];
  int readCnt;

  while(1){
    readCnt = fread((void*)buf, 1, sizeof(buf), src);

    if (readCnt < sizeof(buf)){
      if (feof(src) != 0){
        fwrite((void*) buf, 1, readCnt, des);
        puts("파일 복사 완료");
        break;
      } else {
        puts("파일 복사 실패");
      }
      break;
    }

    fwrite((void *) buf, 1, sizeof(buf), des);
  }

  fclose(src);
  fclose(des);
  return 0;
}
```

-	`fread` 함수는 size 크기의 데이터 count개를 파일로 읽어서 buffer에 저장한다.<br><br>
-	`fwrite` 함수 또한 비슷한 맥락에서 실행된다.<br><br>

### 서식에 따른 데이터 입출력 : fprint, fscanf<br>

```c
typedef struct fren{
    char name[10];
    char sex;
    int age;
} Friend;

int main(void){
    FILE * fp;
    Friend myfren1;
    Friend myfren2;

    fp = fopen("friend.bin", "wb");
    printf("이름 성별 나이순 입력 : ");
    scanf("%s %c %d", myfren1.name, &(myfren1.sex), &(myfren1.age));
    fwrite((void*)&myfren1, sizeof(myfren1), 1, fp);
    fclose(fp);

    fp = fopen("friend.bin", "rb");
    fread((void*)&myfren2, sizeof(myfren2), 1, fp);
    printf("%s %c %d \n", myfren2.name, myfren2.sex, myfren2.age);
    fclose(fp);
    return 0;
}
```

-	구조체 변수를 하나의 바이너리 데이터로 인식하여 처리할 수 있다.<br><br>

### 파일 위치 지시자<br>

-	파일 위치 정보의 갱신을 통해 데이터를 읽고 쓸 위치 정보를 유지하는 멤버를 파일 위치 지시자라고 한다.<br><br>
-	파일 위치 지시자는 파일이 처음 개방되면 무조건 파일의 맨 앞부분을 가리킨다.<br><br>

### fseek, ftell<br>

```c
int fseek(FILE * stream, long offset, int wherefrom);
//성공 시 0, 실패 시 0이 아닌 값을 반환

long ftell(FILE * stream);
//파일 위치 지시자의 위치 정보 반환

int main(void){
  long fpos;
  int i;

  FILE * fp = fopen("text.txt", "wt");
  fputs("1234-", fp);
  fclose(fp);

  fp = fopen("text.txt", "rt");

  for(i=0; i<4; i++){
    putchar(fgetc(fp));
    fpos=ftell(fp);
    fseek(fp, -1, SEEK_END);
    putchar(fgetc(fp));
    fseek(fp, fpos, SEEK_SET);
  }

  fclose(fp);
  return 0;
}
```

-	stream으로 전달된 파일 위치 지시자를 wherefrom에서부터 offset 바이트만큼 이동시키는 의미이다.<br><br>
-	wherefrom에 전달되는 상수
	-	SEEK_SET(0) : 파일 맨 앞에서부터 이동을 시작한다.
	-	SEEK_CUR(1) : 현재 위치에서부터 이동을 시작한다.
	-	SEEK_END(2) : 파일 맨 끝에서부터 이동을 시작한다.<br><br>
-	offset 매개변수에 양의 정수가 전달되면 파일의 마지막을 향해서 파일 위치 지시자가 이동하며, 음수는 파일의 시작 위치를 향해 이동한다.<br><br>
-	함수에서 의미하는 파일의 끝은 마지막 데이터의 다음인 EOF를 의미한다.<br><br>
-	출력이 되면 파일 위치 지시자는 그 다음 인덱스를 가리키게 된다.<br><br>
-	파일 위치 지시자의 위치 정보는 가장 앞 부분의 바이트 위치를 0으로 간주한다.<br><br>

### Reference<br>

-	열혈 C 프로그래밍 (윤성우 저) Chapter 24

---
