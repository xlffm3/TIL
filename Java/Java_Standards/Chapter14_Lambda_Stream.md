Lambda Expression
-----------------

-	람다식의 도입으로 인해 객체지향언어인 Java에 함수형 언어의 장점을 접목시킬 수 있었다.<br><br>
-	람다식은 메서드를 하나의 식으로 표현한 것이다.<br><br>
-	메서드를 람다식으로 표현하면 메서드의 이름과 반환값이 없어지므로, 익명 함수라고 칭하기도 한다.<br><br>
-	기존의 메서드는 클래스와 객체를 생성해야 한다는 제약이 있었지만, 람다식 함수로 메서드의 역할을 대신할 수 있다.<br><br>

람다식 작성
-----------

```java
int max(int a, int b){
  return a > b ? a : b;
} //일반적인 메서드

(int a, int b) -> {return a > b ? a : b;} //람다식
(int a, int b) -> return a > b ? a : b; //에러, return문은 중괄호 생략 불가능
(int a, int b) -> a > b ? a : b //반환값이 있을 경우 return문을 식으로 대신할 수 있음
(a, b) -> a > b ? a : b //타입 추론이 가능한 경우 타입 생략 가능
a -> a*a //매개변수가 1개일 경우 괄호 생략 가능
int a -> a*a //에러, 타입이 있을 경우 생략 불가능
(String name) -> System.out.println(name) //중괄호 안의 문장이 하나일 때 괄호 생략이 가능하며, 세미 콜론도 생략함
```

-	문장(statement)이 아닌 식(expression)으로 끝나는 경우 세미 콜론을 생략한다.<br><br>

함수형 인터페이스
-----------------

```java
(int a, int b) -> a > b ? a : b

new Object(){
  int max(int a, int b){
    return a > b ? a: b;
  }
}

@FunctionalInterface
interface MyFunction{
  public abstract int max(int a, int b);
}

MyFunction f = new MyFunction(){
  public int max(int a, int b){
    return a > b ? a : b;
  }
}

f = (int a, int b) -> a > b ? a : b;
int big = f.max(5, 3);

MyFunction myMethod(){
  return () -> {};
}

void aMethod(MyFunction f){
  f.myMethod();
}

aMethod(() -> {});
```

-	람다식은 익명 클래스의 객체와 동등하다.<br><br>
-	인터페이스를 구현한 익명 객체를 람다식으로 대체가 가능한 이유는, 람다식도 익명 객체이며 메서드의 매개변수의 타입과 개수 그리고 반환값이 일치하기 때문이다.<br><br>
-	람다식은 인터페이스를 통해 다루기로 결정되었으며, 람다식을 다루기 위한 인터페이스를 함수형 인터페이스라고 한다.<br><br>
-	함수형 인터페이스에는 오직 하나의 추상 메서드만 정의되어 있어야 하지만, static 메서드나 default 메서드는 제약이 없다.<br><br>
-	람다식을 참조변수로 다룰 수 있다는 것은 메서드를 통해 람다식을 주고 받을 수 있으며, 변수처럼 메서드를 주고 받는 것이 가능하다는 뜻이다.<br><br>

람다식의 타입과 형 변환
-----------------------

```java
MyFunction f = (MyFunction) (()->{}); //ok
Object obj = (Object)(()->{}); //에러
```

-	람다식은 익명 객체이며 타입을 알 수 없기 때문에, 형 변환이 필요하다.<br><br>
-	람다식은 오직 함수형 인터페이스로만 형 변환이 가능하며, Object 타입으로 형 변환을 하려면 먼저 함수형 인터페이스로 변환해야 한다.<br><br>
-	람다식 내부에서 참조하는 지역변수는 final이 붙지 않았어도 상수로 간주된다.<br><br>

java.util.function
------------------

![image](https://user-images.githubusercontent.com/56240505/72324232-c7798380-36ed-11ea-9bf8-98cc1d03fc77.png)<br><br>

-	자주 쓰이는 형식의 메서드를 함수형 인터페이스로 미리 정의해놓은 패키지이다.<br><br>
-	매번 새로운 함수형 인터페이스를 정의하기 보다, 패키지의 인터페이스를 사용하면 재사용성이나 유지 보수 측면에서 효과적이다.<br><br>

UnaryOperator & BinaryOperator
------------------------------

-	매개변수의 타입과 반환타입의 타입이 모두 일치한다는 점만 제외하고 Function과 동일하다.<br><br>

Function의 합성과 Predicate의 결합
----------------------------------

```java
andThen()
compose()
identity()
and()
or()
negate()
isEqual()
```

-	두 람다식을 합성해서 새로운 람다식을 만들 수 있다.<br><br>
-	사용할 때 키워드의 순서에 유의한다.<br><br>

메서드 참조
-----------

```java
Function<String, Integer> f = (String s) -> Integer.parseInt(s);
Function<String, Integer> f = Integer::parseInt;
Function<Integer, MyClass> f2 = (i) -> new MyClass(i);
Function<Integer, MyClass> f2 = MyClass:new;
Function<Integer, int[]> f = x -> new int[x];
Function<Integer, int[]> f = int[]::new;
```

-	람다식은 하나의 메서드만 호출하는 경우 메서드 참조라는 방법으로 람다식을 간략히 표현할 수 있다.<br><br>
-	\`클래스이름::메서드이름\` 또는 \`참조변수::메서드이름\`으로 바꾼다.<br><br>
-	static 메서드나 인스턴스 메서드를 참조할 때는 클래스 이름을 적지만, 이미 생성된 객체의 메서드를 사용하는 경우 참조 변수를 적는다.<br><br>
-	생성자의 메서드도 참조할 수 있으며, 매개변수가 있는 생성자는 알맞게 함수형 인터페이스를 사용하면 된다.<br><br>

Stream
------

-	반복문을 통해 데이터를 다루는 기존의 방식은 코드가 복잡해지며, 재사용성도 떨어진다.<br><br>
-	또한 데이터 소스마다 다른 방식으로 다뤄야하며, 컬렉션 프레임웍은 같은 기능의 메서드들이 중복해서 정의되어 있다.<br><br>
-	스트림은 데이터 소스를 추상화하고, 데이터를 다루는데 자주 사용되는 메서드들을 정의해 놓았다.<br><br>
-	데이터 소스의 추상화란 데이터 소스가 무엇이던 간에 같은 방식으로 다룰 수 있게 되어 코드 재사용성이 높아짐을 의미한다.<br><br>
-	스트림은 데이터 소스로부터 데이터를 읽기만 할 뿐, 데이터 소스를 변경하지 않는다.<br><br>
-	스트림은 Iterator처럼 일회용이라 한 번 사용하면 닫혀서 다시 사용할 수 없다.<br><br>
-	스트림은 작업을 내부 반복으로 처리하며, 반복문을 메서드의 내부에 숨길 수 있다.<br><br>

스트림의 연산
-------------

-	스트림이 제공하는 연산은 중간 연산과 최종 연산으로 분류되며, 중간 연산은 연산 결과를 스트림으로 반환하기 때문에 연속해서 중간 연산을 연결할 수 있다.<br><br>
-	그러나 최종 연산은 스트림의 요소를 소모하면서 연산을 수행하므로 단 한번만 연산이 가능하다.<br><br>
-	스트림의 연산은 최종 연산이 수행되기 전까지는 중간 연산이 수행되지 않는 지연된 연산이다.<br><br>
-	최종 연산이 수행되어야 비로소 스트림의 요소들이 중간 연산을 거쳐 최종 연산에서 소모된다.<br><br>

병렬 스트림
-----------

-	병렬 스트림은 내부적으로 fork & join 프레임웍을 사용하여 연산을 병렬로 수행한다.<br><br>
-	스트림에 `parallel()` 을 호출하면 된다.<br><br>

Stream<Integer> & IntStream
---------------------------

-	요소의 타입이 T인 스트림은 기본적으로 Stream<T>이지만, 오토박싱과 언박싱으로 인한 비효율을 줄이기 위해 데이터 소스의 요소를 기본형으로 다루는 스트림들이 제공된다.<br><br>

스트림 생성
-----------

```java
Stream<T> Collection.stream()
Stream<String> str = Stream.of("a", "b", "c"); //가변 인자
Stream<String> str = Arrays.stream(new String[]{"a", "b", "c"});
IntStream Arrays.stream(int[])
```

-	기본형 배열을 소스로 하는 스트림을 생성하는 메서드도 있다.<br><br>

특정 범위의 정수
----------------

```Java
IntStream IntStream.range(int begin, int end)
IntStream IntStream.rangeClosed(int begin, int end)
```

-	전자는 경계의 끝 값이 범위에 포함되지 않고, 후자는 포함된다.<br><br>

임의의 수
---------

```java
IntStream ints()
LongStream longs()
DobuleStream doubles()
```

-	스트림의 크기가 정해지지 않은 무한 스트림이기 때문에 `limit()` 를 걸어야 한다.<br><br>
-	매개변수로 스트림의 크기를 지정해주면 유한 스트림을 생성해서 반환하기 때문에 `limit()` 를 사용하지 않아도 된다.<br><br>

람다식 - iterate(), generate()
------------------------------

```java
static <T> Stream<T> iterate(T seed, UnaryOperator<T> f)
static <T> Stream<T> generate(Supplier<T> s)
```

-	람다식을 매개변수로 받아서 계산되는 값들을 요소로 하는 무한 스트림을 생성한다.<br><br>
-	전자는 이전 결과를 이용해서 다음 요소를 계산하지만 후자는 이전 결과를 사용하지 않는다.<br><br>

빈 스트림
---------

```java
Stream emptyStream = Stream.empty();
long count = emptyStream.count(); //0, 스트림 요소의 개수를 반환
```

-	요소가 하나도 없는 비어있는 스트림도 생성이 가능하며, null보다 빈 스트림을 반환하는 것이 낫다.<br><br>

두 스트림의 연결
----------------

```java
Stream<String> str3 = Stream.concat(strs1, strs2);
```

-	연결하는 스트림의 요소는 같은 타입이어야 한다.<br><br>

스트림의 중간 연산
------------------

```java
Stream<T> skip(long n) //요소를 건너뛴다
Stream<T> limit(long maxSize) //스트림의 요소를 제한한다
Stream<T> filter(Predicate<? super T> predicate) //주어진 조건에 맞지 않는 요소를 걸러낸다
Stream<T> distinct() //중복된 요소들을 제거한다
Stream<T> sorted() //기본 정렬 기준으로 스트림 정렬
Stream<T> sorted(Comparator<? super T> comparator) //스트림 정렬
Stream<T> map(Function<? super T, ? extends R> mapper) //스트림 요소 중 원하는 필드만 뽑거나 특정 형태로 변환할 때 사용
Stream<T> peek() //스트림의 요소를 소모하지 않고 조회

MapToInt(), MapToLong(), MapToDouble()
int sum()
OptionalDouble average()
OptionalInt max()
OptionalInt min()
IntSummaryStatistics stat = scoreStream.summaryStatistics();
stat.getCount();
stat.getAverage();

Stream<U> mapToObj(IntFunction<? extends U> mapper) //IntStream을 Stream<T>로 변환시 사용
Stream<Integer> boxed() //IntStream을 Stream<Integerr>로 변환시 사용
```

-	스트림의 요소가 Comparable을 구현한 클래스가 아니면 정렬 시 예외가 발생한다.<br><br>
-	정렬에 사용되는 메서드는 Java API에 정리되어 있다.<br><br>
-	`peek()` 은 연산과 연산 사이에 올바르게 처리되었는지 확인하기 위해 사용하며, 여러 번 사용해도 문제가 되지 않는다.<br><br>
-	`MapTo정수형()` 메서드는 Stream<T> 타입의 스트림을 IntStream과 같은 기본형 스트림으로 변환하는 용도이다.<br><br>
-	스트림의 요소를 숫자로 변환하는 경우 기본형 스트림이 숫자를 다루는데 편리한 메서드를 제공하기 때문이다.<br><br>
-	`sum()` 과 같이 기본형 스트림이 제공하는 메서드는 최종 연산이다.<br><br>
	-	스트림을 또 생성하는 번거로움이 있어서, `summaryStatistics()` 를 통해 다양한 통계 및 계산 메서드를 사용한다.<br><br>

flatMap()
---------

```java
Stream<Stream<String>> strStrStrm = strArrStrm.map(Arrays::stream);
Stream<String> strStrStrm = strArrStrm.flatMap(Arrays::stream);

```

-	스트림의 요소가 배열이거나 `map()` 의 연산 결과가 배열인 경우, 스트림의 타입은 Stream<T[]> 이다.<br><br>
-	이 경우 Stream<T>로 다루는 것이 편하기 때문에, `flatMap()` 을 사용한다.<br><br>
-	단순히 `map()` 을 사용하면 스트림의 스트림이 반환되기 때문에 스트림의 요소가 배열이면 `flatMap()` 을 사용한다.<br><br>

Optional<T> & OptionalInt
-------------------------

```java
public final class Optional<T>{
  private final T value; //T 타입의 참조 변수
}
```

-	Optional<T>는 지네릭 클래스로, T 타입의 객체를 감싸는 래퍼 클래스이다.<br><br>
-	Optional 타입의 객체에는 모든 타입의 참조 변수를 담을 수 있다.<br><br>
-	최종 연산 결과를 Optional 객체에 담아 반환하는 최종 연산들이 많은데, 반환 결과가 null인지 매번 if문으로 체크하는 대신 Optional에 정의된 메서드를 통해 간단히 처리할 수 있다.<br><br>
-	따라서 간결하게 안전한 코드를 작성하는데 도움을 준다.<br><br>

Optional 객체
-------------

```java
Optional<String> optVal = Optional.of("abc"); //객체 생성, null이면 예외 발생
Optional<String> optVal = Optional.ofNullable("abc"); //객체 생성, null 예외 자동 처리
optVal = Optional.<String>empty(); //기본값으로 초기화
String tmp = optVal.get(); //optVal에 저장된 값을 반환, null이면 예외 발생
tmp = optVal.orElse(""); //optVal에 저장된 값이 null이면 ""를 반환

T orElseGet(Supplier<? extends T> other) //null 대체로 반환하는  람다식 지정
T orElseThrow(Supplier<? extends X> exceptionSupplier) //null일 때 지정된 예외 발생
```

<br>

OptionalInt & OptionalLong & OptionalDouble
-------------------------------------------

```java
Optional<T> T get()
OptionalInt int getAsInt()
OptionalLong long getAsLong()
OptionalDouble double  getAsDoubel()
```

-	IntStream과 같은 기본형 스트림에는 Optional도 기본형을 값으로 하는 타입을 반환한다.<br><br>

스트림의 최종 연산
------------------

-	최종 연산의 결과는 스트림 요소의 합과 같은 단일 값이거나, 스트림의 요소가 담긴 배열 또는 컬렉션일 수 있다.<br><br>
-	스트림은 최종 연산 후에 닫힌다.<br><br>

조건 검사
---------

```java
boolean allMatch(Predicate<? super T> predicate)
boolean anyMatch(Predicate<? super T> predicate)
boolean noneMatch(Predicate<? super T> predicate)
Optional<T> findAny() //조건에 일치하는 첫 번째만 반환
Optional<T> findFirst() //병렬 스트림인 경우에 사용
```

-	스트림의 요소에 대해 지정된 조건에 모든 요소가 일치하는 지, 일부가 일치하는지 등을 확인하는데 사용하는 메서드들이다.<br><br>

통계
----

```Java
long count()
Optional<T> max(Comparator<? super T> comparator)
Optional<T> min(Comparator<? super T> comparator)
```

-	대부분의 경우 위의 메서드를 사용하기보다 기본형 스트림으로 변환하거나 `reduce()` 및 `collect()` 를 통해 통계 정보를 얻는다.<br><br>

reduce()
--------

```java
Optional<T> reduce(BinaryOperator<T> accumulator)
```

-	스트림의 요소를 줄여나가면서 연산을 수행하고 최종 결과를 반환한다.<br><br>
-	초기값을 갖을 수 있다.<br><br>

collect()
---------

-	`collect()` 가 스트림 요소를 수집하려면, 어떻게 수집할 것인가에 대한 방법이 정의되어 있어야 하는데 컬렉터(collector)가 이 방법을 정의한다.<br><br>
-	`collect()` 는 스트림의 최종 연산이며, 매개변수로 컬렉터를 요구한다.<br><br>
-	컬렉터는 Collector 인터페이스를 구현한 것이다.<br><br>
-	Collectors 클래스는 static 메서드로 미리 작성된 컬렉터를 제공한다.<br><br>

스트림을 컬렉션 및 배열로 변환
------------------------------

```java
toList()
toSet()
toMap()
toCollection()
toArray()

List<String> names = stream.collect(Collections.toList());
ArrayList<String> list = names.stream().collect(Collectors.toCollection(ArrayList::neew));
Map<String, Person> map = stream.collect(Collectors.toMap(p->p.getRegId(), p->p));
Student[] stuNames = stream.toArray(Student[]::new);
```

-	스트림의 모든 요소를 컬렉션에 수집하려면, Collectors 클래스의 메서드를 사용한다.<br><br>
-	List나 Set이 아닌 특정 컬렉션을 지정하려면, `toCollection()` 메서드에 해당 컬렉션의 생성자 참조를 매개변수로 넣어준다.<br><br>
-	Map은 키와 값을 지정해야 한다.<br><br>
-	`toArray()` 메서드는 특정 T[] 타입으로 변환하기 위해 생성자 참조를 매개변수로 지정해야 하며, 기본 반환 타입은 Object[] 타입이다.<br><br>

통계
----

```java
counting()
summingInt()
averagingInt()
maxBy()
minBy()
```

-	`groupingBy()` 와 함께 사용하면 편하다.<br><br>

reducing()
----------

```java
Collector reducing(BinaryOperator<T> op)
Collector reducing(T identity, BinaryOperator<T> op)
Collector reducing(U identity, Function<T,U> mapper, BinaryOperator<T> op)
```

-	`collect` 메서드로 리듀싱이 가능하며, IntStream은 매개변수 3개짜리 `collect` 만 정의되어 있기 때문에 `boxed()` 를 통해 Stream<Integer>로 변환해야 매개변수가 1개인 `collect()` 를 사용할 수 있다.<br><br>

문자열 결합
-----------

```java
joining()
```

-	문자열 스트림의 모든 요소를 하나의 문자열로 연결해서 반환한다.<br><br>
-	구분자를 지정해줄 수 있으며, 접두사와 접미사도 지정 가능하다.<br><br>
-	스트림의 요소가 문자열이 아니면 `map()` 으로 변환해야 한다.<br><br>
-	`map()` 없이 스트림에 문자열 결합을 시도하면 스트림의 요소에 `toString()` 을 호출한 결과를 결합한다.<br><br>

그룹화와 분할
-------------

```java
Collector groupingBy(Function classifier)
Collector groupingBy(Function classifier, Collector downstream)
Collector groupingBy(Function classifier, Supplier mapFactory, Collector downstream)
Collector partitioningBy(Predicate predicate)
Collector partitioningBy(Predicate predicate, Collector downstream)
```

-	그룹화는 스트림의 요소를 특정 기준으로 그룹화하는 것이며, 분할은 스트림의 요소를 지정된 조건에 일치하는 그룹과 일치하지 않는 그룹으로의 분할을 뜻한다.<br><br>
-	두 개의 그룹으로 나눠야 한다면 파티션이 더 빠르며, 그룹화와 분할의 결과는 Map에 담겨 반환된다.<br><br>
-	`collect()` 메서드 내부에서 통계 관련 메서드와 함께 사용된다.<br><br>

Collector 구현하기
------------------

```java
public Interface Collector<T, A, R>{
  Supplier<A> supplier(); //작업 결과를 저장할 공간 제공
  BiConsumer<A, T> accumulator(); //스트림의 요소를 수집할 방법을 제공
  BinaryOperator<A> combiner(); //두 저장 공간을 병합할 방법 제공(병렬 스트림)
  Function<A, R> finisher(); //결과를 최종적으로 변환할 방법을 제공
  Set<Characteristics> characteristics(); //컬렉터의 특성이 담긴 Set 제공
}
```

-	작업 결과 변환이 필요없다면, 항등 함수인 `Function.identity()` 를 반환하면 된다.<br><br>
-	`characteristics()` 메서드는 CONCURRENT, UNORDERED, IDENTITY_FINISH 등 3가지 속성 중에서 해당하는 것을 Set에 담아 반환하도록 구현하면 된다.<br><br>
-	속성 지정이 필요 없으면 `emptySet()` 을 리턴한다.<br><br>

To-Do
-----

-	Lambda Expression을 통한 Stream 사용 방법 지속적으로 정리하기.<br><br>
-	Reference 사이트를 통해 Lambda & Stream과 친해지기.<br><br>

---

Reference
---------

-	Java의 정석 (남궁성 저) Chapter 14
-	[Java Stream 총 정리](https://futurecreator.github.io/2018/08/26/java-8-streams/)
-	[Java Stream 고급](https://futurecreator.github.io/2018/08/26/java-8-streams-advanced/)
-	[Java 8 Lambda & Stream](https://brunch.co.kr/@springboot/276)
