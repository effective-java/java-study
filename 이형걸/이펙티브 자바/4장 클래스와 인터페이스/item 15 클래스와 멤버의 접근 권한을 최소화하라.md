# item 15 클래스와 멤버의 접근 권한을 최소화하라

## 정보은닉, 캡슐화

- **잘 설계된 컴포넌트는 모든 내부 구현을 완벽히 숨겨, 구현과 API를 깔끔히 분리한다.**
- **오직 API를 통해서만 다른 컴포넌트와 소통하며 서로의 내부 동작 방식에는 전혀 개의치 않는다.**
- 소프트웨어 설계의 근간이 되는 원리

## 정보은닉의 장점
1. 시스템 개발 속도를 높인다.
   - 여러 컴포넌트를 병렬로 개발
2. 시스템 관리 비용을 낮춘다.
   - 각 컴포넌트를 더 빨리 파악, 디버깅, 다른 컴포넌트로 교체하는 부담도 줄어든다.
3. 성능 최적화에 도움을 준다.
   - 다른 컴포넌트에 영향을 주지 않고, target 컴포넌트만 최적화 할 수 있다.
4. 소프트웨어 재사용성을 높인다.
   - 독자적으로 동작하는 컴포넌트는 다른 환경에서도 유용하게 갖다 쓸 수 있다.
5. 큰 시스템을 제작하는 난이도를 낮춰준다.
   - 개발 단계에서 지속적으로 개별 컴포넌트들을 테스트 가능하다.

## 접근 제어자(access modifier)

### 접근 제어자의 기본 원칙 

**모든 클래스와 멤버의 접근성을 가능한 한 좁혀야 한다.**
- 소프트웨어가 올바로 동작하는 한 항상 가장 낮은 접근 수준을 부여해야 한다는 뜻

가장 바깥이라는 의미의 Top-Level class, interface에 부여할 수 있는 접근 수준 : `public`, `package-private`
- public : 공개 API
- package-private : 해당 패키지안에서만 사용 가능

### package 외부에서 쓸 일이 없다면 package-private 으로 선언하자.
- 외부 API가 아닌 내부 구현이 되어 **언제든지 수정이 가능**
- Client에 아무런 피해 없이 다음 release에서 수정, 교체, 제거 가능

### 한 클래스에서만 사용하는 package-private Top-Level class, interface는 이를 사용하는 클래스안에 private static으로 중첩시켜보자(item 24)
- public일 필요가 없는 클래스의 접근 수준을 package-private Top-Level 클래스로 좁히는 것


### 멤버(필드, 메서드, 중첩 클래스, 중첩 인터페이스)에 부여할 수 있는 접근 수준은 4가지이다.
- `private` : 멤버를 선언한 Top-Level class 에서만 접근할 수 있다.
- `package-private` : 멤버가 소속된 패키지안의 모든 클래스에서 접근할 수 있다.
    - 접근 제한자를 명시하지 않았을 때 적용되는 패키지 접근 수준
    - **interface의 멤버는 기본적으로 public이 적용된다.**
- `protected` : `package-private`의 접근 범위를 포함하며, 이 멤버를 선언한 클래스의 하위 클래스에서도 접근할 수 있다.
- `public` : 모든 곳에서 접근할 수 있다.

클래스의 공개 API를 설계(public) 후, 그 외의 모든 멤버는 private으로 만들자.
- 다음으로, 오직 같은 패키지의 다른 클래스가 접근해야 하는 멤버에 한하여 **private제어자를 제거해서 package-private으로 풀어주자.**
- 권한을 풀어주는 일을 너무 자주하게 된다면 시스템에서 컴포넌트를 더 분리해야 하는 것은 아닌지 다시 고민해보자

private, package-private 멤버는 모두 해당 클래스의 구현에 해당하므로 보통 공개 API에 영향을 주지 않는다.
- 단, Serializable을 구현한 클래스에서는 그 필드들도 의도치 않게 공개 API가 될 수 있다.

### public 클래스에서는 멤버의 접근 수준을 package-private에서 protected로 바꾸는 순간 그 멤버에 접근할 수 있는 대상 범위가 엄청나게 넓어진다.
- public 클래스의 protected 멤버는 공개 API이므로 영원히 지원돼야 한다. 
- 내부 동작 방식을 API 문서에 적어 사용자에게 공개해야 할 수도 있다.
- **protected멤버는 적을수록 좋다.**

### 멤버 접근성을 좁히지 못하게 방해하는 제약이 하나 있다. **상위 클래스의 메서드를 재정의할 때는 그 접근 수준을 그 접근 수준을 상위 클래스에서보다 좁게 설정할 수 없다는 것이다.**
- **상위 클래스의 인스턴스는 하위 클래스의 인스턴스로 대체해 사용할 수 있어야 한다는 규칙(LSP: Liskov Substitution Principle)**
- 이 규칙을 어긴다면 하위 클래스를 컴파일할 때 오류가 난다.

### 단지 코드를 Test하려는 목적으로 class, interface, member의 접근 범위를 넓히려 할 때가 있다.
- 적당한 수준까지는 넓혀도 된다.
- EX) class의 private멤버를 package-private까지 풀어주는 것은 허용할 수 있지만, 그 이상은 안된다.
- Test만을 위해서 공개 API(public)로 만들어서는 안 된다.
- Test Code를 Test대상과 같은 패키지에 두면 package-private 요소에 접근할 수 있기 때문이다.

**public 클래스의 인스턴스 필드는 되도록 public이 아니어야 한다.(item 16)**
- field가 가변 객체를 참조하거나, final이 아닌 인스턴스 필드를 public으로 선언하면 그 필드에 담을 수 있는 값을 제한할 힘을 잃게 된다.
- 필드가 수정될 때(lock 획득 같은) 다른 작업을 할 수 없게 되므로 **public 가변 필드를 갖는 클래스는 일반적으로 thread 안전하지 않다.**
- 필드가 final 이면서 불변 객체를 참조하더라도, 내부 구현을 바꾸거 싶어도 그 public 필드를 업애는 방식으로는 refactoring할 수 없게 된다.

### 해당 클래스가 표현하는 추상 개념을 완성하는 데 꼭 필요한 구성요소로써의 상수라면 `public static final` 필드로 공개해도 좋다.
- 이런 필드는 반드시 기본 타입 값이나 불변 객체를 참조해야 한다.

### **클래스에서 public static final 배열 필드를 두거나 이 필드를 반환하는 접근자 메서드를 제공해서는 안된다.**
- 길이가 0이 아닌 배열은 모두 변경 가능하기 때문이다.
- 이런 필드나 접근자를 제공한다면 Client에서 그 배열의 내용을 수정할 수 있게 된다.
- 아래의 2가지 방법 중, 어느 반환 타입이 더 쓰기 편할지, 성능은 어느쪽이 나을지를 고민해보자. 

```java
// 보안 허점이 숨어 있다.
public static final Things[] VALUES = {...};

// 해결책 1. public 배열을 private으로 만들고 public 불변 리스트를 추가하는 것
private static final Thing[] PRIVATE_VALUES = {...};
public static final List<Thing> VALUES = Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));

// 해결책 2. 배열을 private으로 만들고 그 복사본을 반환하는 public 메서드를 추가하는 것(방어적 복사)
private static final Thing[] PRIVATE_VALUES = {...};
public static final Thing[] values() {
    return PRIVATE_VALUES.clone();
}
```

## Java9 에서는 모듈 시스템이라는 개념이 도입되면서 2가지 암묵적 접근 수준이 추가되었다.
- package가 class들의 묶음, Module은 package들의 묶음
- module은 자신에 속하는 package 중 공개(export)할 것들을 (관례상 module-export.java 파일에) 선언한다.
- **protected, public 멤버라도 해당 package를 공개하지 않았다면 module외부에서는 접근할 수 없다!!**
- 모듈 시스템을 활용하면 class를 외부에 공개하지 않으면서도 같은 모듈을 이루는 패키지 사이에서는 자유롭게 공유할 수 있다.

이 암묵적 접근 수준들은 각각 public수준과 protected수준과 같으나, **그 효과가 모듈 내부로 한정되는 변종인 것이다.**

### 모듈에 적용되는 새로운 두 접근 수준은 상당히 주의해서 사용해야 한다.
- 모듈의 JAR파일을 자신의 모듈 경로가 아닌 **애플리케이션의 클래스패스(classpath)** 에 두면 그 모듈안의 모든 패키지는 마치 모듈이 없는 것처럼 행동한다.
- **모듈이 공개했는지 여부와 상관없이, public 클래스가 선언한 모든 public, protected 멤버를 모듈 밖에서도 접근할 수 있게 된다!!**
- 새로 등장한 이 접근 수준을 적극 활용한 대표적인 예가 `JDK 자체`이다.
- Java 라이브러리에서 공개하지 않은 패키지들은 해당 모듈 밖에서는 절대로 접근할 수 없다.

### 접근 보호 방식이 추가된 것 말고도, 모듈은 여러 면에서 Java 프로그래밍에 영향을 준다. 사실 모듈의 장점을 제대로 누리려면 해야할 일이 많다.
- 먼저 패키지들을 모듈 단위로 묶고,
- 모듈 선언에 패키지들의 모든 의존성을 명시한다.
- 그런 다음 소스 트리를 재배치하고,
- 모듈 안으로부터 모듈 시스템을 적용하지 않는 일반 패키지로의 모든 접근에 특별한 조치를 취해야 한다.

JDK 외에도 모듈 개념이 널리 받아들여지지 않아서, 꼭 필요한 경우가 아니라면 당분간은 사용하지 않는 것이 좋을 것이다.

## 결론
- **꼭 필요한 것만 골라 최소한의 public API를 설계하자.**
  - 그 외에는 class, interface, member가 의도치 않게 API로 공개 되는 일이 없도록 해야 한다.
- **public 클래스는 상수용 public static final 필드 외에는 어떠한 public 필드도 가져서는 안된다!!**
  - public static final 필드가 참조하는 객체가 불변인지 확인하라.