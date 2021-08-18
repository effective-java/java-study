# 14장 람다와 스트림(Lambda & stream)

## 들어가면서

Java가 처음 등장한 1996년도 이후로 2번의 큰 변화가 있었는데,
1. JDK1.5 부터 추가된 지네릭스(generics)의 등장
2. JDK1.8 부터 추가된 람다식(lambda expression)의 등장

특히 람다식의 도입으로 인해, 이제 Java는 `객체지향언어인 동시에 함수형 언어`가 되었다.

## 람다식(lambda expression)이란?

**람다식(lambda expression)`** : **메서드를 하나의 식(expression)으로 표현한 것**
- 메서드를 람다식으로 표현하면 **메서드의 이름과 반환값이 없어지므로** 람다식을 `익명함수(anonymous function)` 라고도 한다.

```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random()*5) + 1);

int method() {
    return (int)(Math.random()*5) + 1;
}
```

위에서 `(i) -> (int)(Math.random()*5) + 1` 이 부분이 바로 람다식이다.

람다식과 일반 메서드(함수)와의 차이점
- 모든 메서드는 클래스에 포함되어야 하므로 클래스도 새로 만들어야 하고, 객체도 생성해야만 비로소 `int method()`를 호출할 수 있다.
- 그러나, 람다식은 이 모든 과정 없이 **오직 람다식 자체만으로도 이 메서드의 역할**을 대신할 수 있다.
  - 게다가 람다식으로 인해 **메서드를 변수처럼** 다루는 것이 가능해졌다!!
    - **메서드의 매개변수**로 전달되어지는 것이 가능
    - **메서드의 결과로 반환** 가능

## 람다식 작성하기

람다식은 `익명함수`답게 **메서드에서 이름과 반환타입을 제거**하고 **매개변수 선언부와 몸통{} 사이에 '->' 를 추가**한다.

```java
반환타입 메서드이름 (매개변수 선언) {
    문장들
}

--> 

(매개변수 선언) -> {
    문장들
}

EX) 
int max(int a, int b) {
    return a > b ? a : b;
}

--> 

(int a, int b) -> {
    return a > b ? a : b;
}
```

1. **반환값이 있는 메서드의 경우**
- return문 대신 `식(expression)`으로 대신 할 수 있다.
  - 식의 연산결과가 자동적으로 반환값이 된다.
- 이때는 `문장(statement)`이 아닌 `식`이므로 끝에 `;`을 붙이지 않는다.

2. **람다식에 선언된 매개변수의 타입은 추론이 가능한 경우에 생략 가능**
- **대부분의 경우 생략 가능**
- **람다식에 반환타입이 없는 이유**도 **항상 추론이 가능하기 때문이다.**

[주의] `(int a, b) -> a > b ? a : b` 에서 처럼 두 개의 매개변수 중 어느 하나의 타입만 생략하는 것은 허용되지 않는다. 

```java
(int a, int b) -> { return a > b ? a : b; }

--> 

(int a, int b) -> a > b ? a : b // 중괄호{}, return, ; 가 없어졌다.

-->

(a, b) -> a > b ? a : b
```

3. **선언된 매개변수가 하나뿐인 경우에는 소괄호()를 생략 가능**
- 단, 매개변수의 타입이 있으면 소괄호()를 생략할 수 없다.

4. **중괄호{} 안의 문장이 하나일 때는 중괄호{}를 생략 가능**
- 이 때 문장의 끝에 `;`를 붙이지 않아야 한다.

5. **괄호{} 안의 문장이 return문일 경우 중괄호{}를 생략할 수 없다.**

```java
// 3번 예시
a -> a * a // OK
int a -> a * a // Error

// 4번 예시
(String name, int i) -> {
    System.out.println(name+"="+i);
}

--> 

(name, i) -> System.out.println(name+"="+i)

// 5번 예시
(int a, int b) -> { return a > b ? a : b; } // OK
(int a, int b) ->   return a > b ? a : b  // Error
```

## 함수형 인터페이스(Functional Interface)

**Java에서 모든 메서드는 클래스 내에 포함되어야 하는데, 람다식은 어떤 클래스에 포함되는 것일까?**
- 람다식은 **익명 클래스의 객체**와 동등하다.
  
```java
(int a, int b) -> a > b ? a : b;

--> 

new Object() {
    int max(int a, int b) {
        return (int a, int b) -> a > b ? a : b;
    } 
}
```

**람다식으로 정의된 익명 객체의 메서드를 어떻게 호출할 수 있을 것인가?**
- **참조변수**가 있어야 객체의 메서드를 호출 할 수 있으니까 익명 객체의 주소를 참조 변수 f에 저장

```java
타입 f = (int a, int b) -> a > b ? a : b; // 참조변수의 타입을 뭐로 해야 할까?
```

**참조변수 f의 타입을 뭐로 해야 할까?**
- **참조형**이니까 **클래스 또는 인터페이스**가 가능
  - 그 안에 람다식과 동일한 메서드가 정의되어 있어야 한다.

```java
interface MyFunction {
    public abstract int max(int a, int b);
}

MyFunction f = new MyFunction() {
            int max(int a, int b) {
                return a > b ? a : b;
            } 
        };

int b = f.max(1, 2) // 익명 객체의 메서드를 호출
```

MyFunction인터페이스에 정의된 메서드 max()는 람다식 `(int a, int b) -> a > b ? a : b`과 메서드의 선언부가 일치한다. 
- 위 코드의 **익명 객체 f** -> **람다식**으로 아래와 같이 대체 가능하다.
  
```java
MyFunction f = (int a, int b) -> a > b ? a : b; // 익명 객체를 람다식으로 대체
int b = f.max(1, 2); // 익명 객체의 메서드를 호출
```

---

### **MyFunction인터페이스를 구현한 익명 객체를 람다식으로 대체가 가능한 이유**
- **람다식**도 실제로는 **익명 객체**이고, **MyFunction인터페이스를 구현한 익명 객체의 메서드 max()**와 **람다식의 매개변수와 타입과 개수 그리고 반환값이 일치하기 때문이다.**

**interface를 통해 람다식을 다루기로 결정**
- **람다식을 다루기 위한 인터페이스**를 **함수형 인터페이스(Functional Interface)**

```java
@FunctionalInterface
interface MyFunction { // 함수형 인터페이스 MyFunction을 정의
    public abstract int max(int a, int b);
}
```

[주의] 단, **함수형 인터페이스**에는 오직 **하나의 추상 메서드**만 정의되어 있어야 한다는 **제약**이 있다!!!
- 그래야 **람다식과 인터페이스의 메서드**가 **1:1로 연결**될 수 있기 때문이다.
- **반면에 static메서드와 default메서드의 개수에는 제약이 없다.**
  - 왜?? 
- `@FunctionalInterface`를 붙이면, `컴파일러`가 함수형 인터페이스를 올바르게 정의하였는지 확인해주므로, 꼭 붙이도록 하자!!

EX) lambda 사용 예시

```java
List<String> list = Arrays.asList("aaa", "bbb", "bbb","bbb","bbb");

Collections.sort(list, new Comparator<String>() {
    public int compare(String s1, String s2) {
        return s2.compareTo(s1);
    }
});

--> 

List<String> list = Arrays.asList("aaa", "bbb", "bbb","bbb","bbb");
Collections.sort(list, (s1, s2) -> s2.compareTo(s1));
```

### 함수형 인터페이스 타입의 매개변수와 반환타입

```java
@FunctionalInterface
interface MyFunction { // 함수형 인터페이스 MyFunction을 정의
    void myMethod(); // 추상 메서드
}
```

위와 같이 함수형 인터페이스가 정의되어 있을 때, 

#### 메서드의 매개변수가 함수형 인터페이스인 경우

**메서드의 매개변수(parameter)** 가 **MyFunction타입**이면,
- 이 메서드를 **호출**할 때, **람다식을 참조하는 참조변수**를 **매개변수로 지정**해야한다는 뜻
- 참조변수 없이 **직접 람다식을 매개변수로 지정하는 것도 가능**

```java
void aMethod(MyFunction m) { // 매개변수의 타입이 함수형 인터페이스
    m.myMethod(); // MyFunction에 정의된 메서드 호출
}
...

MyFunction f = () -> System.out.println(myMethod);
aMethod(f);

--> 

aMethod(() -> System.out.println(myMethod)); // 람다식을 매개변수로 지정
```

#### 메서드의 반환타입이 함수형 인터페이스인 경우

1. 함수형 인터페이스의 추상메서드와 동등한 람다식을 가리키는 참조변수를 반환
2. 람다식을 직접 반환

```java
MyFunction myMethod() {
    MyFunction f = ()->{};
    return f;
    // 한 줄로 줄이면, return () -> {};
}
```

**람다식을 참조변수로 다룰 수 있다는 것**
- 메서드를 통해 람다식을 주고 받을 수 있다는 것 -> 변수처럼 메서드를 주고받는 것이 가능해진 것
- 사실상 메서드가 아니라 객체를 주고받는 것이라 근본적으로 달라진 것은 없지만, 람다식 덕분에 예전보다 코드가 더 간결하고 이해하기 쉬워졌다.

**[람다식 사용 예제]**

```java
@FunctionalInterface
interface MyFunction {
	void run();  // public abstract void run();
}

class LambdaEx1 {
	static void execute(MyFunction f) { // 매개변수의 타입이 MyFunction인 메서드
		f.run();
	}

	static MyFunction getMyFunction() { // 반환 타입이 MyFunction인 메서드 
		MyFunction f = () -> System.out.println("f3.run()");
		return f;
	}

	public static void main(String[] args) {
		// 람다식으로 MyFunction의 run()을 구현
		MyFunction f1 = ()-> System.out.println("f1.run()");

		MyFunction f2 = new MyFunction() {  // 익명클래스로 run()을 구현
			public void run() {   // public을 반드시 붙여야 함
				System.out.println("f2.run()");
			}
		};

		MyFunction f3 = getMyFunction();

		f1.run();
		f2.run();
		f3.run();

		execute(f1);
		execute( ()-> System.out.println("run()") );
	}
}
```

```json
실행결과 
f1.run()
f2.run()
f3.run()
f1.run()
run()
```

### 람다식의 타입과 형변환

함수형 인터페이스로 람다식을 참조할 수 있는 것일 뿐, **람다식의 타입**이 **함수형 인터페이스의 타입**과 **일치하는 것은 아니다.**
- **람다식은 익명 객체**, **익명 객체는 타입이 없다.**
  - 정확히는 타입은 있지만 컴파일러가 임의로 이름을 정하기 때문에 알 수 없는 것 
- 그래서 대입 연산자의 양변의 타입을 일치시키기 위해 **형변환이 필요!!**

람다식은 MyFunction인터페이스를 직접 구현하진 않았지만, 이 인터페이스를 구현한 클래스의 객체와 완전히 동일하기 때문에 **형변환을 허용**
- **이 형변환은 생략가능하다.**

람다식은 이름이 없을 뿐, 분명 객체인데도, **Object타입으로 형변환 할 수 없다!!**
- 왜??
- **람다식은 오직 함수형 인터페이스로만 형변환이 가능!!**
- 굳이 Object타입으로 형변환하려면, 먼저 함수형 인터페이스로 변환해야 한다.

```java
@FunctionalInterface
interface MyFunction {
    void method();
}

MyFunction f = (MyFunction) (() -> {}); // 양변의 타입이 다르므로 형변환이 필요

Object obj = (Object) (() -> {}); // Error. 함수형 인터페이스로만 형변환 가능

Object obj = (Object)(MyFunction) (() -> {}); 
String str = (Object)(MyFunction) (() -> {}).toString(); 
```

**[람다식의 타입과 형변환 예제]**

```java
@FunctionalInterface
interface MyFunction {
	void myMethod();  // public abstract void myMethod();
}

class LambdaEx2 {
	public static void main(String[] args) 	{
		MyFunction f = ()->{}; // MyFunction f = (MyFunction)(()->{}); 
		Object obj = (MyFunction)(()-> {});  // Object타입으로 형변환이 생략됨
		String str = ((Object)(MyFunction)(()-> {})).toString();

		System.out.println(f);
		System.out.println(obj);
		System.out.println(str);

//		System.out.println(()->{});	// 에러. 람다식은 Object타입으로 형변환 안됨
		System.out.println((MyFunction)(()-> {}));
//		System.out.println((MyFunction)(()-> {}).toString()); // 에러
		System.out.println(((Object)(MyFunction)(()-> {})).toString());
	}
}
```

실행결과를 보면 **컴파일러**가 람다식의 타입을 어떤 형식으로 만들어내는지 알 수 있다.
- 일반적인 익명 객체의 객체의 타입 : `외부클래스$번호`
- 람다식의 타입 : `외부클래스이름$$Lambda$번호` 

### 외부 변수를 참조하는 람다식

**람다식**도 **익명 객체**, 즉 **익명 클래스의 인스턴스**이므로 람다식에서 외부에 선언된 변수에 접근하는 규칙은 `익명 클래스`에서 배운 것과 **동일하다.**

**[람다식 내에서 외부에 선언된 변수에 접근하는 방법을 보여주는 예제]**

```java
@FunctionalInterface
interface MyFunction {
	void myMethod();
}

class Outer {
	int val=10;	// Outer.this.val				

	class Inner {
		int val=20;	// this.val

		void method(int i) {  // 	void method(final int i) {
			int val=30; // final int val=30;
//			i = 10;      // 에러. 상수의 값을 변경할 수 없음.

			MyFunction f = () -> { // 람다식 내에서 지역변수 i와 val을 참조하고 있다.
				System.out.println("             i :" + i);
				System.out.println("           val :" + val);
				System.out.println("      this.val :" + ++this.val);
				System.out.println("Outer.this.val :" + ++Outer.this.val);	
			};

			f.myMethod();
		}
	} // Inner클래스의 끝
} // Outer클래스의 끝

class LambdaEx3 {
	public static void main(String args[]) {
		Outer outer = new Outer();
		Outer.Inner inner = outer.new Inner();
		inner.method(100);
	}
}
```

- **람다식 내에서 참조하는 지역변수**는 `final`이 붙지 않아도 **상수로 간주**된다.
- 람다식 내에서 지역변수 i와 val을 참조하고 있으므로, **람다식 내에서나 다른 어느 곳에서도 이 변수들의 값을 변경하는 일은 허용되지 않는다.**
- 반면에, Inner클래스, Outer클래스의 인스턴스 변수인 this.val, Outer.this.val은 상수로 간주되지 않으므로 변경해도 된다.
- **외부 지역변수와 같은 이름의 람다식 매개변수는 허용되지 않는다.**

```java
void method(int i) {
    MyFunction f = (i) -> { // Error!! 외부 지역변수와 같은 이름의 람다식 매개변수는 허용되지 않는다.
	    System.out.println("             i :" + i); // 람다식 내에서 지역변수 i와 val을 참조하고 있다.
	    System.out.println("           val :" + val);
	    System.out.println("      this.val :" + ++this.val);
	    System.out.println("Outer.this.val :" + ++Outer.this.val);	
    };
}
```

## java.util.function패키지

대부분의 메서드 타입은 매개변수가 없거나, 한 개, 두 개이고, 반환 값은 없거나 한개이다. 게다가 지네릭 메서드로 정의하면 매개변수나 반환 타입이 달라도 문제가 되지 않는다.

**`java.util.function패키지`** 에 **일반적으로 자주 쓰이는 형식의 메서드**를 **함수형 인터페이스로 미리 정의해 놓았다.**
- 매번 새로운 함수형 인터페이스를 정의하지 말고, 가능하면 이 패키지의 인터페이스를 활용하는 것이 좋다.
- 그렇게 해야, 함수형 인터페이스에 정의된 **메서드 이름도 통일**되고, **재사용성**이나 **유지보수** 측면에서도 좋다.

![java util function패키지의 주요 함수형 인터페이스](https://user-images.githubusercontent.com/56071088/129866597-3a0fe384-8f0b-4e67-8c2e-f3e41048b523.png)

**매개변수**와 **반환값의 유무**에 따라 **4개의 함수형 인터페이스가 정의**되어 있다.
- **Function의 변형**으로 `Predicate`가 있다.
  -  **반환값이 boolean**이라는 것만 제외하면 Function과 동일하다.
  -  Predicate는 **조건식을 함수로 표현**하는데 사용된다.

### 조건식의 표현에 사용되는 Predicate

`Predicate`는 **Function의 변형**으로, 반환타입이 boolean이라는 것만 다르다. Predicate는 조건식을 람다식으로 표현하는데 사용된다.
- 수학에서 


![매개변수가 2개인 함수형 인터페이스](https://user-images.githubusercontent.com/56071088/129867324-6913a4c1-3203-4065-9092-605337a40fd6.png)









