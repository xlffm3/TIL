Collections Framework
---------------------

-	데이터 군을 저장하는 클래스들을 표준화한 설계를 의미한다.<br><br>
-	List는 순서가 있는 데이터의 집합이며, 중복을 허용한다.<br><br>
-	Set은 순서를 유지하지 않는 데이터의 집합이며, 중복을 허용하지 않는다.<br><br>
-	Map은 키와 값이 쌍으로 이루어진 데이터의 집합으로, 순서는 유지되지 않고 키는 중복을 허용하지 않으며 값은 중복을 허용한다.<br><br>

ArrayList vs LinkedList
-----------------------

-	배열은 구조가 간단하며 데이터를 읽는 접근 시간이 LinkedList보다 빠르다.<br><br>
-	그러나 크기를 변경할 수 없으며 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.<br><br>
-	LinkedList는 각 요소들이 자신과 연결된 다음 요소에 대한 참조 주소값과 데이터로 구성되어 있다.<br><br>
-	doubly-linked list 및 doubly circular linked list는 단방향적인 LinkedList의 방향과 순환성을 보완한 것이다.<br><br>
-	순차적으로 추가 및 삭제하는 경우 ArrayList가 성능이 좋지만, 중간에 데이터를 추가 및 삭제하는 경우 LinkedList가 성능이 좋다.<br><br>

PriorityQueue
-------------

-	우선순위가 높은 것부터 꺼내는 구조이며, 각 요소를 힙이라는 자료구조의 형태로 저장한다.<br><br>
-	힙은 이진 트리의 한 종류로서, 가장 큰 값이나 작은 값을 빠르게 찾을 수 있다.<br><br>

ListIterator
------------

-	Iterator에 양방향 조회 기능을 추가한 것으로, List를 구현한 경우만 사용이 가능하다.<br><br>

Arrays
------

-	Array를 다루는데 유용한 메서드들이 정의된 클래스이다.<br><Br>
-	`binarySearch()` 메서드는 배열에 저장된 요소를 검색할 때 사용하며, 인덱스 값을 반환한다.<br><br>
-	이진 검색의 특성상 속도가 빠르지만, 배열이 먼저 정렬되어 있어야 한다.<br><br>
-	`deepToString()` 및 `deepEquals()` 메서드를 통해 다차원 배열의 비교와 문자열 출력이 가능하다.<br><br>

Comparator & Comparable
-----------------------

```java
class Descending implements Comparator{
  public int compare(Object o1, Object o2){
    if (o1 instanceof Comparable && o2 instanceof Comparable){
      Comparable c1 = (Comparable) o1;
      Comparable c2 = (Comparable) o2;
      return c1.compareTo(c2) *) * -1;
    }
    return -1;
  }
}

Arrays.sort(strArr, new Descending());
```

-	Comparable은 기본 정렬 기준을 구현하는데 사용하며, Comparator는 기본 정렬 기준 외에 다른 기준으로 정렬하고자 할 때 사용한다.<br><br>
-	`String.CASE_INTENSITIVE_ORDER` Comparator는 대소문자 구분없이 정렬한다.<br><br>

TreeSet
-------

-	이진 검색 트리 형태로 데이터를 저장하는 자료 구조이며, 정렬이나 검색 및 범위 검색에 높은 성능을 발휘한다.<br><br>
-	저장 순서를 유지하지 않는다.<br><br>
-	모든 노드는 최대 두 개의 자식 노드를 가지며, 왼쪽 자식 노드의 값은 부모 노드의 값보다 작고 오른쪽 자식 노드는 반대로 크다.<br><br>
-	노드의 추가 및 삭제에 시간이 걸린다.<br><br>

Hash Function
-------------

-	해싱이란 해시 함수를 이용해서 데이터를 해시 테이블에 저장하고 검색하는 기법이다.<br><br>
-	데이터가 저장되어 있는 곳을 알려 주기 때문에 다량의 데이터 중에서도 원하는 데이터를 빠르게 찾을 수 있다.<br><br>
-	검색하고자 하는 값의 키로 해시 함수를 호출하면, 해시 함수의 계산 결과인 해시 코드가 반환된다.<br><br>
-	해시 코드로 해당 값이 저장되어 있는 링크드 리스트를 찾고, 검색한 키와 일치하는 데이터를 최종 추출한다.<br><br>
-	서랍과 비유했을 때, 하나의 서랍에 데이터가 많이 저장되어 있는 형태보다 많은 서랍에 하나의 데이터만 저장되어 있는 형태가 빠른 검색 결과를 얻을 수 있다.<br><br>
-	따라서 중복된 해시 코드의 반환을 최소화하기 위해, 새로운 클래스를 정의할 시 `hashCode()` 메서드를 적절하게 오버라이딩 해야 한다.<br><br>

TreeMap
-------

-	범위 검색이나 정렬이 필요한 경우를 제외하면 HashMap의 성능이 더 뛰어나다.<br><br>

Collections
-----------

```java
List syncList = Collections.synchronizedList(new ArrayList());
List checkedList = checkedList(syncList, String.class);
```

-	컬렉션과 관련된 메서드를 제공하며, Arrays 클래스의 메서드와 대부분 유사하다.<br><br>
-	멀티 쓰레드 프로그래밍에서는 하나의 객체를 여러 쓰레드가 동시에 접근할 수 있기 때문에 데이터의 일관성을 유지하기 위해 공유되는 객체에 동기화가 필요하다.<br><br>
-	Vector와 같은 구버젼의 클래스들은 자체 동기화 처리가 되어 있으나, 멀티 쓰레드 프로그래밍이 아닌 경우 불필요한 기능이 되어 성능 저하가 발생된다.<br><br>
-	이후의 자료 구조는 동기화를 자체 처리하지 않고 필요한 경우에만 `synchronized` 관련 메서드를 통해 동기화 처리가 가능하다.<br><br>
-	컬렉션에 저장된 데이터를 보호하기 위해 읽기 전용으로 만드는 경우, `unmodifiable` 관련 메서드를 사용한다.<br><br>
-	인스턴스의 개수를 제한하는 싱글톤 컬렉션은 `singleton` 으로 시작하는 메서드를 사용하며, 반환된 컬렉션은 변경할 수 없다.<br><br>
-	컬렉션에 모든 종류의 객체를 저장할 수 있으나, 지정된 종류의 객체만 저장하기 위해 `checked` 관련 메서드를 사용한다.<br><br>
-	지네릭스가 있기 때문에 `checked` 관련 메서드를 많이 사용하지 않는다.<br><br>

---

Reference
---------

-	Java의 정석 (남궁성 저) Chapter 11
