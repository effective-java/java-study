# item1

# 생성자 대신 정적 팩터리 메서드를 고려

### 클라이언트가 클래스의 인스턴스를 얻는 수단

- public 생성자: 전통적인 수단
- 정적 팩터리 메서드(static factory method)
    - 디자인 패턴에서의 팩터리 메서드와 다름

```java
public static Boolean valueOf(boolean b) {
		return b ? Boolean.True : Boolean.False;
}
```

### 장점

1. 이름을 가질 수 있다.
    - 생성자에 넘기는 매개변수와 생성자 자체만으로는 반환될 객체의 특성을 제대로 설명하기 어렵다
    - BigTnteger(int, int, Random) vs Biginteger.probablePrime
    - 하나의 시그니처로는 생성자를 하나만 만들 수 있다(?)
        - 입력 매개변수들의 순서를 다르게 한 생성자를 새로 추가하는 방식은 좋지 않은 방식
        - 정적 팩터리 메서드는 생성자를 정적 팩터리 메서드로 바꾸고 각각의 차이를 잘 드러내는 네이밍
2. 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다
    - 이로 인핸 불변 클래스는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 불필요한 객체 생성 피할 수 있다
    - 생성 비용이 큰 같은 객체가 자주 요청되는 상황이라면 성능 향상, 플라이웨이트 패턴과 비슷
    - 정적 팩터리 방식의 클래스 == 인스턴스 통제 클래스
3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.
    - API를 만들 때 이 유연성을 응용하면 구현 클래스를 공개하지 않고도 객체를 반환할 수 있어 api를 작게 유지 가능
    - 인터페이스를 정적 팩터리 메서드의 반환 타입으로 사용하는 인터페이스 기반 프레임워크를 만드는 핵심 기술
4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.
    - 클라이언트는 팩터리가 건네주는 객체가 어느 클래스의 인스턴스인지 알 수도 없고 알 필요도 없다.

    ```java
    public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
         Enum<?>[] universe = getUniverse(elementType);
         if (universe == null)
             throw new ClassCastException(elementType + " not an enum");

         if (universe.length <= 64)
             return new RegularEnumSet<>(elementType, universe);
         else
             return new JumboEnumSet<>(elementType, universe);
     }
    ```

5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다
    - 이러한 유연함은 서비스 제공자 프레임워크를 만드는 근간이 된다
        - JDBC

        ```java
        class Car {
            public static Car newTruck() {
                Class<?> truckClz = Class.forName("me.javarouka.vehicle.Truck");
                return truckClz.newInstance();
            }

            public static Car newBus() {
                Class<?> busClz = Class.forName("me.javarouka.vehicle.Bus");
                return busClz.newInstance();
            }
        }
        ```

### 단점

1. 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다

### 정적 팩터리 메서드에 흔히 사용하는 명명 방식

- from: 매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드

```java
Date d = Date.from(instance);
```

- of: 여러 매겨변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드

```java
Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);
```

- valueOf: from과 ofdml 더 자세한 버전
- instance / getInstance: (매개변수를 받는다면) 매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다

```java
StackWalker luke = StackWalker.getInstance(options);
```

- create / newInstance: 위의 내용과 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다
- get"Type": getInstace와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다.

```java
FileStore fs = Files.getFilsStore(path);
```

- new"Type": newInstance와 같으나, 생성할 클래스가 아닌 다른 클래스에 팩터리 메서드를 정의할 때 쓴다.
- type: getType과 newType의 간결한 버전

```java
List<Complaint> litany = Collections.list(legacyLitany);
```