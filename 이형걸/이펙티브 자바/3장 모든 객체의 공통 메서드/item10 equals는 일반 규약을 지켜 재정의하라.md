# item10 equals는 일반 규약을 지켜 재정의하라

**[Object클래스의 equals메서드]**

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

- 두 객체의 같고 다름을 참조변수의 값으로 판단

- **서로 다른 두 객체를 equals메서드로 비교하면 항상 false값을 얻게 된다.**

- **객체를 생성할 때, 메모리의 비어있는 공간을 찾아 생성하므로 서로 다른 두개의 객체가 같은 주소를 갖는 일은 있을 수 없다.**
- 두 개 이상의 참조변수가 같은 주소값을 갖는 것(한 객체를 참조하는 것)은 가능하다.


## equals를 재정의하지 않는 상황
 
equals메서드를 그냥 두면 그 클래스의 인스턴스는 오직 자기 자신과만 같게 된다.

### equals를 재정의하지 말아야할 4가지 상황
1. **각 인스턴스가 본질적으로 고유하다.**
    - 값을 표현하는 것이 아닌 동작하는 개체를 표현하는 클래스
    - EX) `Thread`, Object의 equals메서드는 이러한 클래스에 딱 맞게 구현되었다.
2. **인스턴스의 '논리적 동치성(logical equality)'을 검사할 일이 없다.**
   - 객체의 참조변수 값이 아닌 객체가 가지고 있는 멤버변수들의 값을 비교하는 것이 논리적 동치성
   - 설계자는 클라이언트가 이 방식을 원하지 않거나 애초에 필요하지 않다고 판단할 수도 있다.
   - 설계자가 필요하지 않다고 판단했다면 Object의 기본 equals만으로 해결된다.
3. **상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는다.**
   - EX) `AbstractSet - Set구현체`, `AbstractList - List구현체`, `AbstractMap - Map구현체` 
   - 상위 클래스의 equals메서드를 상속 받아 그대로 쓴다.
4. **클래스가 private이거나 package-private이고 equals 메서드를 호출할 일이 없다.**

이 상황에서는 `equals메서드`를 Error를 호출하도록 Overriding할 수도 있다.

```java
@Override
public boolean equals(Objet obj) {
    throw new ArrserionError(); // 호출 금지!!
}
```

## equals를 재정의해야 하는 상황

```java
객체 식별성(object identity : 두 객체가 물리적으로 같은가)이 아니라 논리적 동치성(logical equality)를 확인해야 하는데,

상위 클래스의 equals가 논리적 동치성을 비교하도록 재정의되지 않았을 때다.
```
- 주로 **값 클래스**
- EX) `Integer, String`

값 클래스라고 해도, 같은 인스턴스가 2 이상 만들어지지 않음을 보장하는 `인스턴스 통제 클래스`, `Enum` 이라면 equals를 재정의하지 않아도 된다.
- 어차피 논리적으로 같은 인스턴스가 2개 이상 만들어지지 않으니, 논리적 동치성, 객체 식별성이 같은 의미가 된다.
- 이럴 때는 Object의 equals가 논리적 동치성까지 확인해준다고 본다.

## equals메서드를 재정의할 때의 규약

**equals 메서드는 동치관계(equivalence relation)를 구현하며, 다음을 만족한다.**

1. **반사성(reflexivity)** : null이 아닌 모든 참조값 x에 대해, x.equals(x)는 true다.
   - 객체는 자기 자신과 같아야 한다.
2. **대칭성(symmetry)** : null이 아닌 모든 참조값 x, y에 대해, x.equals(y)가 true면 y.equals(x)도 true다.
   - 두 객체는 서로에 대한 동치 여부에 똑같이 답해야 한다는 뜻.
- 다음과 같이 대소문자를 구분하지 않는 CaseInsensitiveString 클래스가 있다.

```java
public class CaseInsensitiveString {
    private final String s;

    public CaseInsensitiveString(String s) {
        this.s = s;
    }

    @Override // 대칭성 위배
    public boolean equals(Object o) {
        if (o instanceof CaseInsensitiveString) {
            return s.equalsIgnoreCase(((CaseInsensitiveString)o).s);
        }
        if (o instanceof String) {
            return s.equalsIgnoreCase((String)o);
        }
        return false;
	}

    @Override // String과 비교하는 부분을 없앤 제대로 만든 equals메서드
    public boolean equals(Object obj) {
        return obj instanceof CaseInsensitiveString && ((CaseInsensitiveString) obj).s.equalsIgnoreCase(s);
    }
}
```

- 이 클래스의 equals에서는 일반 문자열(String)과 비교하려 하고 있다.

```java
CaseInsensitiveString cis = new CaseInsensitiveString("Hello");
String s = "Hello";

cis.equals(s); //true
s.equals(cis); //false
```

- CaseInsensitiveString의 equals 자체는 잘 작동하지만 **그 반대는 작동하지 않는다.** String의 equals는 CaseInsensitiveString의 존재를 알지 못하기 때문이다.
- 이를 해결하기 위해서는 equals에서 **일반 String과 비교하는 부분을 없애야 한다.**


3. **추이성(transitivity)** : null이 아닌 모든 참조값 x,y,z에 대해, x.equals(y)가 true이고 y.equals(z)도 true면 x.equals(z)도 true다.

```java
// 2차원 점 클래스
public class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    @Override public boolean equals(Object o) {
        if (!(o instanceof Point)) {
            return false;
        }
        Point p = (Point)o;
        
        return p.x == x && p.y == y;
    }
}
```

```java
// Point 클래스를 확장한 ColorPoint 클래스 (값 추가)
public class ColorPoint extends Point {
    private final Color color;
    
    public ColorPoint(int x, int y, Color color){
        super(x, y);
        this.color = color;
    }
}
```

- ColorPoint 클래스에서 equals를 재정의 하지 않았기 때문에, Point의 구현이 상속되어 색상 정보는 무시된 채 비교를 수행함
    - 규약을 어긴 것은 아니지만, 중요한 정보를 놓치게 됨

**그럼 equals를 어떻게 정의해야할까?**

1. 비교대상이 또 다른 ColorPoint이고, **위치와 색상이 같을 때만 true**를 반환하도록 함
    
```java
// 이 equals 메소드는 대칭성을 위배함
@Override
public boolean equals(Object o) {
    if(!(o instanceof ColorPoint))
        return false;
    return super.equals(o) && ((ColorPoint) o).color == color;
}
```
    
- `Point.equals`는 색상을 무시하고, `ColorPoint.equals`는 입력 매개변수의 클래스 종류가 다르다며 매번 false만 반환할 것
- **`대칭성 위배`**

2. `ColorPoint.equals`가 Point와 비교할 때 색상을 무시하도록 함
    
```java
// 이 equals 메소드는 추이성을 위배함
@Override public boolean equals(Object o) {
    if (!(o instanceof Point))
        return false;
    
    // o가 일반 Point면 색상을 무시하고 비교한다.
    if (!(o instanceof ColorPoint))
        return o.equals(this);
    
    // o가 ColorPoint면 색상까지 비교한다.
    return super.equals(o) && ((ColorPoint) o).color == color;
}
```
    
- p1과 p2, p2와 p3의 비교에선 색상을 무시했지만, p1과 p3의 비교에서는 색상까지 고려했기 false 반환
  - **대칭성은 만족하지만, `추이성을 깨버림`**
- 이 방식은 무한 재귀에 빠질 위험도 있음
  - 이 방식의 equals를 적용한 Point의 또 다른 하위 클래스와 ColorPoint 비교를 진행한다면 StackOverflowError를 일으킴

**객체 지향적 추상화의 이점을 포기하지 않는 한, 구체 클래스를 확장해 새로운 값을 추가하면서 equals 규약을 만족시킬 방법은 존재하지 않음**

3. equals 안의 instanceof 검사를 **getClass 검사**로 바꾸면 규약도 지키고, 값도 추가하면서 구체 클래스를 상속할 수 있을까?
    
```java
// 이 equals 메소드는 리스코프 치환 원칙(LSP)을 위배함
@Override
public boolean equals(Object o) {
    if (o == null || o.getClass() != getClass())
        return false;
        Point p = (Point) o;
        return p.x == x && p.y == y;
    }
```
    
- **같은 구현 클래스의 객체와 비교할 때만 true를 반환함**
- **Point의 하위 클래스는 어디서든 Point로써 활용될 수 있어야 하는데**, 이 방식은 그렇지 못하기에 활용할 수 없음
- **`리스코프 치환 원칙(LSP) 위배`**

```java
리스코프 치환 원칙(Liskov Substitution Principle) : 어떤 타입에 있어 중요한 속성이라면 그 하위 타입에서도 마찬가지로 중요하다. 따라서 그 타입의 모든 메서드가 하위 타입에도 똑같이 잘 작동해야 한다.
- Point의 하위 클래스는 정의상 여전히 Point이므로 어디서든 Point로써 활용될 수 있아야 한다. 는 말이다.
```

**괜찮은 우회방법 - 상속 대신 컴포지션을 사용(item18)**

```java
// equals 규약을 지키면서 값 추가하기
public class ColorPoint {
    private final Point point;
    private final Color color;

    public ColorPoint(int x, int y, Color color) {
        point = new Point(x, y);
        this.color = Objects.requireNonNull(color);
    }

    // 이 ColorPoint의 Point 뷰(view)를 반환
    public Point asPoint() {
        return point;
    }

    @Override public boolean equals(Object o) {
        if (!(o instanceof ColorPoint))
            return false;
        ColorPoint cp = (ColorPoint) o;
        return cp.point.equals(point) && cp.color.equals(color);
    }

    ...
}
```

- **Point를 ColorPoint의 private 필드**로 두고, 
- **ColorPoint와 같은 위치의 일반 Point를 반환하는 뷰(view) 메소드를 public으로 추가(asPoint())**
- Java 라이브러리에도 구체 클래스를 확장해 값을 추가한 클래스가 있음
  - ex. `java.sql.TimeStamp` - `java.util.Date` 를 확장하고 nanoseconds 필드 추가
    - TimeStamp의 equals는 대칭성을 위배하여 Date와 섞어 쓸 때 주의해야 함
    - 둘을 명확히 분리하여 사용한다면 상관없지만, 섞이지 않도록 보장해줄 수단은 없음

```json
추상클래스의 하위 클래스라면 equals 규약을 지키면서도 값을 추가 할 수 있음. → 상위 클래스를 직접 인스턴스화 할 수 없다면 지금까지의 문제들은 일어나지 않음
```

4. **일관성(consistency)** : null이 아닌 모든 참조 값 x,y 에 대해, x.equals(y)를 반복해서 호출하면 항상 true를 반환하거나 항상 false를 반환한다.

- 두 객체가 같다면 수정되지 않는 한 앞으로도 영원히 같아야 함
  - 가변객체는 비교시점에 따라 다르지만, 불변 객체는 한번 다르면 끝까지 달라야 함
  - 불변클래스는 equals가 한번 같다고 한 객체와는 영원히 같다고 답하고, 다르다고 한 객체와는 영원히 다르다고 답하도록 만들어야 함
- **equals의 판단에 신뢰할 수 없는 자원이 끼어들게 해서는 안됨**
  - **문제를 일으키지 않으려면 equals는 항시 메모리에 존재하는 객체만을 사용한 결정적 계산만 수행해야 한다!!**
  
5. **null-아님** : null이 아닌 모든 참조 값 x에 대해, x.equals(null)은 false다.

- 모든 객체가 null과 같지 않아야 함
- 명시적 null 검사 → 필요없음

```java
@Override
public boolean equals(Object o){
    if(o == null)
        return false;
}
```

- 묵시적 null 검사 - 이쪽이 낫다.

```java
@Override
public boolean equals(Object o){
    if(!(o instanceof MyType))
        return false;
    MyType mt = (MyType) o;
    ...
}
```

- **instanceof는 두 번째 피연산자와 무관하게 첫번째 피연산자가 null이면 false 반환하기 때문에**
- **null 검사를 명시적으로 하지 않아도 됨**


**위의 규약들은 equals를 재정의할 때 무적권 지켜야 한다!!**

## **양질의 equals 메소드 구현 방법**

- == 연산자를 사용해 입력이 **자기 자신의 참조인지 확인**한다.
- `instanceof` 연산자로 입력이 올바른 타입인지 확인한다.
- 입력을 올바른 타입으로 **형변환(Casting)** 한다.
- 입력 객체와 자기 자신의 대응되는 '핵심'필드들이 모두 일치하는지 하나씩 검사한다.
- **적절한 비교 연산자 사용**
  - 기본타입 필드 : `==` 
  -  참조 타입 필드 : `equals 메소드` 
  -  float와 double 필드 : `Float.compare(float, float)과 Double.compare(double, double)` 로 비교
  - 배열 필드의 모든 원소가 핵심 필드라면 `Arrays.equals` 메소드들 중 하나를 사용
  - null을 정상값으로 취급하는 참조 타입 필드는 `Object.equals(Object, Object)`로 비교
    - NullPointerException 발생 예방
  - 복잡한 필드를 가진 클래스는 그 필드의 표준형을 저장해둔 후, 표준형끼리 비교 (불변 클래스에 제격)
- **적절한 비교 순서**
  - 다를 가능성이 더 크거나, 비교하는 비용이 싼 필드부터 먼저 비교
  - 동기화용 lock 필드같이 객체의 논리적 상태와 관련 없는 필드는 비교하면 안됨
  - 핵심 필드로부터 계산해낼 수 있는 파생 필드는 굳이 비교할 필요는 없지만 비교하는 쪽이 더 빠른 경우도 있음
    - 파생 필드가 객체 전체의 상태를 대표하는 경우

**equals를 다 구현했다면 다음을 자문해보자 - 대칭적인가? 추이성이 있는가? 일관적인가?**
- 단위 테스트 진행. `AutoValue`로 작성할 경우 생략 가능

### **주의사항**

- **equals를 재정의할 땐 hashCode도 반드시 재정의할 것**
- **너무 복잡하게 해결하려 들지 말 것**
  - 필드들의 동치성만 검사해도 equals규약을 어렵지 않게 지킬 수 있다.
- **Object 외의 타입을 매개변수로 받는 equals 메소드는 선언하지 말 것**
    - `@Override` 를 일관되게 사용함으로써 실수를 예방

## **전형적인 equals 메소드**

```java
public final class PhoneNumber {
    private final short areaCode, prefix, lineNum;

    public PhoneNumber(int areaCode, int prefix, int lineNum) {
        this.areaCode = rangeCheck(areaCode, 999, "지역코드");
        this.prefix   = rangeCheck(prefix,   999, "프리픽스");
        this.lineNum  = rangeCheck(lineNum, 9999, "가입자 번호");
    }

    private static short rangeCheck(int val, int max, String arg) {
        if (val < 0 || val > max)
            throw new IllegalArgumentException(arg + ": " + val);
        return (short) val;
    }

    @Override 
    public boolean equals(Object o) {
        // 1. == 연산자를 사용해 입력이 자기 자신의 참조인지 확인한다.
        if (o == this)
            return true;
        // 2. instanceof 연산자로 입력이 올바른 타입인지 확인한다.
        if (!(o instanceof PhoneNumber))
            return false;
        
        // 3. 입력을 올바른 타입으로 형변환(Casting)한다.
        PhoneNumber pn = (PhoneNumber)o;
        
        return pn.lineNum == lineNum && pn.prefix == prefix
                && pn.areaCode == areaCode;
    }

    // 나머지 코드는 생략 - hashCode 반드시 구현할 것
}
```

## **구글이 만든 AutoValue 프레임워크**

- equals, hashCode 메소드를 작성하고 테스트해줌
  - 클래스에 Annotation 하나만 추가하면 AutoValue가 이 메소드들을 알아서 작성해줌

## 결론
- 꼭 필요한 경우가 아니면 equals를 재정의하지 말자.
- Object.equals가 많은 경우의 비교를 정확히 수행해준다.
- 재정의해야 할 때는 그 클래스의 핵심 필드 모두를 빠짐 없이, 다섯가지 규약을 확실히 지켜가며 비교해야 한다.




