# item16 public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라

## 인스턴스 필드들을 모아놓는 일 외에는 아무 목적도 없는 퇴보한 클래스들이 있다.

**이처럼 퇴보한 클래스는 public이어서는 안 된다!!**

```java
class Point {
    public double x;
    public double y;
}
```

- 이런 클래스는 data필드에 직접 접근할 수 있으니 캡슐화의 이점을 제공 X
- API를 수정하지 않고는 내부 표현을 바꿀 수도 없다.
- 불변식을 보장 X
- 외부에서 필드를 접근할 때 부수 작업을 수행할 수도 없다.

## 접근자와 변경자(mutator) 메서드를 활용해 데이터를 캡슐화한다.

필드들을 모두 private으로 만들고, public 접근자(getter)를 추가한다.

```java
class Point {
    private double x;
    private double y;

    public Point(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    public void setX(double x) { this.x = x; }
    public void setY(double y) { this.y = y; }
}
```

**public 클래스라면 이 방식이 확실히 맞다**
- **package 바깥에서 접근할 수 있는 클래스라면 접근자(getter, setter)를 제공**
- 클래스 내부 표현 방식을 언제든 바꿀 수 있는 유연성을 얻을 수 있다.
  
## package-private 클래스 혹은 private 중첩 클래스라면 데이터 필드를 노출한다 해도 하등의 문제가 없다.

이 방식은 클래스 선언 면에서나 이를 사용하는 Client 코드 면에서나 접근자 방식보다 훨씬 깔끔하다.

- 그 클래스가 표현하려는 추상개념만 올바르게 표현해주면 된다.
- Client 코드가 이 클래스 내부 표현에 묶이기는 하나, Client도 어차피 이 클래스를 포함하는 패키지 안에서만 동작하는 코드일 뿐이다.
- 따라서, package 바깥 코드는 전혀 손대지 않고도 데이터 표현 방식을 바꿀 수 있다.

private 중첩 클래스의 경우라면 수정 범위가 더 좁아져서 이 클래스를 포함하는 외부 클래스까지로 제한된다.

### public 클래스의 필드가 불변이라면 직접 노출할 때의 단점이 조금은 줄어들지만, 여전히 결코 좋은 생각이 아니다.

- API를 변경하지 않고는 표현 방식을 바꿀 수 없고, 필드를 읽을 때 부수 작업을 수행할 수 없다는 단점은 여전하다.
- 단, 불변식을 보장할 수 있게 된다.

**EX) Time 클래스는 각 인스턴스가 유효한 시간을 표현함을 보장한다.**

```java
public final class Time {
    private static final int HOURS_PER_DAY = 24;
    private static final int MINUTES_PER_HOUR = 60;

    public final int hour;
    public final int minute;

    public Time(int hour, int minute) {
        if(hour < 0 || hour >= HOURS_PER_DAY) {
            throw new IllegalArgumentException("시간: " + hour);
        }
        if(minute < 0 || minute >= MINUTES_PER_HOUR) {
            throw new IllegalArgumentException("분: " + minute);
        }
        this.hour = hour;
        this.minute = minute;
    }
    ...
}
```

## 결론
- **public 클래스는 절대 가변 필드를 노출해서는 안 된다!!**
  - 불변 필드라면 노출해도 덜 위험하지만 완전히 안심할 수 없다.
- **package-private 클래스나 private 중첩 클래스에서는 종종 불변이든 가변이늗 필드를 노출하는 편이 나을 때도 있다.** 