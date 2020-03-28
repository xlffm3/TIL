Generics
--------

-	지네릭스란 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시 타입 체크를 해주는 기능이다.<br><br>
-	타입 안정성을 높이고 형 변환을 생략하여 코드가 간결해진다.<br><br>

지네릭 클레스 선언
------------------

```java
class Box<T>{
  T item;
  T[] itemArr; //ok
  T[] tmpArr = new T[itemArr.length]; //에러
  static T item; //에러
  static int compare(T t1, T t2){..} //에러
  void setItem(T item){
    this.item = item;
  }
}

Box<Apple> appleBox = new Box<>(); //ok
Box<Grape> grapeBox = new Box<>();  //ok
```

-	클래스 옆에 <T>를 붙이고, 실제 객체를 생성할 때에는 타입 T 대신에 사용될 타입을 지정해주면 된다.<br><br>
-	Box는 원시 타입이고, T는 타입 변수 혹은 타입 매개 변수, Box<T>는 지네릭 클래스라고 부른다.<br><br>
-	객체별로 다른 타입을 지정하는 것은 가능하지만, static 멤버에 타입 변수 T를 사용할 수 없다.<br><br>
-	T는 인스턴스 변수로 간주되기 때문이다.<br><br>
-	static 멤버는 타입 변수에 지정된 타입, 즉 대입된 타입의 종류에 관계없이 동일한 것이어야 한다.<br><br>
-	따라서 `Box<Apple>.item` 과 `Box<Grape>.item` 이 다른 것이어서는 안 된다는 뜻이다.<br><br>
-	또한 지네릭 배열 타입의 참조 변수를 선언하는 것은 가능하지만, 지네릭 타입의 배열을 생성하는 것은 불가능하다.<br><br>
-	new 연산자는 컴파일 시점에 타입 T가 뭔지 정확하게 알아야 하지만, 지네릭스 T가 어떤 타입이 될지 전혀 알 수 없기 때문이다.<br><br>
-	지네릭 배열은 Reflection API의 메서드나, Object 배열을 생성하여 복사한 뒤 T[]로 형 변환하는 방법 등을 통해 생성한다.<br><br>

지네릭 클래스의 객체 사용
-------------------------

```java
Box<Fruit> box = new Box<>();
box.add(new Fruit()); //ok
box.add(new Apple()); //ok
```

-	타입 T가 Fruit인 경우, `void add(Fruit item)` 이 되어 Fruit의 자손들이 메서드의 매개 변수가 될 수 있다.<br><br>

제한된 지네릭 클래스
--------------------

```java
interface Eatable{...}
class FruitBox<T extends Fruit>{...} //ok
class FruitBox<T extends Eatable> //ok
class FruitBox<T extends Fruit & Eatable> //ok
FruitBox<apple> box = new FruitBox<>(); //ok
FruitBox<Toy> box2 = new FruitBox<>(); //에러
```

-	지네릭스를 통해 한 종류의 타입만 저장하도록 제한할 수 있지만, 원하지 않는 타입들도 지정이 가능하다는 문제가 있다.<br><br>
-	`extends` 키워드를 지네릭 타입에 사용하여 특정 타입의 자손들만 대입하도록 제한할 수 있다.<br><br>
-	인터페이스 구현 제약은 `implements` 를 사용하지 않고 `extends` 를 사용한다.<br><br>
-	여러 개를 동시에 구현해야 한다면 & 기호를 이용한다.<br><br>

와일드 카드
-----------

```java
class Jucier{
  static Juice makeJuice(FruitBox<Fruit> box){...}
}

Jucier.makeJuice(new FruitBox<Fruit>()); //ok
Jucier.makeJuice(new FruitBox<Apple>()); //에러

class Jucier{
  static Juice makeJuice(FruitBox<? extends Fruit> box){...}
}

<? extends T> //T와 그 자손들만 가능, 와일드 카드의 상한 제한
<? super T> //T와 그 조상들만 가능, 와일드 카드의 하한 제한
<?> //제한 없음, <? extends Object> 와 동일
//와일드 카드에서는 & 기호 사용이 불가능

static <T> void sort(List<T> list, Comparator<? super T> c)
```

-	static 메서드에는 타입 매개변수 T를 사용할 수 없기 때문에 지네릭스를 적용하지 않던가, 특정 타입을 지정해야 한다.<br><br>
-	`FruitBox<Apple>` 타입의 객체는 매개변수가 되지 못해 에러가 발생하여, 여러 가지 타입의 매개 변수를 갖는 `makeJuice()` 메서드를 오버로딩 해야 한다.<br><br>
-	그러나 지네릭 타입이 다른 것 만으로는 오버로딩이 성립되지 않아 컴파일 에러가 발생한다.<br><br>
-	따라서 와일드 카드를 통해 상한과 하한을 제한하여 다양한 타입의 매개변수의 사용을 가능하게 한다.<br><br>
-	매개변수 타입을 <? extends Object>로 하면, box의 요소가 Fruit의 자손이라는 보장이 없기 때문에 메서드 내부에서 문제가 발생할 수 있다.<br><br>
-	Comparator를 이용한 정렬 작업에서 와일드 카드를 사용하여 자식들이 모두 같은 기준으로 정렬시킬 수 있다.<br><br>

지네릭 메서드
-------------

```java
class FruitBox<T>{
  static <T> void sort(List<T> list, Comparator<? super T> c){}
}

static Juice makeJuice(FruitBox<? extends Fruit> box){...}
static <T extends Fruit> Juice makeJuice(FruitBox<T> box){...}

Jucier.<Fruit>makeJuice(fruitBox); //ok
Jucier.makeJuice(fruitBox); //ok
```

-	메서드의 선언부에 지네릭 타입이 선언된 메서드를 지네릭 메서드라고 한다.<br><br>
-	`Collections.sort()` 는 대표적인 지네릭 메서드이며, 지네릭 클래스에 정의된 타입 매개변수와 지네릭 메서드에 정의된 타입 매개변수는 별개이다.<br><br>
-	지네릭 메서드는 지네릭 클래스가 아닌 클래스에 정의될 수 있다.<br><br>
-	static 메서드에 지네릭 타입을 선언하고 사용하는 것은 가능하다.<br><br>
-	메서드를 호출 할 때 예제처럼 타입 변수에 타입을 대입해야 하지만, 대부분의 경우 컴파일러가 타입을 추정할 수 있기 때문에 생략할 수 있다.<br><br>
-	지네릭 메서드를 호출할 때 대입된 타입의 생략이 불가능한 경우, 참조 변수나 클래스 이름을 생략할 수 없다.<br><br>

지네릭 타입의 형 변환
---------------------

```java
Box box;
Box<Object> objBox;
Box<String> strBox;
box = (Box) objBox; //ok
objBox = (Box<Object>) box; //ok
objBox = (Box<Object>) strBox; //에러
Box<? extends Object> wBox = new Box<String>(); //ok
FruitBox<? extends Fruit> box;
FruitBox<Apple> appleBox = (FruitBox<Apple>) box; //ok, 경고 발생
```

-	지네릭 타입과 원시 타입간의 형 변환은 항상 가능하지만 경고가 발생한다.<br><br>
-	그러나 대입된 타입이 다른 지네릭 타입 간에는 형 변환이 불가능하다.<br><br>
-	`extends` 키워드를 통해 형 변환이 가능하며, 이는 다형성의 원리를 사용했다.<br><br>
-	appleBox 처럼 반대의 경우도 형 변환이 가능하지만, 미확인 타입으로 경고가 발생한다.<br><br>

지네릭 타입의 제거
------------------

-	컴파일러는 지네릭 타입을 이용해 소스 파일을 체크하고, 필요한 곳에 형 변환을 넣어준다.<br><br>
-	이후 지네릭 타입을 제거하여, 컴파일 된 \*.class 파일에는 지네릭 타입에 대한 정보가 없다.<br><br>

Enums
-----

```java
class Card{
  enum Kind {CLOVER, HEART, DIAMOND, SPADE}
  enum Value {TWO, THREE, FOUR}

  final Kind kind;
  final Value value;
}

if (Card.Kind.CLOVER == Card.Value.TWO) //컴파일 에러
```

-	열거형은 서로 관련된 상수를 편리하게 선언하기 위한 것으로, 여러 상수를 정의할 때 유용하다.<br><br>
-	C 언어는 타입이 달라도 값이 같으면 조건식 결과가 참이었으나, 자바는 타입에 안전한 열거형이라서 실제 값이 같아도 타입이 다르면 컴파일 에러가 발생한다.<br><br>
-	상수의 값이 바뀌면 해당 삼수를 참조하는 모든 소스를 다시 컴파일 해야 하지만, 열거형 상수는 기존의 소스를 다시 컴파일 하지 않는다.<br><br>

열거형의 정의와 사용
--------------------

```java
enum Direction{EAST, SOUTH, WEST, NORTH}

class Unit{
  int x, y;
  Direction dir;

  void init(){
    dir = Direction.EAST;
  }
}
```

-	열거형 상수간의 비교에는 == 연산자를 사용할 수 있어 성능이 빠르다.<br><br>
-	그러나 <나 >와 같은 비교 연산자는 사용이 불가능하며, `compareTo()` 같은 메서드는 사용이 가능하다.<br><br>
-	조건식에도 열거형을 사용할 수 있으며, 열거형의 이름은 적지 않고 상수의 이름만 적는다.<br><br>

java.lang.Enum
--------------

```java
for(Direction d : Direction.values()){ //열거형의 모든 상수를 배열화
  System.out.println(d.name() + d.ordinal()); //열거형 상수의 이름과 정의 순서
}
```

-	`Class<E> getDeclaringClass()` 메서드는 열거형의 Class 객체를 반환한다.<br><br>
-	`T valueOf(Class<T> enumType, String name)` 메서드는 지정된 열거형에서 name과 일치하는 열거형 상수를 반환한다.<br><br>

열거형에 멤버 추가하기
----------------------

```java
enum Direction{
  EAST(1), SOUTH(5), WEST(-1), NORTH(10); //세미 콜론 추가
  private Final int value; //정수를 저장할 필드
  Direction(int value){
    this.value = value;
  }
  public int getValue(){
    return value;
  }  
}
```

-	`ordinal()` 메서드는 열거형 상수가 정의된 순서를 반환하지만, 이를 열거형 상수의 값으로 사용하는 것은 지양한다.<br><br>
-	해당 메서드는 내부적인 용도로만 사용되기 때문이다.<br><br>
-	값이 불연속적인 경우 괄호를 통해 값을 명시하며, 지정된 값을 저장할 수 있는 인스턴스 변수와 생성자를 추가해 주어야 한다.<br><br>
-	열거형의 생성자는 제어자가 묵시적으로 private이기 때문에 외부에서 호출이 불가능하다.<br><br>
-	하나의 열거형 상수에 여러 값을 지정할 수 있으나, 알맞게 인스턴스 변수와 생성자 등을 추가해주어야 한다.<br><br>

열거형에 추상 메서드 추가하기
-----------------------------

```java
enum Transportation{
  BUS(100){int fare(int distance){return distance*BASIC_FARE;}},
  TRAIN(200){int fare(int distance){return distance*BASIC_FARE;}},
  SHIP(300){int fare(int distance){return distance*BASIC_FARE;}},
  AIRPLANE(400){int fare(int distance){return distance*BASIC_FARE;}};

  abstract int fare(int disatnce);
  protected final int BASIC_FARE; //protected로 해야 상수 접근 가능
}
```

-	열거형에 추상 메서드를 선언할 수 있다.<br><br>

열거형의 이해
-------------

```java
abstract class MyEnum<T extends MyEnum<T>> implements Comparable<T>{
  int ordinal;

  public int ordinal(){
    return ordinal;
  }

  public int compareTo(T t){
    return ordinal - t.ordinal;
  }
}

abstract class Direction extends MyEnum{
  static final Direction EAST = new Direction("EAST"){
    Point move(Point p)
    ... //익명 클래스 형식
  };

  abstract Point move(Point p);
}
```

-	열거형 Direction이 정의되어 있다면, 열거형 상수 하나하나가 Direction 객체이다.<br><br>
-	static 상수는 객체의 주소이며, 이 값은 바뀌지 않는 값이므로 == 비교가 가능하다.<br><br>
-	타입을 T로만 선언한다면 타입 T에 `ordinal()` 메서드가 정의되어 있는지 확인할 수 없기 때문에, 예제처럼 명시하여 해당 메서드가 분명히 정의되어 있음을 밝힌다.<br><br>
-	추상 메서드를 새로 추가하면, 클래스 앞에도 abstract 키워드를 붙이고 각 static 상수들도 추상 메서드를 구현해야 한다.<br><br>

Annotation
----------

-	애너테이션이란 프로그램의 소스 코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것이다.<br><br>
-	애너테이션은 주석처럼 프로그래밍 언어에 영향을 미치지 않으며, 유용한 정보를 제공한다.<br><br>
-	표준 애너테이션은 주로 컴파일러를 위한 것으로 컴파일러에게 유용한 정보를 제공한다.<br><br>

애너테이션 타입 정의
--------------------

```java
@interface 애너테이션이름{
  타입 요소이름();
  int count();
  String[] testTools();
  TestType testType(); //enum TestType
  DateTime testDate(); //자신이 아닌 다른 애너테이션을 포함
  ...
}

@interface DateTime{
  ...
}
```

-	애너테이션의 요소는 반환 값이 있고 매개변수는 없는 추상 메서드의 형태를 가지며, 상속을 통해 구현하지 않아도 된다.<br><br>
-	요소의 타입은 기본형, String, enum, 애너테이션, Class만 허용된다.<br><br>
-	() 안에 매개변수를 선언할 수 없다.<br><br>
-	예외를 선언할 수 없다.<br><br>
-	요소를 타입 매개변수로 정의할 수 없다.<br><br>

---

Reference
---------

-	Java의 정석 (남궁성 저) Chapter 12
