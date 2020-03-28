IO(Input & Output)
------------------

-	입출력은 컴퓨터 내부 또는 외부의 장치와 프로그램간의 데이터를 주고 받는 것을 의미한다.<br><br>

Stream
------

-	스트림이란 데이터를 운반하는데 사용되는 연결 통로이다.<br><r>
-	FIFO 구조로 된 단 방향적인 흐름이다.<br><br>

바이트 기반 스트림
------------------

![image](https://user-images.githubusercontent.com/56240505/72342388-b477ab00-370f-11ea-97d6-4a9bb6be5376.png)<br><br>

-	스트림은 바이트 단위로 데이터를 전송한다.<br><br>
-	이들은 모두 InputStream 또는 OutputStream의 자손들이다.<br><br>
-	스트림을 사용한 뒤 반드시 스트림을 닫아야 하며, 표준 입출력 스트림은 닫지 않아도 된다.<br><br>

ByteArrayInputStream & ByteArrayOutputStream
--------------------------------------------

-	바이트 배열에 데이터를 입출력 하는데 사용되는 스트림이며, 다른 곳에 입출력 하기 전에 데이터를 입시로 바이트 배열에 담아 변환 등의 작업을 하는데 사용된다.<br><br>
-	`read()` 및 `write()` 메서드는 IOException을 유발할 수 있기 때문에 예외 처리 블럭이 필요하다.<br><br>

FileInputStream & FileOutputStream
----------------------------------

-	텍스트 파일의 경우 FileReader와 FileWriter을 사용하는 것이 낫다.<br><br>

보조 스트림
-----------

-	보조 스트림은 실제 데이터를 주고 받는 스트림이 아니기 때문에 데이터를 입출력할 수 있는 기능은 없지만, 스트림의 기능을 향상시키거나 새로운 기능을 추가할 수 있다.<br><br>
-	따라서 스트림을 먼저 생성한 다음에 이를 이용해서 보조 스트림을 생성한다.<br><br>

바이트 기반의 보조 스트림
-------------------------

```java
FilterInputStream / FilterOutputStream
BufferedInputStream / BufferedOutputStream
DataInputStream / DataOutputStream
SequenceInputStream
PrintStream
```

-	한 바이트씩 입출력하는 것 보다 바이트 배열인 버퍼를 이용해서 한 번에 여러 바이트를 입출력하는 것이 빠르기 때문에, 버퍼 보조 스트림을 자주 쓴다.<br><br>
-	버퍼가 가득 찾을 때만 출력 소스에 출력을 하기 때문에, `flush()` 나 `close()` 로 마지막에 버퍼에 있는 모든 내용이 출력되도록 한다.<br><br>
-	보조 스트림의 `close()` 메서드는 기반 스트림의 `close()`를 호출하기 때문에, 기반 스트림이 아닌 보조 스트림의 버퍼를 비우면 된다.<br><br>
-	DataStream은 데이터를 읽고 쓰는데 byte 단위가 아닌 8가지 자료형의 단위로 읽고 쓸 수 있다.<br><br>
-	SequenceInputStream은 여러 개의 입력 스트림을 연속적으로 연결해서 하나의 스트림으로부터 데이터를 읽는 것과 같이 처리할 수 있도록 도와준다.<br><br>
-	큰 파일을 여러 개의 작은 파일로 나누었다가 하나의 파일로 합치는 것과 같은 작업에 유리하다.<br><br>
-	PrintStream은 데이터를 기반 스트림에 다양한 형태로 출력할 수 있는 `printf()`, `println()` 등의 메서드를 오버로딩하여 제공한다.<br><br>
-	PrintStream은 문자 기반 스트림의 역할을 수행하며, PrintWriter가 성능이 더 좋기 때문에 가급적 PrintWriter를 사용한다.<br><br>

문자 기반 스트림
----------------

![image](https://user-images.githubusercontent.com/56240505/72342392-b93c5f00-370f-11ea-8c0e-188ca1a9c6f1.png)<br><br>

-	C와 다르게 Java는 한 문자를 의미하는 char 형이 2 byte이기 때문에 1 byte 기반의 스트림만으로 문자를 처리하는데 어려움이 있다.<br><br>
-	따라서 Reader와 Writer라고 하는 문자 기반의 스트림이 제공된다.<br><br>
-	byte 배열이 아닌 char 배열을 사용한다는 점이 다르다.<br><br>
-	마찬가지로 보조 스트림이 존재한다.<br><br>

Reader & Writer
---------------

-	문자 기반 스트림은 여러 종류의 인코딩과 유니코드 간의 변환을 자동적으로 처리해준다.<br><br>

FileReader & FileWriter
-----------------------

-	텍스트 데이터를 읽고 쓰는데 사용된다.<br><br>

PipedReader & PipedWriter
-------------------------

-	쓰레드 간에 데이터를 주고 받을 때 사용되며, 다른 스트림과 달리 입력과 출력 스트림을 하나의 스트림으로 연결해 데이터를 주고 받는다.<br><br>
-	스트림을 생성한 뒤, 한 쓰레드에서 `connect()` 를 호출하여 입력과 출력 스트림을 연결한다.<br><br>
-	한 쪽만 닫아도 나머지 스트림은 닫힌다.<br><br>

StringReader & StringWriter
---------------------------

-	입출력 대상이 메모리인 스트림이며, 출력되는 데이터는 내부의 StringBuffer에 저장된다.<br><br>
-	String도 char 배열이지만, String 자체로 처리하는게 편하다.<br><br>

문자 기반의 보조 스트림
-----------------------

```java
BufferedReader / BufferedWriter
InputStreamReader / OutputStreamWriter
```

-	InputStreamReader / OutputStreamWriter는 바이트 기반 스트림을 문자 기반 스트림으로 연결시켜주는 역할이다.<br><br>
-	바이트 기반 스트림의 데이터를 지정된 인코딩의 문자 데이터로 변환하는 작업을 수행한다.<br><br>
-	인코딩을 지정하지 않으면 OS에서 사용하는 인코딩을 사용한다.<br><br>
-	Buffer 관련 스트림은 `newLine()` 으로 개행하며 `readLine()` 으로 데이터를 라인 단위로 읽을 수 있다.<br><br>
-	두 보조 스트림은 함께 쓰면 편리하다.<br><br>

표준 입출력
-----------

```java
System.in
System.out
System.err
```

-	Java는 표준 입출력 3가지를 자동으로 제공하며, 별도의 스트림 생성하는 코드가 필요없다.<br><br>
-	개행도 사용자 입력으로 인식되지만, 표준 입출력은 내부적으로 BufferedReader를 이용하기 때문에 `readLine()` 으로 라인 단위 데이터를 입력 받는다.<br><br>

표준 입출력의 대상 변경
-----------------------

```java
static void setOut(PrintStream out)
static void setErr(PrintStream err)
static void setIn(InputStream in)
```

-	해당 메서드들은 표준 스트림을 지정한 스트림으로 변경한다.<br><br>
-	이를 통해 콘솔 이외에 다른 입출력 대상으로 변경하는 것이 가능하다.<br><br>

RandomAccessFile
----------------

-	DataInput과 DataOutput을 모두 구현했기 때문에, 하나의 클래스로 파일에 대한 입력과 출력을 모두 할 수 있다.<br><br>
-	내부적으로 파일 포인터를 사용하기 때문에 비순차적으로 파일을 읽고 쓰는데 제약이 없다.<br><br>
-	`seek()` 과 같은 메서드는 C와 다를 것이 없다.<br><br>

File
----

```Java
File f = new File("c:\\test.java");
```

-	File 스트림을 생성하기 위한 URL은 절대 경로와 상대 경로가 있다.<br><br>
-	이미 존재하는 파일을 참조할 때는 문제 없지만, 기존에 없는 파일을 새로 생성할 때는 `createNewFile()` 메서드를 호출해야 한다.<br><br>

Serialization
-------------

-	직렬화란 객체를 데이터 스트림으로 만드는 것을 뜻한다.<br><br>
-	데이터를 스트림에 쓰기 위해 연속적인 데이터로 변환한다.<br><br>
-	반대로 스트림으로부터 데이터를 읽어서 객체를 만드는 것은 역직렬화라고 한다.<br><br>
-	객체는 클래스에 정의된 인스턴스 변수의 집합이며, 객체에는 메서드가 포함되지 않는다.<br><br>
-	객체를 저장한다는 것은 객체의 모든 인스턴스 변수의 값을 저장한다는 의미이다.<br><br>

ObjectInputStream & ObjectOutputStream
--------------------------------------

```java
FileOutputStream fos = new FileOutputStream("objectfile.ser");
ObjectOutputStream out = new ObjectOutputStream(fos);
out.writeObject(new UserInfo());
```

-	보조스트림이기 때문에 기반 스트림이 필요하다.<br><br>
-	역직렬화도 같은 방식이지만, 반환 타입이 Object이기 때문에 형 변환이 필요하다.<br><br>

직렬화가 가능한 클래스 생성
---------------------------

```java
Object obj = new String("abc"); //직렬화가 가능하다
transient Object obj2 = new Object();
```

-	직렬화가 가능한 클래스를 생성하기 위해서는 Serializable 인터페이스를 구현하면 된다.<br><br>
-	조상 클래스가 Serializable 인터페이스를 구현했으면, 자손 클래스를 직렬화할 때 조상 클래스의 인스턴스 변수들도 함께 직렬화된다.<br><br>
-	그러나 자손 클래스만 해당 인터페이스를 구현했다면, 조상 클래스의 인스턴스 변수들은 직렬화 대상이 아니다.<br><br>
-	Object 객체는 직렬화될 수 없다.<br><br>
-	예제는 Object이긴 하지만 실제 저장된 객체는 String 인스턴스이기 때문에 직렬화가 가능하다.<br><br>
-	transient 제어자를 사용하면 직렬화 대상에서 제외되며, 직렬화를 하면 해당 인스턴스 변수의 값은 기본값으로 직렬화된다.<br><br>
-	패스워드 등 보안상 직렬화가 되지 않아야 하는 변수들은 transient 제어자를 사용한다.<br><br>

직렬화 가능한 클래스의 버전 관리
--------------------------------

```java
class MyData implements java.io.Serializable{
  static final long serialVersionUID = 35124141241L;
  int value1;
}

C:\jdk1.8\work\ch15 > serialver MyData
```

-	직렬화된 객체를 역직렬화할 때는 직렬화 했을 때와 같은 클래스를 사용한다.<br><br>
-	그러나 클래스의 이름이 같더라도 클래스의 내용이 변경된 경우 역직렬화는 실패한다.<br><br>
-	객체가 직렬화 될 때 클래스에 정의된 멤버들의 정보를 이용해서 serialVersionUID라는 클래스의 버전을 자동 생성해서 직렬화 내용에 포함한다.<br><br>
-	역직렬화 할 때 클래스의 버전을 비교함으로써 버전이 일치하는지 확인할 수 있다.<br><br>
-	그러나 static 변수나 상수 및 transient 변수가 추가되는 경우에는 직렬화에 영향을 미치지 않기 때문에 클래스 버전을 다르게 인식하도록 할 필요는 없다.<br><br>
-	클래스 버전을 수동으로 관리하기 위해서는 serialVersionUID 변수를 추가로 정의한다.<br><br>
-	해당 변수를 정의해주면, 클래스의 내용이 바뀌어도 클래스의 버전이 자동 생성된 값으로 변경되지 않는다.<br><br>
-	서로 다른 클래스간에 같은 값을 갖지 않도록 serialver.exe 를 사용해서 생성된 값을 사용하는 것이 보통이다.<br><br>
-	serialver.exe 뒤에 클래스의 이름을 적었을 때, UID가 정의되어 있으면 그 값을 출력하고 정의되어 있지 않으면 자동 생성한 값을 출력한다.<br><br>
-	UID 값은 클래스의 멤버들에 대한 정보를 바탕으로 하기 때문에, 해당 정보가 변경되지 않는 한 항상 같은 값을 생성한다.<br><br>

---

Reference
---------

-	Java의 정석 (남궁성 저) Chapter 15
