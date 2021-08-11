# 12장 지네릭스, 열거형, 애너테이션(Generics, Enum, Annotation)

## 지네릭스(Generics)란?

**`지네릭스(Generics)`** : **다양한 타입의 객체들**을 다루는 메서드나 컬렉션 클래스에 **컴파일 시의 타입 체크(compile-type-check)** 를 해주는 기능
- 객체의 타입을 컴파일시에 체크
- **객체의 타입 안전성을 높이고, 형변환의 번거로움이 줄어든다!**

타입 안전성을 높인다 : 
1. **의도하지 않은 타입의 객체가 저장되는 것을 막고,** 
2. **저장된 객체를 꺼내올 때, 원래의 타입과 다른 타입으로 잘못 형변환되어 발생할 수 있는 오류를 줄여준다.**

ex) ArrayList와 같은 컬렉션 클래스에 다양한 종류의 객체를 담을 수 있지만, 보통은 한가지 객체만 담는다. 그럴때마다 매번 타입체크를 하거나 형변환을 해주는 것은 너무 힘들다. 이러한 사항들을 지네릭스개 해결해준다!!

**지네릭스(Generics)의 장점**
1. **타입 안정성**을 제공
2. **타입 체크와 형변환을 생략**할 수 있으므로 **코드가 간결**해진다.

-> **다른 객체의 타입을 미리 명시해주므로 번거로운 형변환을 생략 가능!!!**

## 지네릭 클래스의 선언

**클래스에 선언된 지네릭 타입**

```java
// Before
class Box {
    Object item;

    void setItem(Object item) {
        this.item = item;
    } 

    Object getItem() {
        return item;
    }
}

--> 

// After
class Box<T> {
    T item;

    void setItem(T item) {
        this.item = item;
    } 

    T getItem() {
        return item;
    }
}
```

클래스 옆에 **`<T>`** 를 붙이면 된다. **Object를 모두 T로 바꾼다.**

`T`는 **`타입 변수(type variable)`** 라고 하며, `Type`의 첫 글자에서 따온 것이다.
- 타입 변수에는 **T(Type), E(Element), K(Key) V(Value)** 로
여러가지가 있는데, 상황에 따라 알맞게 문자를 선택해서 사용하면 된다.
- **기호의 종류만 다를 뿐,'임의의 참조형 타입'을 의미한다는 것은 모두 같다.**
  - 기존에는 다양한 종류의 타입을 다루는 메서드의 매개변수나 리턴타입으로 Objext타입의 탐조변수를 많이 사용했고, 그로인해 형변환이 불가피했지만, **이젠 Object타입 대신 원하는 타입을 지정하기만 하면 되는 것이다!**

```java
Box<String> b = new Box<String>(); // 타입 T 대신, 실제 타입을 지정
b.setItem(new Object())  // 에러, String이외에 타입은 지정불가
b.setItem("ABC")  // ok
String item = b.getItem(); // (String)b.getItem(); 처럼 형변환이 필요없음 

class Box { // 지네릭 타입을 String으로 지정
    String item;

    void setItem(String item) {
        this.item = item;
    } 

    String getItem() {
        return item;
    }
} 
```

지네릭이 도입되기 이전의 코드와 호환을 위해, 지네릭 클래스인데도 예전의 방식으로 객체를 생성하는 것이 허용된다!!
- 다만, **지네릭 타입을 지정하지 않아서 안전하지 않다는 경고가 발생**

```java
Box b = new Box(); // ok, T는 Object로 간주
b.setItem("ABC"); // 경고, uncheck or unsafe operation
b.setItem(new Object()); // 경고, uncheck or unsafe operation
```

### 지네릭스의 용어

```java
class Box<T> {}
```

- **`Box<T>`** : **지네릭 클래스**. `T의 Box` 또는 `Box T` 라고 읽는다.
- **`T`** : **타입 변수** 또는 **타입 매개변수**. **(T는 타입 문자)**
- **`Box`** : 원시타입(raw type)

```java
Box<String> b = new Box<String>();
```

- **`지네릭 타입 호출`** : 타입 매개변수에 타입을 지정하는 것
- **`매개변수화된 타입(Parameterized type)`** : **대입된 타입** : 위의 예시에서 String와 같이 지정된 타입을 말한다.

**컴파일 후에** Box< String>, Box< Integer>는 이들의 **원시 타입(raw type)인 Box로 바뀐다! 즉, 지네릭 타입이 제거된다!!**

### 지네릭스의 제한

```java
Box<Apple> appleBox = new Box<Apple>(); // ok. Apple객체만 저장 가능
Box<Grape> grapeBox = new Box<Grape>(); // ok. Grape객체만 저장 가능
```

지네릭 클래스 Box의 객체를 생성할 때, 객체별로 다른 타입을 지정하는 것도 적절하다.

그러나 **모든 객체에 대해 동일하게 동작해야하는 static멤버**에 **타입 변수 T를 사용할 수 없다.**
- **T는 인스턴스 변수로 간주되기 때문이다.**
  - **static멤버는 인스턴스 변수를 참조할 수 없다.**

**지네릭 타입의 배열**을 **생성**하는 것도 **허용되지 않는다.**
- **지네릭 배열 타입의 참조변수를 선언**하는 갓은 **가능**하지만, 
- **'new T[10]'** 과 같이 **배열을 생성**하는 것은 **안된다!** 

**지네릭 배열을 생성할 수 없는 것**은 **new 연산자 때문**인데, new 연산자는 **컴파일 시점에 타입 T가 뭔지 정확히 알아야 한다.**
- 아래 예시의 **Box< T>클래스를 컴파일 하는 시점**에서는 **T가 어떤 타입이 될지 전혀 알 수 없다.**
- **instanceof연산자**도 new연산자와 같은 이유로 T를 피연산자로 사용할 수 없다.

**꼭 지네릭 배열을 생성해야할 필요가 있을 때**는,
- `Reflection API`의 `newInstance()` 와 같이 **동적으로 객체를 생성하는 메서드** 로 **배열을 생성**
- **Object배열을 생성해서 복사**한 다음에 **`T[]`로 형변환하는 방법** 등을 사용한다.

EX)

```java
class Box<T> {
    static T item; // error
    static int compare(T t1, T t2) {} // error
}

class Box<T> {
    T[] itemArr; // ok, T타입의 배열을 위한 참조변수
    
    T[] toArray() {
        T[] tmpArr = new T[itemArr.length]; // error, 지네릭 배열 생성불가
        ...
        return tmpArr;
    }
}
```

## 지네릭 클래스의 객체 생성과 사용

```java
class Box<T> {
    ArrayList<T> list = new ArrayList<T>();
    
    void add(T item) { list.add(item); }
    T get(int i)     { return list.get(i); }
    ArrayList<T> getList() { return list; }
    int size()       { return list.size(); }
    public String toString() { return list.toString(); }
}
```

**Box< T>의 객체에는 한가지 종류, 즉 T타입의 객체만 저장 가능**

**참조변수**와 **생성자**에 **대입된 타입(매개변수화된 타입)이 일치해야 한다!!**
- 일치하지 않으면 에러가 발생

```java
Box<Apple> appleBox = new Box<Apple>(); // ok
Box<Apple> appleBox = new Box<Grape>(); // error
```

**두 매개변수화된 타입**이 **상속 관계**에 있어도 마찬가지로 **에러가 발생**
- Apple이 Fruit의 자손이라고 가정

```java
Box<Fruit> appleBox = new Box<Apple>(); // error, 대입된 타입이 다르다.
```

**두 지네릭 클래스의 타입**이 **상속관계**에 있고, **대입된 타입이 같은 것은 괜찮다.**
- FruitBox는 Box의 자손이라고 가정

```java
Box<Apple> appleBox = new FruitBox<Apple>(); // ok, 다형성
```

**JDK 1.7부터는 추정이 가능한 경우 타입을 생략 가능**
- **참조변수의 타입**으로부터 Box가 Apple타입의 객체만 저장한다는 것을 알 수 있기 때문에, **생성자에 반복해서 타입을 지정해주지 않아도 되는 것**

```java
Box<Apple> appleBox = new Box<Apple>(); // ok
Box<Apple> appleBox = new Box<>(); // ok, jdk 1.7부터 생략가능
```

Box< T>의 객체에 `void add(T item)`으로 객체를 추가할 때
- 대입된 타입과 다른 타입의 객체는 추가할 수 없다.
- 타입 T가 Fruit인 경우, `void add(Fruit item)`이 되므로 Fruit의 자손들은 이 메서드의 매개변수가 될 수 있다.
  - Apple이 Fruit의 자손이라고 가정 

```java
Box<Apple> appleBox = new Box<Apple>(); // ok
Box<Apple> appleBox = new Box<>(); // ok, jdk 1.7부터 생략가능

Box<Fruit> fBox = new Box<Fruit>();
fBox.add(new Fruit()); // ok
fBox.add(new Apple()); // ok. void add(Fruit item)
```

[지네릭 클래스의 객체 생성과 사용 예시]

```java
import java.util.ArrayList;

class Fruit				  { public String toString() { return "Fruit";}}
class Apple extends Fruit { public String toString() { return "Apple";}}
class Grape extends Fruit { public String toString() { return "Grape";}}
class Toy		          { public String toString() { return "Toy"  ;}}

class FruitBoxEx1 {
	public static void main(String[] args) {
		Box<Fruit> fruitBox = new Box<Fruit>();
		Box<Apple> appleBox = new Box<Apple>();
		Box<Toy>   toyBox   = new Box<Toy>();
//		Box<Grape> grapeBox = new Box<Apple>(); // 에러. 타입 불일치

		fruitBox.add(new Fruit());
		fruitBox.add(new Apple()); // OK. void add(Fruit item)

		appleBox.add(new Apple());
		appleBox.add(new Apple());
//		appleBox.add(new Toy()); // 에러. Box<Apple>에는 Apple만 담을 수 있음

		toyBox.add(new Toy());
//		toyBox.add(new Apple()); // 에러. Box<Toy>에는 Apple을 담을 수 없음

		System.out.println(fruitBox);
		System.out.println(appleBox);
		System.out.println(toyBox);
	}  // main의 끝
}

class Box<T> {
	ArrayList<T> list = new ArrayList<T>();
	void add(T item)  { list.add(item); }
	T get(int i)      { return list.get(i); }
	int size() { return list.size(); }
	public String toString() { return list.toString();}
}
```

```json
실행결과
[Fruit, Apple]
[Apple, Apple]
[Toy]
```

## 제한된 지네릭 클래스

**타입 매개변수 T에 지정할 수 있는 타입의 종류를 제한하는 방법**

**지네릭 타입에** **`extends`** 를 사용하면, **특정 타입의 자손들만 대입**할 수 있게 **제한 가능하다!!**
- **< T extends Fruit>**

```java
// Fruit의 자손만 타입으로 지정가능
class FruitBox<T extends Fruit> {
    ArrayList<T> list = new ArrayList<T>();
}

FruitBox<Fruit> fruitBox = new FruitBox<Fruit>(); // ok
fruitBox.add(new Apple()); // ok, Apple이 fruit의 자손
fruitBox.add(new Grape()); // ok, Grape이 fruit의 자손
```

다형성에서 조상타입의 참조변수로 자손 타입의 객체를 가리킬 수 있는 것 처럼, **매개변수화된 타입의 자손 타입도 가능한 것**이다.
- 타입 매개변수 T에 Object를 대입하면, 모든 종류의 객체를 저장할 수 있게 된다.

만일 클래스가 아니라 **interface를 구현해야 한다는 제약이 필요**하다면, 이때도 **`extends`** **키워드를 사용**한다.
- `implements` 키워드를 **사용하지 않는다**는 점에 주의하자.

클래스 Fruit의 자손이면서 Eatable interface도 구현해야 한다면 **`&`** **기호로 연결한다.**

```java
interface Eatable {}
class MeatBox<T extends Eatable> { ... }

class FruitBox<T extends Fruit & Eatable> { ... }
```

[제한된 지네릭 클래스 사용 예시 - **extends, &**]

```java
import java.util.ArrayList;

class Fruit implements Eatable {
	public String toString() { return "Fruit";}
}
class Apple extends Fruit { public String toString() { return "Apple";}}
class Grape extends Fruit { public String toString() { return "Grape";}}
class Toy		          { public String toString() { return "Toy"  ;}}

interface Eatable {}

class FruitBoxEx2 {
	public static void main(String[] args) {
		FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
		FruitBox<Apple> appleBox = new FruitBox<Apple>();
		FruitBox<Grape> grapeBox = new FruitBox<Grape>();
//		FruitBox<Grape> grapeBox = new FruitBox<Apple>(); // 에러. 타입 불일치
//		FruitBox<Toy>   toyBox    = new FruitBox<Toy>();   // 에러.

		fruitBox.add(new Fruit());
		fruitBox.add(new Apple());
		fruitBox.add(new Grape());
		appleBox.add(new Apple());
//		appleBox.add(new Grape());  // 에러. Grape는 Apple의 자손이 아님
		grapeBox.add(new Grape());

		System.out.println("fruitBox-"+fruitBox);
		System.out.println("appleBox-"+appleBox);
		System.out.println("grapeBox-"+grapeBox);
	}  // main
}

class FruitBox<T extends Fruit & Eatable> extends Box<T> {}

class Box<T> {
	ArrayList<T> list = new ArrayList<T>();
	void add(T item)  { list.add(item);      }
	T get(int i)      { return list.get(i); }
	int size()        { return list.size();  }
	public String toString() { return list.toString();}
}
```

```json
실행결과
fruitBox-[Fruit, Apple, Grape]
appleBox-[Apple]
grapeBox-[Grape]
```

## 와일드 카드

### 와일드 카드를 등장 배경

```java
class Juicer { 
	static Juicer makeJuice(FruitBox<Fruit> box) {
		String tmp;
		for(Fruit f : box.getList()) tmp += f + " ";
		return new Juice(tmp);
	}
}

FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
FruitBox<Apple> appleBox = new FruitBox<Apple>();

System.out.println(Jucier.makeJuice(fruitBox)); // ok
System.out.println(Jucier.makeJuice(appleBox)); // error
```

Juicer클래스는 지네릭 클래스가 아닌데다, 지네릭 클래스라고 해도 **static메서드에는 타입 매개변수 T를 매개변수에 사용할 수 없으므로** 아예 **지네릭스를 적용하지 않던가**, 위의 예시와 같이 **특정한 타입을 지정**해줘야 한다.

지네릭 타입을 `FruitBox<Fruit>` 로 고정해 놓으면, `FruitBox<Apple>`타입의 객체는 `makeJuice()`의 매개변수가 될 수 없다.
- 그렇다면 여러 가지 타입의 매개변수를 갖는 makeJuice()를 만들 수 밖에 없다.

```java
// 오버로딩 실패!!
static Juicer makeJuice(FruitBox<Fruit> box) {
	String tmp;
	for(Fruit f : box.getList()) tmp += f + " ";
	return new Juice(tmp);
}
static Juicer makeJuice(FruitBox<Apple> box) {
	String tmp;
	for(Fruit f : box.getList()) tmp += f + " ";
	return new Juice(tmp);
}
```

그러나, **위와 같이 오버로딩**하면, **컴파일 에러가 발생**한다.

**지네릭 타입이 다른 것만으로는 오버로딩이 성립하지 않기 때문이다!!**
- **지네릭 타입은 컴파일러가 컴파일할 때만 사용하고 제거한다.**
  - 그래서 위의 두 메서드는 오버로딩이 아니라 '메서드 중복 정의'이다.

이럴 때 사용하기 위해 고안된 것이 바로 **`와일드 카드`** 이다!!

### 와일드 카드 설명

**`와일드 카드`** : 기호 `?` 로 표현, **어떠한 타입도 될 수 있다.**
- `?` 만으로는 Object타입과 다를게 없으므로, `extends` , `super` 로 (upper_bound) 와 (lower_bound)를 제한할 수 있다.

- **<? extends T>** : 와일드 카드의 **상한 제한. T와 그 자손들만 가능**
- **<? super T>** : 와일드 카드의 **하한 제한. T와 그 조상들만 가능**
- **<?>** : 제한 없음. 모든 타입이 가능. <? extends Object>와 동일
  - **지니렉 클래스와 달리 와일드 카드에는 '&' 기호를 사용할 수 없다. 즉, <? extends T & E>와 같이 사용할 수 없다.**

와일드 카드를 사용해서 makeJuice()의 매개변수 타입을 FruitBox< Fruit> -> FruitBox<? extends Fruit> 으로 바꾸면 된다.
- 그러면 이제 이 메서드의 매개변수로 FruitBox< Fruit>, FruitBox< Apple>, FruitBox< Grape> 모두 가능하게 된다. 

```java
static Juicer makeJuice(FruitBox<? extends Fruit> box) {
	String tmp;
	for(Fruit f : box.getList()) tmp += f + " ";
	return new Juice(tmp);
}
```

매개변수의 타입을 `FruitBox<? extends Object>`로 하면, **모든 종류**의 FruitBox가 이 메서드의 매개변수로 가능해진다.
- 대신, **전과 달리 box의 요소가 Fruit의 자손이라는 보장이 없으므로 아래의 For문에서 box에 저장된 요소를 Fruit타입의 참조변수로 못 받는다.**

```java
static Juicer makeJuice(FruitBox<? extends Object> box) {
	String tmp;
	for(Fruit f : box.getList()) tmp += f + " "; // error, Fruit이 아닐 수 있음
	return new Juice(tmp);
}

class FruitBox<T extends Fruit> extends Box<T> {}
```

그러나, **실제로 테스트 해보면 문제없이 컴파일** 되는데 **그 이유는 바로 지네릭 클래스 FruitBox를 제한**했기 때문이다. 
- **컴파일러**는 **모든 FruitBox의 요소들이 Fruit의 자손이라는 것을 알고 있으므로 문제 삼지 않는 것이다.**

[와일드 카드 사용 예시]

```java
import java.util.ArrayList;

class Fruit		          { public String toString() { return "Fruit";}}
class Apple extends Fruit { public String toString() { return "Apple";}}
class Grape extends Fruit { public String toString() { return "Grape";}}

class Juice {
	String name;

	Juice(String name)	     { this.name = name + "Juice"; }
	public String toString() { return name;		 }
}

class Juicer {
	static Juice makeJuice(FruitBox<? extends Fruit> box) {
		String tmp = "";
		
		for(Fruit f : box.getList()) 
			tmp += f + " ";
		return new Juice(tmp);
	}
}

class FruitBoxEx3 {
	public static void main(String[] args) {
		FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
		FruitBox<Apple> appleBox = new FruitBox<Apple>();

		fruitBox.add(new Apple());
		fruitBox.add(new Grape());
		appleBox.add(new Apple());
		appleBox.add(new Apple());

		System.out.println(Juicer.makeJuice(fruitBox));
		System.out.println(Juicer.makeJuice(appleBox));
	}  // main
}

class FruitBox<T extends Fruit> extends Box<T> {}

class Box<T> {
//class FruitBox<T extends Fruit> {
	ArrayList<T> list = new ArrayList<T>();
	void add(T item) { list.add(item);      }
	T get(int i)     { return list.get(i); }
	ArrayList<T> getList() { return list;  }
	int size()       { return list.size(); }
	public String toString() { return list.toString();}
}
```

```json
실행결과
Apple Grape Juice
Apple Grape Juice
```

### super로 와일드카드의 하한을 제한하는 경우

Collections.sort() 의 메서드 선언부이다.

```java
static <T> void sort(List<T> list, Comparator<? super T> c)
```

`static` 옆에 선언된 `<T>` **메서드에 선언된 지네릭 타입**이다.
- 이러한 메서드를 **지네릭 메서드**라고 한다.

첫 번째 매개변수는 정렬할 대상, 두 번째 매개변수는 정렬할 방법이 정의된 Comparator이다.
- **Comparator의 지네릭 타입**에 **하한 제한이 걸려있는 와일드 카드**가 사용되었다.

위의 문장에서 타입 매개변수 T에 Apple이 대입되면,

```java
static void sort(List<Apple> list, Comparator<? super Apple> c)
```

매개변수의 타입이 **Comparator<? super Apple>** 라는 의미는 **Comparator의 타입 매개변수로 Apple과 그 조상이 가능하다**는 의미
- Comparator<? super Apple>
  - Comparator< Apple>, Comparator< Fruit>, Comparator< Object>
- Comparator<? super Grape>
  - Comparator< Grape>, Comparator< Fruit>, Comparator< Object>

```java
class FruitComp implements Comparator<Fruit> {
	public int compare(Fruit t1, Fruit t2) {
		return t1.weight - t2.weight;
	}
}

Collections.sort(appleBox.getList(), new FruitComp());
Collections.sort(grapeBox.getList(), new FruitComp());
```

FruitComp하나로 모든 Fruit의 Box정렬 가능

[<? super T> : 와일드 카드의 하한을 제한하는 Collections.sort() 예시]

```java
import java.util.*;

class Fruit	{
	String name;
	int weight;
	
	Fruit(String name, int weight) {
		this.name   = name;
		this.weight = weight;
	}

	public String toString() { return name+"("+weight+")";}
	
}

class Apple extends Fruit {
	Apple(String name, int weight) {
		super(name, weight);
	}
}

class Grape extends Fruit {
	Grape(String name, int weight) {
		super(name, weight);
	}
}

class AppleComp implements Comparator<Apple> {
	public int compare(Apple t1, Apple t2) {
		return t2.weight - t1.weight;
	}
}

class GrapeComp implements Comparator<Grape> {
	public int compare(Grape t1, Grape t2) {
		return t2.weight - t1.weight;
	}
}

class FruitComp implements Comparator<Fruit> {
	public int compare(Fruit t1, Fruit t2) {
		return t1.weight - t2.weight;
	}
}

class FruitBoxEx4 {
	public static void main(String[] args) {
		FruitBox<Apple> appleBox = new FruitBox<Apple>();
		FruitBox<Grape> grapeBox = new FruitBox<Grape>();

		appleBox.add(new Apple("GreenApple", 300));
		appleBox.add(new Apple("GreenApple", 100));
		appleBox.add(new Apple("GreenApple", 200));

		grapeBox.add(new Grape("GreenGrape", 400));
		grapeBox.add(new Grape("GreenGrape", 300));
		grapeBox.add(new Grape("GreenGrape", 200));

		Collections.sort(appleBox.getList(), new AppleComp());
		Collections.sort(grapeBox.getList(), new GrapeComp());
		System.out.println(appleBox);
		System.out.println(grapeBox);
		System.out.println();
		Collections.sort(appleBox.getList(), new FruitComp());
		Collections.sort(grapeBox.getList(), new FruitComp());
		System.out.println(appleBox);
		System.out.println(grapeBox);
	}  // main
}

class FruitBox<T extends Fruit> extends Box<T> {}

class Box<T> {
	ArrayList<T> list = new ArrayList<T>();

	void add(T item) {
		list.add(item);
	}

	T get(int i) {
		return list.get(i);
	}

	ArrayList<T> getList() { return list; }

	int size() {
		return list.size();
	}

	public String toString() {
		return list.toString();
	}
}
```

## 지네릭 메서드

**`지네릭 메서드`** : **메서드의 선언부에 지네릭 타입이 선언된 메서드**

- 같은 타입 문자 T를 사용해도 같은 것이 아니라는 것에 주의해야 한다.
- 지네릭 메서드는 지네릭 클래스가 아닌 클래스에도 정의할 수 있다.

```java
class FruitBox<T> {
	static <T> void sort(List<T> list, Compartor<? super T> c)
}
```

**지네릭 클래스 FruitBox에 선언된 타입 매개변수 T**와 **지네릭 메서드 sort()** 에 선언된 **타입 매개변수 T**는 타입 문자만 같을 뿐 **서로 다른 것**이다.

**static멤버에는 타입 매개 변수를 사용할 수 없지만** , **메서드에 지네릭 타입을 선언**하고 **static메서드를 사용하는 것은 가능**하다.
- **메서드에 선언된 지네릭 타입**은 **지역 변수를 선언한 것과 같다고 생각**하면 이해하기 쉽다.
  - **이 타입 매개변수**는 **메서드 내에서만 지역적으로 사용될 것**이므로 **메서드가 static이건 아니건 상관이 없다.**
    - 같은 이유로 내부 클래스에 선언된 타입 문자가 외부 클래스의 타입 문자와 같아도 구별될 수 있다.

앞에 나왔던 makeJuice()를 **지네릭 메서드**로 바꾸면 다음과 같다.

```java
static Juicer makeJuice(FruitBox<? extends Fruit> box) {
    String tmp;
    for(Fruit f : box.getList()) tmp += f + " "; // error, Fruit이 아닐 수 있음
    return new Juice(tmp);
}

-->

static <T extends Fruit> Juicer makeJuice(FruitBox<T> box) {
    String tmp;
    for(Fruit f : box.getList()) tmp += f + " "; // error, Fruit이 아닐 수 있음
    return new Juice(tmp);
}
```

이 메서드를 호출할 때는 아래와 같이 타입 변수에 타입을 대입해야 한다.

```java
FruitBox<Fruit> fruitBox = new FruitBox<Fruit>;
FruitBox<Apple> appleBox = new FruitBox<Apple>;

Juicer.<Fruit>makeJuice(fruitBox);
Juicer.<Apple>makeJuice(appleBox);

// 그러나 대부분의 경우 컴파일러가 타입을 추정할 수 있기 때문에 생략 가능
Juicer.makeJuice(fruitBox);
Juicer.makeJuice(appleBox);
```

- 주의할 점 : 지네릭 메서드를 호출할 때, **대입된 타입을 생략할 수 없는 경우**에는 **참조변수나 클래스 이름을 생략할 수 없다.**

```java
<Fruit>makeJuice(fruitBox);         // error, 클래스 이름 생략 불가
this.<Fruit>makeJuice(fruitBox);    // ok
Juicer.<Fruit>makeJuice(fruitBox);  // ok
```

- 즉, **같은 클래스 내에 있는 멤버들끼리**는 **참조 변수나 클래스이름, 즉 this나 클래스이름을 생략하고 메서드 이름만으로 호출이 가능**하지만, 
- **대입된 타입이 있을 때는 반드시 써줘야 한다.**

**지네릭 메서드**는 **매개변수의 타입이 복잘할 때도 유용**하다.

```java
public static void printAll(ArrayList<? extends Product> list, ArrayList<? extends Product> list2>) {
    for(Unit u: list) {
        System.out.println(u);
    }
}

-->

public static <T extends Product> void printAll(ArrayList<T> list, ArrayList<T> list2>) {
    for(Unit u: list) {
        System.out.println(u);
    }
}
```

[복잡한 지네릭 메서드 예시]

```java
public static <T extends Comparable<? super T>> void sort(List<T> list)
```

1. 일단 와일드 카드를 걷어내자 -> public static < T extends Comparable< T>> void sort(List< T> list)
- List< T>의 요소가 Comparable 인터페이스를 구현한 것이어야 한다는 뜻

다시 와일드 카드를 넣고 이해해보자.

```java
public static <T extends Comparable<? super T>> void sort(List<T> list)
```

- 타입 T를 요소로 하는 List를 매개변수로 허용한다.
- T는 Comapable을 구현한 클래스이어야 하며 -> (< T extends Comparble>), 
- T 또는 그 조상의 타입을 비교하는 Comparable이어야 한다는 것  -> (Compable< ? super T>)
    - 예를 들어, T가 Student이고, Person의 자손이라면, <? super T>는 Student, Person, Object가 모두 가능하다.

## 지네릭 타입의 형변환

지네릭 타입과 원시 타입(raw-type)간의 형변환이 가능할까? 가능하다.

```java
Box box = null;
Box<Object> objBox = null;

box = (Box)objBox;         // ok, 지네릭 타입 -> 원시타입. 경고 발생
objBox = (Box<Object>)box; // ok, 원시 타입 -> 지네릭타입. 경고 발생
```

- 지네릭(Generics) 타입과 넌지네릭(non-generic) 타입간의 형변환은 항상 가능하다
  - 단지,경고가 발생

대입된 타입이 다른 지네릭 타입 간에는 형변환이 가능할까? 불가능하다.

```java
Box<Object> objBox = null;
Box<String> strBox = null;

objBox = (Box<Object>)strBox; // error. Box<String> -> Box<Object>
strBox = (Box<String>)objBox; // error. Box<Object> -> Box<String>
```

- 불가능하다. 대입된 타입이 Object라도 이미 배운 부분 아래의 문장이 안된다는 얘기는 형변환이 안된다는 사실

```java
// Box<Object> objBox = (Box<Object>)new Box<String>();
Box<Object> objBox = new Box<String>(); // error, 형변환 불가능
```

Box< String>이 <? extends Object>로 형변환 가능할까? 가능하다.

```java
Box<? extends Object> wBox = new Box<String>();

Box<String> strBox = new Box<? extends Object>();
```

- **형변환 가능**하다. 
  - makeJuice()메서드에 매개변수에 다형성이 적용될 수 있었던 것
- **반대로의 형변환도 가능하지만, 확인되지 않은 형변환이라는 경고가 발생**한다.
  - <? extends Object>에 대입될 수 있는 타입이 여러 개인데다, Box< String>를 제외한 다른 타입은 Box< String>로 형변환될 수 없기 때문이다.


[실제 java.util.Optional 클래스의 실제 소스]

```java
public final class Optional<T> {
    private static final Optional<?> EMPTY = new Optional<>();
    private final T value;
    		
    public static<T> Optional<T> empty() {
    	Optional<T> t = (Optional<T>) EMPTY;
    	return t;
    }
}
```

static 상수 EMPTY에 비어있는 Optional 객체를 생성해서 저장했다. empty()를 호출하면 EMPTY를 형변환해서 반환한다.

```java
Optional<?> EMPTY = new Optional<>();
-> Optional<? extends Object> EMPTY = new Optional<>();
-> Optional<? extends Object> EMPTY = new Optional<Object>();

Optional<?> EMPTY = new Optional<?>(); // Error. 미확인 타입의 객체는 상성불가
Optional<?> EMPTY = new Optional<Object>(); // OK
Optional<?> EMPTY = new Optional<>(); // OK
```

[주의] class Box< T extends **Fruit**> 의 경우 : Box<?> b = new Box<>; == Box<?> b = new Box< **Fruit**>;이다. 

- 위의 문장에서 **EMPTY의 타입**을 `Optional< Object>` 가 아닌 `Optional<?>` 로한 이유는 **Optional< T>로 형변환이 가능**하기 때문이다.

```java
Optional<?> wopt = new Optional<Object>();
Optional<Object> oopt = new Optional<Object>();

Optional<String> sopt = (Optional<String>)wopt;  // ok. 형변환 가능
Optional<String> sopt = (Optional<String>)oopt;  // error. 형변환 불가
```

- Optional< Object>를 Optional< String>으로 **직접 형변환하는 것은 불가능**하지만, 

**와일드 카드가 포함된 지네릭 타입**으로 **형변환하면 가능**하다. **대신 확인되지 않은 타입으로의 형변환이라는 경고가 발생**한다.

```java
public static<T> Optional<T> empty() {
    Optional<T> t = (Optional<T>) EMPTY; // Optional<?> - > Optional<T>
    return t;
}

Optional<Object> -> Optional<T> // 형변환 불가
Optional<Object> -> Optional<?> -> Optional<T> // 형변환 가능. 경고발생
```

다음과 같이 **와일드 카드가 사용된 지네릭 타입끼리**도 다음과 같은 경우에는 형변환 가능
- 가능하긴 하지만, **와일드 카드는 타입이 확정된 타입이 아니므로**
- **컴파일러는 미확정 타입으로 형변환하는 것이라고 경고**한다.

```java
FruitBox<? extends Object> objBox = null;
FruitBox<? extends String> strBox = null;

strBox = (FruitBox<? extends String>) objBox // ok. 미확정 타입으로 형변환 경고
objBox = (FruitBox<? extends Object>) strBox // ok. 미확정 타입으로 형변환 경고
```

## 지네릭 타입의 제거

**`컴파일러`**는 **지네릭 타입**을 이용해서 **소스파일(.java)을 체크**하고, **필요한 곳에 형변환을 넣어준다.** 그리고 **지네릭 타입을 제거**한다.
- 즉, **컴파일된 파일(*.class)** 에는 **지네릭 타입에 대한 정보가 없는 것**이다.
- 이렇게 하는 주된 이유는 **지네릭이 도입되기 이전의 소스 코드와의 호환성을 유지**하기 위해서이다.
- 앞으로 가능하면 원시 타입을 사용하지 않도록 하자.

### 1.지네릭 타입의 경계(bound)를 제거한다.

- 지네릭 타입이 `< T extends Fruit>` 라면 **T는 Fruit으로 치환된다.**
- `< T>` 인 경우에는 **T는 Object로 치환**된다.
- 그리고 **클래스 옆의 선언은 제거**된다.

```java
class Box<T extends Fruit> {
    void add(T t) {}
}

-->

class Box {
    void add(Fruit t) {}
}
```

### 2. 지네릭 타입을 제거한 후에 타입이 일치하지 않으면, 형변환을 추가한다.
- List의 get()은 Object타입을 반환하므로 형변환이 필요하다.

```java
T get(int i) {
    return list.get(i);
}

-->

Fruit get(int i) {
    return (Fruit)list.get(i);
}
```

- **와일드 카드가 포함되어 있는 경우**에는 다음과 같이 **적절한 타입으로의 형변환이 추가된다.**

```java
static Juice makeJuice(FruitBox<? extends Fruit> box) {
    String tmp;
    for(Fruit f : box.getList()) tmp += f + " ";
    return new Juice(tmp);
}

-->

static Juice makeJuice(FruitBox box) {
    String tmp;
    Iterator it = box.getList().iteratort();
    while(it.hasnext()) {
        tmp += (Fruit)it.next() + " ";
    }
    return new Juice(tmp);
}
```

## 열거형(enums)

## 열거형(enums)이란?

**`열거형(enums)`** : **서로 관련된 상수(constant)** 를 **편리하게 선언하기 위한 것**으로 **여러 상수를 정의**할 때 사용하면 유용하다.
- **JDK 1.5부터 추가**되었다.
- `Java의 열거형`은 **열거형이 갖는 값뿐만 아니라 타입도 관리**하기 때문에 **보다 논리적인 오류를 줄일 수 있다.**

```java
class Card {
    static final int CLOVER = 0;
    static final int HEART = 1;
    static final int DIAMOND = 2;
    static final int SPADE = 3;

    static final int TWO = 0;
    static final int THREE = 1;
    static final int FOUR = 2;
    		
    final int kind;
    final int num;
}

-->

class Card {
    enum Kind { CLOVER, HEART, DIAMOND, SPADE } // 열거형 Kind 정의
    enum Value { TWO, THREE, FOUR }
    		
    final Kind kind;
    final Value value;
}
```

`Java의 열거형`은 **타입에 안전한 열거형(typesafe enum)** 
- **실제 값이 같아도 타입이 다르면 컴파일 에러가 발생**한다.

```java
if(Card.CLOVER == Card.TWO)            // true지만 false이어야 의미상 맞음
if(Card.Kind.CLOVER == Card.Value.TWO) // 컴파일 에러, 값은 같지만 타입이 다름
```

**상수의 값이 바뀌면**, 해당 상수를 참조하는 모든 소스를 다시 컴파일 해야 하지만 **열거형 상수를 사용하면 기존의 소스를 다시 컴파일하지 않아도 된다.**

## 열거형의 정의와 사용

**열거형 정의하는 방법** : **`괄호{}`** **안에 상수의 이름을 나열한다.**

```java
enum 열거형이름 { 상수명1, 상수명2, ... }

enum Direction { EAST, SOUTH, WEST, NORTH }
```

**열거형에 정의된 상수를 사용하는 방법** : **`열거형이름.상수명`**
- 클래스의 static 변수 참조하는 것과 동일

```java
class Unit {
    int x, y;
    Direction dir; // 열거형을 인스턴스 변수로 선언
    		
    void init() {
        dir = Direction.EAST;
    }
}
```

**열거형 상수간의 비교**에는 **`==`** 를 **사용할 수 있다.**
- equals가 아닌 **== 로 비교가 가능하다** 는 것은 **그만큼 빠른 성능을 제공**한다는 뜻
- 그러나 >, < 와 같은 비교연산자는 사용할 수 없고, **compareTo()는 사용 가능하다.**

```java
if(dir == Direction.EAST) {
...
} 
else if(dir > Direction.WEST) { // error
...
} 
else if(dir.compareTo(Direction.WEST) > 0 ) { // ok
...
}
```

**switch문의 조건식**에도 **열거형 사용 가능**
- **주의할 점** : **case문에** 열거형의 이름은 적지 않고 **상수의 이름만 적어야 한다.**

```java
void move() {
    switch(dir) {
    	case EAST: x++; // Direction.EAST라고 쓰면 안된다.
            break;
    	case WEST: x--;
            break;
    	case NORTH: y++;
            break;
    	case SOUTH: y--;
            break;
    }
}
```

### 모든 열거형의 조상 - java.lang.Enum

열거형 Direction에 정의된 **모든 상수를 출력**하려면, 다음과 같이 한다.

```java
Direction[] dArr = Direction.values();
for(Direction d : dArr) {
    System.out.println("%s = %d", d.name(), d.ordinal());
}
```

**`values()`** : **열거형의 모든 상수를 배열에 담아 반환**한다.
- 이 메서드는 **모든 열거형이 가지고 있는 것**으로 **컴파일러가 자동으로 추가**해준다.

**`ordinal()`** 은 **모든 열거형의 조상**인 **java.lang.Enum 클래스**에 정의된 것으로, **열거형 상수가 정의된 순서(0부터) 정수를 반환**한다.

[Enum 클래스의 메서드]

![Enum클래스의 메서드](https://user-images.githubusercontent.com/56071088/128177075-23015a98-787a-498d-879d-213a8724c04b.png)

**`static E valueOf(String name)`** : 컴파일러가 자동으로 추가해주는 메서드(values()처럼)

```java
static E values()
static E valueOf(String name)
```

이 메서드는 열거형 상수의 이름으로 문자열 상수에 대한 참조를 얻을 수 있게 해준다.

```java
Direction d = Direction.valueOf("WEST");
System.out.println(d); // WEST
System.out.println(Direction.WEST == Direction.valueOf("WEST")) // true
```

[열거형 enums 사용 예제]

```java
enum Direction { EAST, SOUTH, WEST, NORTH }

class EnumEx1 {
	public static void main(String[] args) {
		Direction d1 = Direction.EAST;
		Direction d2 = Direction.valueOf("WEST");
		Direction d3 = Enum.valueOf(Direction.class, "EAST");

		System.out.println("d1="+d1);
		System.out.println("d2="+d2);
		System.out.println("d3="+d3);

		System.out.println("d1==d2 ? "+ (d1==d2));
		System.out.println("d1==d3 ? "+ (d1==d3));
		System.out.println("d1.equals(d3) ? "+ d1.equals(d3));
//		System.out.println("d2 > d3 ? "+ (d1 > d3)); // ¿¡·¯
		System.out.println("d1.compareTo(d3) ? "+ (d1.compareTo(d3)));
		System.out.println("d1.compareTo(d2) ? "+ (d1.compareTo(d2)));

		switch(d1) {
			case EAST: // Direction.EAST¶ó°í ¾µ ¼ö ¾ø´Ù.
				System.out.println("The direction is EAST."); 
				break;
			case SOUTH:
				System.out.println("The direction is SOUTH."); 
				break;
			case WEST:
				System.out.println("The direction is WEST."); 
				break;
			case NORTH:
				System.out.println("The direction is NORTH."); 
				break;
			default:
				System.out.println("Invalid direction."); 
//				break;
		}

		Direction[] dArr = Direction.values();

		for(Direction d : dArr)  // for(Direction d : Direction.values()) 
			System.out.printf("%s=%d%n", d.name(), d.ordinal()); 
	}
}
```

```json
실행결과
d1=EAST
d2=WEST
d3=EAST
d1==d2 ? false
d1==d3 ? true
d1.equals(d3) ? true
d1.compareTo(d3) ? 0
d1.compareTo(d2) ? -2
The direction is EAST.
EAST=0
SOUTH=1
WEST=2
NORTH=3
```

## 열거형에 멤버 추가하기

Enum 클래스에 정의된 ordinal()이 열거형 상수가 정의된 순서를 반환하지만, **이 값을 열거형 상수의 값으로 사용하지 않는 것이 좋다.**
- 이 값은 내부적인 용도로만 사용되기 위한 것이기 때문이다.
- 열거형 상수의 값이 불연속적인 경우에는 **열거형 상수의 이름 옆에 원하는 값을 괄호와 함께 적어주면 된다.**

```java
enum Direction { EAST(1), SOUTH(5), WEST(-1), NORTH(10) }
```

그리고 지정된 **값을 저장할 수 있는 인스턴스 변수**와 **생성자**를 **새로 추가**해 주어야 한다.
- 이 때, **먼저 열거형 상수를 모두 정의한 다음에** 다른 멤버들을 추가해야 한다는 것이다.
- 열거형 상수의 마지막에 **`;`** 도 추가

```java
enum Direction { 
    EAST(1), SOUTH(5), WEST(-1), NORTH(10); // 끝에 ; 추가해야함
    
    private final int value // 정수를 저장할 필드 추가
    
    Direction(int value) {  // 생성자 추가, 기본 private
    	this.value = value;
    }

    public int getValue() { return value; }
} 
```

- **열거형의 인스턴스 변수는 반드시 final이어야 하는 제약은 없지만**, value는 열거형 상수의 값을 저장하기 위한 것이므로 final을 붙였다.
- 열거형 Direction에 새로운 생성자가 추가되었지만, 아래와 같이 **열거형의 객체를 생성할 수 없다.**
  - **열거형의 생성자**는 **묵시적으로 private**이기 때문이다.

```java
Direction d = new Direction(1); // error, 열거형의 생성자는 외부에서 호출 불가
```

필요하다면, **하나의 열거형 상수에 여러 값을 지정**할 수도 있다.
- **그에 맞게 인스턴스 변수와 생성자 등을 새로 추가**해야한다.

```java
enum Direction { 
    EAST(1, ">"), SOUTH(5, "V"), WEST(-1, "<"), NORTH(10, "^");
    
    private final int value;
    private final String symbol;
    		
    Direction(int value, String symbol) {
    	this.value = value;
    	this.symbol = symbol;
    }

    public int getValue() { return value; }
    public int getSysmbol() { return symbol; }
} 
```

[열거형에 멤버 추가한 예제]

```java
    enum Direction { 
	EAST(1, ">"), SOUTH(2,"V"), WEST(3, "<"), NORTH(4,"^");

	private static final Direction[] DIR_ARR = Direction.values();
	private final int value;
	private final String symbol;

	Direction(int value, String symbol) { // private Direction(int value)
		this.value  = value;
		this.symbol = symbol;
	}

	public int getValue()      { return value;  }
	public String getSymbol()  { return symbol; }

	public static Direction of(int dir) {
        if (dir < 1 || dir > 4) {
            throw new IllegalArgumentException("Invalid value :" + dir);
        }
        return DIR_ARR[dir - 1];		
	}	

	// 방향을 회전시키는 메서드. num의 값만큼 90도씩 시계방향으로 회전한다.
	public Direction rotate(int num) {
		num = num % 4;

		if(num < 0) num +=4; // num이 음수일 때는 시계반대 방향으로 회전 

		return DIR_ARR[(value-1+num) % 4];
	}

	public String toString() {
		return name()+getSymbol();
	}
} // enum Direction

class EnumEx2 {
	public static void main(String[] args) {
		for(Direction d : Direction.values()) 
			System.out.printf("%s=%d%n", d.name(), d.getValue()); 

		Direction d1 = Direction.EAST;
		Direction d2 = Direction.of(1);

		System.out.printf("d1=%s, %d%n", d1.name(), d1.getValue());
		System.out.printf("d2=%s, %d%n", d2.name(), d2.getValue());

		System.out.println(Direction.EAST.rotate(1));
		System.out.println(Direction.EAST.rotate(2));
		System.out.println(Direction.EAST.rotate(-1));
		System.out.println(Direction.EAST.rotate(-2));
	}
}
```

```json
실행결과
EAST=1
SOUTH=2
WEST=3
NORTH=4
d1=EAST, 1
d2=EAST, 1
SOUTH V
WEST <
NORTH ^
WEST <
```


## 열거형에 추상 메서드 추가하기

열거형 Transportation은 운송 수단의 종류 별로 상수를 정의, 각 운송 수단에는 기본요금이 책정되어 있다.

```java
enum Transportation {
    BUS(100), TRAIN(150), SHIP(100), AIRPLANE(300);
    		
    private final int BASIC_FARE;

    private Transportation(int basicFare) {
    	BASIC_FARE = basicFare
    }
    		
    int fare() { // 운송 요금을 반환
    	return BASIC_FARE;
    }
}
```

그러나 거리에 따라 요금을 계산하는 방식이 각 운송 수단마다 다르기 때문에 부족하다.
- 이럴 때, **열거형**에 **추상 메서드** fare(int distance)를 선언하면 **각 열거형 상수가 이 추상 메서드를 반드시 구현**해야 한다.

```java
enum Transportation {
    BUS(100) {
    	int fare(int distance) { return distance * BASIC_FARE; }
    },
    TRAIN(150) {int fare(int distance) { return distance * BASIC_FARE; }},
    SHIP(100)  {int fare(int distance) { return distance * BASIC_FARE; }},
    AIRPLANE(300) {int fare(int distance) { return distance * BASIC_FARE; }};

    abstract int fare(int distance); // 거리에 따라 요금을 계산하는 추상 메서드
    	
    protected final int BASIC_FARE;  // protected로 해야 각 상수에서 접근 가능

    private Transportation(int basicFare) {
    	BASIC_FARE = basicFare
    }
    	
    public int getBasicFare() {
    	return BASIC_FARE;
    }
}
```

위의 코드는 **열거형에 정의된 추상 메서드**를 **각 상수가 어떻게 구현**하는지 보여준다.
- 마치 익명 클래스를 작성한 것처럼 보일 정도로 유사하다.
- 이때 **열거형의 인스턴스 변수 BASIC_FARE의 접근 제어자** private를 **protected로 변경해야 각 상수에서 접근 가능하다.** 

[열거형에 추상 메서드 추가 예제]

```java
enum Transportation { 
	BUS(100)      { int fare(int distance) { return distance*BASIC_FARE;}},
	TRAIN(150)    { int fare(int distance) { return distance*BASIC_FARE;}},
	SHIP(100)     { int fare(int distance) { return distance*BASIC_FARE;}},
	AIRPLANE(300) { int fare(int distance) { return distance*BASIC_FARE;}};

	protected final int BASIC_FARE; // protected로 해야 각 상수에서 접근가능
	
	Transportation(int basicFare) { // private Transportation(int basicFare) {
		BASIC_FARE = basicFare;
	}

	public int getBasicFare() { return BASIC_FARE; }

	abstract int fare(int distance); // 거리에 따른 요금 계산
}

class EnumEx3 {
	public static void main(String[] args) {
		System.out.println("bus fare="     +Transportation.BUS.fare(100));
		System.out.println("train fare="   +Transportation.TRAIN.fare(100));
		System.out.println("ship fare="    +Transportation.SHIP.fare(100));
	    System.out.println("airplane fare="+Transportation.AIRPLANE.fare(100));
	}
}
```

```json
실행결과
bus fare=10000
train fare=15000
ship fare=10000
airplane fare=30000
```

**각 열거형 상수가 추상 메서드** fare()를 똑같은 내용으로 구현했지만, **다르게 구현될 수도 있게 하려고 추상 메서드로 선언한 것**이다.
- 사실 열거형에 추상 메서드를 선언할 일은 그리 많지 않다.

## 열거형의 이해

**열거형이 내부적으로 어떻게 구현**되었는지에 대해 알아보자.

```java
enum Direction { EAST, SOUTH, WEST, NORTH }

--> 

class Direction {
    static final Direction East = new Direction("EAST");
    static final Direction SOUTH = new Direction("SOUTH");
    static final Direction NORTH = new Direction("NORTH");
    static final Direction WEST = new Direction("WEST");

    private String name;
    
    private Direction(String name) {
    	this.name = name;
    }
}
```

사실은 **열거형 상수 하나하나가 Direction 객체**이다.
- **Direction 클래스의 static 상수 EAST, SOUTH, WEST, NORTH의 값**은 **`객체의 주소`** 이고, 이 값은 **바뀌지 않는 값**이므로 **`==`** 로 **비교가 가능한 것**이다.

**모든 열거형**은 **추상 클래스 Enum의 자손**이므로 Enum을 흉내내어 MyEnum을 작성하면 다음과 같다.

```java
abstract class MyEnum<T extends MyEnum<T>> implements Comparable<T> {
    static int id = 0; // 객체에 붙일 일련번호
    int ordinal;
    String name = "";
    		
    public int ordinal() { return ordinal; }
    
    MyEnum(String name) {
    	this.name = name;
    	ordinal = id++;   // 객체를 생성할 때마다 id의 값을 증가시킨다.
    }

    public int compareTo(T t) {
    	return ordinal - t.ordinal();
    }
}
```

- **객체가 생성될 때마다** 번호를 붙여서 **인스턴스 변수 ordinal에 저장**한다.
- **Comparable 인터페이스를 구현**해서 **열거형 상수간의 비교가 가능**하도록 되어 있다.

만약 클래스를 abstract class MyEnum< T> implements Comparable< T> 처럼 선언했다면, compareTo()를 위와 같이 간단히 작성할 수 없다.
- **타입 T에 ordinal()이 정의되어 있는지 확인할 수 없기 때문**이다.
  - t.ordinal() // Error. 타입 T에 ordinal()이 있나?

```java
abstract class MyEnum<T> implements Comparable<T> {
    ...
    public int compareTo(T t) {
    	return ordinal - t.ordinal(); // Error. 타입 T에 ordinal()이 있나?
    }
```

따라서 MyEnum< T extends MyEnum< T>>와 같이 선언한 것이다. 이것은 **타입 T가 MyEnum< T>의 자손이어야 한다는 의미**이다.
- 타입 T가 MyEnum의 자손이므로 ordinal()이 정의되어 있는 것은 분명하므로 형변환 없이도 에러가 나지 않는다.

그리고 **추상 메서드를 새로 추가**하면 **클래스 앞**에도 **`abstract`** 를 붙여줘야 하고, **각 static 상수들도 추상 메서드를 구현**해주어야 한다.
- 이것이 **추상 메서드를 추가하면, 각 열거형 상수가 추상 메서드를 구현해야하는 이유**이다.
    
[익명 클래스의 형태로 추상 메서드를 구현]

```java
abstract class Direction extends MyEnum {
    static final Direction East = new Direction("EAST") { // 익명 클래스
        Point move(Point p) { ... }
    };
    static final Direction SOUTH = new Direction("SOUTH") { // 익명 클래스
        Point mPoint p) { ... }
    };
    static final Direction NORTH = new Direction("NORTH") { // 익명 클래스
        Point mPoint p) { ... }
    };
    static final Direction WEST = new Direction("WEST") { // 익명 클래스
        Point move(Point p) { ... }
    };

    private String name;
    
    private Direction(String name) {
        this.name = name;
    }
    
    abstract Point move(Point p);
}
```

[Enum 추상 클래스 구현 예제]

```java
abstract class MyEnum<T extends MyEnum<T>> implements Comparable<T> {
	static int id = 0;
		
	int ordinal;
	String name = "";

	public int ordinal() { return ordinal; }
	
	MyEnum(String name) {
		this.name = name;
		ordinal = id++;	
	}

	public int compareTo(T t) {
		return ordinal - t.ordinal();
	}
}

abstract class MyTransportation extends MyEnum {
	static final MyTransportation BUS   = new MyTransportation("BUS", 100) {
		int fare(int distance) { return distance * BASIC_FARE; }
	};
	static final MyTransportation TRAIN = new MyTransportation("TRAIN", 150) {
		int fare(int distance) { return distance * BASIC_FARE; }
	};
	static final MyTransportation SHIP  = new MyTransportation("SHIP", 100) {
		int fare(int distance) { return distance * BASIC_FARE; }
	};

    static final MyTransportation AIRPLANE = new MyTransportation("AIRPLANE", 300) {
		int fare(int distance) { return distance * BASIC_FARE; }
	};

	abstract int fare(int distance); // Ãß»ó ¸Þ¼­µå

	protected final int BASIC_FARE;

	private MyTransportation(String name, int basicFare) {
		super(name);
		BASIC_FARE = basicFare;
	}

	public String name()     { return name; }
	public String toString() { return name; }
}

class EnumEx4 {
	public static void main(String[] args) {
		MyTransportation t1 = MyTransportation.BUS;
		MyTransportation t2 = MyTransportation.BUS;
		MyTransportation t3 = MyTransportation.TRAIN;
		MyTransportation t4 = MyTransportation.SHIP;
		MyTransportation t5 = MyTransportation.AIRPLANE;
			
		System.out.printf("t1=%s, %d%n", t1.name(), t1.ordinal());
		System.out.printf("t2=%s, %d%n", t2.name(), t2.ordinal());
		System.out.printf("t3=%s, %d%n", t3.name(), t3.ordinal());
		System.out.printf("t4=%s, %d%n", t4.name(), t4.ordinal());
		System.out.printf("t5=%s, %d%n", t5.name(), t5.ordinal());
		System.out.println("t1==t2 ? "+(t1==t2));
		System.out.println("t1.compareTo(t3)="+ t1.compareTo(t3));
	}
}
```

## 애너테이션(annotation)

**`애너테이션(annotation)`** : 프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것

Java를 개발한 사람들은 소스코드에 대한 문서를 따로 만들기보다 **소스코드와 문서**를 **하나의 파일로 관리**하는 것이 낫다고 생각했다.
- 그래서 소스코드의 주석 `/** ~ */` 에 **소스코드에 대한 정보를 저장**하고, **소스코드의 주석**으로부터 **HTML 문서를 생성해내는 프로그램(javadoc.exe)** 을 만들어서 사용했다.

**모든 애너테이션의 조상**인 **Annotation 인터페이스**의 소스코드 일부이다.

![Annotation에시](https://user-images.githubusercontent.com/56071088/128182145-6dbe0938-ddd4-4c81-82b2-893e7f99a897.png)


/**로 시작하는 주석 안에 소스코드에 대한 설명들이 있고, 그 안에 **`@`** 이 붙은 태그들이 눈에 뛸 것이다.

**미리 정의된 태그들을 이용**해서 **주석 안에 정보를 저장**하고, **javadoc.exe** 라는 프로그램이 **이 정보를 읽어서 문서를 작성하는데 사용**한다.

위의 기능을 응용하여, **프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것**이 바로 **애너테이션**이다.
- 애너테이션은 주석(comment)처럼 **프로그래밍 언어에 영향을 미치지 않으면서도 다른 프로그램에게 유용한 정보를 제공할 수 있다는 장점**이 있다.
- 예를 들어, 자신이 작성한 소스코드 중에서 특정 메서드만 테스트하기를 원한다면, 다음과 같이 `@Test`라는 애너테이션을 메서드 앞에 붙인다.
  - `@Test`는 이 메서드를 테스트해야 한다는 것을 테스트 프로그램에게 알리는 역할을 할 뿐, 메서드가 포함된 프로그램 자체에는 아무런 영향을 미치지 않는다.
  - 주석처럼 존재하지 않는 것이나 다름 없다.

**애너테이션**은 **JDK에서 기본적으로 제공하는 것**과 **다른 프로그램에서 제공하는 것들이 존재**
- 어느 것이든 그저 약속된 형식으로 정보를 제공하기만 하면 될 뿐이다.

**JDK에서 제공하는 표준 애너테이션**은 **주로 컴파일러**를 위한 것으로 **컴파일러에게 유용한 정보를 제공**한다.

**JDK에서 제공하는 표준 애너테이션**은 **새로운 애너테이션을 정의할 때** 사용하는 **메타 에너테이션도 제공**한다.

## 표준 애너테이션 

- 빌트인
- 메타 애너테이션

자바에서 기본적으로 제공하는 애터네이션들은 몇 개 없다
- **`메타 애너테이션(meta annotation)`** : 에너테이션을 정의하는데 사용되는 애너테이션의 애너테이션

![표준 애너테이션](https://user-images.githubusercontent.com/56071088/128183465-80f0739d-43d3-45bf-8622-e6c2cc14f038.png)


### @Override

**`@Override`** : **메서드 앞에만 붙일 수 있는 애너테이션**으로, **조상의 메서드를 오버라이딩하는 것**이라는 걸 **컴파일러에게 알려주는 역할**을 한다.
- 오버라이딩할 때 조상 메서드의 이름을 잘못 써도 컴파일러는 이것이 잘못된 것인지 알지 못한다.
- 그러나 `@Override 애너테이션`을 붙이면, **컴파일러**가 **같은 이름의 메서드가 조상에 있는지 확인하고 없으면, 에러메시지를 출력**한다.


### @Deprecated

**`@Deprecated`** : **다른 것으로 대체되었으니 더 이상 사용하지 않을 것을 권한다는 의미**

새로운 버전의 JDK가 소개될 때, 새로운 기능이 추가될 뿐만 아니라 기존의 부족했던 기능들을 개선하기도 한다. 이 과정에서 기존의 기능을 대체할 것들이 추가되어도, 이미 여러 곳에서 사용되고 있을지 모르는 기존의 것들을 함부로 삭제할 수 없다.

그래서 더 이상 사용되지 않는 필드나 메서드에 @Deprecated를 붙이는 것이다.
- 이 애너테이션이 붙은 대상은 다른 것으로 대체되었으니 더 이상 사용하지 않을 것을 권한다는 의미이다.

![Deprecated 사용 예시](https://user-images.githubusercontent.com/56071088/128184024-8d857b9d-fea1-4afc-a650-5f50aa889213.png)

해당 소스파일이 `deprecated`된 대상을 사용하고 있으며, **`Xlint:deprecation`** 옵션을 붙여서 **다시 컴파일하면 자세한 내용을 알 수 있다는 뜻**이다.

[@Deprecated 사용 예시]

```java
class NewClass{
	int newField;

	int getNewField() { 
		return newField;
	}	

	@Deprecated
	int oldField;

	@Deprecated
	int getOldField() { 
		return oldField;
	}
}

class AnnotationEx2 {
	public static void main(String args[]) {
		NewClass nc = new NewClass();

		nc.oldField = 10; //@Depreacted가 붙은 대상을 사용
		System.out.println(nc.getOldField()); //@Depreacted가 붙은 대상을 사용
	}
}
```

```json
c:\jdk1.8\work\ch12\javac AnnotaionEx2.java
Note: AnnotationEx2.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.

c:\jdk1.8\work\ch12\java AnnotationEx2
10
```

`@Deprecated`가 붙은 대상을 사용하여 고의적으로 위의 메시지가 나타나도록 했다.

**메시지가 나타났지만 컴파일도 실행도 잘되었다.** `@Deprecated`가 붙은 대상을 사용하지 않도록 권할 뿐 **강제성은 없기 때문이다.**

### @FunctionalInterface

**`@FunctionalInterface`** : **함수형 인터페이스를 선언할 때, 컴파일러가 함수형 인터페이스를 올바르게 선언했는지 확인하고, 잘못된 경우 에러를 발생시킨다.**

- 필수는 아니지만, 붙이면 실수를 방지할 수 있으므로 함수형 인터페이스를 선언할 때는 이 애너테이션을 반드시 붙이도록 하자

[참고] **함수형 인터페이스**는 **추상 메서드가 하나뿐이어야 한다는 제약**이 있다.  

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run(); // 추상메서드
}
```

### @SuppressWarnings

**`@SuppressWarnings`** : 컴파일러가 보여주는 경고메시지가 나타나지 않게 억제해준다.

- 경우에 따라 경고가 발생할 것을 알면서도 묵인해야 할 때, 해당 대상에 반드시 @SuppressWarnings를 붙여서 컴파일 후에 어떤 경고 메시지도 나타나지 않게 해야한다.

주로 사용하는 억제할 수 있는 경고 메시지의 종류
- **`deprecation`** : **@Deprecated가 붙은 대상을 사용해서 발생하는 경고**
- **`unchecked`** : **지네릭스로 타입을 지정하지 않았을 때 발생하는 경고**
- **`rawtypes`** : **지네릭스를 사용하지 않아서 발생하는 경고**
- **`varargs`** : **가변인자의 타입이 지네릭 타입일 때 발생하는 경고**
  
[사용법]

```java
@SuppressWarnings("unchecked")
ArrayList list = new ArrayList(); // 지네릭 타입을 지정하지 않았음
list.add(obj)                     // 여기서 경고가 발생
```

**둘 이상의 경고 억제** : 배열에서처럼 괄호{}를 추가로 사용해야 한다.

```java
@SuppressWarnings({"unchecked", "deprecation", "rawtypes", "varargs"})
```

`@SuppressWarnings`로 억제할 수 있는 경고 메시지의 종류는 **JDK의 버젼이 올라가면서 계속 추가**될 것이기 때문에, **이전 버젼에서는 발생하지 않던 경고가 새로운 버젼에서 발생할 수 있다.**

새로 추가된 경고 메시지를 억제하려면, 경고 메시지의 종류를 알아야 하는데, **`-Xlint`** 옵션으로 **컴파일** 해서 나타나는 **경고의 내용 중**에서 **대괄호[] 안**에 있는 것이 바로 **메시지의 종류다.**

[@SuppressWarnings 사용 예제]

```java
import java.util.ArrayList;

class NewClass{
	int newField;

	int getNewField() { 
		return newField;
	}	

	@Deprecated
	int oldField;

	@Deprecated
	int getOldField() { 
		return oldField;
	}
}

class AnnotationEx3 {
	@SuppressWarnings("deprecation")     // deprecation관련 경고를 억제
	public static void main(String args[]) {
		NewClass nc = new NewClass();

		nc.oldField = 10;                     //@Depreacted가 붙은 대상을 사용
		System.out.println(nc.getOldField()); //@Depreacted가 붙은 대상을 사용

		@SuppressWarnings("unchecked")               // 지네릭스 관련 경고를 억제
		ArrayList<NewClass> list = new ArrayList();  // 타입을 지정하지 않음.
		list.add(nc);
	}
}
```

- main 메서드 내에서 일어나는 deprecation과 관련된 경고를 억제
- 해당 대상에만 애너테이션을 붙여서 경고의 억제범위를 최소화하는 것이 좋다.

### @SafeVarargs

**메서드에 선언된 가변인자의 타입**이 `non-reifiable` 타입일 경우, **해당 메서드를 선언하는 부분과 호출하는 부분에서** `unchecked` 경고가 발생한다.

해당 코드에 문제가 없다면 이러한 경고를 억제하기 위해 **`SafeVarargs`** 를 사용해야 한다.

- `reifiable 타입` : 컴파일 후에도 제거되지 않는 타입, **컴파일 후에도 타입 정보가 유지되는 타입** 
  - reifiable : 다시 ~화(化) 할 수 있는
- `non-reifiable 타입` : **컴파일 후에 제거되는 타입**
  - 지네릭 타입들은 대부분 컴파일 시에 제거되므로 non-refiable타입이다.

**@SafeVarargs 애너테이션**은 **static이나 final이 붙은 메서드와 생성자**에만 붙일 수 있다. 즉, **오버라이딩 될 수 있는 메서드에는 사용할 수 없다**는 뜻이다.

[java.util.Arrays의 asList() 예시]

```java
// 이 메서드는 매개변수로 넘겨받은 값들로 배열을 만들어서 새로운 ArrayList객체를 만들어서 반환하는데 이 과정에서 경고가 발생한다.
public static <T> List<T> asList(T... a) {
	return new ArrayList<T>(a); // ArrayList(E[] array)를 호출. 경고 발생
}
```

asList()의 매개변수가 가변인자인 동시에 지네릭 타입이다. 메서드에 선언된 타입 T는 Object로 바뀐다.
- 즉, Object[]가 된다.
- Object[] 에는 모든 타입의 객체가 들어올 수 있으므로, 이 배열로 ArrayList< T> 를 생성하는 것은 위험하다고 경고를 하는 것

그러나 asList()가 호출되는 부분을 컴파일러가 테해서 타입 T가 아닌 다른 타입이 들어가지 못하게 할 것이므로 위의 코드는 아무런 문제가 없다!!
- 이럴 때는 메서드 앞에 **`@SafeVarargs`** 를 붙이면, 이 메서드를 호출하는 곳에서 발생하는 경고도 억제된다.

- **`@SafeVarargs`** : 메서드 선언뿐만 아니라 메서드가 호출되는 곳에도 발생하는 경고도 억제
- **`@SuppressWarnings("unchecked")`** : 메서드를 선언하는 곳과 호출하는 부분에 각각 따로 똑같이 붙여줘야 한다.

**`@SafeVarargs`** 로 **`unchecked`** 경고는 억제할 수 있지만, **`varargs`** 경고는 억제할 수 없기 때문에 **습관적으로 @SafeVarargs, @SuppressWarnings("varargs")를 같이 붙인다.**

```java
@SafeVarargs // 'unchecked' 경고를 억제한다.
@SuppressWarnings("varargs") // 'varargs' 경고를 억제한다.
public static <T> List<T> asList(T... a) {
	return new ArrayList<T>(a); // ArrayList(E[] array)를 호출. 경고 발생
}
```

[@SafeVarargs 어노테이션 사용 예제]

```java
import java.util.Arrays;

class MyArrayList<T> {
	T[] arr;

	@SafeVarargs
	@SuppressWarnings("varargs")
	MyArrayList(T... arr) {
		this.arr = arr;
	}
	
	@SafeVarargs
//	@SuppressWarnings("unchecked")
	public static <T> MyArrayList<T> asList(T... a) { 
        return new MyArrayList<>(a);
    }

	public String toString() {
		return Arrays.toString(arr);
	}
}

class AnnotationEx4 {
//	@SuppressWarnings("unchecked")
	public static void main(String args[]) {
		MyArrayList<String> list = MyArrayList.asList("1","2","3");

		System.out.println(list);
	}
}
```

## 메타 애너테이션

**`메타 애너테이션`** : `어노테이션을 위한 어노테이션`, 즉, **어노테이션을 붙이는 어노테이션으로 정의**할 때 **어노테이션의 적용대상(target)이나 유지기간(retention)등을 지정하는데 사용**된다.
- 메타 어노테이션은 **java.lang.Annotation패키지**에 포함되어 있다.

### @Target

**`@Target`** : **어노테이션이 적용가능한 대상을 지적**하는데 사용된다.

[@Target을 이용한 @SuppressWarnings 정의 예시]

```java
@Target({TYPE, FIELD, METHOD, PARAMETER})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
		String[] value();
}
```

- 여러 개의 값을 지정할 때는 **배열에서처럼 괄호{}를 사용**해야 한다.

[@Target으로 지정할 수 있는 애너테이션 적용대상의 종류]

![@Target](https://user-images.githubusercontent.com/56071088/128487420-5fcf7400-8fb6-498a-bc3e-068a44e8a53f.png)

위의 표는 **java.;ang.annotation.ElementType**이라는 **열거형**에 정의되어 있다.
- **static import문**을 사용하면 `ElementType.TYPE -> TYPE`와 같이 간단하게 사용할 수 있다.

- **`TYPE`** : **타입을 선언**할 때, 애너테이션을 붙일 수 있다. 
- **`TYPE_USE`** : **해당 참조형 타입의 변수**를 **선언**할 때 붙일 수 있다.
- **`FIELD`** : **해당 기본형 타입을 선언**할 때. 어노테이션을 붙일 수 있다.

```java
import static java.lang.annotation.ElementType.*;

@Target({TYPE, TYPE_USE, FIELD}) // 적용 대상
public @interface MyAnnotation { } // MyAnnotation 정의

@MyAnnotation // 적용대상 TYPE
class MyClass {
	@MyAnnotation // 적용대상 FIELD
	int i;
	
	@MyAnnotation // 적용대상 TYPE_USE인 경우
	Myclass mc;
}
```

### @Retention

**`@Retention`** : **어노테이션이 유지되는 기간을 지정**하는데 사용된다.

[@Retention어노테이션의 유지 정책(retention policy)]

![@Retention](https://user-images.githubusercontent.com/56071088/128488776-a4f5b4b2-a59b-4823-986c-9f142eab8a98.png)

#### SOURCE

**`SOURCE`** : **소스파일(.java)에만 존재. 클래스파일(.class)에는 존재하지 않음.**
- `@Override`, `@SuppressWarnings` 처럼 컴파일러가 사용하는 어노테이션은 **유지 정책이 SOURCE**이다.
- 컴파일러를 직접 작성하는 것이 아니면, 이 유지정책은 필요없다.

#### RUNTIME

**`RUNTIME`** : **클래스파일(.class)에만 존재. 실행시에 사용가능**
- 유지정책을 `RUNTIME`으로 하면, **실행시에 리플렉션(reflection)** 을 통해 **클래스 파일(.class)에 저장된 애너테이션의 정보를 읽어서 처리**할 수 있다.
  - `@FunctionalInterface`는 @Override처럼 컴파일러가 체크해주는 애너테이션이지만, **실행 시에도 사용**되므로 **유지정책이 RUNTIME**이다.  

#### CLASS

**`CLASS`** : **클래스파일(.class)에만 존재. 실행시에 사용불가. 기본값**
- 컴파일러가 애너테이션의 정보를 클래스파일에 저장할 수 있게 하지만, 클래스파일이 JVM에 로딩될 때는 애너테이션의 정보가 무시되어 실행 시에 애너테이션에 대한 정보를 얻을 수 없다.
- 이것이 `CLASS`가 유지정책의 기본값임에도 불구하고 잘 사용되지 않는 이유이다.

**지역변수에 붙은 애너테이션은 컴파일러만 인식할 수 있으므로, 유지정책이 RUNTIME인 애너테이션을 지역변수에 붙여도 실행 시에는 인식되지 않는다.**

### @Documented

**`@Documented`** : **애너테이션에 대한 정보가 javadoc으로 작성한 문서에 포함**되도록 한다.
- Java에서 제공하는 기본 애너테이션 중에 **@Override, @SuppressWarnings**를 제외하고는 모두 이 메타애너테이션이 붙어 있다.

[@Documented 예시]

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface FunctionalInterface {}
```

### @Inherited

**`@Inherited`** : **애너테이션이 자손 클래스에 상속되도록 한다.**
- **@Inherited가 붙은 애너테이션을 조상 클래스**에 붙이면, **자손 클래스에도 이 애너테이션이 붙은 것과 같이 인식**된다.

```java
@Inherited  // @SupperAnno가 자손까지 영향 미치게
@interface SupperAnno {} 

@SupperAnno
class Parent {}
class Child extends Parent {} // Child에 애너테이션이 붙은 것으로 인식
```

### @Repeatable



  