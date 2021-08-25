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

### 특정 범위의 정수 - range(), rangeClosed()

지정된 범위의 연속된 정수를 stream으로 생성해서 반환하는 range(), rangeClosed()
- **range() : 경계의 끝인 end가 범위에 포함 X**
- **rangeClosed() : end가 범위에 포함 O**

```java
IntStream IntStream.range(int begin, int end)
IntStream IntStream.rangeClosed(int begin, int end)

IntStream intStream = IntStream.range(1, 5); // 1,2,3,4 
IntStream intStream = IntStream.rangeClosed(1, 5); // 1,2,3,4,5
```

### 임의의 수 - ints(), longs(), doubles()

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

### 람다식 - iterate(), generate()

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

### 파일 - list()

java.nio.file.Files는 파일을 다루는데 필요한 유용한 메서드들을 제공하는데, 
- **list()** : 지정된 디렉토리(dir)에 있는 파일의 목록을 소스로 하는 스트림을 생성해서 반환.

```java
 Stream<Path> Files.list(Path dir) // Path는 하나의 파일 또는 디렉토리
```

### 빈 스트림 - empty()

**empty()** : **요소가 하나도 없는 비어있는 스트림을 생성할 수도 있다.** 
- 스트림에 연산을 수행한 결과가 하나도 없을 때
  - **null보다는 빈 스트림을 return하는 것이 낫다.**

```java
Stream emptyStream = Stream.empty(); // empty()는 빈 스트림을 생성해서 반환
long count = emptyStream.count(); // count의 값은 0
```

### 두 스트림의 연결 - concat()

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