# item3

# private 생성자나 열거 타입으로 싱글턴임을 보증하라

### 싱글턴

- 인스턴스를 오직 하나만 생성할 수 있는 클래스를 말한다.
    - ex) 함수와 같은 무상태 객체, 설계상 유일해야 하는 시스템 컴포넌트
- 클래스를 싱글턴으로 만들면 이를 사용하는 클라이언트를 테스트하기가 어려워질 수 있다.
    - 타입을 인터페이스로 정의한 다음 해당 인터페이스를 구현해서 만든 싱글턴이 아니라면 싱글턴 인스턴스를 가짜(mock) 구현으로 대체할 수 없기 때문이다.
- 만드는 방식
    - 두 방식 모두 생성자는 private로 감춰두고, 유일한 인스턴스에 접근할 수 있는 수단으로 public static 멤버를 하나 마련해둔다.
    1. public static 멤버가 final 필드인 방식
        - private 생성자는 public static final 필드인 Elvis.INSTANCE를 초기화할 때 한번만 호출된다.
        - public, protected 생성자가 없으므로 Elvis 클래스가 초기화될 때 만들어진 인스턴스가 전체 시스템에서 하나뿐임이 보장된다.
            - 예외: 권한이 있는 클라이언트는 리플렉션 api인 AccessibleObject.setAccessible을 사용해 private 생성자를 호출할 수 있다.
            - 이러한 공격을 방어하려면 생성자를 수정하여 두 번째 객체가 생성되려 할 때 예외를 던지도록 한다.
        - 장점
            - 해당 클래스가 싱글턴임이 API에 명백히 드러난다
            - public static 필드가 final 이니 절대로 다른 객체를 참조할 수 없다
            - 간결하다

        ```java
        public class Elvis {
            **public static final Elvis INSTANCE = new Elvis();**
            private Elvis() { ... }

            public void leaveTheBuilding() { ... }
        }
        ```

    2. 정적 팩터리 메서드를 public static 멤버로 제공
        - Elvis.getInstance는 항상 같은 객체의 참조를 반환하므로 제2의 Elvis 인스턴스는 만들어지지 않다
            - 리플렉션을 통한 예외는 똑같이 적용
        - 장점
            - API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있다.
            - 정적 팩터리를 제네릭 싱글턴 팩터리를 만들 수 있다.
            - 정적 팩터리의 메서드 참조를 공급자(supplier)로 사용할 수 있다.
                - Elvis::get Instance → Supplier<Elvis>

        ```java
        public class Elvis {
        	 	private static final Elvis INSTANCE = new Elvis();
            private Elvis() { }
            **public static Elvis getInstance() { return INSTANCE; }**

            public void leaveTheBuilding() {...}
        }
        ```

    - 두 가지 방식으로 만든 싱글턴 클래스를 직렬화하는 경우 Serializable을 구현하는 것만으로 부족
        - 모든 인스턴스 필드를 일시적(transient)이라고 선언하고 readResolve 메서드를 제공해야 한다.
        - 그렇지 않으면 직렬화된 인스턴스를 역직렬화할 때 마다 새로운 인스턴스가 생성된다

        ```java
        public Object readResolve() {
            // 진짜 Elvis를 반환하고, 가짜 Elvis는 가비지 컬렉터에 맡긴다.
            return INSTANCE;
          }
        }
        ```

    3. 원소가 하나인 열거 타입을 선언

    - public 필드 방식과 비슷하지만  더 간결하고, 직렬화 가능하고 좋다
    - 단, 만들려는 싱글턴이 Enum 외의 클래스를 상속해야 한다면 사용 불가

    ```java
    public enum Elvis {
      INSTANCE;

      public void leaveTheBuilding() {...}
    }
    ```