# 동일성과 동등성_ String/StringBuffer/StringBuilder

## 동일성(Identity)

두 객체와 완전히 같은 경우를 의미함

- 두 객체의 메모리 주소가 같기 때문에 두 참조 변수가 같은 객체를 가리키고 있음
- 즉, 내용과 주소값이 모두 같음
- 자바에서 동일성은 비교 연산자 ‘==’로 확인할 수 있음

![String_5.png](%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%83%E1%85%B3%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC_%20String%20StringBuffer%20StringB%20c270799c368d46c5a91e7ffaea17dbe2/String_5.png)

```java
Number number1 = new Number(1);
Number number2 = number1;

System.out.println(number1 == number2);  // true
```

## 동등성(Equality)

두 객체가 같은 정보(내용)를 가지고 있음을 의미함 

- 즉, 동일함은 동등함을 보장하지만, 반대로 동등함은 동일함을 보장하지 않음
- 자바에서 동등성을 비교하기 위해서는 equals()와 hashCode()를 오버라이딩해서 사용해야 함

![String_6.png](%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%83%E1%85%B3%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC_%20String%20StringBuffer%20StringB%20c270799c368d46c5a91e7ffaea17dbe2/String_6.png)

### equals 메소드

모든 객체의 조상인 Object 객체에서 정의하고 있는 equals()는 단순히 동일성 비교를 하고 있음 

```java
public boolean equals(Object obj) {
		return (this == obj);
}
```

따라서 String 클래스는 아래와 같이 equals()를 재정의하여 인자로 전달된 String의 문자열을 비교하고 있음 

```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

### hashCode 메소드

객체의 주소 값을 이용해서 해싱 기법을 통해 해시코드를 만든 뒤 반환하는 메소드 → 해시코드는 주소값으로 만든 고유한 숫자값임 

equals()를 오버라이딩 하고 실행했을 때, hashCode()를 오버라이딩 안하면 다음과 같은 경고문이 뜸 

![String_7.png](%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%83%E1%85%B3%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC_%20String%20StringBuffer%20StringB%20c270799c368d46c5a91e7ffaea17dbe2/String_7.png)

자바는 “**equals()의 결과가 true인 두 객체의 해시코드는 반드시 같아야 한다”**는 규칙을 가지고 있기 때문에, equals()를 객체의 주소가 아닌 객체의 필드 값을 비교하기 위해 오버라이딩 했다면 hashCode()도 오버라이딩 해줘야 함 

**❓왜 equals()의 결과가 true인 두 객체의 해시코드는 반드시 같아야 할까?**

- hash 값을 사용하는 Collection Framework(HashSet, HashMap, HashTable)을 사용할 때 문제가 되기 때문

```java
class Person {
    public String name;

    public Person(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person p = (Person) o;
        return Objects.equals(name, p.name);
    }
}

public class ClassTest {
    public static void main(String[] args) throws Exception {
        Person p1 = new Person("홍길동");
        Person p2 = new Person("홍길동");

        // 두 객체의 해시 코드
        System.out.println(p1.hashCode()); // 460141958
        System.out.println(p2.hashCode()); // 1163157884

        // 해시코드가 달라도, equals를 재정의 했기 때문에 동등함
        System.out.println(p1.equals(p2)); // true

        Set<Person> people = new HashSet<>();
		    people.add(p1);
		    people.add(p2);
				
				// ⁉️논리적으로 equals 결과가 true이므로 1이 나와야 하는데 2가 출력됨 
		    System.out.println(people.size());   
    }
}
```

따라서 hashCode()를 재정의해야 한다 

```java
class Person {
    public String name;

    public Person(String name) {
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Person p = (Person) o;
        return Objects.equals(name, p.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name); // name 필드의 해시코드를 반환한다
    }
}

public class ClassTest {
    public static void main(String[] args) throws Exception {
        Person p1 = new Person("홍길동");
        Person p2 = new Person("홍길동");

        // 두 객체의 해시 코드
        System.out.println(p1.hashCode()); // 54150093
        System.out.println(p2.hashCode()); // 54150093

        // 해시코드가 달라도, equals를 재정의 했기 때문에 동등함
        System.out.println(p1.equals(p2)); // true

        // SET를 생성하고 두 객체 데이터를 추가한다
        Set<Person> people = new HashSet<>();
        people.add(p1);
        people.add(p2);

        // 그리고 SET의 길이를 출력한다
        System.out.println(people.size()); // 1
    }
}
```

---

## String 클래스

String 클래스에는 문자열을 저장하기 위해서는 문자열 배열 변수(char[]) value를 인스턴스 변수로 정의해놓고 있음

```java
public final class String implements java.io.Serializable, Comparable {
		private char[] value;
		...
}
```

즉, 인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스 변수(value)에 문자형 배열(char[])로 저장되는 것

→ 한 번 생성된 String 인스턴스가 갖고 있는 문자열은 읽어 올 수만 있고, 변경할 수는 없음 

- ‘+’ 연산자를 이용하여 문자열을 결합하는 경우, 인스턴스 내의 문자열이 바뀌는 게 아니라 새로운 문자열이 담긴 String 인스턴스가 생성되는 것
    
    ```java
    String a = 'a';
    String b = 'b';
    String a = a + b;
    ```
    
    ![String_1.jpg](%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%83%E1%85%B3%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC_%20String%20StringBuffer%20StringB%20c270799c368d46c5a91e7ffaea17dbe2/String_1.jpg)
    
- **따라서 문자열 간의 결합이나 추출 등 문자열을 다루는 작업이 많이 필요한 경우에는 메모리 공간을 절약하기 위해 String 클래스 대신 StringBuffer 클래스를 사용하는 것이 좋음**

### 문자열을 만드는 2가지 방법

문자열을 만들 때는 문자열 리터럴을 지정하는 방법과, String 클래스의 생성자를 사용해서 만드는 방법이 있음

```java
String str1 = "abc";
String str2 = "abc";

String str3 = new String("abc");
String str4 = new String("abc");

/*
str1 == str2 ? true
str1.equals(str2) ? true

str3 == str4 ? false
str3.equals(str4) ? true
*/
```

![String_2.jpg](%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%83%E1%85%B3%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC_%20String%20StringBuffer%20StringB%20c270799c368d46c5a91e7ffaea17dbe2/String_2.jpg)

 

## StringBuffer 클래스

내부적으로 문자열 편집을 위한 버퍼를 가지고 있으며 StringBuffer 인스턴스를 생성할 때 그 크기를 지정할 수 있기 때문에, String 클래스와 달리 변경이 가능함 

- 이때 편집할 문자열의 크기를 고려하여 버퍼의 크기를 충분히 잡아주는 것이 좋음 (편집 중인 문자열이 버퍼의 크기를 넘어서게 되면 버퍼의 크기를 늘려주는 작업이 추가로 수행되어야 하기 때문에 효율이 떨어짐)

```java
// 자바 내부 클래스 
public StringBuffer(int length) {
		value = new char[length];
		shared = false;
}

public StringBuffer() {
		this(16);   // 버퍼의 크기를 지정하지 않으면 버퍼의 크기는 16이 됨 
}

public StringBuffer(String str) {
		this(str.length() + 16);   // 지정한 문자열의 길이보다 16이 더 크게 버퍼를 생성함 
		append(str);
}

// 사용자 코드 
StringBuffer sb = new StringBuffer(100);
sb.append("abcd");

StringBuffer sb = new StringBuffer();

StringBuffer sb = new StringBuffer("Hi");
```

- String 클래스에서는 equals 메소드를 오버라이딩해서 문자열의 내용을 비교하도록 구현되어 있지만, StringBuffer 클래스는 equals 메소드를 오버라이딩하지 않아서 StringBuffer 클래스의 equals 메소드를 사용해도 ‘==’로 비교한 것과 같은 결과를 얻음

```java
StringBuffer sb = new StringBuffer("abc");
StringBuffer sb2 = new StringBuffer("abc");

// StringBuffer의 내용을 String으로 변환해서 내용을 비교해야 함
String s = sb.toString();
String s2 = sb2.toString();

/*
sb == sb ? false
sb.equals(sb2) ? false
s.equals(s2) ? true
*/
```

## StringBuilder 클래스

StringBuffer 클래스와 거의 유사함!

- 생성자를 포함한 메소드 수, 메소드의 기능, 메소드의 이름과 매개변수의 선언이 일치함

StringBuilder 클래스는 Java 5에서 등장한 클래스이고, 이전에는 StringBuffer 클래스가 사용된 것 

```java
StringBuilder sb = new StringBuilder(64);
sb.append("abcd");

StringBuilder sb = new StringBuilder();

StringBuilder sb = new StringBuilder("abc");
```

단, StringBuffer 클래스는 스레드에 안전하지만(동기화 보장), StringBuilder 클래스는 스레드에 안전하지 않다(동기화 보장하지 않음)는 것이 차이점임 

- 멀티 스레드에 안전하게 설계된 StringBuffer 클래스는 속도가 느림
- 멀티 스레드와 상관없는 상황에서 사용할 목적으로 StringBuilder 클래스를 만들게 된 것

## 💡 String vs StringBuffer vs StringBuilder

- String과 StringBuffer, StringBuilder의 차이점은 String은 immutable(불변), StringBuffer/StringBuilder는 mutable(변함)이라는 것
    - String 클래스는 문자열 연산이 많은 경우 성능이 좋지 않음
    - But, 멀티 스레드 환경에서 동기화를 신경쓰지 않아도 되고 내부 데이터를 자유롭게 공유 가능하다는 장점이 있음
- StringBuffer와 StringBuilder의 차이점은 동기화 여부임
    
    ![String_3.png](%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%83%E1%85%B3%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC_%20String%20StringBuffer%20StringB%20c270799c368d46c5a91e7ffaea17dbe2/String_3.png)
    
    StringBuffer 클래스의 내부를 살펴보면, synchronized 키워드를 사용하여 메소드를 선언함 
    
    ![String_4.png](%E1%84%83%E1%85%A9%E1%86%BC%E1%84%8B%E1%85%B5%E1%86%AF%E1%84%89%E1%85%A5%E1%86%BC%E1%84%80%E1%85%AA%20%E1%84%83%E1%85%A9%E1%86%BC%E1%84%83%E1%85%B3%E1%86%BC%E1%84%89%E1%85%A5%E1%86%BC_%20String%20StringBuffer%20StringB%20c270799c368d46c5a91e7ffaea17dbe2/String_4.png)
    
    StringBuilder 클래스에서 제공하는 메소드는 synchronized 키워드가 존재하지 않음 
    

### 면접질문

1. Java에서 ‘==’와 ‘equals()’의 차이에 대해 말씀해주세요 
2. 동일성과 동등성에 대해 설명해주세요 
3. String, StringBuilder, StringBuffer 각각의 차이에 대해 설명해주세요 

### 출처

도서 ‘Java의 정석’

도서 ‘윤성우의 열혈 Java 프로그래밍’

[https://hudi.blog/identity-vs-equality/](https://hudi.blog/identity-vs-equality/)

[https://creampuffy.tistory.com/140](https://creampuffy.tistory.com/140)

[https://inpa.tistory.com/entry/JAVA-☕-equals-hashCode-메서드-개념-활용-파헤치기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-equals-hashCode-%EB%A9%94%EC%84%9C%EB%93%9C-%EA%B0%9C%EB%85%90-%ED%99%9C%EC%9A%A9-%ED%8C%8C%ED%97%A4%EC%B9%98%EA%B8%B0)

[https://12bme.tistory.com/42](https://12bme.tistory.com/42)

[https://developer-talk.tistory.com/776](https://developer-talk.tistory.com/776)