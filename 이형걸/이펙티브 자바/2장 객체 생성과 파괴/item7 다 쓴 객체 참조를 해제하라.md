# item7 다 쓴 객체 참조를 해제하라

## JAVA처럼 Garbage Collector가 있는 언어라도 메모리 관리에 신경써야 한다.

**[메모리 누수가 일어나는 위치는 어디인가?]**

```java
public class Stack {
	private Obejct[] elements;
	private int size = 0;
	
	private static final int DEFAULT_INITAL_CAPACITY = 16;

	public Stack() {
		elements = new Object[DEFAULT_INITAL_CAPACITY];
	}

	public void push(Object e) {
		ensureCapacity();
		elements[size++];
	}

	public Object pop() {
		if (size == 0) {
			throw new EmptyStackException();
		}
		return elements[--size];
	}

	private void ensureCapacity() {
		if (elements.length == size) {
			elements = Arrays.copyOf(elements, 2 * size + 1);
		}
}
```

- 이 code의 문제점은 `메모리 누수`
- 이 코드에서는 stack이 커졌다가 줄어들었을 때 stack에서 꺼내진 객체들을 Garbage Collector가 회수하지 않는다.
- stack이 그 객체들의 **다 쓴 참조(obsolete reference)** 를 여전히 가지고 있기 때문이다.

`다 쓴 참조(obsolete reference)` : **다시 쓰지 않을 참조**
- 위 코드에서 elements 배열의 활성 영역 밖의 참조들이 다 쓴 참조에 해당
- 활성 영역은 index가 size보다 작은 원소들로 구성된다.

Garbage Collection 언어에서는 **의도치 않게 객체를 살려두는 메모리 누수**를 찾기가 아주 까다롭다.
- 객체 참조 하나를 살려두면 그 객체가 참조하는 모든 객체를 회수해가지 못한다.
- 그래서 단 몇 개의 객체가 매우 많은 객체를 회수하지 못하게 할 수 있고 잠재적으로 성능에 악영향을 줄 수 있다.

**해법은 간단**
- 해당 참조를 다 썼을 때 `null 처리(참조 해제)`하면 된다.
- 다 쓴 참조를 null 처리하면, null 처리한 참조를 다시 사용하려 할 때, NullPointerException 발생으로 인한 오류의 빠른 발견의 이점도 따라온다.

**[제대로 구현한 pop 메서드]**

```java
	public Object pop() {
		if (size == 0) {
			throw new EmptyStackException();
		}
		Object result = elements[--size];
		elements[size] = null; // 다 쓴 참조 해제
		return result;
	}
```

## 참조 해제 하는 조건은?

**모든 객체를 다 쓰자마자 일일이 null 처리하는 것은 바람직하지도 않고 그럴 필요도 없다.**
- `객체 참조를 널처리하는 일은 예외적인 경우여야 한다.`
- 가장 좋은 방법은 그 참조를 담은 변수를 유효 범위 밖으로 밀어내는 것이다.
- 변수의 범위를 최소가 되게 정의했다면(아이템 57) 이 일은 자연스럽게 이뤄진다.

**그렇다면 null 처리는 언제 해야 할까?**
- Stack 클래스가 메모리 누수에 취약한 것은 **Stack이 자기 메모리를 직접 관리하기 때문이다.**
- 이 스택은 elements 배열로 저장소 풀을 만들어 원소들을 관리한다.
- 배열의 활성 영역에 속한 원소들이 사용되고 비활성 영역은 쓰이지 않는 것을 **프로그래머만 알고 가비지 컬렉터는 알 길이 없고 가비지 컬렉터가 보기에는 똑같이 유효한 객체이기 때문이다.**
- **프로그래머는 비활성 영역이 되는 순간 null 처리해서 해당 객체를 더는 쓰지 않을 것임을 Garbage Collector에 알려야 한다.**

## 메모리 누수의 원인

**일반적으로 자기 메모리를 직접 관리하는 클래스라면 프로그래머는 항시 메모리 누수에 주의해야 한다.**
- 원소를 다 사용한 즉시 그 원소가 참조한 객체들을 다 null 처리 해줘야 한다.
  
**캐시 역시 메모리 누수를 일으키는 주범이다.**
- **캐시 외부에서 키를 참조하는 동안만 엔트리가 살아 있는 캐시**가 필요하다면 `WeakHashMap`을 사용하자.
- 보통은 시간이 지날수록 엔트리의 가치를 떨어뜨리는 방식을 사용하고, 이런 방식에서는 쓰지 않는 엔트리를 이따금 청소해줘야 한다.
  - ScheduledThreadPoolExceutor 같은 백그라운드 스레드를 활용
  - 캐시에 새 엔트리를 추가할 때 부수 작업으로 수행(쓰지 않는 엔트리를 청소)
  - `LinkedHashMap` : remveEldestEntry메서드를 써서 캐시에 새 엔트리를 추가할 때, 쓰지 않는 엔트리를 청소한다.
  - 더 복잡한 캐시를 만들고 싶다면 `java.lang.ref패키지`를 직접 활용해야 할 것이다.

**메모리 누수의 세 번째 주범은 바로 리스너 혹은 콜백이다.**
- 클라이언트가 콜백을 등록만 하고 명확히 해지하지 않는다면, 콜백은 게쏙 쌓여갈 것이다.
- 이럴 때 콜백을 약한 참조(weak reference)로 저장하면 Garbage Collector가 즉시 수거해간다.
- 예를 들어 WeakHashMap에 Key로 저장하면 된다.

## 결론
- 메모리 누스는 겉으로는 잘 드러나지 않는다.
- 이러한 누수는 철저한 코드 리뷰나 힙 프로파일러 같은 디버깅 도구를 동원해야만 발견되기도 한다
- **따라서, 예방법을 익혀두는 것이 매우 중요한다.**
