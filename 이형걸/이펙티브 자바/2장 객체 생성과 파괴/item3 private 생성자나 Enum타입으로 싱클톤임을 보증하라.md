# item3 : private 생성자나 Enum타입으로 싱클톤임을 보증하라

**`싱글톤(Singleton)`** : **instnace를 오직 하나만 생성할 수 있는 클래스를 의미**
- 싱글톤의 전형적인 예 
  - 무상태(stateless) 객체 
    - 인스턴스 변수가 없는 객체
  - 설계상 우일해야 하는 시스템 컴포넌트

싱글톤(singleton)의 문제점
- 클래스를 싱글톤으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있다.
  - 타입을 인터페이스로 정의한 다음 그 인터페이스를 구현해서 만든 싱글톤이 아니라면
  - singleton instance를 가짜(mock) 구현으로 대체할 수 없기 때문이다.

싱글톤(singleton)의 구현 방식
1. **public static final 필드 방식**
2. **정적 팩토리 방식**
3. **원소가 하나인 열거(Enum) 타입을 선언**
- 2 방식 모두 `생성자는 private`으로 감춘다.
- 유일한 instance에 접근할 수 있는 수단으로 `public static 멤버`를 하나 만든다.

 ## 1. public static final 필드 방식의 싱글톤

```java
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();

    private Elvis { ... }

    public void leaveTheBuilding() { ... }
}
```

- private생성자는 `public static final Elvis INSTANCE`를 초기화할 때, **딱 한번만 호출된다.**
- public, protected생성자가 없기 때문에 **Elvis클래스가 초기화될 때 만들어진 instance**가 `전체 시스템에서 하나뿐임이 보장`된다.
- Client는 손 쓸 방법이 없다.
  - 예외가 단 한가지 있긴 하다.
    - **권한이 있는 클라이언트**는 `리플렉션 API`인 `AccessibleObject.setAccesssible`을 사용해 private생성자를 호출할 수 있다.
    - **이러한 공격을 방어하려면 생성자을 수정하여 두 번째 객체가 생성되려 할 때 예외를 던지게 하면 된다.**

## 2. 정적 팩토리 방식의 싱글톤

```java
public class Elvus {
    private static final Elvis INSTANCE = new Elvis();

    private Elvis() { ... }

    public static Elvis getInstance() {
        return INSTANCE;
    }

    // 싱글톤임을 보장해주는 readResolve메서드
    private Object readResolve() {
        // '진짜' Elvis instance를 반환하고, 가짜 Elvis는 Garbage Collector에 맡긴다.
        return INSTANCE;
    }

    public void leaveTheBuilding() { ... }
}
```

- `Elvis.getInstance()` 는 항상 같은 객체의 참조를 반환하므로 제2의 Elvis instance는 만들어지지 않는다.
  - 하지만 역시 **권한이 있는 클라이언트**는 `리플렉션 API`인 `AccessibleObject.setAccesssible`을 사용해 private생성자를 호출할 수 있다는 예외 발생 가능하다.


### public static final 필드 방식의 싱글톤의 장점

1. 해당 클래스가 싱글톤임이 API에 명백히 드러나다는 것
- public static 필드가 final이니 절대 바꿀 수 없으므로, 절대로 다른 객체를 참조할 수 없다.

2. 간결성이 좋아진다.

### 정적 팩토리 방식의 싱글톤의 장점

1. (마음이 바뀌면) API를 바꾸지 않고도 싱글톤이 아니게 변경할 수 있다.
- 유일한 인스턴스를 반환하던 팩토리 메서드가 (예컨데) 호출하는 스레드별로 다른 인스턴스르 넘겨주게 할 수 있다.

2. 정적 팩토리를 Generic 싱글톤 팩토리로 만들 수 있다는 점
3. 정점 팩토리의 메서드 참조를 공급자(Supplier)로 사용할 수 있다는 점
- `Elvis::getInstance`를 `Supllier< Elvis>` 로 사용하는 식

이러한 장점들이 필요하지 않다면 public static final 필드 방식을 사용하자.

### 싱글톤 클래스를 직렬회 하는 방법

단순히 Serializable을 구현하는 것으로는 부족하다.
- **모든 인스턴스 필드를 일시적(transient)로 선언**하고
- **readResolve메서드를 제공**해야 한다.

이렇게 하지 않으면 직렬화된 인스턴스를 역직렬화할 때 마다 새로운 인스턴스가 생성된다.
- 가짜 Elvis 생성을 막으려면, Elvis클래스에 readResolve메서드를 생성하자.

```java
// 싱글톤임을 보장해주는 readResolve메서드
    private Object readResolve() {
        // '진짜' Elvis instance를 반환하고, 가짜 Elvis는 Garbage Collector에 맡긴다.
        return INSTANCE;
    }

```

## 원소가 하나인 열거(Enum) 타입을 선언

**[열거(Enum) 타입 방식의 싱글톤 - 바람직한 방법]**

```java
public enum Elvis {
    INSTANCE;

    public void leaveTheBuilding() { ... }
}
```

public 필드 방식과 비슷하지만, 
- 더 간결하고, 
- 추가 노력 없이 직렬화할 수 있고, 
- 심지어 아주 복잡한 직렬화 상황이나 리플렉션 공격에서도 제2의 instance가 생기는 일을 완벽히 막아준다.

**대부분 상황에서는 원소가 하나뿐인 열거(Enum)타입이 싱클톤을 만드는 가장 좋은 방법!!**
- 단, 만드련느 싱글톤이 Enum 외의 클래스를 상속해야 한다면 이 방법은 사용할 수 없다.
- 열거 타입이 다른 인터페이스를 구현하도록 선언할수는 있다.