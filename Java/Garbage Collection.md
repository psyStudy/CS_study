# Garbage Collection

## Garbage Collection이란

메모리 관리 기법, 프로그램이 동적으로 할당했던 메모리 영역 중 필요 없게 된 영역을 해제하는 기능

- 동적으로 할당했던 메모리 영역 - 힙 영역(인스턴스 저장)의 메모리
- 필요 없게 된 영역 - 어떤 변수도 가리키지 않게 된 영역

```java
String str1 = new String("My String");
String str2 = new String("Your String");

str1 = null;   // 참조 관계 소멸
str2 = null;   // 참조 관계 소멸 
```

![gc_1.jpg](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_1.jpg)

즉 인스턴스에 레퍼런스가 있다면 Reachable(객체가 참조되고 있는 상태)로 구분되고, 객체에 유효한 레퍼런스가 없다면 Unreachable(객체가 참조되고 있지 않은 상태)로 구분해 수거해버림

### 장점

- 메모리를 수동으로 관리하던 것에서 비롯된 에러를 예방할 수 있음
    - 개발자의 실수로 인한 **메모리 누수**: 더이상 필요하지 않은 메모리가 해제되지 않고 남아있는 에러 → 메모리 누수가 반복되면 메모리 고갈로 프로그램이 중단될 수 있음
    - 해제된 메모리를 또 해제하는 이중 해제
    - 해제된 메모리에 접근
- C와 C++에서 힙 영역의 메모리를 사용하기 위해서는 개발자가 직접 동적 메모리 영역을 할당 받고 해제해야 했지만, Java에서는 동적 메모리 영역 관리를 GC가 알아서 수행해주므로 편하게 개발에 집중할 수 있음

### 단점

- GC의 메모리 해제 시점을 개발자가 정확히 알기 어려움
- 어떤 메모리를 해제할지 결정하는데 비용이 듦 → GC가 동작하는 동안에는 다른 동작을 멈추기 때문에 오버헤드가 발생함(=Stop-The-Word 현상, STW 현상)

## Garbage Collection의 알고리즘

### 1. Tracing(추적) 알고리즘

- Java, C#, Go 등 많은 언어들이 이 기법을 사용하고 있음

### 1-1. Mark And Sweep (표시하고 쓸기)

가장 기본적인 GC 알고리즘 

- 각 메모리 할당 영역에 표시를 위해 1비트의 메모리(flag)를 남겨둠
- 1) **Mark 과정**: 프로그램 실행 중 적절한 타이밍에 GC Root 전체를 순회하며 사용하고 있는 영역의 flag를 ‘사용중’ 상태로 설정함

> **GC Root**
- GC Root에서 시작해 이 Root가 참조하는 모든 객체, 또 그 객체들이 참조하는 다른 객체들을 탐색해 내려가며 마크할 수 있음 
- GC Root가 될 수 있는 것: 실행 중인 스레드, 정적 변수, 로컬 변수, JNI 레퍼런스
> 
> 
> ![gc_12.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_12.png)
> 
- 2) **Sweep 과정**: Mark 되지 않은 영역(참조하고 있지 않은 객체)를 힙에서 제거함
- 3) **Compact 과정**: Sweep 후 분산된 객체들을 힙의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 압축함 (GC 방식에 따라 수행하지 않는 경우도 있음)

![gc_2.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_2.png)

Mark 단계에서 메모리 내용이 변경되지 않아야 하기 때문에, 전체 시스템의 실행이 정지된다는 문제점이 있음

### 1-2. Tri-Color Marking (삼색 표시 기법)

기본적으로 Mark And Sweep와 같은 기법이지만 표기 단계에서 2가지가 아닌 3가지 정보 중 하나로 메모리를 표시한다는 차이가 있음 

- 1) 각각의 객체를 흰색, 회색, 검은색으로 분리함 → **알고리즘이 시작할 때는 변수가 가리키는 객체들이 회색으로 표시되며, 그 외 모든 객체는 흰색으로 표시됨**
    - **흰색**은 더이상 접근이 불가능한 객체를 가리킴
    - **회색**은 접근 가능한 객체를 가리킴, 단 회색 객체가 참조하는 다른 객체들은 아직 검사되지 않았음을 의미
    - **검은색**은 접근 가능한 객체이면서, 검은색 객체가 참조하는 다른 객체들도 흰색 객체를 참조하지 않음을 의미
    
    ![gc_3.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_3.png)
    
- 2) 회색으로 표시된 객체 중 하나를 선택해 검은색으로 표시하고, 이 객체가 가리키는 모든 객체들을 회색으로 표시함
    
    ![gc_4.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_4.png)
    

- 3) 회색 객체가 하나도 남지 않을 때까지 위 과정을 반복함
    
    ![gc_5.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_5.png)
    

- 4) 남은 흰색 객체는 접근 불가능한 객체이므로 모두 해제함
    
    ![gc_6.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_6.png)
    

프로그램 실행 중에도 병행하여 수행할 수 있다는 장점

단, Marking 중간에 메모리 내 참조가 수정되면 잘못 Marking 되는 경우가 생길 수 있음 

### 1-3. Generational GC (세대별 쓰레기 수집)

접근 불가능한 객체들의 대부분이 생성된지 얼마 안된 것이라는 사실이 많은 프로그램을 통해 통계적으로 관찰됨 

- 위의 특성을 이용해 각각의 객체를 할당된 시간에 따라 세대별로 구분하여, 각 세대별로 서로 다른 메모리 영역에 객체를 할당함
- 만약 한 세대의 메모리 영역이 꽉 차면, 이 메모리 영역에서 살아남은 객체를 더 오래된 메모리 영역으로 옮김
- JVM에서 가장 흔하게 사용되고 있는 Sun JVM이 이 방식을 사용함

![gc_7.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_7.png)

- **Old 영역**
    - Young 영역에서 Reachable 상태를 유지하여 살아남은 객체가 복사되는 영역
    - Young 영역보다 크게 할당되며, 영역의 크기가 큰 만큼 가비지는 적게 발생함
    - Old 영역에 대한 가비지 컬렉션을 **Major GC** 또는 Full GC라고 부름

- **Young 영역**
    - 새롭게 생성된 객체가 할당되는 영역
    - 대부분의 객체가 금방 Unreachable 상태가 되기 때문에, 많은 객체가 Young 영역에 생성되었다가 사라짐
    - Young 영역에 대한 가비지 컬렉션을 **Minor GC**라고 부름
    - **Eden 영역**
        - new를 통해 새로 생성된 객체가 위치함
        - 정기적인 가비지 수집 후 살아남은 객체들은 Survivor 영역으로 보냄
    - **Survivor 0 / Survivor 1**
        - 최소 1번의 GC 이상 살아남은 객체가 존재하는 영역
        - Survivor 0 또는 Survivor 1 둘 중 하나는 꼭 비어 있어야 함

### 2. Reference Counting(참조 횟수 계산) 알고리즘

한 메모리에서 다른 메모리를 참조하면 그 다른 메모리의 참조 횟수에 1을 더하고, 참조를 중단하면 참조 횟수에 1을 뺌 → 참조 횟수가 0이 되면 바로 소멸됨 

- PHP와 Swift 언어에서 주된 GC 기법으로 사용함

![gc_8.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_8.png)

- **장점**
    - 추적 알고리즘에 비해 구현이 간단함
    - 실제로 접근이 불가능해지는 시점과 그러한 접근이 불가능해지는 것을 알아내는 시점이 떨어져 있는 추적 알고리즘에 비해 메모리 해제가 빠르게 이루어짐
- **단점**
    - 모든 대입문에 대해 참조횟수를 변경하도록 작성해야 하기 때문에 오버헤드가 발생하여 프로그램이 느려질 수 있음
    - 순환 참조 문제가 발생할 수 있음 (A가 B를 가리키고 B가 A를 가리키면 둘 다 참조횟수가 1이 되는데, A와 B 모두 접근할 수 없는 경우에도 둘 다 참조 횟수가 0이 아니라서 해제할 수 없음)

## ➕ Generational GC를 더 자세히 알아보자

![gc_9.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_9.png)

### Minor GC 과정

객체가 계속 생성되어 Eden 영역이 꽉 차게 되면 Minor GC가 실행됨 

1. Mark 동작을 통해 reachable 객체를 탐색함 
2. Eden 영역에서 살아남은 객체는 Survior 0으로 이동하고, Eden 영역에서 사용되지 않는 unreachable 객체는 메모리를 해제시킴 → 살아남은 모든 객체들은 age값이 1씩 증가함 
3. 또다시 Eden 영역이 가득 차게 되면 다시 한 번 Minor GC를 발생하고 Mark함 
4. reachable한 객체는 비어있는 Survior 1로 이동하고, Eden 영역에서 사용되지 않는 unreachable 객체는 메모리를 해제시킴 → 살아남은 모든 객체들은 age값이 1씩 증가함 
5. 일정 수준의 age bit를 넘어가면, 오래도록 참조될 객체라고 판단하고 해당 객체를 Old Generation으로 넘겨줌(Promotion)
    - Java 8에서는 Parallel GC 방식 기준으로 age bit가 15가 되면 Promotion을 진행함

### Major GC 과정

객체들이 계속 Promotion되어 Old 영역의 메모리가 부족해지면 발생함 

- Old 영역은 Young 영역에 비해 큰 공간을 가지고 있기 때문에 Minor GC에 비해 10배 이상의 많은 시간이 소모됨 → Stop-The-World 문제 발생

## 자바의 Garbage Collection 방식 종류

### Serial GC

- 서버의 CPU 코어가 1개일 때 사용하기 위해 개발된 가장 단순한 GC
- GC를 처리하는 스레드가 1개이기 때문에, 가장 Stop-The-World 시간이 긺
- Minor GC에는 Mark-Sweep을 사용하고, Major GC에는 Mark-Sweep-Compact를 사용함
- 성능이 좋지 않아 보통 실무에서 사용하는 경우는 없음

### Parallel GC

- **Java 8의 디폴트 GC**
- Serial GC와 기본적인 알고리즘은 같지만, Young 영역의 Minor GC를 멀티 스레드로 수행함 (Old 영역은 여전히 싱글 스레드 사용)
- Serial GC에 비해 Stop-The-World 시간이 감소함

### G1 GC

- Java 9+ 버전의 디폴트 GC
- 기존의 GC 알고리즘에서는 힙 영역을 물리적으로 고정된 Young/Old 영역으로 나누어 사용하였지만, G1 GC는 Region이라는 개념을 새로 도입해서 사용함
    - 전체 힙 영역을 Region이라는 영역으로 체스 같이 분할하여 상황에 따라 Eden, Survior, Old 등 역할을 고정이 아닌 동적으로 부여
    - 메모리가 많이 차있는 Region을 인식하여 우선적으로 GC함 → 결과적으로 GC 빈도가 줄어들게 됨
    - 이전의 GC들은 Young 영역에 있는 객체들이 GC가 돌 때마다 살아남으면 Eden → Survivor0 → Survivor1로 순차적으로 이동했지만, G1 GC에서는 더 효율적이라고 생각하는 위치로 객체를 재할당함
        - Survivor1 영역에 있는 객체가 Eden 영역으로 할당하는 것이 더 효율적이라고 판단될 경우 Eden 영역으로 이동시킴

![gc_10.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_10.png)

### ZGC

- Java 15에 release
- 대량의 메모리 처리를 위해 디자인 된 GC
- ZGC는 ZPage라는 영역을 사용함 → G1의 Region은 크기가 고정적이지만, Zpage는 2MB 배수로 동적으로 운영됨 (큰 객체가 들어오면 2의 배수만큼 영역을 늘려 처리)
- 힙 영역의 크기가 증가하여도 Stop-The-World의 시간이 절대 10ms를 넘지 않는다는 것을 최대 장점으로 내세움

![gc_11.png](Garbage%20Collection%20b4eca73ed6ef482aa8d10aecf990ff69/gc_11.png)

### 면접질문

1. 가비지 컬렉션이 어떤 원리에 의해 동작하는지 설명해주세요
2. 가비지 컬렉션의 알고리즘 기법에 대해 설명해주세요

### 출처

도서 ‘윤성우의 열혈 Java 프로그래밍’

[https://ko.wikipedia.org/wiki/쓰레기_수집_(컴퓨터_과학)](https://ko.wikipedia.org/wiki/%EC%93%B0%EB%A0%88%EA%B8%B0_%EC%88%98%EC%A7%91_(%EC%BB%B4%ED%93%A8%ED%84%B0_%EA%B3%BC%ED%95%99))

[https://inpa.tistory.com/entry/JAVA-☕-가비지-컬렉션GC-동작-원리-알고리즘-💯-총정리](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EA%B0%80%EB%B9%84%EC%A7%80-%EC%BB%AC%EB%A0%89%EC%85%98GC-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC)

[https://blog.siner.io/2021/12/26/garbage-collection/](https://blog.siner.io/2021/12/26/garbage-collection/)

[https://namu.wiki/w/쓰레기 수집?from=가비지 컬렉션](https://namu.wiki/w/%EC%93%B0%EB%A0%88%EA%B8%B0%20%EC%88%98%EC%A7%91?from=%EA%B0%80%EB%B9%84%EC%A7%80%20%EC%BB%AC%EB%A0%89%EC%85%98)

[https://stackoverflow.com/questions/33206313/default-garbage-collector-for-java-8](https://stackoverflow.com/questions/33206313/default-garbage-collector-for-java-8)