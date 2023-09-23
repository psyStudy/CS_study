# 람다식 Lambda Expression

# 0. 익명 클래스 Anonymous Class

내부 클래스의 일종으로, 프로그램에서 일시적으로 한 번만 사용되고 버려지는 객체.

- 정의와 동시에 객체를 생성할 수 있다.
- 어느 메소드에서 **부모 클래스의 자원을 상속받아 재정의하여 사용할 자식 클래스가 한번만 사용**되고 버려질 자료형이면, 굳이 상단에 클래스를 정의하기보다는, **지역 변수처럼** 익명 클래스로 정의하고 스택이 끝나면 삭제되도록 하는 것이 유지보수면에서나 프로그램 메모리면에서나 이점을 얻을 수 있다.

## 사용법

1. 익명 자식 객체를 생성하는 방법
2. 익명 구현 객체를 생성하는 방법 = 함수형 인터페이스를 사용하는 방법

### 익명 자식 객체를 생성하는 방법

1. 필드의 초기값
2. 로컬변수의 초기값
3. 매개변수의 매개값

```java
package Anonymous;

public class Insect { // 곤충 부모 클래스 : 일시적으로 정의된 익명 객체를 담아둘 변수같은 개념

	void attack(){
		System.out.println("곤충은 공격을 한다");
	}
}

public class Anonymous { //부모(곤충)객체 필드에 자식 익명객체를 바로 정의하여 초기값을 할당

	//★★방법 1 : 필드에 익명자식 객체를 생성 
	Insect spider1 = new Insect(){
		
		String name = "무당거미";
		//거미줄을 치다.
		void cobweb(){
			System.out.println("사각형으로 거미줄을 친다.");
		}
		
		@Override
		void attack() {
			System.out.println(name + " 독을 발사한다.");
		}
	};
	
	//★★방법2 : 로컬변수의 초기값으로 대입
	void method1(){
		Insect spider2 = new Insect(){
			
			String name = "늑대거미";
			//거미줄을 치다.
			void cobweb(){
				System.out.println("육각형으로 거미줄을 친다.");
			}
			
			@Override
			void attack() {
				System.out.println(name + " 앞니로 문다.");
			}
		};
		
		//로컬변수이기 때문에 메서드에서 바로 사용
		spider2.attack();
	}
	
	//★★방법3 : 익명객체 매개변수로 대입
	void method2(Insect spider){
		spider.attack();
	}
	
	
}
```
### 익명 구현 객체(클래스)를 생성하는 방법
 [-> 인터페이스를 사용하는 방법](https://github.com/psyStudy/CS_study/blob/main/Java/%ED%95%A8%EC%88%98%ED%98%95%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4.md)

---

# 1. 람다식

## 2.1. 개념

함수를 하나의 식(Expression)으로 표현한 것.

- 함수를 람다식으로 표현하면 메소드의 이름이 필요없다.
- 등장이유 : 불필요한 코드를 줄이고, 가독성을 높이기 위해
- 익명 함수의 한 종류이다.
    - 익명함수 : 이름이 없는 함수. 모두 1급 객체에 해당.
    - 1급 객체인 함수는 변수처럼 사용가능하며, 매개변수로 전달 가능하다.
- java의 함수는 1급 객체가 아니지만, lambda를 통해 이를 보완한 것.
    - (일반)함수도 익명 클래스를 사용하여 비슷하게 사용할 수 있음
    
    ```java
    Test test = new Test(){
    	System.out.println("hi");
    };
    ```
    

### [참고] 익명클래스 vs Lambda

- 람다와 익명 클래스는 처리방식이 내부적으로 유사하지만 같은 개념은 아니다.
- 익명 클래스는 익명의 새로운 클래스를 생성하지만, 람다는 새로운 메서드를 생성한다.
- 익명 클래스의 this는 새로 생성된 클래스를 의미하고, 람다의 this는 람다를 가진 클래스를 의미한다.

### 특징

- 람다식 내에서 사용되는 지역변수를 final이 붙지 않아도 상수로 간주된다.
- 람다식안에 선언된 지역 변수명은 다른 변수명과 중복될 수 없다.

### 장점

1. 코드를 간결하게 만들 수 있다.
2. 식에 개발자의 의도가 명확히 드러나 가독성이 높아진다.
3. 함수를 만드는 과정없이 한번에 처리할 수 있어 생산성이 높아진다.
4. 병렬 프로그래밍이 용이하다. (1급 객체로, Thread-safe한 성질 때문)

### 단점

1. 람다식은 재사용이 불가능하다.
2. 디버깅이 어렵다.
3. 람다를 남발하면 비슷한 함수가 중복 생성되어 코드가 지저분해질 수 있다.
4. 재귀로 만들경우 부적합.

## 2.2. 사용법&예시

### 사용법

1. 람다는 매개변수 화살표(→) 함수 몸체로 사용할 수 있다.
2. 함수몸체가 단일 실행문이면 괄호{}를 생략할 수 있다.
3. 함수 몸체가 return 문으로만 구성되어 있으면 괄호{}를 생략할 수 없다.
4. 선언된 type과 선언되지 않은 type을 같이 사용 할 수 없다.
5. type(자료형)을 생략할 수 있는데, 람다식이 본문을 넘겨줄 때 알아서 결정된다. (컴파일러가 알아서 정의)

```java
//기존 방식
반환티입 메소드명 (매개변수, ...) {
	실행문
}
// 람다 방식
(매개변수, ... ) -> { 실행문 ... }
```

### 예시1

```java
//정상적인 유형
(int x) -> x+1
(x) -> x+1
x -> x+1
(int x) -> { return x+1; }
x -> { return x+1; }

(int x, int y) -> x+y
(x, y) -> x+y
(x, y) -> { return x+y; }

(String lam) -> lam.length()
lam -> lam.length()
(Thread lamT) -> { lamT.start(); }
lamT -> { lamT.start(); }

//잘못된 유형 : 선언된 type과 선언되지 않은 type을 같이 사용 할 수 없다.
(x, int y) -> x+y
(x, final y) -> x+y
```

### 예시2

기존 자바 문법 → 람다식 문법
- [Runnalbe 함수형 인터페이스 참조](https://github.com/psyStudy/CS_study/blob/main/Java/%ED%95%A8%EC%88%98%ED%98%95%20%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4.md)

```java
new Thread(new Runnable() {
   @Override
   public void run() { 
      System.out.println("Welcome Heejin blog"); 
   }
}).start();
```

```java
new Thread(()->{
      System.out.println("Welcome Heejin blog");
}).start();
```

---

# 면접질문

- Lambda Expression 은 무엇인가요?
    - script1) 식별자 없이 실행이 가능한 함수입니다. 함수인데 함수를 따로 만들지 않고 코드한줄에 함수를 써서 그것을 호출하는 방식입니다. 자바 8부터 지원하고 코드가 간결해 가독성이 좋다는 것이 장점입니다. 그러나 람다식으로 만든 함수는 재사용이 불가하고 디버깅이 까다롭다는 단점이 있습니다.
    - script2) Lambda Expression이란 Functional Interface를 구현하는 객체를 만들지 않고도 메서드로 전달할 수 있는 익명 함수를 하나의 식으로 표현할 수 있도록 단수화한 것입니다. 즉, 특정 메소드 사용을 위해 일회용 객체를 만들지 않아도 되기 때문에 간단하게 작성 가능하고 가독성이 증가합니다. 너무 남용하면 코드를 이해하는데 어려움이 생길 수 있고 디버깅이 어렵다는 단점이 있습니다

# 출처

- 함수형 프로그래밍 : [https://mangkyu.tistory.com/111](https://mangkyu.tistory.com/111)
- 프로그래밍 패러다임 : [https://ko.wikipedia.org/wiki/프로그래밍_패러다임](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D_%ED%8C%A8%EB%9F%AC%EB%8B%A4%EC%9E%84)
- 람다식 : [https://mangkyu.tistory.com/113](https://mangkyu.tistory.com/113)
- 람다식 : [https://khj93.tistory.com/entry/JAVA-람다식Rambda란-무엇이고-사용법](https://khj93.tistory.com/entry/JAVA-%EB%9E%8C%EB%8B%A4%EC%8B%9DRambda%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-%EC%82%AC%EC%9A%A9%EB%B2%95)
- 1급 객체 : [https://velog.io/@sjsrkdgks/1급-객체First-class-citizen](https://velog.io/@sjsrkdgks/1%EA%B8%89-%EA%B0%9D%EC%B2%B4First-class-citizen)
- 익명 클래스 : [https://inpa.tistory.com/entry/JAVA-☕-익명-클래스Anonymous-Class-사용법-마스터하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9D%B5%EB%AA%85-%ED%81%B4%EB%9E%98%EC%8A%A4Anonymous-Class-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0)
- 질문 : [https://haejun0317.tistory.com/240](https://haejun0317.tistory.com/240)
- 익명클래스 : [https://limkydev.tistory.com/226](https://limkydev.tistory.com/226)