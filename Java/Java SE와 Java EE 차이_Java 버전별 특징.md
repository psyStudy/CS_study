# Java SE와 Java EE 차이_Java 버전별 특징

## 자바 프로그래밍 언어 플랫폼

- 자바 애플리케이션을 개발 및 관리하기 위한 환경, 즉 자바 프로그래밍 언어로 작성된 응용 프로그램을 실행하는데 도움이 되는 소프트웨어 또는 프로그램 모음
- Java 언어, JDK, JRE, 자바 컴파일러, JVM으로 구성되어 있음
- 자바 플랫폼의 종류에는 Java SE, Java EE, Java ME가 있음

### Java SE (Java Standard Edition)

- 가장 대중적인 자바 플랫폼(우리가 일반적으로 설치하는 JDK)
- 자바 언어의 대부분의 패키지가 포함됨
    - `java.lang.*`, `java.util.*`, `java.awt.*`, `javax.rmi.*`, `javax.net.*` 등
- 자바 프로그래밍 언어의 핵심 기능들을 제공함
    - 기초적인 타입, 네트워킹, 보안, 데이터베이스 처리, 그래픽 사용자 인터페이스 개발, XML 파싱 등
- 기타 클래스 라이브러리 및 도구 키트로 가상머신, 개발도구, 배포기술, 부가 클래스 라이브러리, 툴킷 등을 제공함

### Java EE (Java Enterprise Edition)

- Java SE에서 API(lib 디렉터리에 포함되어 있는 JAR 파일들)가 추가된 것
- 기업용 에디션으로, 서버 개발에 필요한 기능을 다수 포함함
    - JSP, Servlet, JDBC, JNDI, JTA, EJB 등
- 대규모, 다계층, 확장성, 신뢰성을 갖춘 애플리케이션을 개발하고 실행하기 위한 환경을 제공함

### Java ME (Java Micro Edition)

- 휴대전화(=피쳐폰), 셋톱박스 등에서 자바 프로그래밍 언어를 지원하기 위해 만들어진 플랫폼
- 작은 장치에서 동작하는 전용 클래스 라이브러리들을 제공함
- 현재는 자체적인 OS를 가진 스마트폰이 대중화되었기 때문에 잘 쓰이지는 않음

---

## JDK 종류

Java 소스코드는 오픈소스이기 때문에 다양한 종류의 JDK가 존재함 (리눅스가 Ubuntu, CentOS, RedHat 계열이 있는 것처럼)

![java8_4.png](Java%20SE%E1%84%8B%E1%85%AA%20Java%20EE%20%E1%84%8E%E1%85%A1%E1%84%8B%E1%85%B5_Java%20%E1%84%87%E1%85%A5%E1%84%8C%E1%85%A5%E1%86%AB%E1%84%87%E1%85%A7%E1%86%AF%20%E1%84%90%E1%85%B3%E1%86%A8%E1%84%8C%E1%85%B5%E1%86%BC%20c270799c368d46c5a91e7ffaea17dbe2/java8_4.png)

![java8_3.jpg](Java%20SE%E1%84%8B%E1%85%AA%20Java%20EE%20%E1%84%8E%E1%85%A1%E1%84%8B%E1%85%B5_Java%20%E1%84%87%E1%85%A5%E1%84%8C%E1%85%A5%E1%86%AB%E1%84%87%E1%85%A7%E1%86%AF%20%E1%84%90%E1%85%B3%E1%86%A8%E1%84%8C%E1%85%B5%E1%86%BC%20c270799c368d46c5a91e7ffaea17dbe2/java8_3.jpg)

Oracle JDK의 점유율이 가장 높았지만, 최근 AWS JDK의 점유율이 1위를 기록함 

[https://zdnet.co.kr/view/?no=20230503065956](https://zdnet.co.kr/view/?no=20230503065956)

---

![java8_1.png](Java%20SE%E1%84%8B%E1%85%AA%20Java%20EE%20%E1%84%8E%E1%85%A1%E1%84%8B%E1%85%B5_Java%20%E1%84%87%E1%85%A5%E1%84%8C%E1%85%A5%E1%86%AB%E1%84%87%E1%85%A7%E1%86%AF%20%E1%84%90%E1%85%B3%E1%86%A8%E1%84%8C%E1%85%B5%E1%86%BC%20c270799c368d46c5a91e7ffaea17dbe2/java8_1.png)

[https://www.oracle.com/kr/java/technologies/java-se-glance.html](https://www.oracle.com/kr/java/technologies/java-se-glance.html)

![java8_2.png](Java%20SE%E1%84%8B%E1%85%AA%20Java%20EE%20%E1%84%8E%E1%85%A1%E1%84%8B%E1%85%B5_Java%20%E1%84%87%E1%85%A5%E1%84%8C%E1%85%A5%E1%86%AB%E1%84%87%E1%85%A7%E1%86%AF%20%E1%84%90%E1%85%B3%E1%86%A8%E1%84%8C%E1%85%B5%E1%86%BC%20c270799c368d46c5a91e7ffaea17dbe2/java8_2.png)

자바에서 가장 많이 쓰이는 버전은 Java 8, 11, 17이며, 이 세 가지 버전이 많이 사용되는 이유는 LTS이기 때문

- LTS(Long Term Support): 장기간에 걸쳐 지원, 출시 이후 8년간 보안 업데이트와 버그 수정을 지원해줌
- LTS 이외에 6개월 간격으로 non-LTS 버전들이 출시되는데, 이러한 버전들은 짧은 기간 동안만 해당 버전을 지원해줌
- Java 8이 오랜 Support를 보장하고(2030년까지), 기존 서비스와의 호환 문제 때문에 현업에서 가장 많이 씀!

![java8_5.png](Java%20SE%E1%84%8B%E1%85%AA%20Java%20EE%20%E1%84%8E%E1%85%A1%E1%84%8B%E1%85%B5_Java%20%E1%84%87%E1%85%A5%E1%84%8C%E1%85%A5%E1%86%AB%E1%84%87%E1%85%A7%E1%86%AF%20%E1%84%90%E1%85%B3%E1%86%A8%E1%84%8C%E1%85%B5%E1%86%BC%20c270799c368d46c5a91e7ffaea17dbe2/java8_5.png)

[https://www.jetbrains.com/ko-kr/lp/devecosystem-2021/java/#Java_which-versions-of-java-do-you-regularly-use](https://www.jetbrains.com/ko-kr/lp/devecosystem-2021/java/#Java_which-versions-of-java-do-you-regularly-use)

## Java 8

- 오라클이 자바 인수 후 출시한 첫 번째 LTS 버전
- 32bit를 지원하는 마지막 공식 자바 버전
- **새로운 날짜와 시간 API 지원 (LocalDateTime 등)**

```java
// 기존의 Date 클래스가 아니라 LocalData, LocalTime, LocalDataTime 클래스 사용
LocalDate localDate = LocalDate.now();

System.out.println(localDate.getDayOfWeek().toString()); // WEDNESDAY
System.out.println(localDate.getDayOfMonth());  // 15
```

- **람다식, Stream API 지원**
- **Optional 지원**
    - Null 핸들링을 위한 클래스로, 옵셔널 클래스에는 널 or 널이 아닌 값을 담을 수 있음
    - 널인 경우 orElseThrow 메소드를 사용하여 에러를 던지거나 ifpresent 메소드로 분기 처리를 하는 등 널 처리를 세련되게 할 수 있음

```java
// 일반적인 Null 처리
String str = "name";
if (str != null) {
		System.out.println("name: " + str);
}

// Optional 처리
String str2 = "name";
Optional<String> opt = Optional.ofNullable(str);
opt.ifPresent(s -> System.out.println("name: " + str));
```

- **인터페이스에서도 디폴트 메소드 사용 가능**
    - 예전에는 인터페이스 내에는 추상 메소드만 사용할 수 있었음

```java
public interface DefaultInterFace {
    default void print(String name) {
        System.out.println("print name:" + name);
    }
}
```

- **함수형 인터페이스(1개의 추상 메소드만 가지고 있는 인터페이스) 지원**

```java
@FunctionalInterface
public interface WorkFunctionalInterface {
    public void work();
}
```

## Java 11

- New Garbage Collector, ZGC 추가 (새로운 Garbage Collector 도입)
- JAR파일 형식을 확장하여 여러 버전의 클래스 파일을 하나의 JAR안에 공존 가능
- jlink(JRE를 생성해주는 도구), JShell(메인 Method 없이 자바 코드를 넣고 즉석에서 실행 가능한 도구) 지원
- 인터페이스 내 private Method 사용 가능
- List, Set, Map 인터페이스에 immutable 생성을 할 수 있는 새로운 Method 추가
- 기존 ifPresent Method 경우 Optional 객체가 값을 담고 있는 경우만 처리를 하였으나, 추가 된 ifPresentOrElse Method는 해당 객체가 값이 없을 경우 처리할 내용까지 정의가 가능
- http2를 구현하는 신규 클라이언트 API 제공, 기존 HttpURLConnection API 대체 가능
- Non-Blocking Backpressure를 이용한 비동기 스트림 처리 지원 API 추가
- isBlank, lines, strip, stripLeading, stripTrailing, repeat 등 신규 String Method 추가

## Java 17

- 신규 Mac OS 렌더링 파이프라인 (Mac OS용 Java 파이프라인 구현)
- 텍스트 블록 기능 추가 (기존 String을 여러 줄 작성할 때 사용 가능한 기능, 가독성 있는 코드 지원)
- Record Data class 추가 (immutable 객체를 생성하는 새로운 유형의 클래스로 기존 toString, equals, hashCode Method에 대한 구현을 자동 제공)
- 봉인(Sealed) 클래스 등장 (무분별한 상속을 막기 위한 목적으로 등장한 기능으로 지정한 클래스 외 상속을 허용하지 않으며, 지정한 클래스 외 상속 불가능)
- 향상된 의사 난수 생성기 (의사 난수 생성기를 위한 새로운 인터페이스 타입과 구현을 제공)

자세한 내용은 [https://techblog.gccompany.co.kr/우리팀이-jdk-17을-도입한-이유-ced2b754cd7](https://techblog.gccompany.co.kr/%EC%9A%B0%EB%A6%AC%ED%8C%80%EC%9D%B4-jdk-17%EC%9D%84-%EB%8F%84%EC%9E%85%ED%95%9C-%EC%9D%B4%EC%9C%A0-ced2b754cd7) 참고

### 면접질문

1. Java SE와 Java EE 애플리케이션 차이에 대해서 설명해주세요
2. Java 버전은 어떤 걸 사용했는지?

2-1. Java8에서 추가된 기능에 대해서 설명해주세요

### 출처

[https://doozi316.github.io/java/2020/07/01/WEB20/](https://doozi316.github.io/java/2020/07/01/WEB20/)

[https://www.ibm.com/docs/ko/i/7.3?topic=java-platform](https://www.ibm.com/docs/ko/i/7.3?topic=java-platform)

[https://ko.myservername.com/java-components-java-platform](https://ko.myservername.com/java-components-java-platform)

[https://techvu.dev/135](https://techvu.dev/135)

[https://youngram2.tistory.com/80](https://youngram2.tistory.com/80)

[https://developsd.tistory.com/113](https://developsd.tistory.com/113)

[https://velog.io/@devbro/Java-업데이트-설명-어떤-Java-버전을-설치해야-하나](https://velog.io/@devbro/Java-%EC%97%85%EB%8D%B0%EC%9D%B4%ED%8A%B8-%EC%84%A4%EB%AA%85-%EC%96%B4%EB%96%A4-Java-%EB%B2%84%EC%A0%84%EC%9D%84-%EC%84%A4%EC%B9%98%ED%95%B4%EC%95%BC-%ED%95%98%EB%82%98)