# item5 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

보통의 클래스들이 **하나 이상의 자원(resource)에 의존**한다.
- 이러한 클래스들을 **정적 유틸리티(utility) 클래스**로 구현,
- **싱글톤(singleton)으로 구현**하는 경우도 있다.

**[정적 유틸리티를 잘못 사용한 예 - 유연하지 않고 테스트하기 어렵다.]**

```java
public class SpellChecker {
    private static final Lexicon dictionary = ...;

    private SpellChecker() {} // 객체 생성 방지

    public static boolean isValid(String word) {...}

    public static List<String> suggestions(String typo) {...}
}
```

**[싱글톤을 잘못 사용한 예 - 유연하지 않고 테스트 하기 어렵다.]**

```java
public class SpellChecker {
    private final Lexicon dictionary = ...;

    private SpellChecker(...) {}
    private static SpellChecker INSTANCE = new SpellChecker(...);

    public boolean isValid(Srting word) {...}
    public List<String> suggestions(String typo) {...}
}
```

- 2 방식 모두 사전을 단 1개만 사용한다는 점에서 좋지 않다.
  - 실전에서는 다양한 상황이 발생하고, 사전 하나로 이 모든 쓰임에 대응할 수는 없다.

SpellChecker가 여러 사전을 사용할 수 있어도 마찬가지다.
- `dictionary 필드`의 final제어자를 없애고, 다른 사전으로 교체하는 메서드를 추가할 수 있지만,
- **이 방안은 어색하고 오류를 내기 쉽고 멀티 쓰레드 환경(Multi-Thread)에서는 쓸 수 없다.**
- **사용하는 자원에 따라 동작이 달라지는 클래스에는 정적 유틸리티 클래스나 싱글톤 방식이 적합하지 않다.**

## 인스턴스를 생성할 때 생성자에 필요한 자원을 넘겨주는 방식 - 생성자 의존 주입 - 의존 객체 주입의 한 형태 


- 클래스가 여러 자원 인스턴스를 지원
- 클라이언트가 원하는 자원(dictionary)를 사용

**[의존 객체 주입은 유연성 과 테스트 용이성을 높여준다.]**

```java
public class SpellChecker {
    private final Lexicon dictionary;

    public SpellChecker(Lexicon dictionary) {
        this.dictionary = Objects.requireNonNull(dictionary);
    }

    public boolean isValid(Srting word) {...}
    public List<String> suggestions(String typo) {...}
}
```

- 예시에서는 dictionary라는 딱 하나의 자원을 사용했지만, **자원이 몇 개든 의존 관계가 어떻든 상관없이 잘 동작한다.**
- 불변(immutable) 아이템을 보장하여 (같은 자원을 사용하려는) 여러 클라이언트가 의존 객체들을 안심하고 공유할 수 있기도 하다.
- `의존 객체 주입`은 **생성자, 정적 팩토리, 빌더** 모두에 똑같이 응용할 수 있다.

이 패턴의 변형으로, **생성자에 자원 팩토리를 넘겨주는 방식이 있다.**
- 팩토리란 호출할 때마다 특정 타입의 인스턴스를 반복해서 만들어주는 객체
- `팩토리 메서드 패턴(Factory Method Pattern)`
- Java 8에서 소개한 Supplier< T> 인터페이스가 팩토리를 표현한 완벽한 예시
- Supplier< T>를 입력으로 받는 메서드는 일반적으로 **한정적 와일드 카드 타입**을 사용해 **팩터리의 타입 매개변수**를 제한해야 한다.
    - 이 방식을 사용해 **클라이언트는 자신이 명시한 타입의 하위 타입**이라면 무엇이든 생성할 수 있는 팩터리를 넘길 수 있다.
    - ex) `Mosaic create(Supplier<? extends Tile> tileFactory)`

의존 객체 주입이 유연성과 테스트 용이성을 개선해주긴 하지만, 의존성이 수 천 개나 되는 큰 프로젝트에서는 코드를 어지럽게 만들기도 한다.
- Dagger, Guice, Spring같은 의존 객체 주입 프레임워크를 사용하면 이런 어질러짐을 해소헐 수 있다.

## 결론, 핵심정리
- 클래스가 내부적으로 하나 이상의 자원에 의존하고, 그 자원이 클래스 동작에 영향을 준다면 싱글턴과 정적 유틸리티 클래스는 사용하지 않는 것이 좋다.
- 이 자원들을 클래스가 직접 만들게 해서도 안된다. 대신 필요한 자원 혹은 팩터리를 생성자 혹은 정적 팩터리나 빌더에 넘겨주자.
- 의존 객체 주입이라 하는 이 기법은 클래스의 유연성, 재사용성, 테스트 용이성을 기막히게 개선해준다.









