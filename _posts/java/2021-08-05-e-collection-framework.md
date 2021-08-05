---
layout: post
title: "05.컬렉션 프레임워크"
author: "SangKyenog Lee"
tags: Java
---

< Java의 정석 >을 공부하고 정리 했습니다

## 컬렉션 프레임웍(collections framework)
> 데이터군을 저장하는 클래스들을 표준화한 설계

### 핵심 인터페이스

List
> 순서가 있는 데이터의 집합, 데이터의 중복 허용
- ArrayList, LinkedList, Stack, Vector

Set
> 순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다.
- HashSet, TreeSet

Map
> 키와 값의 쌍으로 이루어진 데이터의 집합
> 순서는 유지되지 않으며, 키는 중복을 허용하지 않고, 값은 중복을 허용한다.
- HashMap, TreeMap, Hashtable, Properties

#### Collection 인터페이스

메소드

1. boolean add(Object o), addAll(Collection c)
> 지정된 객체 또는 Collection 객체들을 Collection에 추가
2. void clear()
> Collection의 모든 객체를 삭제한다.
3. boolean contains(Object o), containsAll(Collection c)
> 지정된 객체 또는 Collection의 객체들의 포함되어 있는지 확인한다.
4. boolean equals(Object o)
> 동일한 Collection인지 비교한다.
5. int hashCode()
> hashcode 반환
6. boolean isEmpty()
> Collection이 비어있는지 확인한다.
7. Iterator iterator()
> Collection의 Iterator를 얻어서 반환한다.
8. boolean remove(Object o)
> 지정된 객체 삭제
9. int size()
> Collection에 저장된 객체의 개수를 반환한다.
10. Object[] toArray()
> Collectio에 저장된 객체를 객체배열로 반환한다.

#### Map 인터페이스
Key, Value를 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는데 사용한다. 키는 중복이 불가능하고 값은 중복이 허용되며, 만약 중복된 키로 값이 저장된다면 기존의 값이 사라지고 새로운 값이 저장된다.
##### 주요 메서드
Set keySet(): Map에 저장된 모든 key객체를 반환한다.
Collection values(): Map에 저장된 모든 value객체를 반환한다.
Object get(Object key): key객체에 대응하는 value객체 반환
Object put(Object key, Object value): Map에 value객체를 key객체에 연결하여 저장한다.

Key객체는 중복을 허용하지 않기때문에 Set형식, value는 중복을 허용하기 때문에 Collection 형식으로 반환한다.

### ArrayList
- 배열이라고 보면 됨

장점
- 데이터를 가지고 오는데 가장 빠르다

단점
- 크기를 변경할 수 없다.
- 비순차적인 데이터의 추가 또는 삭제 시간이 많이 걸린다.

### LinkedList
- 배열의 단점을 보완하기 위해서 LinkedList가 나옴
    - ArrayList와 쓰임새 구별 (접근 속도는 ArrayList가 빠름)

클래스는 다음과 같이 노드로 구성된다
```java
class Node{
    Node next; // 다음 요소 저장
    Object obj; // 데이터를 저장
}
```

위와 같은 단방향 연결리스트는 이전 요소에 대한 접근이 어렵다
이 점을 보완하기 위해 더블 링크드 리스트가 나왔다.

```java
class Node{
    Node next;
    Node previous; // 이전 요소의 주소를 저장
    Object obj;
}
```

결론
1. 순차적으로 추가/삭제하는 경우에는 ArrayList보가 LinkedList보다 빠르다
2. 중간 데이터를 추가/삭제하는 경우에는 LinkedList가 ArrayList보다 빠르다.

### Stack and Queue

stack : push and pop (Last In First Out)
- ArrayList가 적합

stack 메소드
1. boolean empty()
2. Object peek()
3. Object pop()
4. Object push(Object item)
5. int search(Object o)

stack 활용 예
1. 수식계산
2. 괄호검사
3. undo/redo
4. 웹브라우저 뒤로/앞으로

queue : offer and poll (Firts In First Out)
- LinkedList가 적합 (데이터를 꺼낼때마다 빈공간을 채우기 위해 복사가 일어나므로)

queue 메소드
1. boolean add(Object o)
2. Object remove()
3. Object element()
4. Object offer(Object o)
5. Object poll()
6. Object peek()

queue 활용 예
1. 최근사용문서
2. 인쇄작업 대기목록
3. 버퍼

### priorityQueue
> Queue인터페이스의 구현체 중의 하나로 저장한 순서에 관계없이 우선순위가 높은것부터 꺼낸다.
- 저장공간으로 배열을 사용하며, 각 요소를 힙 자료구조 형태로 저장한다.

```java
Queue pq = new PriorityQueue(); // 선언
```

### Deque
> Queue는 한 쪽 끝에서만 추가/삭제가 가능하지만 Deque는 양쪽 끝에서 모두 추가/삭제 가능

덱은 Stack/queue 두가지 용도로 모두 사용가능하다.

|Deque|Queue|Stack|
|--|--|--|
|offerLast()|offer()|push()|
|pollLast()|x|pop()|
|pollFirst()|poll()|x|
|peekFirst()|peek()|x|
|peekLast()|x|peek()|

### Iterator, ListIterator, Enumeration
Enumeration은 구버전이고 ListIterator는 Iterator의 성능을 향상 시킨 것

### Iterator
Iterator인터페이스는 컬렉션에 저장된 각 요소에 접근하는 기능을 가진다.

|메서드|설명|
|--|--|
|boolean hasNext()|읽어 올 요소가 남아있는지 확인|
|Object next()|다음 요소를 읽어온다.|
|void remove()|next()로 읽어온 요소 삭제|

```java
List list = new ArrayList();
Iterator it = list.iterator;

while(it.hasNext()) {
	System.out.println(it.next());
}
```

### ListIterator
ListIterator는 Iterator에 양방향 조회기능을 추가한 것이며, ArrayList, LinkedList 같은 List인터페이스를 구현한 컬렉션에서만 사용 가능하다.

|메서드|설명|
|--|--|
|boolean hasMoreElements()|읽어 올 요소가 남아있는지 확인|
|Object nextElement()|다음 요소를 읽어온다.|
|void remove()|next()로 읽어온 요소 삭제|
|Object previous()|이전 요소를 읽어온다.|
|int previousIndex()|이전 요소의 index를 반환|

### Arrays
배열의 복사 - copyOf(), copyOfRange()
```java
int[] arr = {0, 1, 2, 3, 4};
int[] arr2 = Arrays.copyOf(arr, arr.length);
int[] arr3 = Arrays.copyOfRange(arr, 2, 4) // 2이상 4미만
```
배열 채우기 - fill(), setAll()
```java
int[] arr = new int[5];
Arrays.fill(arr, 9); // 9로 배열 채우기
```
setAll()은 함수형 인터페이스를 구현한 객체나 람다식을 매개변수로 지정해야 한다.
```java
Arrays.setAll(arr, () -> (int)(Math.random() * 5) + 1);
```
배열 정렬, 검색 - sort(), binarySearch()
```java
int[] arr = {3, 2, 0, 1, 4};
Arrays.sort(arr);
int idx = Arrays.binarySearch(arr, 2);
```

배열 List로 변환 - asList(Object... a)
```java
List list = Arrays.asList(new Integer[]{1, 2, 3, 4, 5}); // list = [1, 2, 3, 4, 5]
List list = Arrays.asList(1, 2, 3, 4, 5) // list = [1, 2, 3, 4, 5]
```

### Comparator, Comparable
Arrays.sort()도 Charater클래스의 Comparable의 구현에 의해 정렬된 것이다.
클래스에 Comparable이 구현되어 있다면 해당 클래스는 정렬이 가능한 것을 의미한다.

값이 비교대상보다 작으면 음수 같으면 0 크면 1을 반환한다.

Comparable은 기본적으로 오름차순으로 정렬되어 있고, 그 외에 내림차순이나 다른 기준에 의해서 정렬을 원한다면 Comparator를 사용해서 기준을 만들자.

String의 comparable은 기본적으로 사전순으로 정렬되도록 작성되어 있는데 역순으로 할 시에
```java
class Decending implements Comparator {
	public int compare(Object o1, Object o2) {
    	if (o1 instanceof Comparable && o2 instanceof Comparable) {
        	Comparable c1 = (Comparable)o1;
            Comparable c2 = (Comparable)o2;
            return c1.compareTo(c2) * -1; // 기본 정렬의 역방향
        }
        return -1
    }
}
```
로 할 수 있다. `compare 매개변수가 Object라서 Comparable로 형변환을 해야한다.`

### HashMap
Map에서 key, value를 묶어서 저장하는 방식에 해싱을 사용해서 많은 양의 데이터를 검색하는데 뛰어난 성능을 보인다.

|메소드|설명|
|--|--|
|boolean containsKey(Object key)|HashMap에 지정된 키가 포함되어 있는지 알려준다.|
|Set entrySet()|HashMap에 저장된 키와 값을 엔트리의 형태로 Set에 저장해서 반환|
|get(key)|가져오기|
|Set keySet()|모든 키가 저장된 Set 반환|
|put(key,value)|넣기|
|remove(key)|해당 키 객체 제거|
|Collection values()| 모든 값을 컬렉션의 형태로 변환|

### TreeMap
HashMap의 성능이 좋지만, 범위 검색이나 정렬이 필요한 경우 사용하자.

|메소드|설명|
|--|--|
|firstEntry()|가장 작은 키와 값의 쌍 반환|
|firstKey()|가장 작은 키 반환|
|lastEntry()|가장 큰 키와 값의 쌍 반환|
|lastKey()|가장 큰 키 반환|