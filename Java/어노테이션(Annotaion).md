# 어노테이션 Annotaion

# 1. 개념

주석. 소스코드에 추가하여 사용할 수 있는 메타 데이터의 일종.

- 메타 데이터 : 애플리케이션이 처리해야할 데이터가 아니라, 컴파일 과정과 실행과정에서 코드를 어떻게 처리해야하는지 알력주기 위한 추가정보.

@기호를 붙여 사용한다.

JDK 1.5 버전이상에서 사용 가능

클래스 파일에 임베드되어 컴파일러에 의해 생성된 후 JVM에 포함되어 동작한다. 

### 용도

- 컴파일러에게 코드 작성 문법 에러를 체크하도록 정보 제공
- SW개발 툴이 빌드나 배치시 코드를 자동으로 생성할 수 있도록 정보제공
- 실행시(런타임시) 특정 기능을 실행하도록 정보를 제공

# 2. 빌트인 어노테이션

자바에서 제공하는 어노테이션 중 개발 시 자주 이용하는 것들.

### @Override

- 선언한 **메서드가 오버라이드 되었다는 것을 나타냅니다.**
- 만약 상위(부모) 클래스(또는 인터페이스)에서 해당 메서드를 찾을 수 없다면 컴파일 에러를 발생 시킵니다

```java
@Override
public void getInfo() {
  System.out.println("test");
}
```

### @Deprecated

- 해당 **메서드가** **더 이상 사용되지 않음을 표시**합니다.
- 하위호환을 위해 메소드 자체를 없애지는 못하지만 사용하지 말 것을 사용자에게 알리고 싶을 때 사용.
- 만약 사용할 경우 **컴파일 경고를 발생** 키십니다.

```java
@Deprecated
public void deprecatedMethod() {
  System.out.println("test");
}
```

### @SuppressWarnings

- 선언한 곳의 컴파일 **경고를 무시**하도록 합니다.
- 컴파일러 경고는 경고일 뿐… 경고 상황을 개발자가 알고 잇는 경우 컴파일 로그가 지저분해지고 진짜 잡아야하는 경고를 찾기 힘들기 때문에 사용한다.

### @SafeVarargs

- Java7 부터 지원하며, 제너릭 같은 가변인자의 매개변수를 사용할 때의 경고를 무시합니다.

### @FunctionalInterface

- Java8 부터 지원하며, 함수형 인터페이스를 지정하는 어노테이션입니다.
- 만약 메서드가 존재하지 않거나, 1개 이상의 메서드(default 메서드 제외)가 존재할 경우 컴파일 오류를 발생 시킵니다.

# 3. 메타 어노테이션

커스텀 어노테이션을 만들때 사용하는 어노테이션.

### @Retention

- 자바 컴파일러가 어노테이션을 다루는 방법을 기술하며, 특정 시점까지 영향을 미치는지를 결정합니다.
- 종류는 다음과 같습니다.
    - **RetentionPolicy.SOURCE** : 컴파일 전까지만 유효. (컴파일 이후에는 사라짐)
    - **RetentionPolicy.CLASS** : 컴파일러가 클래스를 참조할 때까지 유효.
    - **RetentionPolicy.RUNTIME** : 컴파일 이후에도 JVM에 의해 계속 참조가 가능. (리플렉션 사용)

### @Documented

- 해당 어노테이션을 Javadoc에 포함시킵니다.

### @Target

- 어노테이션이 적용할 위치를 선택합니다.
- 종류는 다음과 같습니다.
    - **ElementType.PACKAGE** : 패키지 선언
    - **ElementType.TYPE** : 타입 선언
    - **ElementType.ANNOTATION_TYPE** : 어노테이션 타입 선언
    - **ElementType.CONSTRUCTOR** : 생성자 선언
    - **ElementType.FIELD** : 멤버 변수 선언
    - **ElementType.LOCAL_VARIABLE** : 지역 변수 선언
    - **ElementType.METHOD** : 메서드 선언
    - **ElementType.PARAMETER** : 전달인자 선언
    - **ElementType.TYPE_PARAMETER** : 전달인자 타입 선언
    - **ElementType.TYPE_USE** : 타입 선언

### @Inherited

- 어노테이션의 상속을 가능하게 합니다.

### @Repeatable

- Java8 부터 지원하며, 연속적으로 어노테이션을 선언할 수 있게 해줍니다.

# 4. 커스텀 어노테이션 만들기

@ interface [어노테이션 이름] 이라는 형태로 어노테이션을 정의한다.

생성되는 어노테이션에 대한 메타 어노테이션은 어노테이션 정의 앞쪽에 붙여준다.

- 예시 - MyAnnotation 생성 : 선언

```java
@Target({ElementType.METHOD}) // 이 어노테이션은 메소드에 붙일 수 있고
@Retention(RetentionPolicy.RUNTIME) // runtiome에 적용됨
public @interface MyAnnotation {
        String value() default "MyAnnotation : default value"
}
```

- 적용 - 자바의 리플렉션을 활용해 특정 목적으로 사용할 수 있음.
    - (아직은 그저 문자열 정보를 가지고 있을 뿐)
    - 자바 리플렉션 : 컴파일 시간이나 실행시간에 동적으로 특정 클래스의 정보를 객체를 틍해 분석 및 추출해내는 프로그래밍 기술

```java
class MyObject {
    @MyAnnotation
    public void testMethod1() {
        System.out.println("This is testMethod1");
  }

    @MyAnnotation(value = "My new Annotation")
    public void testMethod1() {
        System.out.println("This is testMethod1");
  }
}
```

- 자바 리플렉션을 통해 어노테이션 값을 가져올 수 있다.

```java
public class MyMain {
    public static void main(String[] args) {
        Method[] methodList = MyObject.class.getMethods();

        for (Method method : methodList) {
            if(method.isAnnotationPresent(MyAnnotation.class)) {
                MyAnnotation annotation=method.getDeclaredAnnotation(MyAnnotation.class);
                String value=annotation.value();
                System.out.println(method.getName() + ":" + value);
            }
        }
    }
}
```

---

# 면접질문

- 어노테이션이 무엇입니까?
- 어노테이션의 용도는 무엇입니까?

# 출처

- [https://bangu4.tistory.com/199](https://bangu4.tistory.com/199)
- [https://hbase.tistory.com/169](https://hbase.tistory.com/169)
- [https://geminihoroscope.tistory.com/90](https://geminihoroscope.tistory.com/90)

# 참고하면 좋은 글

- 우아한 기술블로그 - 시의적절한 커스텀 어노테이션 : [https://techblog.woowahan.com/2684/](https://techblog.woowahan.com/2684/)