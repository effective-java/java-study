# 14장 람다와 스트림(Lambda & stream)

## 들어가면서

Java가 1996년에 처음 등장한 이후로 2번의 큰 변화다 있었는데, 
1. 첫번째는 **JDK1.5**부터 추가된 **지네릭스(generics)의 등장**
2. 두번째는 **JDK1.8**부터 추가된 **람다식(Lambda expression)의 등장**이다.

특히, 람다식의 도입으로 인해, 이제 Java는 **객체지향언어인 동시에 함수형 언어**가 되었다.

## 람다식(Lambda expression)이란?

**`람다식(Lambda expression)`** : **메서드를 하나의 식(expression)으로 표현한 것**
- 람다식은 **함수**를 간략하면서도 **명확한 식**으로 표현할 수 있게 한다.
- 메서드를 람다식으로 표현하면 **메서드의 이름과 반환값이 없어지므로**, 람다식을 **`익명함수(anonymous function)`** 이라고도 한다.

```java
int[] arr = new int[5];
Arrays.setAll(arr, (i) -> (int)(Math.random()*5)+1);

int method() {
    return (int)(Math.random()*5)+1;
}
```

**람다식은 오직 람다식 자체만으로도 이 메서드의 역할을 대신할 수 있다.**
- 모든 메서드는 클래스에 포함되어야 하므로 클래스도 새로 만들어야 하고, 객체도 생성해야만 비로소 이 메서드를 호출할 수 있다. 
- 그러나 람다식은 이 모든 과정이 필요가 없다.

**람다식으로 인해 메서들르 변수처럼 다루는 것이 가능해졌다.**
- **메서드의 매개변수**로 전달되어지는 것이 가능
- **메서드의 결과로 반환**될 수 있다.

## 람다식 작성하기

메서드에서 **이름과 반환타입을 제거**하고 **매개변수 선언부**와 **몸통{} 사이에** `->` 를 추가한다.

**[람다식 작성법 예시]**

```java
// 기존의 메서드 작성
반환타입 메서드이름 (매개변수 선언) {
    문장들
}

--> 

// 람다식 작성
(매개변수 선언) -> {
    문장들
}

ex)

// 기존의 메서드 작성
int max(int a, int b) {
    return a > b ? a : b;
}

--> 

// 람다식 작성
(int a, int b) -> {
    return a > b ? a : b;
}
```

1. **반환값이 있는 메서드의 경우**return문 대신 `식(expression)`으로 대신 할 수 있다.
- 식의 연산결과가 자동적으로 반환값이 된다.
- 이때는 문장(statement)이 아닌 식(expression)이므로 끝에 `;`를 붙이지 않는다.

```java
(int a, int b) -> { return a > b ? a : b; }

-->

(int a, int b) -> a > b ? a : b
```

1. **람다식에 선언된 매개변수의 타입은 추론이 가능한 경우 생략이 가능하다.**
- **대부분의 경우 생략이 가능하다.**
- 람다식에 반환타입이 없는 이유도 항상 추론이 가능하기 때문이다.
- 두 매개변수 중 어느 하나의 타입만 생략하는 것은 허용되지 않는다.

```java
(int a, int b) -> a > b ? a : b

--> 

(a, b) -> a > b ? a : b
```

3. **선언된 매개변수가 하나뿐인 경우에는 소괄호()를 생략할 수 있다.**
- 단, 매개변수의 타입이 있으면 소괄호()를 생략할 수 없다.

```java
a -> a + a //ok
int a -> a + a //Error
```

4. **중괄호{}안의 문장이 하나일 때는 중괄호{}를 생략할 수 있다.**
- 이때, 문장의 끝에 `;`를 붙이지 않아야 한다는 것에 주의하자.
- 중괄호{} 안의 문장이 return문인 경우 중괄호{}를 생략할 수 없다.

```java
ex1)
(String name, int i) -> {
    System.out.println(name + i);
}

--> 

(String name, int i) -> System.out.println(name + i)

ex2)
(int a, int b) -> { return a > b ? a : b; } //ok

-->

(int a, int b) -> return a > b ? a : b; //Error
```

## 함수형 인터페이스(Functional Interface)

**람다식**은 **익명 클래스의 객체**와 **동등**하다.

람다식으로 정의된 익명 객체의 메서드를 어떻게 호출 할 수 있을까?
1. 참조변수가 있어야 객체의 메서드를 호출할 수 있다.
2. 그 참조변수의 타입은 클래스 또는 인터페이스가 가능하다.
3. 람다식과 동등한 메서드가 정의되어 있어야 한다.

```java
타입 f = (int a, int b) -> a > b ? a : b; // 참조변수의 타입을 뭐로 해야 할까?

----------------------------

interface MyFunction {
    public abstract int max(int a, int b);
}

MyFunction f = new MyFunction() {
    public int max(int a, int b) {
        return a > b ? a : b;
    }
}

int b = f.max(1, 2); // 익명 객체의 메서드를 호출

--> 

MyFunction f = (int a, int b) -> a > b ? a : b; // 익명 객체를 람다식으로 대체
int b = f.max(1, 2); / 익명 객체의 메서드를 호출
```

MyFunction인터페이스를 구현한 **`익명 객체`** 를 **`람다식`** 으로 **대체가 가능한 이유**
- **람다식도 실제로는 익명 객체**
- MyFunction인터페이스를 구현한 익명 객체의 메서드max()와 람다식의 **매개변수의 타입과 개수 그리고 반환값이 일치**하기 떄문이다.

---

**`함수형 인터페이스`** : **람다식을 다루기 위한 인터페이스**

```java
@FunctionalInterface
interface MyFunction {
    public abstract int max(int a, int b); 
}
```

**함수형 인터페이스에는 오직 하나의 추상 메서드만 정의되어 있어야 한다는 제약이 있다.**
- 그래야 람다식과 인터페이스의 메서드가 1:1로 연결될 수 있기 때문이다.
- **반면에 static메서드와 default메서드의 개수에는 제약이 없다.**
  - 왜??
- **`@FunctionalInterface`** 를 붙이면, 컴파일러가 함수형 인터페이스를 올바르게 정의하였는지 확인해주므로, 꼭 붙이도록 하자.


**[람다식 사용 예시]**

```java
List<String> list = Arrays.asList("aaa", "aaa","aaa","aaa","aaa");

Collections.sort(list, new Comparator<String>() {
    public int compare(String s1, String s2) {
        return s2.compareTo(s1);
    }
});

-->

List<String> list = Arrays.asList("aaa", "aaa","aaa","aaa","aaa");
Collections.sort(list, (s1, s2) -> s2.compareTo(s1));
```

### 함수형 인터페이스 타입의 매개변수와 반환타입

1. **메서드의 매개변수가 함수형 인터페이스인 경우**
- 이 메서드를 호출할 때 **람다식을 참조하는 참조변수**를 **매개변수로 지정**해야 한다는 의미
- 또는 참조변수 없이 **직접 람다식을 매개변수로 지정**

```java
void aMethod(MyFunction f) {
    f.myMethod();
}

MyFunction f = () -> System.out.println("MyMethod");
aMethod(f);

-->

aMethod(() -> System.out.println("MyMethod"));
```

2. **메서드의 반환타입이 함수형 인터페이스인 경우**
- 함수형 인터페이스의 **추상 메서드**와 **동등한 람다식**을 가리키는 **참조변수를 반환**
- **람다식을 직접 반환**

```java
MyFunction myMethod() {
    MyFunction f  = () -> {};
    return f;
    // return () -> {};
}
```

**람다식 덕분에 변수처럼 메서드를 주고 받는 것이 가능해졌다!!**
- 사실상 메서드가 아니라 **객체를 주고받는 것**이라 **근본적으로 달라진 것은 아무것도 없다.**

**[함수형 인터페이스 타입의 매개변수와 반환타입 에제]**

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
f2.un()
f3.run()
f1.run()
run()
```

### 람다식의 타입과 형변환

**람다식의 타입이 함수형 인터페이스(Functional Interface)의 타입과 일치하는 것은 아니다.**
- **람다식**은 **익명객체**이고, **익명객체는 타입이 없다.**
- 정학하게 타입은 있지만, 컴파일러가 임의로 정하기 때문에 알 수 없는 것이다.

1. 람다식이 인터페이스를 구현한 객체와 **메서드의 매개변수의 개수와 타입, 반환값이 일치**하기 때문에 **형변환을 허용**한다.
- 이 형변환은 **생략 가능**하다.

```java
@FunctionalInterface
interface MyInterface {
    public abstract void method();
}

MyFunction f = (MyFunction) (()->{});
```

2. 람다식은 이름은 없지만 분명 객체인데도, **Object타입으로 형변환 할 수 없다.**
- **람다식은 오직 함수형 인터페이스로만 형변환이 가능하다.**

```java
Object obj = (Object) (()->{}); //Error

Object obj = (Object) (MyFuncion) (()->{}); 
String str = ( (Object) (()->{}) ).toString(); 
```

**[람다식의 타입과 형변환 예제]**

```java
@FunctionalInterface
interface MyFunction {
   void myMethod();  // public abstract void myMethod();
}

class LambdaEx2 {
   public static void main(String[] args)    {
      MyFunction f = ()->{}; // MyFunction f = (MyFunction)(()->{}); 
      Object obj = (MyFunction)(()-> {});  // Object타입으로 형변환이 생략됨
      String str = ((Object)(MyFunction)(()-> {})).toString();

      System.out.println(f);
      System.out.println(obj);
      System.out.println(str);

//      System.out.println(()->{});   // 에러. 람다식은 Object타입으로 형변환 안됨
      System.out.println((MyFunction)(()-> {}));
//      System.out.println((MyFunction)(()-> {}).toString()); // 에러
      System.out.println(((Object)(MyFunction)(()-> {})).toString());
   }
}
```

- 일반적인 익명객체 타입 : `외부클래스이름$번호`
- 람다식의 타입 : `외부클래스이름$$Lambda$번호`

### 외부 변수를 참조하는 람다식

**람다식도 익명객체, 즉 익명 클래스의 인스턴스이므로 람식에서 외부에 선언된 변수에 접근하는 규칙은 익명 클래스에서의 규칙과 동일하다.**

**[람다식내에서 외부에 선언된 변수에 접근하는 방법을 보여주는 예제]**

```java
@FunctionalInterface
interface MyFunction {
   void myMethod();
}

class Outer {
   int val=10;   // Outer.this.val            

   class Inner {
      int val=20;   // this.val

      void method(int i) {  //    void method(final int i) {
         int val=30; // final int val=30;
//         i = 10;      // 에러. 상수의 값을 변경할 수 없음.

         MyFunction f = () -> {
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

- **람다식 내에서 참조하는 지역변수는 final이 붙지 않았어도 상수로 간주된다.**
  - 이 예시에서는 람다식 내에서 지역변수 i, val을 참조하고 있으므로 **람다식 내에서나 다른 어느 곳에서도 이 변수들의 값을 변경하는 일은 허용되지 않는다.**
- 반면에, **Inner클래스와 Outer클래스의 인스턴스 변수인 this.val과 Outer.this.val은 상수로 간주되지 않으므로 값을 변경해도 된다.**
- **외부 지역변수와 같으 이름의 람다식 매개변수는 허용되지 않는다.**
  - 아래의 예시에서 매개변수로 넘어온 지역변수인 int i를 람다식의 매개변수로 사용하는 것을 허용하지 않는다.
 
```java
void method(int i) {
   MyFunction f = (int i) -> { // Error. 외부 지역변수와 이름이 주복됨
      System.out.println("             i :" + i);
      System.out.println("           val :" + val);
      System.out.println("      this.val :" + ++this.val);
      System.out.println("Outer.this.val :" + ++Outer.this.val)
   };
}
```

## java.util.function패키지

**`java.util.function패키지`** 에 **일반적으로 자주 쓰이는 형식의 메서드를 함수형 인터페이스로 미리 정의**해 놓았다. 
- 매번 새로운 함수형 인터페이스를 정의하지 말고, 가능하면 이 패키지의 인터페이스를 활용하는 것이 좋다.
  - **메서드의 이름 통일, 재사용성, 유지보수성 증가**  

![java util function패키지의 주요 함수형 인터페이스](https://user-images.githubusercontent.com/56071088/130580336-648639a0-d484-4988-a32b-149581712ca4.png)

타입문자 `T` 는 `Type`, `R`은 `Return Type`

- 매개변수와 반환값의 유무에 따라 4개의 함수형 인터페이스가 정의됨
- Function의 변형으로 Predicate
- **`Predicate`** : 반환값이 boolean인 점, 나머지는 Fucntion과 동일
  - **조건식**을 표현하는데 사용된다.
  - **조건식**을 람다로 표현하는데 사용됨

**[조건식의 표현에 사용되는 Predicate 예시]**

```java
Predicate<String> isEmptyStr = s -> s.length()==0;
String s = "";

if(isEmptyStr.test(s)) { // if(s.length() == 0)
   System.out.println("This is an empty String.");
}
```

**[함수형 인터페이스 사용 예시]**

```java
import java.util.function.*;
import java.util.*;

class LambdaEx5 {
	public static void main(String[] args) {
		Supplier<Integer>  s = ()-> (int)(Math.random()*100)+1;
		Consumer<Integer>  c = i -> System.out.print(i+", "); 
		Predicate<Integer> p = i -> i%2==0; 
		Function<Integer, Integer> f = i -> i/10*10; // i의 일의 자리를 없앤다.

		List<Integer> list = new ArrayList<>();	
		makeRandomList(s, list);
		System.out.println(list);
		printEvenNum(p, c, list);
		List<Integer> newList = doSomething(f, list);
		System.out.println(newList);
	}

	static <T> List<T> doSomething(Function<T, T> f, List<T> list) {
		List<T> newList = new ArrayList<T>(list.size());

		for(T i : list) {
			newList.add(f.apply(i));
		}	

		return newList;
	}

	static <T> void printEvenNum(Predicate<T> p, Consumer<T> c, List<T> list) {
		System.out.print("[");
		for(T i : list) {
			if(p.test(i))
				c.accept(i);
		}	
		System.out.println("]");
	}

	static <T> void makeRandomList(Supplier<T> s, List<T> list) {
		for(int i=0;i<10;i++) {
			list.add(s.get());
		}
	}
}
```

### 매개변수가 2개인 함수형 인터페이스

![매개변수가 두 개인 함수형 인터페이스](https://user-images.githubusercontent.com/56071088/130580339-834e3436-45af-443e-8156-e8dc8825b069.png)

**매개변수가 2개인 함수형 인터페이스**는 이름 앞에 **`접두사 Bi`** 가 붙는다.
- 알파벳에서 `T`의 다음 문자인 `U, V, W` 를 매개변수의 타입으로 사용하는 것일 뿐 별다른 의미는 없다.
- `Supplier`는 매개변수가 없고 반환값만 존재하는데, 메서드는 2개의 값을 반환할 수 없으므로 BiSupplier가 없는 것이다.

**2개 이상의 매개변수를 갖는 함수형 인터페이스가 필요하다면 직접 만들어야 한다.**

**[3개의 매개변수를 갖는 함수형 인터페이스 예시]**

```java
@FunctionalInterface
interface TriFunction<T,U,V,R> {
   R apply(T t, U u, V v);
}
```
### UnaryOperator와 BinaryOperator

![UnaryOperator와 BinaryOperator](https://user-images.githubusercontent.com/56071088/130580342-ec018eba-b19a-4f48-a580-efb6e19d0d99.png)

- Function의 또 다른 변형
- **매개변수의 타입과 반환타입이 모두 일치한다.**
- UnaryOperator, BinaryOperator의 조상은 각각 Function, BiFunction이다.

### 컬렉션 프레임워크와 함수형 인터페이스

![컬렉션 프레임워크와 함수형 인터페이스](https://user-images.githubusercontent.com/56071088/130580343-c0b9d7ce-13c2-433d-bb23-93a738be464c.png)

- 컬렉션 프레임워크의 인터페이스에 다수의 디폴트 메서드들이 추가됨.
  - **그 중의 일부는 함수형 인터페이스를 사용**
  - 단순화 하기 위해 와일드 카드는 생략했다.

**[컬렉션 프레임워크의 함수형 인터페이스 사용 예시]**

```java
import java.util.*;

class LambdaEx4 {
	public static void main(String[] args) 	{
		ArrayList<Integer> list = new ArrayList<>();
		for(int i=0;i<10;i++)
			list.add(i);

		// list의 모든 요소를 출력
		list.forEach(i->System.out.print(i+","));
		System.out.println();

		// list에서 2 또는 3의 배수를 제거한다.
		list.removeIf(x-> x%2==0 || x%3==0);
		System.out.println(list);

      // list의 각 요소에 10을 곱한다.
		list.replaceAll(i->i*10); 
		System.out.println(list);

		Map<String, String> map = new HashMap<>();
		map.put("1", "1");
		map.put("2", "2");
		map.put("3", "3");
		map.put("4", "4");

		// map의 모든 요소를 {k,v}의 형식으로 출력한다.
		map.forEach((k,v)-> System.out.print("{"+k+","+v+"},"));
		System.out.println();
	}
}
```

```java
// list의 모든 요소를 출력
list.forEach(i->System.out.print(i+","));

// list에서 2 또는 3의 배수를 제거한다.
list.removeIf(x-> x%2==0 || x%3==0);

// list의 각 요소에 10을 곱한다.
list.replaceAll(i->i*10); 

// map의 모든 요소를 {k,v}의 형식으로 출력한다.
map.forEach((k,v)-> System.out.print("{"+k+","+v+"},"));
```

### 기본형을 사용하는 함수형 인터페이스

![기본형을 사용하는 함수형 인터페이스](https://user-images.githubusercontent.com/56071088/130580345-6e650f43-3224-42ed-b927-c16c9caf53d6.png)

기본형 타입을 처리할 때는 Wrapper Class를 사용하지 않고, 보다 효율적으로 처리할 수 있도록 **기본형을 사용하는 함수형 인터페이스들을 제공한다.**

**[기본형을 사용하는 함수형 인터페이스를 사용하는 예제]**

```java
import java.util.function.*;
import java.util.*;

class LambdaEx6 {
	public static void main(String[] args) {
		IntSupplier  s = ()-> (int)(Math.random()*100)+1;
		IntConsumer  c = i -> System.out.print(i+", "); 
		IntPredicate p = i -> i%2==0; 
		IntUnaryOperator op = i -> i/10*10; // i의 일의 자리를 없앤다.

		int[] arr = new int[10];

		makeRandomList(s, arr);
		System.out.println(Arrays.toString(arr));
		printEvenNum(p, c, arr);
		int[] newArr = doSomething(op, arr);
		System.out.println(Arrays.toString(newArr));
	}

	static void makeRandomList(IntSupplier s, int[] arr) {
		for(int i=0;i<arr.length;i++) {
			arr[i] = s.getAsInt();  // get()이 아니라 getAsInt()임에 주의
		}
	}

	static void printEvenNum(IntPredicate p, IntConsumer c, int[] arr) {
		System.out.print("[");
		for(int i : arr) {
			if(p.test(i))
				c.accept(i);
		}	
		System.out.println("]");
	}

	static int[] doSomething(IntUnaryOperator op, int[] arr) {
		int[] newArr = new int[arr.length];

		for(int i=0; i<newArr.length;i++) {
			newArr[i] = op.applyAsInt(arr[i]); // apply()가 아님에 주의
		}	

		return newArr;
	}
}
```

**`IntUnaryOperator`** :  **`Function<Integer, Integer>`** , **`IntFunction`** 보다 **AutoBoxing & Unboxing** 의 **횟수가 줄어들어 더 성능이 좋다.**
- IntToIntFunction이 없는 이유 :
  - IntUnaryOperator가 그 역할을 하기 때문이다.

**매개변수의 타입**과 **반환타입이 일치**할 때는 Function대신 **UnaryOperator를 사용하자.**  

```java
import java.util.function.*;
import java.util.*;

// Error!! a의 타입을 알 수 없으므로 선언 불가
// Function f  = (a) -> 2*a; 

// OK. 매개변수의 타입과 반환타입이 Integer
Function<Integer, Integer> f = (a) -> 2*a;

// OK. 매개변수 타입은 int, 반환타입은 Integer
IntFunction<Integer> f = (a) -> 2*a;

// 매개변수 타입은 int, 반환타입은 int 
IntUnaryOperator f = (a) -> 2*a; 
```

## Function의 합성과 Predicate의 결합

```java
Function
default <V> Function<T, V> andThen(Function<? super R, ? extends V> after)
default <V> Function<V, R> compose(Function<? super V, ? extends T> before)
static <T> Function<T, T> identity()

Predicate
default Predicate<T> and(Predicate<? super T> other)
default Predicate<T> or(Predicate<? super T> other)
default Predicate<T> negate()
static <T> Predicate<T> isEqual(Object targetRef)
```

**`java.util.function패키지`** 의 **함수형 인터페이스**에는 추상메서드 외에도 **default 메서드, static 메서드**가 정의되어 있다.
- 원래 Function인터페이스는 반드시 2개의 타입을 지정해 줘야하기 때문에, 2 타입이 같아도 Function< T> 라고 쓸 수 없다. 
- **반드시 Function< T,T> 라고 써야 한다.**

### Function의 합성

수학에서 두 함수를 합성해서 하나의 새로운 함수를 만들어내는 것 처럼, **두 람다식을 합성해서 새로운 람다식을 만들 수 있다.**
- 두 함수의 합성은 어느 함수를 먼저 적용하느냐에 따라 달라진다.
  - 함수 f, g가 있을 때
  - EX1) `f.andThen(g)` : 함수 f를 적용하고, 그 다음에 함수 g를 적용한다.
  - EX2) `f.compose(g)` : g를 먼저 적용하고, f를 적용한다.

**[Function인터페이스의 default 메서드인 andThen 사용 예시]**
- 문자열을 숫자로 변환하는 함수 g, 숫자를 2진 문자열로 변환하는 함수 g를 andThen()으로 합성

```java
Function<String, Integer> f = (s) -> Integer.parseInt(s, 16);
Function<Integer, String> g = (i) -> Integer.toBinaryString(i);

Function<String, String> h = f.andThen(g);

System.out.println(h.apply("FF")); // "FF" -> 255 -> "11111111"
```

**[Function인터페이스의 default 메서드인 compose 사용 예시]**
- compose를 사용해서 반대로 함수를 합성

```java
Function<Integer, String> g = (i) -> Integer.toBinaryString(i);
Function<String, Integer> f = (s) -> Integer.parseInt(s, 16);

Function<Integer, Integer> h = f.compose(g);

System.out.println(h.apply(2)); // 2 -> "10" -> 16
```

**[Function인터페이스의 static 메서드인 identity() 사용 예시]**
- **`identity()`** : 함수를 적용하기 이전과 이후가 동일한 `항등 함수`가 필요할 때 사용한다.
- 이 함수를 람다식으로 표현 : `x -> x`
- 항등 함수는 함수에 x를 대입하면 결과가 x인 함수를 말한다. `f(x) = x` 
- 항등 함수는 잘 사용하지 않는 편
- **나중에 map()으로 변환작업할 때, 변환없이 그대로 처리하고자할 때 사용된다.**

```java
Function<String, String> f = x -> x;
// Function<String, String> f = Function.identity(); // 위의 문장과 동일

System.out.println(f.apply("AAA")); // AAA가 그대로 출력됨.
```

### Predicate의 결합

여러 조건식을 논리 연산자인 `&&, ||, !` 으로 연결해서 하나의 식을 구성할 수 있는 것 처럼, **여러 Predicate을 and(), or(), negate()로 연결해서 하나의 Predicate로 결합할 수 있다.**

**[Predicate인터페이스의 default메서드 사용 예시]**

```java
Predicate<Integer> p = i -> i < 100;
Predicate<Integer> q = i -> i < 200;
Predicate<Integer> r = i -> i % 2 == 0;
Predicate<Integer> notP = p.negate(); // i >= 100

// 100 <= i && (i < 200 || i%2 == 0)
Predicate<Integer> all = notP.and.(q.or(r));

Predicate<Integer> all = notP.and(i -> i < 200).or(i -> i%2 == 0);
```

Predicate의 끝에 negate()를 붙이면 조건식 전체가 부정이 된다.

**[Predicate인터페이스의 static메서드인 isEqual() 사용 예시]**
- **`isEqual()`** : 두 대상을 비교하는 Predicate를 만들 때 사용
- isEqual()의 매개변수로 비교대상을 하나 지정
- 또 다른 비교대상은 test()의 매개변수로 지정

```java
Predicate<String> p = Predicate.isEqual(str1);
boolean result = p.test(str2); // str1과 str2가 같은지 비교하여 결과를 반환

// 오히려 합친 문장이 이해하기 더 쉽다.
boolean result = Predicate.isEqual(str1).test(str2);
```

**[Function의 합성과 Predicate의 결합 사용 예시]**

```java
import java.util.function.*;

class LambdaEx7 {
	public static void main(String[] args) {
		Function<String, Integer>	f  = (s) -> Integer.parseInt(s, 16);
		Function<Integer, String>	g  = (i) -> Integer.toBinaryString(i);

		Function<String, String>	h  = f.andThen(g);
		Function<Integer, Integer>  h2 = f.compose(g);

		System.out.println(h.apply("FF")); // "FF" → 255 → "11111111"
		System.out.println(h2.apply(2));   // 2 → "10" → 16


		Function<String, String> f2 = x -> x; // 항등 함수(identity function)
		System.out.println(f2.apply("AAA"));  // AAA가 그대로 출력됨

		Predicate<Integer> p = i -> i < 100;
		Predicate<Integer> q = i -> i < 200;
		Predicate<Integer> r = i -> i%2 == 0;
		Predicate<Integer> notP = p.negate(); // i >= 100

		Predicate<Integer> all = notP.and(q).or(r);
		System.out.println(all.test(150));       // true

		String str1 = "abc";
		String str2 = "abc";
		
		// str1과 str2가 같은지 비교한 결과를 반환
		Predicate<String> p2 = Predicate.isEqual(str1); 
		boolean result = p2.test(str2);   
		System.out.println(result);
	}
}
```

## 메서드 참조(method reference)

**`메서드 참조(method reference)`** : 말 그대로 메서드를 참조한다. 
- **람다식이 하나의 메서드만 호출하는 경우, 람다식을 더욱 간략하게 표현하는 방법**

1. **static메서드 참조** 

문자열을 정수로 변환하는 람다식

```java
// 메서드 // 값을 받아서 Integer.paeseInt()에게 넘겨주는 일만 할 뿐이다. 
int wrapper(String s) {
   return Integer.paeseInt(s);
}

// 람다식
Function<String, Integer> f = (String s) -> Integer.parseInt(s);

// 메서드 참조
Function<String, Integer> f = Integer::parseInt; // 메서드 참조
```

- 예시의 메서드 참조에서 람다식의 일부가 생략되었지만,
  - 컴파일러는 생략된 부분을 우변의 parseInt메서드의 선언부
  - 좌변의 Function인터페이스에 지정된 Generic타입으로부터 쉽게 알아낼 수 있다.

2. **인스턴스메서드 참조**

두 문자열이 동등한지 확인하는 equals() 메서드

```java
// 람다식
BiFunction<String, String, Boolean> f = (s1, s2) -> s1.equals(s2);

// 메서드 참조
BiFunction<String, String, Boolean> f = String::equals;
```

- **참조변수의 타입**만 봐도 람다식이 2개의 String타입의 매개변수를 받는다는 것을 알 수 있다. 
  - 람다식의 매개변수들은 없어도 된다. 

**Boolean을 반환하는 equals라는 이름의 메서든느 다른 클래스에도 존재할 수 있기 때문에** equals앞에 **`클래스 이름`** 은 반드시 필요하다.

3. **특정 객체 인스턴스 메서드 참조** 

**이미 생성된 객체의 메서드**를 람다식에서 사용한 경우 클래스 이름 대신 **그 객체의 참조변수**를 적어줘야 한다.

```java
MyClass obj = new MyClass();
Function<String, Boolean> f = (x) -> obj.equals(x); // 람다식
Function<String, Boolean> f = obj::equals; // 메서드 참조
```

### 람다식을 메서드 참조로 변환하는 방법 총 정리

|종류|람다|메서드 참조|
|---|---|---|
|static메서드 참조| (x) -> ClassName.method(x) |ClassName::method|
|인스턴스메서드 참조| (obj, x) -> obj.method(x) |ClassName::method|
|특정 객체 인스턴스 메서드 참조| (x) -> obj.method(x) |ClassName::method|

```json
하나의 메서드만 호출하는 람다식은

`클래스이름::메서드이름` 또는 `참조변수::메서드이름` 으로 바꿀 수 있다.
```

### 생성자의 메서드 참조

생성자를 호출하는 람다식도 메서드 참조로 변환할 수 있다.

```java
// 매개변수가 없는 생성자
Supplier<MyClass> s = () -> new MyClass(); // 람다식
Supplier<MyClass> s = MyClass::new; // 메서드 참조

// 매개변수가 있는 생성자
Function<Integer, MyClass> f = (i) -> new MyClass(i); // 람다식
Function<Integer, MyClass> f2 = MyClass::new; // 메서드 참조

BiFunction<Integer, String, MyClass> bf = (i, s) -> new MyClass(i, s); // 람다식
BiFunction<Integer, String, MyClass> bf2 = MyClass::new; // 메서드 참조

// 배열을 생성할 때
Function<Integer, int[]> f = x -> new int[x]; // 람다식
Function<Integer, int[]> f2 = int[]::new; // 메서드 참조
```

- 매개변수가 있는 생성자라면
  - 매개변수의 개수에 따라 알맞은 함수형 인터페이스를 사용하면 된다.
  - 필요하다면 함수형 인터페이스를 새로 정의해야 한다.

**`메서드 참조`** 는 **람다식**을 마치 **static변수**처럼 다룰 수 있게 해준다.

**`메서드 참조`** 는 코드를 간략히 하는데 유용해서 많이 사용된다.

**람다식을 메서드 참조로 변환하는 연습을 많이 해서 빨리 익숙해지자!!**

# Stream

## Stream이란?

우리는 많은 수의 데이터를 다룰 때, Collection, Array에 데이터를 담고 원하는 결과를 얻기 위해 for문, Iterator를 이용해서 코드를 작성했다.

이러한 방식의 문제점들

1. **너무 길고 알아보기 어렵다.**
2. **데이터 소스마다 다른 방식으로 다뤄야 한다.**
   - 그나마, Collection, Iterator와 같은 인터페이스를 이용해서 컬렉션을 다루는 방식을 표준화하긴 했지만,
   - 각 컬렉션 클래스에는 같은 기능의 메서드들이 중복해서 정의되어 있다.
     - EX) Collections.sort(), Arrays.sort()    

**`Stream`** : **데이터 소스를 추상화하고, 데이터를 다루는데 자주 사용되는 메서드들을 정의해 놓았다.** 
- **데이터를 추상화 했다는 의미** : **데이터 소스가 무엇이던 간에 같은 방식으로 다룰 수 있게 되었다는 것, 코드의 재사용성 향상을 의미**
- 스트림을 이용하면, 배열이나 컬렉션뿐만 아니라 파일에 저장된 데이터도 모두 같은 방식으로 다룰 수 있다.

**[스트림 사용 예시]**

```java
// 문자열 배열과 같은 내용의 문자열을 저장하는 List가 있을 때
String[] strArr = { "aaa", "bbb", "ccc" };
List<String> strList = Arrays.asList(strArr);

// 두 데이터 소스를 기반으로 하는 스르림을 생성
Stream<String> strStream1 = strList.stream(); // 스트림을 생성
Stream<String> strStream2 = Arrays.stream(strArr); // 스트림을 생성

// 두 스트림으로 데이터 소스의 데이터를 읽어서 정렬하고 화면에 출력하는 방법
// 단, 데이터 소스가 정렬되는 것은 아니라는 것에 주의
strStream1.sorted().forEach(System.out::println);
strStream2.sorted().forEach(System.out::println);

---

// stream을 사용하지 않고 출력하기
Arrays.sort(strArr);
Collections.sort(strList);

for(String str : strArr) {
   System.out.println(str);
}

for(String str : strList) {
   System.out.println(str);
}
```

**두 스트림의 데이터 소스는 다르지만, 정렬하고 출력하는 방법은 완전히 동일하다.**
- **스트림을 사용한 코드**가 **간결, 이해하기 쉼고, 재사용성도 높다.**

### 스트림(stream)은 데이터 소스를 변경하지 않는다.

**`stream`** 은 **데이터 소스로 부터 데이터를 읽기만할 뿐, 데이터 소스를 변경하지 않는다는 차이가 있다.**
- 필요하다면, **정렬된 결과**를 **Collection이나 Array**에 **담아서 반환**할 수도 있다.

```java
// 정렬된 결과를 새로운 List에 담아서 반환한다.
List<String> sortedList = strStream2.sorted().collect(Collections.toList());
```

### 스트림은 일회용이다.

**`스트림`** 은 Iterator처럼 **일회용이다.**
- Iterator로 Collection의 요소를 모두 읽고 나면 다시 사용할 수 없는 것 처럼,
- **스트림도 한번 사용하면 닫혀서 다시 사용할 수 없다.**
- **필요하다면 스트림을 다시 생성해야 한다.**

```java
strStream1.sorted().forEach(System.out::println);
int numOfStr = strStream1.count(); // Error. 스트림이 이미 닫혔음.
```

### 스트림은 작업을 내부 반복으로 처리한다.

스트림을 이용한 작업이 **간결할 수 있는 비결** 중의 하나가 바로 **`내부 반복`** 이다.
- **`내부 반복`** : **반복문**을 **메서드의 내부에 숨길 수 있다는 것**을 의미
- **forEach()** : 스트림에 정의된 메서드 중의 하나, 매개변수에 대입된 람다식을 데이터 소스의 모든 요소에 적용 

```java
for(String str : strList) {
   System.out.println(str);
}

--> stream.forEach(System.out::println);

void forEach(Consumer<? super T> action) {
   Objects.requireNonNull(action); // 매개변수의 Null 체크

   for(T t : src) { // 내부 반복
      action.accept(T);
   }
}
```

- forEach() : 메서드 안으로 for문을 넣은 것
- 수행할 작업은 매개변수로 받는다.

### 스트림의 연산

스트림이 제공하는 **다양한 연산**을 이용해서 **복잡한 작업들을 간단히 처리**할 수 있다.
- 마치 Database에 SELECT문으로 질의(query)하는 것과 같은 느낌이다.
- **연산(operation)** : **스트림에 정의된 메서드 중**에서 **데이터 소스를 다루는 작업**

스트림이 제공하는 연산은 중간연산과 최종 연산으로 분류

1. **중간연산** : **연산결과가 스트림인 연산.** 연산결과를 스트림으로 반환
   - 중간 연산을 연속해서 연결할 수 있다.
2. **최종연산** : **연산결과가 스트림이 아닌 연산.** 스트림의 요소를 소모하면서 연산을 수행
   - 단 한번만 연산이 가능

```java
stream.distinct().limit(5).sorted().forEach(System.out::println);

--> 

String[] strArr = {"aa", "aa","aa","aa","aa", "aa"};
Stream<String> stream = Stream.of(strArr); // 문자열 배열이 소스인 스트림
Stream<String> filterStream = Stream.filter(); // 걸러내기(중간연산)
Stream<String> distinctedStream = Stream.distinct(); // 중복제거(중간연산)
Stream<String> sortedStream = Stream.sorted(); // 정렬(중간연산)
Stream<String> limitedStream = Stream.limit(5); // 스트림 자르기(중간연산)

int total = stream.count(); // 요소 개수 세기(최종 연산)
```

#### 스트림의 중간 연산 목록

![스트림의 중간 연산](https://user-images.githubusercontent.com/56071088/130747720-06396c59-73b2-45b5-aa87-0d788e11aed0.png)

#### 스트림의 최종 연산 목록

![스트림의 최종 연산](https://user-images.githubusercontent.com/56071088/130747712-1cfeed3a-9177-4276-ac45-dff0b709eb3b.png)

- **중간 연산**은 **`map()`** , **`flatMao()`** 이 핵심
- **최종 연선** 은 **`reduce()`** , **`collect()`** 이 핵심
- 최종 연산 목록은 Stream클래스에 정의된 연산들만 나열한 것이다.

**Optional**은 **일종의 래퍼 클래스(Wrapper class)**로 **내부에 하나의 객체를 저장**할 수 있다.

### 지연된 연산

**최종 연산**이 **수행되기 전까지는 중간연산이 수행되지 않는다!!**
- 스트림에 대해 distinct(), stream()이 호출해도 즉각적인 연산이 수행되는 것은 아니다.
- **중간 연산을 호출하는 것** : **어떤 작업이 수행되어야하는지를 지정해주는 것**
- 최종 연산이 수행되어야 비로소 스트림의 요소들이 중간 연산을 거쳐 최종 연산에서 소모된다.

### Stream<Integer>와 IntStream

요소의 타입이 T인 스트림은 기본적으로 Stream< T>

**Autoboxing & Unboxing**으로 인해 **비효율성을 줄이기** 위해 데이터 소스의 요소를 **기본형**으로 다루는 스트림, **IntStream, LongStream, DoubleStream**이 제공된다.
- IntStream에는 int타입의 값으로 작업하는데 유용한 메서드들이 포함되어 있다.

### 병렬 스트림(parallel stream)

스트림으로 데이터를 다룰 때의 장점 중 하나가 **병렬 처리**가 쉽다는 점이다.
- **fork&join framework**처럼 **`병렬 스트림`** 은 **내부적으로** 이 framework를 이용해서 자동적으로 **연산을 병렬로 수행**한다.
- **parallel()** : 병렬로 연산을 수행하도록 지시
- **sequential()** : 병렬로 처리하지 않게 지시
- **parallel(), sequential()** : **새로운 스크림을 생성하는 것이 아닌 스트림의 속성을 변경하는 것**
- **병렬처리가 항상 더 빠른 결과를 주는 것은 아니라는 것을 명심 또 명심하자.**

```java
int sum = strStream.parallel() // strStream을 병렬 스트림으로 전환
      .mapToInt(s -> s.length())
      .sum(); 
```

## 스트림 만들기

### Collection을 통해 스트림 만들기 - stream()

컬렉션들의 최고 조상인 Collection에 stream()이 정의되어 있다.
- Collection의 자손인 List, Set을 구현한 Collection클래스들은 모두 이 메서드로 스트림을 생성할 수 있다.
- **`stream()`** : 해당 컬렉션을 소스(source)로 하는 스트림을 반환한다.

**[Collection의 stream() 메서드]**

```java
Stream<T> Collecion.stream()
```

**[List로부터 스트림을 생성하는 코드]**

```java
List<Integer> list = Arrays.asList(1,2,3,4,5); // 가변인자
Stream<Integer> intStream = list.stream(); // list를 소스로 하는 Collection생성

intStream.forEach(System.out::println); // 스트림의 모든 요소를 출력 
intStream.forEach(System.out::println); // Error!! 스트림이 이미 닫혔다.
```

- forEach()가 스트림의 요소를 소모하면서 작업을 수행, 
- 같은 스트림에 forEach()를 2번 호출할 수 없다.
- 스트림의 요소를 한번 더 출력하려면 스트림을 새로 생성
  - 스트림의 요소가 소모되는 것, 소스의 요소가 소모되는 것이 아니다.

### 배열(Array)를 통해 스트림 만들기 - Stream.of(), Arrays.stream()

배열을 소스로 하는 스트림을 생성하는 메서드
- Stream, Arrays에 static메서드로 정의되어 있다.

```java
Stream<T> Stream.of(T... values) // 가변 인자
Stream<T> Stream.of(T[])
Stream<T> Arrays.stream(T[])
Stream<T> Arrays.stream(T[], int startInclusive, int endExclusive)

// int, long, double과 같은 기본형 배열을 소스로 하는 스트림을 생성하는 메서드
IntStream IntStream.of(int... values) // Stream이 아니라 IntStream
IntStream IntStream.of(int[])
IntStream Arrays.stream(int[])
IntStream Arrays.stream(int[] array, int startInclusive, int endExclusive)
```

**[문자열 스트림 생성 예제]**

```java
Stream<String> strStream = Stream.of("a", "a", "a"); // 가변인자
Stream<String> strStream = Stream.of(new String[]{"a", "a", "a"}); 
Stream<String> strStream = Arrays.stream(new String[]{"a", "a", "a"});
Stream<String> strStream = Arrays.stream(new String[]{"a", "a", "a"}, 0, 3);
```

### 특정 범위의 정수를 통해 스트림 만들기 - range(), rangeClosed()

지정된 범위의 연속된 정수를 stream으로 생성해서 반환하는 range(), rangeClosed()
- **range() : 경계의 끝인 end가 범위에 포함 X**
- **rangeClosed() : end가 범위에 포함 O**

```java
IntStream IntStream.range(int begin, int end)
IntStream IntStream.rangeClosed(int begin, int end)

IntStream intStream = IntStream.range(1, 5); // 1,2,3,4 
IntStream intStream = IntStream.rangeClosed(1, 5); // 1,2,3,4,5
```

### 임의의 수를 통해 스트림 만들기 - ints(), longs(), doubles()

난수를 생성하는데 사용하는 Random클래스에는 아래와 같은 인스턴스 메서드들이 포함되어 있다. 
- 이 메서드들은 **해당 타입의 난수들로 이루어진 스트림을 반환**

```java
IntStream ints() // 무한 스트림(infinite stream)
LongStream longs()
DoubleStream doubles()

IntStream ints(long streamSize) // 유한 스트림
LongStream longs(long streamSize)
DoubleStream doubles(long streamSize)

IntStream ints(int begin, int end) // 지정된 범위의 난수를 발생시키는 스트림
LongStream longs(long begin, long end) // end는 범위에 포함되지 않는다.(exclusive)
DoubleStream doubles(double begin, double end)

IntStream intStream = new Random().ints(); // 무한 스트림
intStream.limit(5).forEach(System.out::println); // 5개의 요소만 출력

IntStream intStream = new Random().ints(5); // 크기가 5인 난수 스트림을 반환
```

```java
Integer.MIN_VALUE <= ints() <= Integer.MAX_VALUE
Long.MIN_VALUE <= longs() <= Long.MAX_VALUE 
0.0 <= doubles < 1.0 
```

### 람다식을 통해 스트림 만들기 - iterate(), generate()

Stream 클래스의 iterate(), generate()는 람다식을 매개변수로 받아서, 이 **람다식에 의해 계산되는 결과값들을 요소**로 하는 **무한 스트림**을 생성한다.

```java
static <T> Stream<T> iterate(T seed, UnaryOperator<T> f)
static <T> Stream<T> generate(Supplier<T> s)
```

**`iterate()`** : **이전 결과에 대해 종속적**, seed값 부터 시작해서, 람다식 f에 의해 계산된 결과를 다시 seed값으로 해서 계산을 반복한다.

**`generate()`** : **이전 결과에 대해 독립적**, iterator()와 달리, 이전 결과를 이용해서 다음 요소를 계산하지 않는다.
- generate()의 매개변수 타입이 Supplier< T> 이므로, **매개변수가 없는 람다식만 허용**

**iterate()와 generate()에 의해 생성된 스트림은 아래와 같이 기본형 스트림 타입의 참조변수로 다룰 수 없다.**


```java
IntStream evenStream = Stream.iterate(0, n->n+2);  //error
DoubleStream randomStream = Stream.generate(Math::random);  //error
```

굳이 필요하다면, 아래와 같이 mapToInt()와 같은 메서드로 변환을 해야한다. (Stream, IntStream 변환)

```java
IntStream evenStream = Stream.iterate(0, n->n+2).mapToInt(Integer::valueOf);
Stream<Integer> stream = evenStream.boxed(); // IntStream -> Stream<Integer>
```

### 파일을 통해 스트림 만들기 - list()

java.nio.file.Files는 파일을 다루는데 필요한 유용한 메서드들을 제공하는데, 
- **list()** : 지정된 디렉토리(dir)에 있는 파일의 목록을 소스로 하는 스트림을 생성해서 반환.

```java
 Stream<Path> Files.list(Path dir) // Path는 하나의 파일 또는 디렉토리
```

### 빈 스트림을 통해 스트림 만들기 - empty()

**empty()** : **요소가 하나도 없는 비어있는 스트림을 생성할 수도 있다.** 
- 스트림에 연산을 수행한 결과가 하나도 없을 때
  - **null보다는 빈 스트림을 return하는 것이 낫다.**

```java
Stream emptyStream = Stream.empty(); // empty()는 빈 스트림을 생성해서 반환
long count = emptyStream.count(); // count의 값은 0
```

### 두 스트림의 연결을 통해 스트림 만들기 - concat()

**concat()** : Stream의 static메서드, **두 스트림을 하나로 연결**
- **연결하려는 두 스트림의 요소는 같은 타입**

```java
String[] str1 = {"123", "456" };
String[] str2 = {"aaa", "bbb", "cc"};

Stream<String> strs1 = Stream.of(str1);
Stream<String> strs2 = Stream.of(str2);
Stream<String> strs3 = Stream.concat(strs1, strs2); // 두 스트림을 하나로 연결
```

## 스트림의 중간 연산

### 스트림 자르기 - skip(), limit()

skip(), limit() : 스트림의 일부를 잘라낼 때 사용
- skip(3) : 처음 3개를 건너뛴다.
- limit(5) : 스트림의 요소를 5개로 제한

```java
Stream<T> skip(long n)
Stream<T> limit(long maxSize)

IntStream skip(long n)
IntStream limit(long maxSize)
```

```java
IntStream intStream = IntStream.rangeClosed(1, 10) // 1~10의 요소를 가진 스트림
intStream.skip(3).limit(5).forEach(System.out::print); // 45678
```

### 스트림의 요소 걸러내기 - filter(), distinct()

- distinct() : 스트림에서 중복된 요소들을 제거
- filter() : 주어진 조건(Predicate)에 맞지 않는 요소를 걸러낸다.
  - filter()는 매개변수로 Predicate을 필요, 
  - 연산결과 boolean인 람다식도 Predicate으로 가능 

```java
Stream<T> filter(Predicate<? super T> predicate)
Stream<T> distinct()
```

```java
// distinct() 예시
IntStream intStream  = IntStream.of(1,2,33,3,3,4,5,5,6);
intStream.distinct().forEach(System.out::print); //123456

// filter() 예시 
IntStream intStream = IntStream.rangeClosed(1, 10) // 1~10의 요소를 가진 스트림
intStream.filter(i -> i%2==0).forEach(System.out::print); //2,4,6,8,10

// filter() 여러번 사용 가능
intStream.filter(i -> i%2!=0 && i%3!=0).forEach(System.out::print); //157
intStream.filter(i -> i%2!=0).filter(i -> i%3!=0).forEach(System.out::print); //157
```

### 스트림을 정렬 - sorted()

**`sorted()`** : **스트림을 정렬할 때 사용**
- 지정된 Comparator을 이용하여 정렬
  - Comparator대신 int값을 반환하는 Lambda expression을 사용하는 것도 가능
- Comparator을 지정하지 않으면, 스트림 요소의 기본 정렬 기준(Comparable)으로 정렬
  - 스트림의 요소가 Comparable을 구현한 클래스가 아니면 예외 발생 

```java
Stream<T> sorted()
Stream<T> sorted(Comparator<? super T> comparator)
```

```
Stream<String> strStream = Stream.of("dd", "aaa", "CC", "cc", "b");
strStream.sorted().forEach(System.out::print); // CCaaabccdd
```

#### Comparator의 default메서드, Comparator의 static메서드

JDK1.8부터 **정렬**에 용이한 **Comparator인터페이스**에 **default, static메서드**가 많이 추가됨

**[Comparator의 default메서드]**

```java
reversed()
thenComaparing(Comparator<T> other)
thenComaparing(Function<T, U> keyExtractor)
thenComaparing(Function<T, U> keyExtractor, Comparator<T> keyComp)
thenComaparing(ToIntFunction<T> keyExtractor)
thenComaparing(ToLongFunction<T> keyExtractor)
thenComaparing(ToDoubleFunction<T> keyExtractor)
```

**[Comparator의 static메서드]**

```java
naturalOrder()
reverseOrder()
comparing(Function<T, U> keyExtractor)
comparing(Function<T, U> keyExtractor, Comparator<U> keyComparator)
comparingInt(ToIntFunction<T> keyExtractor)
comparingLong(ToLongFunction<T> keyExtractor)
comparingDouble(ToDoubleFunction<T> keyExtractor)
nullsFirst(Comparator<T> comparator)
nullsLast(Comparator<T> comparator)
```

이러나 저러나 가장 많이 사용되는 기본적인 메서드는 **comparing()**이다.
- 스트림의 요소가 Comparable을 구현한 경우, 매개변수 하나짜리를 사용하면 되고
- 그렇지 않은 경우, 추가 매개변수로 정렬기준(Comparator)을 따로 지정해주면 된다.

비교 대상이 기본형인 경우, comparaing() 대신 **Autoboxing & Unboxing의 과정이 없어 더 효율적인 기본형 메서드**를 사용한다. **(comparingInt(), comparingLong(), comparingDouble())**

정렬 조건을 추가할 때는 **thenComaparing()**을 사용한다.

```java
comparing(Function<T, U> keyExtractor)
comparing(Function<T, U> keyExtractor, Comparator<U> keyComparator)

comparingInt(ToIntFunction<T> keyExtractor)
comparingLong(ToLongFunction<T> keyExtractor)
comparingDouble(ToDoubleFunction<T> keyExtractor)

thenComaparing(Comparator<T> other)
thenComaparing(Function<T, U> keyExtractor)
thenComaparing(Function<T, U> keyExtractor, Comparator<T> keyComp)

thenComaparing(ToIntFunction<T> keyExtractor)
thenComaparing(ToLongFunction<T> keyExtractor)
thenComaparing(ToDoubleFunction<T> keyExtractor)
```

**[학생 스트림(studentStream)을 반(ban)별, 성적(totalScore)순, 그리고 이름(name)순으로 정렬 후 출력하는 예시]**

```java
studentStream.sorted(Comparator.comparing(Student::getBan)
      .thenComparing(Student::getTotalScore)
      .thenComparing(Student::getName))
      .forEach(System.out::println);
```

**[스트림 정렬 메서드 sorted() 예제]**

학생의 성적을 반별 오름차순, 총점별 내림차순으로 정렬하여 출력한다.
- 정렬하는 코드를 짧게 하려고, Comparable을 구현해서 총점별 내림차순 정렬이 Student클래스의 기본정렬(naturalOrder())이 되도록 했다.

```java
import java.util.*;
import java.util.stream.*;

class StreamEx1 {
	public static void main(String[] args) {
	     Stream<Student> studentStream = Stream.of(
							new Student("이자바", 3, 300),
							new Student("김자바", 1, 200),
							new Student("안자바", 2, 100),
							new Student("박자바", 2, 150),
							new Student("소자바", 1, 200),
							new Student("나자바", 3, 290),
							new Student("감자바", 3, 180)
						);

	     studentStream.sorted(Comparator.comparing(Student::getBan) // 반별 정렬
			    	  .thenComparing(Comparator.naturalOrder()))    // 기본 정렬
					  .forEach(System.out::println);
	}
}

class Student implements Comparable<Student> {
	String name;
	int ban;
	int totalScore;

	Student(String name, int ban, int totalScore) { 
		this.name =name;
		this.ban =ban;
		this.totalScore =totalScore;
	}

	public String toString() { 
	    return String.format("[%s, %d, %d]", name, ban, totalScore).toString(); 
	}

	String getName()     { return name;}
	int getBan()         { return ban;}
	int getTotalScore()  { return totalScore;}

   // 총점 내림차순을 기본 정렬로 한다.
	public int compareTo(Student s) { 
		return s.totalScore - this.totalScore;
	}
}
```

### 스트림을 변환 - map()

**`map()`** : 스트림의 요소에 저장된 값 중에서 **원하는 필드만 뽑아내거나 특정 형태로 변환해야 할 때 사용**
- **매개변수로 T 타입을 R 타입으로 변환해서 반환하는 함수를 지정**

```java
Stream<R> map(Function<? super T, ? extends R> mapper)
```

**[File의 스트림에서 파일의 이름만 뽑아서 출력하는 예제]**

```java
Stream<File> fileStream = Stream.of(new File("Ex1.java"), new File("Ex1"), new File("Ex1.bak"), new File("Ex2.java"), new File("Ex1.txt"));
 
// map()으로 Stream을 Stream으로 변환
Stream filenameStream = fileStream.map(File::getName);
filenameStream.forEach(System.out::println);    // 스트림의 모든 파일의 이름을 출력
```

**[map()을 여러번 사용하는 예제]**

File의 스트림에서 파일의 확장자만을 뽑은 다음 중복을 제거해서 출력한다.

```java
fileStream.map(File::getName) // Stream<File> -> Stream<String>
  .filter(s -> s.indexOf('.') != -1) // 확장자 없는 것 제외
  .map(s -> s.substring(s.indexOf('.') + 1)) // Stream<String> -> Stream<String>
  .map(String::toUpperCase) // 모두 대문자로 변환
  .distinct() // 중복제거   
  .forEach(System.out::print); // JAVABAKTXT
```

### 스트림을 조회 - peek()

**`peek()`** : **연산과 연산 사이에 올바르게 처리되었는지 확인할 때 사용**
- **forEach()와 달리 스트림의 요소를 소모하지 않으므로** 연산 사이에 여러 번 끼워 넣어도 문제가 되지 않는다.
- filter(), map()의 결과를 확인할 때 유용하게 사용될 수 있다.


```java
import java.io.*;
import java.util.stream.*;

public class StreamEx2 {
	public static void main(String[] args) {
		File[] fileArr = { new File("Ex1.java"), new File("Ex1.bak"),
			new File("Ex2.java"), new File("Ex1"), new File("Ex1.txt")
		};

		Stream<File> fileStream = Stream.of(fileArr);

      fileStream.map(File::getName) // Stream<File> -> Stream<String>
            .filter(s -> s.indexOf('.') != -1) // 확장자 없는 것 제외
            .peek(s -> System.out.printf("filename=%s%n", s)) // 파일명 출력
            .map(s -> s.substring(s.indexOf('.')+1)) // 확장자만 추출
            .peek(s -> System.out.printf("extension=%s%n", s)) // 확장자 출력
            .forEach(System.out::println);	

		System.out.println();
	}
}
```

```json
출력결과

filename=Ex1.java
extension=java
java
filename=Ex1.bak
extension=bak
bak
filename=Ex2.java
extension=java
java
filename=Ex1.txt
extension=txt
txt
```

### mapToInt(), mapToLong(), mapToDouble()

**`mapToInt(), mapToLong(), mapToDouble()`** : Stream< T> 타입의 스트림을 기본형 스트림으로 변환할 때 사용하는 것
- map()은 연산 결과로 Stream< T> 타입의 스트림을 반환
- mapToInt()와 자주 사용되는 메서드로 : Integer의 parseInt(), valueOf()

```java
IntStream mapToInt(ToIntFunction<? super T> mapper)
LongStream mapToLong(ToLongFunction<? super T> mapper)
DoubleStream mapToDouble(ToDoubleFunction<? super T> mapper)  

Stream<String> -> IntStream 변환 : mapToInt(Integer::parseInt)
Stream<Integer> -> IntStream 변환 : mapToInt(Integer::intValue)
```

count()만 지원하는 Stream< T> 와 달리 **IntStream과 같은 기본형 스트림은 숫자를 다루는데 편리한 메서드들을 제공**
- max()와 min()은 Stream에도 정의되어 있지만 매개변수로 Comparator를 지정해야 한다.
- **이 메서드들은 최종 연산**이기 때문에 **호출 후에 스트림이 닫힌다는 점을 주의**
  - EX) sum(), average()를 연속해서 호출할 수 없다. 

```java
int sum()                   스트림 모든 요소의 총합
OptionalDouble average()    sum()/count()
OptionalInt max()           스트림 요소 중 제일 큰 값
OPtionalInt min()           스트림 요소 중 제일 작은 값

IntStream scoreStream = studentStream.mapToInt(Student::getTotalScore);

long totalScore = scoreStream.sum(); // sum()은 최종연산이라 호출 후 스트림이 닫힌다.
OptionalDouble average = scoreStream.average(); // Error!! 스트림이 이미 닫혀있다.

double d = average.getasDouble(); // OptionalDouble에 저장된 값을 꺼내서 d에 저장한다.
```

#### summaryStatistics()

그래서 sum(), average()를 모두 호출해야 할 때, 스트림을 또 생성해야하므로 불편하다. 그래서 **summaryStatistics()** 라는 **메서드가 따로 제공된다.**

```java
IntSummaryStatistics stat = scoreStream.summaryStatistics();

long totalCount = stat.getCount();
long totalScore = stat.getSum();
double avgScore = stat.getAverage();
int minScore = stat.getMin();
int maxScore = stat.getMax();
```

#### 기본형 스트림 -> Stream< T> 변환 - mapToObj() / 기본형 스트림 -> Stream<Integer> 로 변환 - boxed()

```java
Stream<U> mapToObj(IntFunction<? extends U> mapper)
Stream<Integer> boxed()
```

**[로또번호를 생성해서 출력하는 예제]**

mapToObj()를 이용 - IntStream -> Stream< String>으로 변환

```java
IntStream intStream = new Random().ints(1,46);  // 1~45사이의 정수
Stream<String> lottoStream = intStream.distinct().limit(6).sorted()
         .mapToObj(i->i+",");    // 정수를 문자열로 변환
lottoStream.forEach(System.out::print); // 12,14,23,29,45
```

#### String, StringBuffer에 저장된 문자들을 IntStream으로 변환 - CharSequence의 chars()

```java
IntStream charStream = "12345".chars(); // default IntStream chars()
int charSum = charStream.map(ch -> ch-'0').sum(); // charSum = 15
```

**[mapToInt(), mapToLong(), mapToDouble(), summaryStatistics() 사용 예제]**

```java
import java.util.*;
import java.util.stream.*;

class StreamEx3 {
	public static void main(String[] args) {
		Student[] stuArr = {
			new Student("이자바", 3, 300),
			new Student("김자바", 1, 200),
			new Student("안자바", 2, 100),
			new Student("박자바", 2, 150),
			new Student("소자바", 1, 200),
			new Student("나자바", 3, 290),
			new Student("감자바", 3, 180)	
		};

		Stream<Student> stuStream = Stream.of(stuArr);

		stuStream.sorted(Comparator.comparing(Student::getBan)
				 .thenComparing(Comparator.naturalOrder()))
				 .forEach(System.out::println);

		stuStream = Stream.of(stuArr); // 스트림을 다시 생성한다. 
	     IntStream stuScoreStream= stuStream.mapToInt(Student::getTotalScore);

		IntSummaryStatistics stat = stuScoreStream.summaryStatistics();
		System.out.println("count="+stat.getCount());
		System.out.println("sum="+stat.getSum());
		System.out.printf("average=%.2f%n", stat.getAverage());
		System.out.println("min="+stat.getMin());
		System.out.println("max="+stat.getMax());
	}
}

class Student implements Comparable<Student> {
	String name;
	int ban;
	int totalScore;
	Student(String name, int ban, int totalScore) { 
		this.name = name;
		this.ban  = ban;
		this.totalScore = totalScore;
	}

	public String toString() { 
	   return String.format("[%s, %d, %d]", name, ban, totalScore).toString(); 
	}

	String getName()	{ return name;}
	int getBan()		{ return ban;}
	int getTotalScore() { return totalScore;}

	public int compareTo(Student s) {
		return s.totalScore - this.totalScore;
	}
}
```

### flatMap() - Stream< T[]> 를 Stream< T> 로 변환

**`flatMap()`** : **스트림의 요소가 배열이거나, map()의 연산결과가 배열(스트림의 타입: Stream< T[]>) 을 Stream< T> 로 변환**

```java
// 요소가 문자열 배열(String[])인 스트림
Stream<String[]> strArrStrm = Stream.of(
         new String[]{"abc", "def", "ghi"},
         new String[]{"ABC", "DEF"m "JKLMN"}
         );

// Stream<String[]>을 map(Arrays::stream)으로 변환한 결과는 Stream<String>이 아닌 Stream<Stream<String>>.
// 즉 스트림의 스트림이다.
Stream<Stream<String>> strStrStrm = strArrStrm.map(Arrays::stream);

//map(Arrays::stream) 대신 flatMap(Arrays::stream) 사용하면 각 배열이 하나의 스트림의 요소가 된다.
Stream<String> strStrm = strArrStrm.flatMap(Arrays::stream);
```

![map(Arrays stream)](https://user-images.githubusercontent.com/56071088/131468488-ef8514c1-ddb7-4a36-bd96-9e10dc70375b.JPG)


![flatMap(Arrays stream)](https://user-images.githubusercontent.com/56071088/131468478-c1c9a07c-466c-449d-a2d9-164f50760e5a.JPG)

```json
// Arrays.stream(T[])
map(Arrays::stream) : Stream<String[]> -> Stream<Stream<String>>

flatMap(Arrays::stream) : Stream<String[]> -> Stream<String>
```

**[또 다른 예제 - 여러 문장을 요소로 하는 스트림을 split()으로 나눠서 요소가 단어인 스트림을 만들기]**

```java
String[] lineArr = {
   "Belive or not It is true",
   "Do or do not There is no try"
};
 
Stream<String> lineStream = Arrays.stream(lineArr);

Stream<Stream<String>> strArrStream = lineStream.map(line -> Stream.of(line.split(" +")));

Stream<String> strArrStream = lineStream.flatMap(line -> Stream.of(line.split(" +")));
```

```json
map(line -> Stream.of(line.split(" +"))) : Stream<String> -> Stream<Stream<String>>

flatMap(line -> Stream.of(line.split(" +"))) : Stream<String> -> Stream<String>
```

**[스트림의 스트림을 하나의 스트림으로 합칠 때 - flatMap()]**

```java
Stream<String> strStrm = Stream.of("abc", "def", "jkl");
Stream<String> strStrm2 = Stream.of("ABC", "DEF", "JKL");
 
Stream<Stream<String>> strmStrm = Stream.of(strStrm, strStrm2);

Stream<String> strStream = strmStrm
      .map(s -> s.toArray(String[]::new)) // Stream<Stream<String>> -> Stream<String[]>
      .flatMap(Arrays::stream); // Stream<String[]> -> Stream<String>
```

- toArray() : 스트림을 배열로 변환해서 반환
  - 매개변수를 지정하지 않으면 Object[]을 반환하므로 (String[]::new)와 같이 특정 타입의 생성자를 지정해줘야 한다.

**[flatMap() 사용 예제]**

```java
import java.util.*;
import java.util.stream.*;

class StreamEx4 {
	public static void main(String[] args) {

		Stream<String[]> strArrStrm = Stream.of(
			new String[]{"abc", "def", "jkl"},
			new String[]{"ABC", "GHI", "JKL"}
		);

//		Stream<Stream<String>> strStrmStrm = strArrStrm.map(Arrays::stream);
		Stream<String> strStrm = strArrStrm.flatMap(Arrays::stream);

		strStrm.map(String::toLowerCase)
			   .distinct()
			   .sorted()
			   .forEach(System.out::println);
		System.out.println();

		String[] lineArr = {
			"Believe or not It is true",
			"Do or do not There is no try",
		};

		Stream<String> lineStream = Arrays.stream(lineArr);
		lineStream.flatMap(line -> Stream.of(line.split(" +")))
				  .map(String::toLowerCase)
				  .distinct()
				  .sorted()
				  .forEach(System.out::println);
		System.out.println();

		Stream<String> strStrm1 = Stream.of("AAA", "ABC", "bBb", "Dd");
		Stream<String> strStrm2 = Stream.of("bbb", "aaa", "ccc", "dd");

		Stream<Stream<String>> strStrmStrm = Stream.of(strStrm1, strStrm2);

		Stream<String> strStream = strStrmStrm
									.map(s -> s.toArray(String[]::new))  
		    						.flatMap(Arrays::stream);
		strStream.map(String::toLowerCase)
				 .distinct()
				 .forEach(System.out::println);
	}
}
```

```json
실행 결과

abc
def
ghi
jkl

believe
do
is
it
no
not
or
there
true
try

aaa
abc
bbb
dd
ccc
```

## Optional< T>와 OptionalInt

**`Optional<T>`** : Generic클래스, `T타입의 객체`를 감싸는 **래퍼 클래스**
- **Optional타입의 객체**에는 **모든 타입의 참조변수**를 담을 수 있다.
  - java.util.Optional은 JDK1.8부터 추가되었다.
- **최종 연산 결과**를 그냥 반환하는 것이 아닌 **Optional객체에 담아서 반환**
  - 객체에 담아서 반환하면, 반환한 결과가 null인지 매번 if문으로 check하는 대신 Optional에 정의된 메서드를 통해서 간단히 처리 가능
  - **null check를 위한 if 문 없이도 NullPointerException이 발생하지 않는 보다 간결하고 안전한 코드 작성 가능**
    - null check를 위한 if문을 메서드 안으로 넣음.

-> **간접적으로 null을 다룬다. 다루고 있는 객체의 null값을 Optional 객체 안에 넣는다.**

```java
public final class Optional<T> {
   private final T value; // T 타입의 참조변수
   ...
}
```

### Optional객체 생성하기

**`of(), ofNullable()`** : **Optional객체를 생성**
- **`ofNullalble()`** : 참조변수의 값이 null일 가능성이 있을 때 사용.

**empty()** : Optional< T> 타입의 참조변수를 기본값으로 초기화할 때 사용.
- null로 초기화할 수 있지만, empty()로 초기화 하는 것이 바람직하다.

```java
String str = "abc";
Optional<String> optVal = Optional.of(str);
Optional<String> optVal = Optional.of("abc"));
Optional<String> optVal = Optional.of(new String("abc"));

Optional<String> optVal = Optional.of(null); // NullPointerException발생
Optional<String> optVal = Optional.ofNullable(null); // OK

Optional<String> optVal = null; // null로 초기화
Optional<String> optVal = Optional.<String>empty(); // 빈 객체로 초기화 // Optional.empty
```

**Optional 객체로 한 단계를 더 거치게 하면 0x100이 null이더라도 NullPointerException이 나지 않는다. (0x200은 null이 될 수 없다.)**

![Optional 객체 생성](https://user-images.githubusercontent.com/56071088/131481931-b2a1d629-fa16-46ff-968c-5a6b911eb026.JPG)

### Optional 객체의 값 가져오기 - get(), orElse(), orElseGet(), orElseThrow()

- **`get()`** : Optional객체에 저장된 값을 가져옴.
- **`orElse()`** : Optional객체에 저장된 값이 null인 경우를 대비해서 사용. 대체 값을 지정할 수 있다.
- **`orElseGet()`** : null을 대체할 값을 반환하는 Lambda식을 지정할 수 있다.
- **`orElseThrow()`**: null일 때 지정된 예외를 발생시킨다.

```java
Optional<String> optVal = Optional.of("abc");
String str1 = optVal.get(); // optVal에 저장된 값을 반환. null이면 예외발생
String str2 = optVal.orElse(""); // optVal에 저장된 값이 null이면 ""를 반환

T orElseGet(Supplier<? extends T> other)
T orElseThrow(Supplier<? extends X> exceptionSupplier)
// X - Type of the exception to be thrown

String str3 = optVal.orElseGet(String::new); // () -> new String() 와 동일
String str4 = optVal.orElseThrow(NullPointerException::new); // null이면 예외를 발생
```

**Stream처럼 filter(), map(), flatMap() 사용가능**
- map()의 연산결과가 Optional<Optional< T>> 일 때, flatMap()을 사용하면 Optional< T> 를 얻는다.
- 만일 Optional 객체의 값이 null이면, 이 메서드들은 아무 일도 하지 않는다.

```java
int result = Optional.of("123")
      .filter(x -> x.length() > 0)
      .map(Integer::parseInt).orElse(-1); // result = 123

int result = Optional.of("")
      .filter(x -> x.length() > 0)
      .map(Integer::parseInt).orElse(-1); // result = -1
```

#### Optional객체의 값이 존재하는지 확인 - isPresent(), ifPresent()

**`isPresent()`** : **Optional객체의 값이 null이면 false, 아니면 true를 반환**

**`ifPresent(Consumer<T> block)`** : **값이 있으면 주어진 람다식을 실행, 없으면 아무 일도 하지 않는다.**

```java
if(str != null) {
   System.out.println(str);
}

-->

if(Optional.ofNullable(str).isPresent()) {
   System.out.println(str);
}

--> 

Optional.ofNullable(str).ifPresent(System.out::println);
```

**ifPresent()**는 **Optional< T>를 반환**하는 **findAny(), findFirst()와 같은 최종 연산**과 잘 어울린다.

```java
// Stream<T>에 정의된 Optional<T>를 반환하는 메서드들
// Optional<T>를 반환하는 최종 연산은 몇 개 없다.
Optional<T> findAny()
Optional<T> findFirst()
Optional<T> max(Comparator<? super T> comaprator)
Optional<T> min(Comparator<? super T> comaprator)
Optional<T> reduce(BinaryOperator<T> accumulator)
```

### OptionalInt, OptionalLong, OptionalDouble

기본형 스트림에는 Optional도 기본형을 값으로 하는 OptionalInt, OptionalLong, OptionalDouble을 반환한다.

**[IntStream에 정의된 메서드들]**

```java
OptionalInt findAny()
OptionalInt findFirst()
OptionalInt reduce(IntBinaryOperator op)
OptionalInt max()
OptionalInt min()
OptionalDouble average()
```

**[OptionalInt객체의 값을 가져오는 메서드 - getAsInt()]**

|Optional클래스|값을 반환하는 메서드|
|---|---|
|Optional< T>|T get()|
|OptionalInt|int getAsInt()|
|OptionalLong|long getAsLong()|
|OptionalDouble|double getAsDouble()|

**[OptionalInt클래스 정의]**

```java
public final class OptionalInt {
   ...
   private final boolean isPresent; // 값이 저장되어 있으면 true
   private final int value; // int타입의 변수
}
```

**[빈 OptionalInt객체와의 비교]**

기본형 int의 기본값은 0이므로 아무런 값도 갖지 않는 OptionalInt에 저장되는 값은 0이다.

```java
OptionalInt opt = OptionalInt.of(0); // OptionalInt에 0을 저장
OptionalInt opt2 = OptionalInt.empty(); // int타입의 변수


System.out.println(opt.isPresent()); // true
System.out.println(opt2.isPresent()); // false

System.out.println(opt.getAsInt()); // 0
System.out.println(opt2.getAsInt()); // NoSuchElementException발생

System.out.println(opt.equals(opt2));	// false

//--------------------------

// Optional객체의 경우 null을 저장하면 비어있는 것과 동일하게 취급한다.
Optional<String> opt = Optional.ofNullalble(null); 
Optional<String> opt2 = Optional.empty(); 

System.out.println(opt.equals(opt2)); // true
```

**[Optional< T>와 OptionalInt에 대한 예시]**

```java
import java.util.*;
import java.util.stream.*;

class OptionalEx1 {
	public static void main(String[] args) {
		Optional<String>  optStr = Optional.of("abcde");
		Optional<Integer> optInt = optStr.map(String::length);
		System.out.println("optStr="+optStr.get());
		System.out.println("optInt="+optInt.get());

		int result1 = Optional.of("123")
							  .filter(x->x.length() >0)
							  .map(Integer::parseInt).get();

		int result2 = Optional.of("")
							  .filter(x->x.length() >0)
							  .map(Integer::parseInt).orElse(-1);

		System.out.println("result1="+result1);
		System.out.println("result2="+result2);

		Optional.of("456").map(Integer::parseInt)
					      .ifPresent(x->System.out.printf("result3=%d%n",x));

		OptionalInt optInt1  = OptionalInt.of(0);   // 0을 저장
		OptionalInt optInt2  = OptionalInt.empty(); // 빈 객체를 생성

		System.out.println(optInt1.isPresent());   // true
		System.out.println(optInt2.isPresent());   // false
		
		System.out.println(optInt1.getAsInt());   // 0
//		System.out.println(optInt2.getAsInt());   // NoSuchElementException
		System.out.println("optInt1 ="+optInt1);
		System.out.println("optInt2="+optInt2);
	     	System.out.println("optInt1.equals(optInt2)?"+optInt1.equals(optInt2));
	
		Optional<String> opt  = Optional.ofNullable(null); // null을 저장
		Optional<String> opt2 = Optional.empty();          // 빈 객체를 생성
		System.out.println("opt ="+opt);
		System.out.println("opt2="+opt2);
		System.out.println("opt.equals(opt2)?"+opt.equals(opt2)); // true

		int result3 = optStrToInt(Optional.of("123"), 0);
		int result4 = optStrToInt(Optional.of(""), 0);

		System.out.println("result3="+result3);
		System.out.println("result4="+result4);
	}

	static int optStrToInt(Optional<String> optStr, int defaultValue) {
		try {
			return optStr.map(Integer::parseInt).get();
		} catch (Exception e){
			return defaultValue;
		}			
	}
}
```

```json
실행결과

optStr=abcde
optInt=5
result1=123
result2=-1
result3=456
true
false
0
optInt1 =OptionalInt[0]
optInt2=OptionalInt.empty
optInt1.equals(optInt2)?false
opt =Optional.empty
opt2=Optional.empty
opt.equals(opt2)?true
result3=123
result4=0
```

## 스트림의 최종 연산

**`최종 연산`** : **스트림의 요소를 소모해서 결과를 만든다.**
- 최종 연산 후에는 스트림이 닫히게 되고 더 이상 사용할 수 없다.
- 최종 연산의 결과 : 스트림 요소의 합과 같은 단일 값, 스트림의 요소가 담긴 배열, 컬렉션

### 스트림의 요소를 출력 - forEach()

**`forEach()`** : 반환 타입이 void, 스트림의 요소를 출력

```java
void forEach(Consumer<? super T> action)
```

### 조건 검사 - allMatch(), anyMatch(), noneMatch(), findFirst(), findAny()

**`allMatch(), anyMatch(), noneMatch()`** : **스트림의 요소에 대해 지정된 조건에 모든 요소가 일치하는지, 일부가 일치하는지, 어떠한 요소도 일치하지 않는지 확인**
- 모두 매개변수로 Prediate, boolean값을 반환

```java
boolean allMatch (Predicate<? super T> predicate)
boolean anyMatch (Predicate<? super T> predicate)
boolean noneMatch (Predicate<? super T> predicate)


// 학생성적 스트림에서 총점이 낙제점(총점 100점) 이하인 학생 확인
boolean noFailed = stuStream.anyMatch(s->s.getTotalScore()<=100);

// 스트림 요소 중 조건에 일치하는 첫 번째 것을 반환
// 주로 filter()와 함께 사용하여 조건에 맞는 스트림의 요소가 있는지 확인
Optional<Student> stu = stuStream.filter(s->s.getTotalScore()<=100).findFirst();

// 병렬스트림의 경우 findFirst() 대신 findAny() 사용
Optional<Student> stu = parallelStream.filter(s->s.getTotalScore()<=100).findAny();
```

- findAny(), findFirst()의 반환 값 : Optional< T>, 스트림의 요소가 없을 때는 내부적으로 null을 저장하고 있는 Optional객체를 반환한다.

### 통계 - count(), sum(), average(), max(), min()

- 기본형 스트림이 아닌 경우 통계 관련 메서드는 아래 3개 뿐이다.
- 기본형 스트림의 min(), max()와 달리 매개변수로 Comparator가 필요하다.
- 대부분의 경우, 기본형 스트림으로 반환하거나
- reduce(), collect()를 사용해서 통계정보를 얻는다.

```java
long count()
Optional<T> max(Comparator<? super T> comparator)
Optional<T> min(Comparator<? super T> comparator)
```

### 리듀싱 - reduce()

**`reduce()`** : **스트림의 요소를 줄여나가면서 연산을 수행하고 최종 결과를 반환**
- 그래서 매개변수의 타입이 BinaryOperator< T>
- **처음 두 요소를 가지고 연산한 결과를 가지고 그 다음 요소와 연산한다.**
- 이 과정에서 스트림의 요소를 하나씩 소모하게되어 그 결과를 반환한다.

**reduce()를 사용하는 방법은 간단하다.**
- **초기값(identity)과 어떤 연산(BinaryOperator)으로 스트림의 요소를줄여나갈 것인지만 결정하면 된다.**

```java
Optional<T> reduce(BinaryOperator<T> accumulator)

// 연산결과의 초기값(identity)을 갖는 reduce()
// 초기값과 스트림의 첫 번째 요소로 연산을 시작
// 스트림의 요소가 하나도 없는 경우, 초기값(identity)를 반환
// 따라서, 반환 값이 Optional<T>가 아니라 T 이다.
T reduce(T identity, BinaryOperator<T> accumulator)
U reduce(U identity, BiFunction<U,T,U> accumulator, BinaryOperator<U> combiner)
```

- identity : 초기값
- accumulator : 이전 연산결과와 스트림의 요소에 수행할 연산
- combiner :  병렬처리된 결과를 합치는데 사용할 연산(병렬 스트림)

```java
// T reduce(T identity, BinaryOperator<T> accum)
// -> int reduce(int identity, IntBinaryOperator accum)
int count = intStream.reduce(0, (a, b) -> a + 1); // count()

int sum = intStream.reduce(0, (a, b) -> a + b);   // sum()

int max = intStream.reduce(Integer.MIN_VALUE, (a, b) -> a > b ? a : b); // max()

int min = intStream.reduce(Integer.MAX_VALUE, (a, b) -> a < b ? a : b); // min()

-------

// OptionalInt reduce(IntBinaryOperator accumulator) 
OptionalInt max = intStream.reduce((a, b) -> a > b ? a : b); // max()
OptionalInt min = intStream.reduce((a, b) -> a < b ? a : b); // min()

OptionalInt max = intStream.reduce(Integer::max); // int max(int a, int b)
OptionalInt min = intStream.reduce(Integer::min); // int min(int a, int b)

int maxValue = max.getAsInt(); // OptionalInt에 저장된 값을 maxValue에 저장
```

**[reduce()의 내부 동작]**

```java
int a = identity; // 초기값을 a에 저장

for(int b : stream) {
	a = a + b;	// 모든 요소의 값을 a에 누적한다.
}

return a;

-->

T reduce(T identity, BinaryOperator<T> accumulator) {
   T a = identity;

   for(T b : stream) {
      a = accumulator.apply(a, b);
   }

   return a;
}
```

**[스트림의 최종 연산 사용 예시]**

```java
import java.util.*;
import java.util.stream.*;

class StreamEx5 {
	public static void main(String[] args) {
		String[] strArr = {
			"Inheritance", "Java", "Lambda", "stream",
			"OptionalDouble", "IntStream", "count", "sum"
		};

		Stream.of(strArr).forEach(System.out::println);

		boolean noEmptyStr = Stream.of(strArr).noneMatch(s->s.length()==0);
		Optional<String> sWord = Stream.of(strArr)
							           .filter(s->s.charAt(0)=='s').findFirst();

		System.out.println("noEmptyStr="+noEmptyStr);
		System.out.println("sWord="+ sWord.get());

		// Stream<String[]>À» IntStreamÀ¸·Î º¯È¯
		IntStream intStream1 = Stream.of(strArr).mapToInt(String::length);
		IntStream intStream2 = Stream.of(strArr).mapToInt(String::length);
		IntStream intStream3 = Stream.of(strArr).mapToInt(String::length);
		IntStream intStream4 = Stream.of(strArr).mapToInt(String::length);

		int count = intStream1.reduce(0, (a,b) -> a + 1);
		int sum   = intStream2.reduce(0, (a,b) -> a + b);

		OptionalInt max = intStream3.reduce(Integer::max);  
		OptionalInt min = intStream4.reduce(Integer::min);

		System.out.println("count="+count);
		System.out.println("sum="+sum);
		System.out.println("max="+ max.getAsInt());
		System.out.println("min="+ min.getAsInt());
	}
}
```

```json
실행결과

Inheritance
Java
Lambda
stream
OptionalDouble
IntStream
count
sum
noEmptyStr=true
sWord=stream
count=8
sum=58
max=14
min=3
```

## collect()

**`collect()`** : **스트림의 요소를 수집하는 최종 연산**
- **스트림의 요소를 수집하기 위해 수집하는 방법**을 정의한 것이 **컬렉터(Collector)**

컬렉터는 Collector interface를 구현한 것으로, 직접 구현할 수도 있고 미리 작성된 것을 사용할 수도 있다.

Collectors class는 미리 작성된 다양한 종류의 Collector를 반환하는 static메서드를 가지고 있다.

```json
collect() : 스트림의 최종 연산. 매개변수로 collector가 필요하다.
Collector : 인터페이스. collector는 이 인터페이스를 구현해야한다.
Collectors : 클래스. static 메서드로 미리 작성된 collector를 제공한다.
```

```java
Object collect(Collector collector) // Collector를 구현한 클래스의 객체를 매개변수로
Oject collect(Supplier supplier, BiConsumer accumulator, BiConsumer combiner)
```

- collect()는 매개변수의 타입이 Collector, 매개변수가 Collector interface를 구현한 class의 객체이어야 한다는 의미
- 매개변수가 3개인 collect()는 자주 사용되지는 않지만, Collector인터페이스를 구현하지 않고 간단히 람다식으로 수집할 때 사용하면 편리하다.

### 스트림을 컬렉션과 배열로 변환 - toList(), toSet(), toMap(), toCollection(), toArray()

**Collectios클래스의 메서드들 : 스트림의 모든 요소를 컬렉션에 수집**
- List나 Set이 아닌 특정 컬렉션을 지정하려면, **toCollection()** 에 해당 컬렉션의 생성자 참조를 매개변수로 넣어주면 된다.

```java
List<String> names = stuStream.map(Student::getName)
         .collect(Collectors.toList());

ArrayList<String> list = names.stream()
         .collect(Collectors.toCollection(ArrayList::new));
                              
// Map은 객체의 어떤 필드를 키와 값으로 사용할지를 지정해야 한다.
// 요소의 타입이 Person인 스트림에서 사람의 주민번호(regId)를 키로 하고, 값으로 Person 객체를 그대로 저장
Map<String, Person> map = personStream.collect(Collectors.toMap(p->p.getRegId(), p->p)

// 스트림에 저장된 요소들을 T[] 타입의 배열로 변환하려면 toArray() 사용
// 단, 해당 타입의 생성자 참조를 매개변수로 지정해줘야 한다. 
// 지정하지 않으면 반환되는 배열의 타입은 Object[]
Student[] stuNames = studentStream.toArray(Student[]::new);   // OK
Student[] stuNames = studentStream.toArray();                 // 에러
Object[] stuNames = studentStream.toArray();                  // OK
```

### 통게 - counting(), summingInt(), averagingInt(), maxBy(), minBy()

다른 최종연산들이 제공하는 통계 정보를 collect()로 똑같이 얻을 수 있다.

```java
long count = stuStream.count();
long count = stuStream.collect(Collectors.counting());

long totalScore = stuStream.mapToInt(Student::getTotalScore).sum();
long totalScore = stuStream.collect(Collectors.summingInt(Student::getTotalScore));

OptionalInt topScore = studentStream.mapToInt(Student::getTotalScore).sum();
Optional<Student> topStudent = stuStream.max(Comparator.comparingInt(Student::getTotalScore));
Optional<Student> topStudent = stuStream.collect(Collectors.maxBy(Comparator.comparingInt(Student::getTotalScore)));

IntSummaryStatistics stat = stuStream.mapToInt(Student::getTotalScore).summaryStatistics();
IntSummaryStatistics stat = stuStream.collect(Collectors.summarizingInt(Student::getTotalScore));
```

### 리듀싱 - reducing()

reducing() 역시 collect()로 가능하다. 
- IntStream에는 매개변수 3개인 collect()만 정의되어 있다.
- boxed() 를 통해 IntStream -> Stream< Integer> 로 변환하면 매개변수 1개인 collect() 사용 가능


**[Collectors.reducing()]** 

```java
// 아래의 메서드 목록 역시 와일드 카드를 제거하여 간략히 함.
Collector reducing(BinaryOperator<T> op)
Collector reducing(T identity, BinaryOperator<T> op)
Collector reducing(U identity, Function<T, U> mapper, BinaryOperator<T> op)
```

**[reducing() 사용 예시]**

```java
IntStream intStream = new Random().ints(1, 46).distinct().limit(6);

OptionalInt max = intStream.reduce(Integer::max);
Optional<Integer> max = intStream.boxed().collect(Collectors.reducing(Integer::max));

Long sum = intStream.reduce(0,(a,b) -> a + b);
Long sum = intStream.boxed().collect(Collectors.reducing(0,(a,b) -> a + b));

int grandTotal = stuStream.map(Student::getTotalScore).reduce(0, Integer::sum);
int grandTotal = stuStream.collect(Collectors.reducing(0, Student::getTotalScore, Integer::sum));
```

### 문자열 결합 - joining()

**`joining()`** : **문자열 스트리므이 모든 요소를 하나의 문자열로 연결해서 반환**
- 구분자 지정 가능
- 접두사, 접미사 지정 가능
- 스트림의 요소가 CharSequence의 자손인 StringBuffer, StringBuilder만 결합이 가능
- 스트림의 요소가 문자열이 아닌 경우에는 map()으로 먼저 스트림의 요소를 문자열로 변환해야 한다.

```java
String studentNames = stuStream.map(Student::getName).collect(Collectors.joining());
String studentNames = stuStream.map(Student::getName).collect(Collectors.joining(","));
String studentNames = stuStream.map(Student::getName).collect(Collectors.joining(",", "[", "]"));

// 만약 map()없이 스트림에 바로 Collectors.joining()하면, 스트림의 요소에 toString()을 호출한 결과를 결합한다.
String studentInfo = stuStream.collect(Collectors.joining(","));
```

**[collect() 메서드 사용 예시]**

```java
import java.util.*;
import java.util.stream.*;
import static java.util.stream.Collectors.*;

class StreamEx6 {
	public static void main(String[] args) {
		Student[] stuArr = {
			new Student("이자바", 3, 300),
			new Student("김자바", 1, 200),
			new Student("안자바", 2, 100),
			new Student("박자바", 2, 150),
			new Student("소자바", 1, 200),
			new Student("나자바", 3, 290),
			new Student("감자바", 3, 180)	
		};

		// 학생 이름만 뽑아서 List<String>에 저장
		List<String> names = Stream.of(stuArr).map(Student::getName)
									          .collect(Collectors.toList());
		System.out.println(names);

		// 스트림을 배열로 변환
		Student[] stuArr2 = Stream.of(stuArr).toArray(Student[]::new);

		for(Student s : stuArr2)
			System.out.println(s);

		// 스트림을 Map<String, Student>로 변환. 학생 이름이 key 
		Map<String,Student> stuMap = Stream.of(stuArr)
						                   .collect(Collectors.toMap(s->s.getName(), p->p));
		for(String name : stuMap.keySet())
			System.out.println(name +"-"+stuMap.get(name));
		
		long count = Stream.of(stuArr).collect(counting());
		long totalScore = Stream.of(stuArr)
                                .collect(summingInt(Student::getTotalScore));
		System.out.println("count="+count);
		System.out.println("totalScore="+totalScore);

		totalScore = Stream.of(stuArr)
			               .collect(reducing(0, Student::getTotalScore, Integer::sum));
		System.out.println("totalScore="+totalScore);

		Optional<Student> topStudent = Stream.of(stuArr)
		                                     .collect(maxBy(Comparator.comparingInt(Student::getTotalScore)));
		System.out.println("topStudent="+topStudent.get());

		IntSummaryStatistics stat = Stream.of(stuArr)
					                      .collect(summarizingInt(Student::getTotalScore));
		System.out.println(stat);

		String stuNames = Stream.of(stuArr)
							    .map(Student::getName)
							    .collect(joining(",", "{", "}"));
		System.out.println(stuNames);
	}
}


class Student implements Comparable<Student> {
	String name;
	int ban;
	int totalScore;

	Student(String name, int ban, int totalScore) { 
		this.name =name;
		this.ban =ban;
		this.totalScore =totalScore;
	}

	public String toString() { 
	   return String.format("[%s, %d, %d]", name, ban, totalScore).toString(); 
	}

	String getName() { return name;}
	int getBan() { return ban;}
	int getTotalScore() { return totalScore;}

	public int compareTo(Student s) {
		return s.totalScore - this.totalScore;
	}
}
```

```json
실행결과

[이자바, 김자바, 안자바, 박자바, 소자바, 나자바, 감자바]
[이자바, 3, 300]
[김자바, 1, 200]
[안자바, 2, 100]
[박자바, 2, 150]
[소자바, 1, 200]
[나자바, 3, 290]
[감자바, 3, 180]
안자바-[안자바, 2, 100]
김자바-[김자바, 1, 200]
박자바-[박자바, 2, 150]
나자바-[나자바, 3, 290]
감자바-[감자바, 3, 180]
이자바-[이자바, 3, 300]
소자바-[소자바, 1, 200]
count=7
totalScore=1420
totalScore=1420
topStudent=[이자바, 3, 300]
IntSummaryStatistics{count=7, sum=1420, min=100, average=202.857143, max=300}
{이자바,김자바,안자바,박자바,소자바,나자바,감자바}
```

### 그룹화와 분할 - groupingBy(), partitioningBy()

- **`그룹화`** : **스트림의 요소를 특정 기준으로 그룹화하는 것을 의미**
- **`분할`** : **스트림의 요소를 2가지, 지정된 조건에 일치하는 그룹과 일치하지 않는 그룹으로의 분할을 의미**
- groupingBy() : 스트림의 요소를 Function으로 분류
- partitioningBy() : Predicate으로 분류
- 메서드의 정의를 보면, Function, Predicate부분만 다르고 두 메서드가 동일하다
- 스트림의 2개의 그룹으로 나눠야 한다면, partitioningBy()가 빠르고,
- 나머지의 경우에는 groupingBy()가 빠르다.
- 그룹화와 분할의 결과는 Map에 담겨 반환된다.

```java
// groupingBy()
Collector groupingBy(Function classifier)
Collector groupingBy(Function classifier, Collector downstream)
Collector groupingBy(Function classifier, Supplier mapFactory, Collector downstream)

// partitioningBy()
Collector partitioningBy(Predicate predicate)
Collector partitioningBy(Predicate predicate, Collector downstream)
```

**[예시에 사용될 Student 클래스와 스트림 stuStream]**

```java
class Student {
   String name;    // 이름
   boolean isMale; // 성별
   int hak;        // 학년
   int ban;        // 반
   int score;        // 점수

   Student(String name, boolean isMale, int hak, int ban, int score) { 
      this.name    = name;
      this.isMale    = isMale;
      this.hak    = hak;
      this.ban    = ban;
      this.score  = score;
   }

   String    getName()  { return name;}
   boolean isMale()   { return isMale;}
   int        getHak()   { return hak;}
   int        getBan()   { return ban;}
   int        getScore() { return score;}
   
   public String toString() { 
      return String.format("[%s, %s, %d학년 %d반, %3d점]", name, isMale ? "남":"여", hak, ban, score); 
   }
   
   enum Level {    // 성적을 상, 중, 하 3단계로 분류
      HIGH, MID, LOW
   }
}// class Student

Stream<Student> stuStream = Stream.of(
   new Student("나자바", true,  1, 1, 300),    
   new Student("김지미", false, 1, 1, 250),    
   new Student("김자바", true,  1, 1, 200),    
   new Student("이지미", false, 1, 2, 150),    
   new Student("남자바", true,  1, 2, 100),    
   new Student("안지미", false, 1, 2,  50),    
   new Student("황지미", false, 1, 3, 100),    
   new Student("강지미", false, 1, 3, 150),    
   new Student("이자바", true,  1, 3, 200),    
   new Student("나자바", true,  2, 1, 300),    
   new Student("김지미", false, 2, 1, 250),    
   new Student("김자바", true,  2, 1, 200),    
   new Student("이지미", false, 2, 2, 150),    
   new Student("남자바", true,  2, 2, 100),    
   new Student("안지미", false, 2, 2,  50),    
   new Student("황지미", false, 2, 3, 100),    
   new Student("강지미", false, 2, 3, 150),    
   new Student("이자바", true,  2, 3, 200)
);
```

### partitioningBy()에 의한 분류

1. 학생들을 성별로 나누어 List에 담는다.

```java
1. 기본 분할
Map<Boolean, List<Student>> stuBySex = stuStream.collect(Collectors.partitioningBy(Student::isMale)); // 학생들을 성별로 분할
 
List<Student> maleStudent = stuBySex.get(true); // 남학생
List<Student> femaleStudent = stuBySex.get(false); // 여학생
```

2. counting()을 추가해서 남학생의 수와 여학생의 수를 구한다.

```java
2. 기본 분할 + 통계 정보
Map<Boolean, List<Student>> stuNumBySex = stuStream
         .collect(partitioningBy(Student::isMale), counting());
 
System.out.println("남학생 수 : " + stuNumBySex.get(true)); // 8
System.out.println("여학생 수 : " + stuNumBySex.get(false)); // 10
```

3. 남학생 1등과 여학생 1등을 구해보자.

```java
// 반환타입은 Optional<Student>
// maxBy의 반환타입이 Optional<Student>
Map<Boolean, Optional<Student>> topScoreBySex = stuStream
         .collect(
            Collectors.partitioningBy(Student::isMale, 
               maxBy(comparingInt(Student::getScore))));

System.out.println(topScoreBySex.get(true)); // Optional [[남일등, 남, 1, 1, 300]]
System.out.println(topScoreBySex.get(false)); // Optional [[김지미, 여, 1, 1, 250]]

// 반환타입이 Student
// collectingAndThen(), Optional::get을 함께 사용
Map<Boolean, student> topScoreBySex = stuStream
         .collect(
            Collectors.partitioningBy(
               Student::isMale, collectingAndThen(
                  maxBy(comparingInt(Student::getScore)), Optional::get)));

System.out.println(topScoreBySex.get(true)); // [남일등, 남, 1, 1, 300]
System.out.println(topScoreBySex.get(false)); // [김지미, 여, 1, 1, 250]
```

4. **이중 분핳** : 성적이 150점 아래인 학생들을 성별로 분류하고 싶을 때, partitioningBy()를 한번 더 사용

```java
Map<Boolean, Map<Boolean, List<Student>>> failedStuBySex = stuStream.collect(
         Collectors.partitioningBy(Student::isMale, 
            Collectors.partitioningBy(s->s.getScore() < 150)));

List<Student> failedMaleStu = failedStuBySex.get(true).get(true);
List<Student> failedFemaleStu = failedStuBySex.get(false).get(true);
```

**[partitioningBy() 사용 예제]**

```java
import java.util.*;
import java.util.function.*;
import java.util.stream.*;
import static java.util.stream.Collectors.*;
import static java.util.Comparator.*;

class Student {
	String name;
	boolean isMale; // 성별
	int hak;		    // 학년
	int ban;		    // 반
	int score;

	Student(String name, boolean isMale, int hak, int ban, int score) { 
		this.name	= name;
		this.isMale	= isMale;
		this.hak	= hak;
		this.ban	= ban;
		this.score  = score;
	}
	String	getName()  { return name;}
	boolean isMale()    { return isMale;}
	int		getHak()   { return hak;}
	int		getBan()   { return ban;}
	int		getScore() { return score;}

	public String toString() { 
		return String.format("[%s, %s, %d학년 %d반, %3d점]",
			name, isMale ? "남":"여", hak, ban, score); 
	}
   // groupingBy()에서 사용
	enum Level { HIGH, MID, LOW }  // 성적을 상, 중, 하 세 단계로 분류
}

class StreamEx7 {
	public static void main(String[] args) {
		Student[] stuArr = {
			new Student("나자바", true,  1, 1, 300),	
			new Student("김지미", false, 1, 1, 250),	
			new Student("김자바", true,  1, 1, 200),	
			new Student("이지미", false, 1, 2, 150),	
			new Student("남자바", true,  1, 2, 100),	
			new Student("안지미", false, 1, 2,  50),	
			new Student("황지미", false, 1, 3, 100),	
			new Student("강지미", false, 1, 3, 150),	
			new Student("이자바", true,  1, 3, 200),	

			new Student("나자바", true,  2, 1, 300),	
			new Student("김지미", false, 2, 1, 250),	
			new Student("김자바", true,  2, 1, 200),	
			new Student("이지미", false, 2, 2, 150),	
			new Student("남자바", true,  2, 2, 100),	
			new Student("안지미", false, 2, 2,  50),	
			new Student("황지미", false, 2, 3, 100),	
			new Student("강지미", false, 2, 3, 150),	
			new Student("이자바", true,  2, 3, 200)	
		};

		System.out.printf("1. 단순분할(성별로 분할)%n");
		Map<Boolean, List<Student>> stuBySex =  Stream.of(stuArr)
				.collect(partitioningBy(Student::isMale));

		List<Student> maleStudent   = stuBySex.get(true);
		List<Student> femaleStudent = stuBySex.get(false);

		for(Student s : maleStudent)   System.out.println(s);
		for(Student s : femaleStudent) System.out.println(s);

		System.out.printf("%n2. 단순분할 + 통계(성별 학생수)%n");
		Map<Boolean, Long> stuNumBySex = Stream.of(stuArr)
				.collect(partitioningBy(Student::isMale, counting()));

		System.out.println("남학생 수 :"+ stuNumBySex.get(true));
		System.out.println("여학생 수 :"+ stuNumBySex.get(false));


		System.out.printf("%n3. 단순분할 + 통계(성별 1등)%n");
		Map<Boolean, Optional<Student>> topScoreBySex = Stream.of(stuArr)
				.collect(partitioningBy(Student::isMale, 
					maxBy(comparingInt(Student::getScore))
				));
		System.out.println("남학생 1등 :"+ topScoreBySex.get(true));
		System.out.println("여학생 1등 :"+ topScoreBySex.get(false));

		Map<Boolean, Student> topScoreBySex2 = Stream.of(stuArr)
			.collect(partitioningBy(Student::isMale, 
				collectingAndThen(
					maxBy(comparingInt(Student::getScore)), Optional::get
				)
			));
		System.out.println("남학생 1등 :"+ topScoreBySex2.get(true));
		System.out.println("여학생 1등 :"+ topScoreBySex2.get(false));

		System.out.printf("%n4. 다중분할(성별 불합격자, 100점 이하)%n");

		Map<Boolean, Map<Boolean, List<Student>>> failedStuBySex = 
			Stream.of(stuArr).collect(partitioningBy(Student::isMale, 
				partitioningBy(s -> s.getScore() <= 100))
			); 
		List<Student> failedMaleStu   = failedStuBySex.get(true).get(true);
		List<Student> failedFemaleStu = failedStuBySex.get(false).get(true);

		for(Student s : failedMaleStu)   System.out.println(s);
		for(Student s : failedFemaleStu) System.out.println(s);
	}
}
```

```json
실행결과

1. 단순분할(성별로 분할)
[나자바, 남, 1학년 1반, 300점]
[김자바, 남, 1학년 1반, 200점]
[남자바, 남, 1학년 2반, 100점]
[이자바, 남, 1학년 3반, 200점]
[나자바, 남, 2학년 1반, 300점]
[김자바, 남, 2학년 1반, 200점]
[남자바, 남, 2학년 2반, 100점]
[이자바, 남, 2학년 3반, 200점]
[김지미, 여, 1학년 1반, 250점]
[이지미, 여, 1학년 2반, 150점]
[안지미, 여, 1학년 2반,  50점]
[황지미, 여, 1학년 3반, 100점]
[강지미, 여, 1학년 3반, 150점]
[김지미, 여, 2학년 1반, 250점]
[이지미, 여, 2학년 2반, 150점]
[안지미, 여, 2학년 2반,  50점]
[황지미, 여, 2학년 3반, 100점]
[강지미, 여, 2학년 3반, 150점]

2. 단순분할 + 통계(성별 학생수)
남학생 수 :8
여학생 수 :10

3. 단순분할 + 통계(성별 1등)
남학생 1등 :Optional[[나자바, 남, 1학년 1반, 300점]]
여학생 1등 :Optional[[김지미, 여, 1학년 1반, 250점]]
남학생 1등 :[나자바, 남, 1학년 1반, 300점]
여학생 1등 :[김지미, 여, 1학년 1반, 250점]

4. 다중분할(성별 불합격자, 100점 이하)
[남자바, 남, 1학년 2반, 100점]
[남자바, 남, 2학년 2반, 100점]
[안지미, 여, 1학년 2반,  50점]
[황지미, 여, 1학년 3반, 100점]
[안지미, 여, 2학년 2반,  50점]
[황지미, 여, 2학년 3반, 100점]
```

### groupingBy()에 의한 분할

**groupingBy()로 그룹화를 하면 기본적으로 List< T>에 담는다.**
- 원한다면, toList() 대신, toSet(), toCollection(HashSet::new)을 사용 가능하다.
  - 단, Map의 Generic타입도 같이 변경해줘야 한다.

1. stuStream을 반별로 그룹짓기

```java
Map<Integer, List<Student>> stuByBan = stuStream
         .collect(Collectors.groupingBy(Student::getBan, toList()));   //toList()생략가능

Map<Integer, HashSet<Student>> stuByHak = stuStream
         .collect(Collectors.groupingBy(Student::getHak, toCollection(HashSet::new)));
```

2. stuStream을 성적의 등급(Student.Level)으로 그룹화
- 모든 학생을 HIGH, MID, LOW으로 분류하여 집계

```java
Map<Student.Level, Long> stuByLevel = stuStream
      .collect(Collectors.groupingBy(s->
         { if(s.getScore()>=200) return Student.Level.HIGH;
           else if(s.getScore()>=100) return Student.Level.MID;
           else return Student.Level.LOW;
         }, counting()));
// MID - 8명, HIGH - 8명, LOW - 2명
```

3. groupingBy()를 여러번 사용
- 학년별로 그룹화 후, 반별 그룹화

```java
Map<Integer, Map<Integer, List<Student>>> stuByHakAndBan = stuStream
      .collect(Collectors.groupingBy(Student::getHak,
         Collectors.groupingBy(Student::getBan)));
```

4. 학년별로 그룹화 한 후, 각 반의 1등을 출력
- collectingAndThen(), maxBy() 사용

```java
Map<Integer, Map<Integer, Student>> topStuByHakAndBan = stuStream
      .collect(Collectors.groupingBy(Student::getHak,
         Collectors.groupingBy(Student::getBan, 
            collectingAndThen(
               maxBy(comparingInt(Student::getScore)), Optional::get))));
```

5. 학년별, 반별로 그룹화한 다음에, 성적 그룹으로 변환(mapping)하여, Set에 저장

```java
Map<Integer, Map<Integer, Set<Student.Level>>> stuByHakAndBan = stuStream
      .collect(Collectors.groupingBy(Student::getHak,
         Collectors.groupingBy(Student::getBan,
            mapping(s-> {
               if(s.getScore()>=200) return Student.Level.HIGH;
               else if(s.getScore()>=100) return Student.Level.MID;
               else return Student.Level.LOW;
               } , toSet()))));
```

**[groupingBy() 사용 예제]**

```java
import java.util.*;
import java.util.function.*;
import java.util.stream.*;
import static java.util.stream.Collectors.*;
import static java.util.Comparator.*;

class Student {
	String name;
	boolean isMale; // 성별
	int hak;		// 학년
	int ban;		// 반
	int score;

	Student(String name, boolean isMale, int hak, int ban, int score) { 
		this.name	= name;
		this.isMale	= isMale;
		this.hak	= hak;
		this.ban	= ban;
		this.score  = score;
	}

	String	getName()  { return name;}
	boolean isMale()   { return isMale;}
	int		getHak()   { return hak;}
	int		getBan()   { return ban;}
	int		getScore() { return score;}

	public String toString() { 
		return String.format("[%s, %s, %d학년 %d반, %3d점]", name, isMale ? "남":"여", hak, ban, score); 
	}

	enum Level {
		HIGH, MID, LOW
	}
}

class StreamEx8 {
	public static void main(String[] args) {
		Student[] stuArr = {
			new Student("나자바", true,  1, 1, 300),	
			new Student("김지미", false, 1, 1, 250),	
			new Student("김자바", true,  1, 1, 200),	
			new Student("이지미", false, 1, 2, 150),	
			new Student("남자바", true,  1, 2, 100),	
			new Student("안지미", false, 1, 2,  50),	
			new Student("황지미", false, 1, 3, 100),	
			new Student("강지미", false, 1, 3, 150),	
			new Student("이자바", true,  1, 3, 200),	

			new Student("나자바", true,  2, 1, 300),	
			new Student("김지미", false, 2, 1, 250),	
			new Student("김자바", true,  2, 1, 200),	
			new Student("이지미", false, 2, 2, 150),	
			new Student("남자바", true,  2, 2, 100),	
			new Student("안지미", false, 2, 2,  50),	
			new Student("황지미", false, 2, 3, 100),	
			new Student("강지미", false, 2, 3, 150),	
			new Student("이자바", true,  2, 3, 200)	
		};

		System.out.printf("1. 단순그룹화(반별로 그룹화)%n");
		Map<Integer, List<Student>> stuByBan = Stream.of(stuArr)
				                                     .collect(groupingBy(Student::getBan));
		
		for(List<Student> ban : stuByBan.values()) {
			for(Student s : ban) {
				System.out.println(s);
			}
		}

		System.out.printf("%n2. 단순그룹화(성적별로 그룹화)%n");
		Map<Student.Level, List<Student>> stuByLevel = Stream.of(stuArr)
				.collect(groupingBy(s-> {
						 if(s.getScore() >= 200) return Student.Level.HIGH;
					else if(s.getScore() >= 100) return Student.Level.MID;
					else                         return Student.Level.LOW;
				}));

		TreeSet<Student.Level> keySet = new TreeSet<>(stuByLevel.keySet());

		for(Student.Level key : keySet) {
			System.out.println("["+key+"]");

			for(Student s : stuByLevel.get(key))
				System.out.println(s);
			System.out.println();
		}

		System.out.printf("%n3. 단순그룹화 + 통계(성적별 학생수)%n");
		Map<Student.Level, Long> stuCntByLevel = Stream.of(stuArr)
				.collect(groupingBy(s-> {
						 if(s.getScore() >= 200) return Student.Level.HIGH;
					else if(s.getScore() >= 100) return Student.Level.MID;
					else                         return Student.Level.LOW;
				}, counting()));

		for(Student.Level key : stuCntByLevel.keySet())
			System.out.printf("[%s] - %d명, ", key, stuCntByLevel.get(key));
		System.out.println();
/*
		for(List<Student> level : stuByLevel.values()) {
			System.out.println();
			for(Student s : level) {
				System.out.println(s);
			}
		}
*/
		System.out.printf("%n4. 다중그룹화(학년별, 반별)%n");
		Map<Integer, Map<Integer, List<Student>>> stuByHakAndBan =
          Stream.of(stuArr)
				.collect(groupingBy(Student::getHak,
						 groupingBy(Student::getBan)
				));

		for(Map<Integer, List<Student>> hak : stuByHakAndBan.values()) {
			for(List<Student> ban : hak.values()) {
				System.out.println();
				for(Student s : ban)
					System.out.println(s);
			}
		}

		System.out.printf("%n5. 다중그룹화 + 통계(학년별, 반별 1등)%n");
		Map<Integer, Map<Integer, Student>> topStuByHakAndBan = Stream.of(stuArr)
				.collect(groupingBy(Student::getHak,
						 groupingBy(Student::getBan,
							collectingAndThen(
								maxBy(comparingInt(Student::getScore)),
								Optional::get
							)
						)
				));

		for(Map<Integer, Student> ban : topStuByHakAndBan.values())
			for(Student s : ban.values())
				System.out.println(s);

		System.out.printf("%n6. 다중그룹화 + 통계(학년별, 반별 성적그룹)%n");
		Map<String, Set<Student.Level>> stuByScoreGroup = Stream.of(stuArr)
			.collect(groupingBy(s-> s.getHak() + "-" + s.getBan(),
					mapping(s-> {
						 if(s.getScore() >= 200) return Student.Level.HIGH;
					else if(s.getScore() >= 100) return Student.Level.MID;
						 else                    return Student.Level.LOW;
					} , toSet())
			));

		 Set<String> keySet2 = stuByScoreGroup.keySet();

		for(String key : keySet2) {
			System.out.println("["+key+"]" + stuByScoreGroup.get(key));
		}
	}  // main의 끝
}
```

```json
실행결과

1. 단순그룹화(반별로 그룹화)
[나자바, 남, 1학년 1반, 300점]
[김지미, 여, 1학년 1반, 250점]
[김자바, 남, 1학년 1반, 200점]
[나자바, 남, 2학년 1반, 300점]
[김지미, 여, 2학년 1반, 250점]
[김자바, 남, 2학년 1반, 200점]
[이지미, 여, 1학년 2반, 150점]
[남자바, 남, 1학년 2반, 100점]
[안지미, 여, 1학년 2반,  50점]
[이지미, 여, 2학년 2반, 150점]
[남자바, 남, 2학년 2반, 100점]
[안지미, 여, 2학년 2반,  50점]
[황지미, 여, 1학년 3반, 100점]
[강지미, 여, 1학년 3반, 150점]
[이자바, 남, 1학년 3반, 200점]
[황지미, 여, 2학년 3반, 100점]
[강지미, 여, 2학년 3반, 150점]
[이자바, 남, 2학년 3반, 200점]

2. 단순그룹화(성적별로 그룹화)
[HIGH]
[나자바, 남, 1학년 1반, 300점]
[김지미, 여, 1학년 1반, 250점]
[김자바, 남, 1학년 1반, 200점]
[이자바, 남, 1학년 3반, 200점]
[나자바, 남, 2학년 1반, 300점]
[김지미, 여, 2학년 1반, 250점]
[김자바, 남, 2학년 1반, 200점]
[이자바, 남, 2학년 3반, 200점]

[MID]
[이지미, 여, 1학년 2반, 150점]
[남자바, 남, 1학년 2반, 100점]
[황지미, 여, 1학년 3반, 100점]
[강지미, 여, 1학년 3반, 150점]
[이지미, 여, 2학년 2반, 150점]
[남자바, 남, 2학년 2반, 100점]
[황지미, 여, 2학년 3반, 100점]
[강지미, 여, 2학년 3반, 150점]

[LOW]
[안지미, 여, 1학년 2반,  50점]
[안지미, 여, 2학년 2반,  50점]


3. 단순그룹화 + 통계(성적별 학생수)
[HIGH] - 8명, [LOW] - 2명, [MID] - 8명, 

4. 다중그룹화(학년별, 반별)

[나자바, 남, 1학년 1반, 300점]
[김지미, 여, 1학년 1반, 250점]
[김자바, 남, 1학년 1반, 200점]

[이지미, 여, 1학년 2반, 150점]
[남자바, 남, 1학년 2반, 100점]
[안지미, 여, 1학년 2반,  50점]

[황지미, 여, 1학년 3반, 100점]
[강지미, 여, 1학년 3반, 150점]
[이자바, 남, 1학년 3반, 200점]

[나자바, 남, 2학년 1반, 300점]
[김지미, 여, 2학년 1반, 250점]
[김자바, 남, 2학년 1반, 200점]

[이지미, 여, 2학년 2반, 150점]
[남자바, 남, 2학년 2반, 100점]
[안지미, 여, 2학년 2반,  50점]

[황지미, 여, 2학년 3반, 100점]
[강지미, 여, 2학년 3반, 150점]
[이자바, 남, 2학년 3반, 200점]

5. 다중그룹화 + 통계(학년별, 반별 1등)
[나자바, 남, 1학년 1반, 300점]
[이지미, 여, 1학년 2반, 150점]
[이자바, 남, 1학년 3반, 200점]
[나자바, 남, 2학년 1반, 300점]
[이지미, 여, 2학년 2반, 150점]
[이자바, 남, 2학년 3반, 200점]

6. 다중그룹화 + 통계(학년별, 반별 성적그룹)
[1-1][HIGH]
[2-1][HIGH]
[1-2][MID, LOW]
[2-2][MID, LOW]
[1-3][HIGH, MID]
[2-3][HIGH, MID]
```

## Collector 구현하기

Collector를 구현한다는 것은 **Collector 인터페이스(interface)** 를 구현한다는 것을 의미

```java
public interface Collector<T, A, R> {
	Supplier<A> supplier();
	BiConsumer<A, T> accumulator();
	BinaryOperator<A> combiner();
	Function<A, R> finisher();

	Set<Characteristics> characterisitcs(); // 컬렉터의 특성이 담긴 Set을 반환
	...
}
```

- 직접 구현해야하는 것은 5개의 메서드
- characterisitcs() 를 제외하면 모두 반환형이 **함수형 인터페이스**
  - **즉, 4개의 람다식(lambda expression)** 을 작성하면 되는 것이다.

```java
supplier()  : 작업 결과를 저장할 공간을 제공
accumulator() : 스트림의 요소를 어떻게 supplier()가 제공할 공간에 수집(collect)할 것인가를 정의
combiner() : 두 저장공간을 병합할 방법을 제공(병렬 스트림), 여러 쓰레드에 의해 처리된 결과를 어떻게 합칠 것인가를 정의
finisher() : 결과를 최종적으로 변환할 방법을 제공, 작업 결과를 변환하는 일을 하는데 변환이 필요없다면, 항등 함수인 Function.identity()를 반환하면 된다.
```

```java
public Function finisher() {
	return Function.identity(); // 항등 함수를 반환. return x -> x; 와 동일
}
```



- characterisitcs() : **컬렉터가 수행하는 작업의 속성**에 대한 정보를 제공하기 위한 것

```java
Characteristics.CONCURRENT : 병렬로 처리할 수 있는 작업
Characteristics.UNORDERED : 스트림의 요소의 순서가 유지될 필요가 없는 작업
Characteristics.IDENTITY_FINISH : finisher()가 함등 함수인 작업

위의 3가지 속성 중에서 해당하는 것을 Set에 담아서 반환하도록 구현하면 된다.

열거형 Characvterisitcs 는 Collector 내에 정의되어 있다.
```

```java
public Set<Characvterisitcs> characteristics() {
	return Collections.unmodifiableSet(EnumSet.of(
		Collector.Characteristics.CONCURRENT,
		Collector.Characterisitcs.UNORDERED
	));
}

// 아무런 속성을 지정하고 싶지 않을 때
Set<Characvterisitcs> characteristics() {
	return Collections.emptySet(); // 지정할 특성이 없는 경우 비어있는 Set을 반환
}
```

- 결국 Collector도 내부적으로 처리하는 과정이 reducing과 같다는 것을 의미
  - finisher()를 제외하고 supplier(), accumulator(), combiner()는 모두 앞서 reducing에 나왔던 개념들이다.

- IntStream의 count(), sum(), max(), min()등이 reduce()로 구현되어 있다.
- collect()로도 count()와 같은 기능을 수행할 수 있다.

```java
long count = stuStream.count();
long count = stuStream.collect(counting());
```

#### reduce()와 collect()의 차이점

- 이 둘은 근본적으로 하는 일은 같다.
- collect() : 그룹화와 분할, 집계 등에 유용하게 쓰이고,
  - 병렬화에 있어서 reduce()보다 collect()가 더 유리하다.
- 결론적으로, reduce()에 대해서 잘 이해했다면, Collector를 구현하는 일이 어렵지 않을 것이다.


**[String배열의 모든 문자열을 하나의 문자열로 합치는 예제]**

- 이 기능을 하는 Collector를 Customizing 해보자

```java
String[] strArr = {"aaa", "bbb", "ccc"};
StringBuffer sb = new StringBuffer(); // supplier() 저장할 공간을 생성

for(String temp : strArr) {
	sb.append(temp); // accumulator(), sb에 요소를 저장
}

String result = sb.toString(); // finisher(), StringBuffer를 String으로 변환
```

-->

```java
import java.util.*;
import java.util.function.*;
import java.util.stream.*;

class CollectorEx1 {
	public static void main(String[] args) {
		String[] strArr = { "aaa","bbb","ccc" };
		Stream<String> strStream = Stream.of(strArr);

		String result = strStream.collect(new ConcatCollector());

		System.out.println(Arrays.toString(strArr));
		System.out.println("result="+result);
	}
}

class ConcatCollector implements Collector<String, StringBuilder, String> {
	@Override
	public Supplier<StringBuilder> supplier() {
		return () -> new StringBuilder();
//		return StringBuilder::new;
	}

	@Override
	public BiConsumer<StringBuilder, String> accumulator() {
		return (sb, s) -> sb.append(s);
//		return StringBuilder::append;
	}

	@Override
	public Function<StringBuilder, String> finisher() {
		return sb -> sb.toString();
//		return StringBuilder::toString;
	}

	@Override
	public BinaryOperator<StringBuilder> combiner() {
		return (sb, sb2)-> {
			sb.append(sb2);
			return sb;
		};
	}

	@Override
	public Set<Characteristics> characteristics() {
		return Collections.emptySet();
	}
}
```

```java
실행결과
[aaa, bbb, ccc]
result = aaabbbccc
```

## 스트림의 변환

스트림간의 변환에 있어서 어떤 메서드를 써야할지 혼동이 올 때 참고하면 좋을 듯 하다.

**[스트림 변환에 사용되는 메서드]**

1. 스트림 -> 기본형 스트림

|from|to|변환메서드|
|---|---|---|
|Stream < T>|IntStream|mapToInt(ToIntFunction< T> mapper)|
|Stream < T>|LongStream|mapToLong(ToLongFunction< T> mapper)|
|Stream < T>|DoubleStream|mapToDouble(ToDoubleFunction< T> mapper)|

2. 기본형 스트림 -> 스트림 

|from|to|변환메서드|
|---|---|---|
|IntStream|Stream< Integer>|boxed()|
|LongStream|Stream< Long>|boxed()|
|DoubleStream|Stream< Long>|boxed()|
|IntStream, LongStream, DoubleStream|Stream< U>|mapToObj(DoubleFunction< U> mapper)|

3. 기본형 스트림 -> 기본형 스트림

|from|to|변환메서드|
|---|---|---|
|IntStream|||
|LongStream|LongStream|asLongStream()|
|DoubleStream|Stream< Long>|asDoubleStream()|

4. 스트림 -> 부분 스트림

|from|to|변환메서드|
|---|---|---|
|Stream< T>|Stream< T>|skip(long n)|
|IntStream|IntStream|limit(long maxSize)|

5. 2개의 스트림 -> 스트림

|from|to|변환메서드|
|---|---|---|
|Stream< T>, Stream< T>|Stream< T>|concat(Stream< T> a, Stream< T> b)|
|IntStream, IntStream|IntStream|concat(IntStream a, IntStream b)|
|LongStream, LongStream|LongStream|concat(LongStream a, LongStream b)|
|DoubleStream, DoubleStream|DoubleStream|concat(DoubleStream a, DoubleStream b)|

6. 스트림의 스트림 -> 스트림

|from|to|변환메서드|
|---|---|---|
|Stream< Stream< T>>|Stream< T>|flatMap(Function mapper)|
|Stream< IntStream>|IntStream|flatMapToInt(Function mapper)|
|Stream< LongStream>|LongStream|flatMapToLong(Function mapper)|
|Stream< DoubleStream>|DoubleStream|flatMapToDouble(Function mapper)|

7. 스트림 <-> 병렬 스트림

|from|to|변환메서드|
|---|---|---|
|Stream< T>|Stream< T>|parallel() // 스트림 -> 병렬 스트림|
|IntStream|IntStream|sequential() // 병렬 스트림 -> 스트림|
|LongStream|LongStream|위의 메서드들과 동일|
|DoubleStream|DoubleStream|위의 메서드들과 동일|

8. 스트림 -> 컬렉션

|from|to|변환메서드|
|---|---|---|
|Stream< T>, IntStream, LongStream, DoubleStream|Collection< T>|collect(Collectors.toCollection(Supplierfactory)) |
|Stream< T>, IntStream, LongStream, DoubleStream|List< T>|collect(Collectors.toList())|
|Stream< T>, IntStream, LongStream, DoubleStream|Set< T>|collect(Collectors.toSet())|


9. 컬렉션 -> 스트림

|from|to|변환메서드|
|---|---|---|
|Collection< T>, List< T>, Set< T>|Stream< T>|stream() |

10. 스트림 -> Map

|from|to|변환메서드|
|---|---|---|
|Stream< T>, IntStream, LongStream, DoubleStream|Map< K,V>|collect(Collectors.toMap(Function key, Function value)) |
|Stream< T>, IntStream, LongStream, DoubleStream|Map< K,V>|collect(Collectors.toMap(Function k, Function v, BinaryOperator)) |
|Stream< T>, IntStream, LongStream, DoubleStream|Map< K,V>|collect(Collectors.toMap(Function k, Function v, BinaryOperator merge, Supplier mapSupplier)) |

11. 스트림 -> 배열

|from|to|변환메서드|
|---|---|---|
|Stream< T>|Object[]|toArray()|
|Stream< T>|T[]|toArray(IntFunction< A[]>) generator|
|IntStream, LongStream, DoubleStream|int[], long[], double[]|toArray()|


















