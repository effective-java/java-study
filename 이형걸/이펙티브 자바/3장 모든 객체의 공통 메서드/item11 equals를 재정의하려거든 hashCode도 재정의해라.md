# item11 equals를 재정의하려거든 hsahCode도 재정의해라

## equals를 재정의한 클래스 모두에서 hashCode도 재정의해야 한다!!

그렇지 않으면 hashCode 일반 규약을 어기게 되어 해당 클래스의 인스턴스를 HashMap, HashSet 같은 컬렉션의 원소로 사용할 때 문제를 일으킬 것이다.

## **Object 명세 중 hashCode 관련 규약**

1. equals 비교에 사용되는 정보가 변경되지 않았다면, 애플리케이션이 실행되는 동안 그 객체의 hashCode 메소드는 몇 번을 호출해도 일관되게 항상 같은 값을 반환해야 한다. 단, 애플리케이션을 다시 실행한다면 이 값이 달라져도 상관없다.
2. **equals(Object)가 두 객체를 같다고 판단했다면, 두 객체의 hashCode는 똑같은 값을 반환해야 한다.**
3. equals(Object)가 두 객체를 다르다고 판단했더라도, 두 객체의 hashCode가 서로 다른 값을 반환할 필요는 없다. 단, 다른 객체에 대해서는 다른 값을 반환해야 해시테이블의 성능이 좋아진다.

hashCode를 재정의를 잘못했을 때 **equals(Object)가 두 객체를 같다고 판단했다면, 두 객체의 hashCode는 똑같은 값을 반환해야 한다.** 는 조항이 가장 문제가 된다.
- **즉, 논리적으로 같은 객체는 같은 hashCode를 반환해야 한다.**

### hashCode를 재정의하지 않은 PhoneNumber 클래스 테스트

- item10에 나왔던 PhoneNumber클래스

```java
Map<PhoneNumber, String> m = new HashMap<>();
m.put(new PhoneNumber(707, 867, 5309), "제니");

System.out.println(m.get(new PhoneNumber(707, 867, 5309)));  // null
```
PhoneNUmber클래스는 hashCode()를 재정의하지 않았기 때문에 **논리적 동치인 두 객체가 서로 다른 해시코드를 반환하여 두 번째 규약을 지키지 못한다.**

- get메소드 실행 시, "제니"가 아닌 null이 반환됨
  - 2개의 PhoneNumber 인스턴스가 사용되면서 다른 hashCode를 반환하기 때문에 get메소드가 엉뚱한 hash Bucket에 가서 객체를 찾음
  - 만약에 두 인스턴스가 같은 hash Bucket에 담겨있더라도, HashMap은 해시코드가 다른 엔트리끼리는 동치성 비교를 시도조차 하지 않도록 최적화되어 있기 때문에 똑같이 null을 반환한다.

**적절한 `hashCode`를 재정의해준다면 해결된다.**

## **최악의 hashCode 구현법(하지만 적법하긴 함)**

```java
@Override 
public int hashCode() { return 42; }
```

- 동치인 모든 객체에 똑같은 해시코드를 반환하니 적법함
- **but, 모든 객체가 Hash Table의 Bucket 하나에 담겨 마치 연결 리스트처럼 동작함**
    
**→ 평균 수행 시간이 O(1)에서 O(n)으로 느려짐**

## **좋은 hashCode 구현법**

- 좋은 해시 함수 : **서로 다른 인스턴스에 다른 해시코드를 반환한다.** 이것이 3번째 규약이 요구하는 속성이다.
- 이상적인 해시 함수 : 주어진 인스턴스들을 **32 Bit 정수 범위에 균일하게 분배한다.**

### 좋은 hashCode를 작성하는 요령

### 1. **int 변수 result를 선언한 후 값 c로 초기화한다.**
- c는 해당 객체의 `첫번째 핵심 필드`를 단계 2.a 방식으로 계산한 해시코드
- 핵심 필드 : equals에 사용되는 필드

### 2. **해당 객체의 나머지 핵심 필드 f 각각에 대해 다음 작업을 수행한다.**
1. 해당 필드의 해시코드 c를 계산한다.
   1. **기본 타입 필드** : Type.hashCode(f)를 수행한다.
      - Type : **해당 기본 타입의 박싱 클래스(Wrapper class)다.**
   2. **참조 타입 필드**면서 동시에 equals 메소드가 이 필드의 equals를 재귀적으로 호출해 비교한다면, **이 필드의 hashCode를 재귀적으로 호출한다.** 
      - 계산이 더 복잡해질 것 같으면, **이 필드의 표준형(canonical reporesentation)을 만들어 그 표준형의 hashCode를 호출한다.** 
      - 필드의 값이 null이면 전통적으로 0을 사용한다.
   3. **필드가 배열**이라면, **핵심 원소 각각을 별도 필드처럼 다룬다.** 
    - 이상의 규칙을 재귀적으로 적용해 각 핵심 원소의 해시코드를 계산한 다음, 
    - 단계 2.b 방식으로 갱신한다. 
    - 배열에 핵심 원소가 하나도 없다면 단순히 상수(0)를 사용한다. 모든 원소가 핵심 원소라면 Arrays.hashCode를 사용한다.
2. 단계 2.a에서 계산한 해시코드 c로 result를 갱신한다. 코드로는 다음과 같다
- `result = 31 * result + c;`
        
### 3. **result를 반환한다.**

### 4. hashCode를 다 구현했다면, 이 메소드가 동치인 인스턴스에 대해 똑같은 해시코드를 반환할지 검증하기 위해 단위테스트를 작성할 것

- **파생 필드는 해시코드 계산에서 제외해도 된다.**
  - 즉, 다른 필드로부터 계산해 낼 수 있는 필드는 모두 무시해도 됨
  - 또한 **equals 비교에 사용되지 않은 필드는 반드시 제외해야 함**
- 단계 2.b의 곱셈 **31 * result는 필드를 곱하는 순서에 따라 result 값이 달라지게 함**
  - String의 hashCode를 곱셈 없이 구현한다면 모든 `아나그램(anagram, 구성하는 철자가 같고 순서만 다른 문자열)`의 해시코드가 같아질 것이다.
  - **곱할 숫자가 31인 이유**
    - `31`이 `홀수`, `소수(prime)` 이기 때문이다.
    - 만약 짝수를 곱한다면 시프트 연산과 같은 결과를 내기 때문에 overflow가 발생해 정보를 잃게 된다.
    - 소수를 곱하는 이유는 명확하지 않지만 전통적으로 그렇게 해왔다.
    - 시프트 연산과 뺄셈으로 대체해 최적화 할 수 있음 → `31 * i = (i << 5) - i`

## **전형적인 hashCode 메소드**

- PhoneNumber 클래스에 적용한 hashCode 메소드

```java
@Override
public int hashCode() {
    int result = Short.hashCode(areaCode);
    result = 31 * result + Short.hashCode(prefix);
    result = 31 * result + Short.hashCode(lineNum);
    return result;
}
```

- 이 메소드는 PhoneNumber 인스턴스의 핵심 필드 3개만을 사용해 계산을 수행한다.
    - 비결정적(undeterministic) 요소가 전혀 없기 때문에 동치 인스턴스들은 같은 해시코드를 가질 것이다.
- 단순하고, 충분히 빠르고, 서로 다른 전화번호들은 다른 해시 버킷들로 분배해준다.

## **구아바의 com.google.common.hash.Hashing**

- 위에서 소개한 제작 요령도 품질이나 기능면에서 충분히 훌륭하지만, 
- **단, 해시 충돌이 더욱 적은 방법을 꼭 써야 한다면 구아바의 com.google.common.hash.Hashing을 참고하자.**

## **Object 클래스의 hash**

Object 클래스는 임의의 개수만큼 객체를 받아 해시코드를 계산해주는 정적 메서드인 `hash`를 제공함

이 메소드를 활용하면 앞의 요령대로 구현한 코드와 비슷한 수준의 hashCode 함수를 단 한줄로 작성할 수 있다.
- 하지만 **속도는 더 느리다.**
  - 입력 인수를 담기 위한 배열이 만들어지고, 입력 중 기본 타입이 있다면 박싱(auto-boxing)과 언박싱(unboxing)도 거쳐야 하기 때문이다.
- hash 메소드는 성능에 민감하지 않은 상황에서만 사용할 것

### Object클래스의 hashCode()를 적용한 Phone Number클래스의 hashCode 메소드

- 성능이 아쉽다.

```java
@Override
public int hashCode(){
    return Objects.hash(lineNum, prefix, areaCode);
}
```

## **hash 캐싱**

클래스가 **불변(immutable)**이고 해시코드를 계산하는 비용이 크다면, **캐싱(caching)하는 방식**을 고려해야 함

- 이 타입의 객체가 주로 **해시의 키로 사용되는 경우**
  - **인스턴스가 만들어질 때 해시코드를 계산해 둬야 한다.**
- **해시의 키로 사용되지 않는 경우엔 지연 초기화(lazy initialization) 전략 적용함**
  - 즉, **hashCode가 처음 불릴 때 계산**
  - 필드를 지연 초기화 하려면 **Thread-Safe**까지 고려해야 함


### 해시코드를 지연 초기화하는 hashCode 메소드 - 쓰레드 안정성(Thread-Safe)까지 고려해야 한다.

- hashCode 필드의 초기값은 흔히 생성되는 객체의 해시코드와는 달라야 함

```java
private int hashCode;  // 자동으로 0으로 초기화함

@Override
public int hashCode(){
    int result = hashCode;
    if (result == 0){
        result = Short.hashCode(areaCode);
        result = 31 * result + Short.hashCode(prefix);
        result = 31 * result + Short.hashCode(lineNum);
        hashCode = result;
    }
    return result;
}
```

### **주의 사항**

1. **해시코드를 계산할 때 핵심 필드를 생략해서는 안 된다.**
   - 속도가 빨라질 순 있지만, 해시 품질이 나빠져 해시테이블의 성능을 심각하게 떨어뜨릴 수 있음
2. **hashCode가 반환하는 값의 생성 규칙을 API 사용자에게 자세히 공표하지 말자.**
   - 그래야 클라이언트가 이 값에 의지하지 않게 되고, 추후에 계산 방식을 바꿀 수도 있음
   - 자세한 규칙을 공표하지 않는다면, 해시 기능에서 결함을 발견했거나 더 나은 해시 방식을 알아낸 경우 다음 릴리스에서 수정할 수 있음

## **결론**
- equals를 재정의할 때는 hashCode도 반드시 재정의해야 한다. 그렇지 않으면 프로그램이 제대로 동작하지 않을 것이다.
- 재정의한 hashCode는 Object의 API 문서에 기술된 일반 규약을 따라야 하며, 서로 다른 인스턴스라면 되도록 해시코드도 서로 다르게 구현해야 한다.
- 재정의 하기 어렵다면 `AuvoValue 프레임워크` 를 이용하자. equals, hashCode메서드를 자동으로 만들어준다.

