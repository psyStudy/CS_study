# 컬렉션 프레임워크(List, Set, Queue, Map)

## 컬렉션 프레임워크란

- 프레임워크란 개발자들이 쓸 수 있도록 잘 정의된 클래스들의 모임으로 컬렉션 관련된 클래스의 정의에 적용되는 **설계 원칙 또는 구조가 존재**하기 때문에 컬렉션 라이브러리가 아닌 컬렉션 프레임워크라고 함
- 컬렉션 프레임워크는 리스트, 스택, 정렬, 이진 탐색 등의 자료구조와 알고리즘을 제네릭 기반의 클래스와 메소드로 미리 구현해 놓은 결과물임

![collections_1.png](%E1%84%8F%E1%85%A5%E1%86%AF%E1%84%85%E1%85%A6%E1%86%A8%E1%84%89%E1%85%A7%E1%86%AB%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%8B%E1%85%AF%E1%84%8F%E1%85%B3(List,%20Set,%20Queue,%20Map)%20b4eca73ed6ef482aa8d10aecf990ff69/collections_1.png)

인스턴스를 저장하는 컬렉션 클래스들은 위의 인터페이스 중 하나를 구현하게 되어 있으며, 구현한 인터페이스에 따라서 컬렉션 클래스의 데이터 저장 방식이 결정됨 

- 즉, 컬렉션 클래스들을 기반으로 생성되는 컬렉션 인스턴스들은 인스턴스의 저장을 목적으로 함
- 컬렉션 관련 클래스들과 인터페이스들은 java.util 패키지로 대부분 묶여 있음

## List<E>를 구현하는 컬렉션 클래스

List<E> 인터페이스를 구현하는 컬렉션 클래스는 **ArrayList<E>**와 **LinkedList<E>**가 있음

둘은 인스턴스의 저장 순서를 유지하며, 동일한 인스턴스의 중복 저장을 허용한다는 점에서는 동일하지만, 인스턴스를 저장하는 방식에는 차이가 있음

```java
// 컬렉션 인스턴스 생성
// 코드에 유연성을 주기 위해 List=ArrayList 방식으로 선언 -> List=LinkedList로 교체 가능해짐
List<String> list = new ArrayList<>();  
// List<String> list = new LinkedList<>();로 선언해도 나머지 코드 변화 없음  

// 컬렉션 인스턴스에 문자열 인스턴스 저장 
list.add("Toy");
list.add("Box");

// 저장된 문자열 인스턴스의 참조 
// 출력 결과 -> Toy Box
for(int i = 0; i < list.size(); i++)
		System.out.print(list.get(i) + '\t');

// 첫 번째 인스턴스 삭제
list.remove(0);
```

### ArrayList<E> vs LinkedList<E>

![collections_2.png](%E1%84%8F%E1%85%A5%E1%86%AF%E1%84%85%E1%85%A6%E1%86%A8%E1%84%89%E1%85%A7%E1%86%AB%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%8B%E1%85%AF%E1%84%8F%E1%85%B3(List,%20Set,%20Queue,%20Map)%20b4eca73ed6ef482aa8d10aecf990ff69/collections_2.png)

1. **ArrayList<E>**
- 장점
    - 저장된 인스턴스의 참조가 빠름 (인덱스 값을 통해 원하는 위치에 바로 접근 가능)
- 단점
    - 저장 공간을 늘리는 과정에서 시간이 비교적 많이 소요됨
        - ArrayList<E> 인스턴스는 내부적으로 배열을 생성해서 인스턴스를 저장하는데, 필요하면 그 배열의 길이를 스스로 늘림 → 이때 한 번 생성된 배열은 길이를 늘릴 수 없으므로, 더 긴 배열로 교체됨
    - 인스턴스의 삭제 과정에서 많은 연산이 필요하기 때문에 느릴 수 있음
        - 배열 중간에 위치한 인스턴스를 삭제할 경우, 삭제한 위치를 비워 두지 않기 위해서 그 뒤에 저장되어 있는 인스턴스들을 한 칸씩 앞으로 이동하는 과정을 진행해야 하기 때문

1. **LinkedList<E>**
- 장점
    - 저장공간을 늘리는 과정이 간단함
    - 저장된 인스턴스의 삭제 과정이 단순함
        - 화물 열차의 중간 칸을 없앨 때는 해당 칸을 빼고서 그 칸의 앞과 뒤를 연결하면 됨
- 단점
    - 저장된 인스턴스의 참조 과정이 배열에 비해 복잡하기 때문에 느릴 수 있음
        - 연결 리스트는 중간에 위치한 열차 칸에 바로 접근이 안되기 때문에, 열차 중간 칸에 저장된 인스턴스를 참조하려면 열차 맨 앞 칸, 또는 맨 뒤 칸에서부터 한 칸씩 건너가는 구조임

### 반복자 Iterator

저장된 인스턴스들을 순차적으로 참조할 때 사용하는 인스턴스

- Collection<E>가 Iterable<T>를 상속함
    - `public interface Collection<E> extends Iterable<E>`
    - 컬렉션 클래스는 Iterable 인터페이스의 추상 메소드인 iterator()를 구현하며, iterator()는 Iterator 객체를 반환함
        
        ![collections_3.png](%E1%84%8F%E1%85%A5%E1%86%AF%E1%84%85%E1%85%A6%E1%86%A8%E1%84%89%E1%85%A7%E1%86%AB%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A6%E1%84%8B%E1%85%B5%E1%86%B7%E1%84%8B%E1%85%AF%E1%84%8F%E1%85%B3(List,%20Set,%20Queue,%20Map)%20b4eca73ed6ef482aa8d10aecf990ff69/collections_3.png)
        

- Iterator<E>의 메소드
    - `E next()` - 다음 인스턴스의 참조 값을 반환함
    - `boolean hasNext()` - next 메소드 호출 시 참조 값 반환 가능 여부 확인
    - `void remove()` - next 메소드 호출을 통해 반환했던 인스턴스 삭제
    
    ```java
    List<String> list = new LinkedList<>();
    Iterator<String> itr = list.iterator();   // 반복자 획득
    
    while(itr.hasNext()) {
    		str = itr.next();
    		if(str.equals("Box"))
    				itr.remove();
    ```
    

- 반복자가 다시 첫 번째 인스턴스를 가리키게 하려면 반복자를 다시 얻어야 함
- List<E>를 구현하는 클래스의 인스턴스들만 얻을 수 있는 ‘양방향 반복자’도 있음
    - `public ListIterator<E> listIterator()`을 통해 얻을 수 있음
    - 양쪽 방향으로 이동 가능함 (next(), previous() 등의 메소드 사용)

## Set<E>를 구현하는 컬렉션 클래스

Set<E> 인터페이스를 구현하는 컬렉션 클래스는 **HashSet<E>**과 **TreeSet<E>**이 있음

- HashSet은 저장 순서가 유지되지 않으며, 데이터의 중복 저장을 허용하지 않음

```java
Set<String> set = new HashSet<>();
set.add("Toy");
set.add("Box");
set.add("Box");

// 반복자를 이용한 전체 출력 
// 출력 결과 -> Box Toy
for(Iterator<String> itr = set.iterator(); itr.hasNext();)
		System.out.print(itr.next() + '\t')
```

- TreeSet은 정렬된 상태가 유지되면서 인스턴스가 저장됨 (트리 자료구조)
    - 정렬의 기준은 Comparable<T> 인터페이스의 구현을 통해 정할 수 있음

```java
class Person implements Comparable<Person> {
		private String name;
		private int age;
		
		public Person(String name, int age) {
				this.name = name;
				this.age = age;
		}

		@Override
		public String toString() {
				return name + " : " + age;
		}

		@Override
		public int compareTo(Person p) {
				return this.age - p.age;
		}
}

TreeSet<Person> tree = new TreeSet<>();
tree.add(new Person("YOON", 37));
tree.add(new Person("HONG", 53));
tree.add(new Person("PARK", 22));

// 출력 결과 -> PARK : 22  YOON: 37  HONG : 53
for(Person p : tree)
		System.out.print(p + '\t');
```

## Queue<E>를 구현하는 컬렉션 클래스

Queue<E> 인터페이스를 구현하는 컬렉션 클래스는 **LinkedList<E>**와 **Deque<E>**가 있으며, 스택과 큐 자료구조를 구현함 

- LinkedList<E>는 List<E>를 구현하면서 동시에 Queue<E>를 구현하는 컬렉션 클래스임

```java
Queue<String> que = new LinkedList<>();
que.offer("Box");
que.offer("Toy");
que.offer("Robot");

// 무엇이 다음에 나올지 확인 
// 출력 결과 -> next : Box
System.out.print("next: " + que.peek());

// 첫 번째, 두 번째 인스턴스 꺼내기 
// 출력 결과 -> Box Toy
System.out.print(que.poll() + '\t');
System.out.print(que.poll());
```

- Queue<E> 인터페이스를 대표하는 메소드
    - `boolean add(E e)` - 넣기
    - `E remove()` - 꺼내기
    - `E element()` - 확인하기
    
- 단, 위의 메소드들은 꺼낼 인스턴스가 없을 때 혹은 저장공간이 부족할 때 예외를 발생시킴 → 아래 세 메소드는 동일한 상황에서 예외를 발생시키지 않고 해당 상황을 알리기 위한 특정 값(null 또는 false)를 반환함
    - `boolean offer(E e)` - 넣기, 넣을 공간이 부족하면 false 반환
    - `E poll()` - 꺼내기, 꺼낼 대상 없으면 null 반환
    - `E peek()` - 확인하기, 확인할 대상 없으면 null 반환

---

- Deque<E> 인터페이스를 대표하는 메소드
    - 앞으로 넣고, 꺼내고, 확인하기
        - `void addFirst(E e)` - 넣기
        - `E removeFirst()` - 꺼내기
        - `E getFirst()` - 확인하기
    - 뒤로 넣고, 꺼내고, 확인하기
        - `void addLast(E e)` - 넣기
        - `E removeLast()` - 꺼내기
        - `E getLast()` - 확인하기
        
- 단, 위의 메소드들은 꺼낼 대상이 없거나 공간이 부족해서 넣지 못할 때 예외를 발생시킴 → 아래 메소드들은 동일한 상황에서 예외를 발생시키지 않고 특정 값을 반환함
    - 앞으로 넣고, 꺼내고, 확인하기
        - `void offerFirst(E e)` - 넣기, 공간 부족하면 false 반환
        - `E pollFirst()` - 꺼내기, 꺼낼 대상 없으면 null 반환
        - `E peekFirst()` - 확인하기, 확인할 대상 없으면 null 반환
    - 뒤로 넣고, 꺼내고, 확인하기
        - `void offerLast(E e)` - 넣기, 공간 부족하면 false 반환
        - `E pollLast()` - 꺼내기, 꺼낼 대상 없으면 null 반환
        - `E peekLast()` - 확인하기, 확인할 대상 없으면 null 반환

## Map<K, V>를 구현하는 컬렉션 클래스

Map<K, V> 인터페이스를 구현하는 컬렉션 클래스는 **HashMap<K, V>**과 **TreeMap<K, V>**이 ****있음

두 클래스의 공통점은 Value를 저장할 때 이를 찾을 때 사용하는 Key를 함께 저장한다는 것으로, Key는 중복될 수 없지만, Key만 다르다면 Value는 중복될 수 있음

차이점은 트리 자료구조를 기반으로 구현된 TreeMap<K, V>는 정렬 상태를 유지한다는 것 (정렬 대상은 Key)

- Map<K, V> 클래스는 Iterable<T> 인터페이스를 구현하지 않으므로, 반복자를 통한 순차적 접근이 불가능함 → 순차적 접근을 위해 `keySet()`을 사용함

```java
HashMap<Integer, String> map = new HashMap<>();

// Key-Value 기반 데이터 저장
map.put(45, "Brown");
map.put(37, "James");
map.put(23, "Martin");

// 데이터 탐색 -> 23번: Martin
System.out.println("23번: " + map.get(23));

// 데이터 삭제
map.remove(37);

// Key만 담고 있는 컬렉션 인스턴스 생성 
Set<Integer> ks = map.keySet()

// 전체 Key 출력 -> 37 23 45
for(Integer n : ks)
		System.out.print(n.toString() + '\t');

// 전체 Value 출력 -> James Brown Martin
for(Integer n : ks)
		System.out.print(map.get(n).toString() + '\t');
```

### 면접질문

1. 컬렉션 프레임워크에 대해서 설명해주세요
2. java Map 인터페이스 구현체의 종류 / java Set 인터페이스 구현체의 종류 / java List 인터페이스 구현체의 종류에 대해 설명해주세요

### 출처

도서 ‘윤성우의 열혈 Java 프로그래밍’