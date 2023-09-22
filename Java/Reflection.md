# Reflection API 
## Reflection API란

**구체적인 클래스 타입을 알지 못해도** 그 클래스의 메소드, 타입 변수들에 접근할 수 있도록 해주는 자바의 API

- 자바는 정적인 언어이기 때문에 동적인 문제를 해결하기 위해서 리플렉션을 사용함
    - 정적 언어: 컴파일 시점에 타입을 결정 ex) Java, C, C++ 등
    - 동적 언어: 런타임 시점에 타입을 결정 ex) Python, Javascript 등
- 리플렉션은 애플리케이션 개발에서보다는 프레임워크, 라이브러리에서 많이 사용함
    - 프레임워크, 라이브러리는 사용하는 사람이 어떤 클래스를 만들지 모르기 때문
<br></br>
### 장단점
- **장점**
    - 런타임 시점에서 클래스의 인스턴스를 생성하고, 접근 제어자와 관계 없이 필드와 메소드에 접근하여 필요한 작업을 수행할 수 있는 유연성을 가지고 있음
- **단점**
    - 캡슐화를 저해함
    - 런타임 시점에서 인스턴스를 생성하므로, 컴파일 시점에서 해당 타입을 체크할 수 없음
    - 런타임 시점에서 인스턴스를 생성하므로, 구체적인 동작 흐름을 파악하기 어려움
    - 단순히 필드 및 메소드에 접근할 때보다 리플렉션을 사용하여 접근할 때 느림

<br></br>

## 사용 방법
1. 리플렉션을 사용하기에 앞서, 힙 영역에 로드된 클래스 타입의 객체를 가져와야 함 
    - 1. 클래스.class로 가져오는 방법
    - 2. 인스턴스.getClass()로 가져오는 방법
    - 3. Class.forName(”클래스명”)으로 가져오는 방법
    - 어떤 방식을 사용하든 해시 값이 같으므로 상황에 따라 적절하게 사용하면 됨

```java
public class Member {

    private String name;
    protected int age;
    public String hobby;

    public Member() {
    }

    public Member(String name, int age, String hobby) {
        this.name = name;
        this.age = age;
        this.hobby = hobby;
    }

		private void speak(String message) {
        System.out.println(message);
    }
}

public class Main {
    public static void main(String[] args) throws ClassNotFoundException {
        Class<Member> memberClass = Member.class;
        System.out.println(System.identityHashCode(memberClass));

        Member member = new Member("홍길동", 23, "독서");
        Class<? extends Member> memberClass2 = member.getClass();
        System.out.println(System.identityHashCode(memberClass2));

        Class<?> memberClass3 = Class.forName("{패키지명}.Member");
        System.out.println(System.identityHashCode(memberClass3));
    }
}

/* 실행 결과
1740000325
1740000325
1740000325 
*/
```
<br></br>
2. 가져 온 클래스 타입을 통해, 해당 클래스의 인스턴스를 생성할 수 있음
    - `getConstructors()` 통해 생성자를 얻어올 수 있음
    - `newInstance()` 통해 인스턴스를 동적으로 생성해줄 수 있음

```java
public class Main {
    public static void main(String[] args) throws Exception {
        // Member의 모든 생성자 출력 
        Member member = new Member();
        Class<? extends Member> memberClass = member.getClass();
        Arrays.stream(memberClass.getConstructors()).forEach(System.out::println);

        // Member의 기본 생성자를 통한 인스턴스 생성
        Constructor<? extends Member> constructor = memberClass.getConstructor();
        Member member2 = constructor.newInstance();
        System.out.println("member2 = " + member2);

        // Member의 다른 생성자를 통한 인스턴스 생성
        Constructor<? extends Member> fullConstructor =
            memberClass.getConstructor(String.class, int.class, String.class);
        Member member3 = fullConstructor.newInstance("홍길동", 23, "독서");
        System.out.println("member3 = " + member3);
    }
}

/* 실행 결과
public Member()
public Member(java.lang.String,int,java.lang.String)
member2 = Member{name='null', age=0, hobby='null'}
member3 = Member{name='홍길동', age=23, hobby='독서'} 
*/
```
<br></br>
3. 가져 온 클래스 타입을 통해, 인스턴스의 필드와 메소드를 접근 제어자와 상관없이 접근하여 사용할 수 있음 
    - `getDeclaredFields()` 통해 클래스의 인스턴스 변수를 모두 가져올 수 있음
        - `get()`을 통해 필드값을 반환받을 수 있고, `set()`을 통해 필드값을 수정할 수 있음
        - 이때 private 접근 제어자가 있는 필드에 접근할 때는 `setAccessible()`의 인자를 true로 넘겨줘야 함
    - `getDeclaredMethod()` 통해 클래스의 메소드를 가져올 수 있음
        - 이때 메소드의 이름과 파라미터의 타입을 인자로 넘겨줘야 함
        - 마찬가지로 private 접근 제어자가 있는 필드에 접근할 때는 `setAccessible()`의 인자를 true로 넘겨줘야 함
        - `invoke()` 통해 가져온 메소드를 호출하여 사용할 수 있음

```java
public class Main {
    public static void main(String[] args) throws Exception {
        Member member = new Member("홍길동", 23, "독서");
        Class<? extends Member> memberClass = member.getClass();

        // 필드 접근
        Field[] fields = memberClass.getDeclaredFields();
        for (Field field : fields) {
            field.setAccessible(true);
            System.out.println(field.get(member));
        }
				
        fields[0].set(member, "임꺽정");
        System.out.println(member);

        // 메소드 접근
        Method speakMethod = memberClass.getDeclaredMethod("speak", String.class);
        speakMethod.setAccessible(true);
				speakMethod.invoke(member, "안녕하세요");
    }
}

/* 실행 결과
홍길동
23
독서
Member{name='임꺽정', age=23, hobby='독서'}
안녕하세요
*/
```

<br></br>

## 실제 사례
```java
@Controller
@RequestMapping("/articles")
public class ArticleController {    
    @Autowired    
    private ArticleService articleService;       
       ....

    @PostMapping
    public String write(UserSession userSession, ArticleDto.Request articleDto){
       ...
    }

    @GetMapping("/{id}")
    public String show(@PathVariable int id, Model model) {
       ...
    }
}
```

- ArticleController를 작성한 개발자는 클래스의 정보를 알지만, Spring은 알지 못함 
→ 스프링이 ArticleController를 알아내기 위해서 리플렉션을 사용함

<br></br>
<br></br>

### 면접질문
1. Reflection이란 무엇인지 설명해주세요
생각보다 엄청 딥한 개념이라.. 면접에서도 묻지 않는 것 같다 

<br></br>
### 출처
[https://steady-coding.tistory.com/609](https://steady-coding.tistory.com/609)     
[https://dublin-java.tistory.com/53](https://dublin-java.tistory.com/53)
