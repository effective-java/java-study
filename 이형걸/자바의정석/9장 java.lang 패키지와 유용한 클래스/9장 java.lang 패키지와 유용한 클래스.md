# 9장 java.lang 패키지와 유용한 클래스

**`java.lang 패키지`** : Java Programming의 가장 기본이 되는 패키지. 
- java.lang 패키지의 클래스들은 **import문 없이도 사용** 할 수 있게 되어 있다.

## Object 클래스

- Object클래스는 모든 클래스의 최고 조상
- Object클래스의 멤버들은 모든 클래스에서 바로 사용 가능하다.
- Object클래스는 **멤버변수는 없고** , **오직 11개의 메서드** 만 갖고 있다.
- 이 메서드들은 모든 인스턴스가 갖는 기본적인 것들이다.

<img src="https://user-images.githubusercontent.com/56071088/124707648-16475880-df34-11eb-95e4-65c1044c47f2.png"  width="500" height="350">

### equals(Object obj)

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

- 두 객체의 같고 다름을 **참조변수의 값으로 판단**

- **서로 다른 두 객체를 equals메서드로 비교하면 항상 false값을 얻게 된다.**
  - 객체를 생성할 때, 메모리의 비어있는 공간을 찾아 생성하므로 **서로 다른 두개의 객체가 같은 주소를 갖는 일은 있을 수 없다.**
  - 두 개 이상의 참조변수가 같은 주소값을 갖는 것(한 객체를 참조하는 것)은 가능하다.

- Object클래스로부터 상속받은 equals메서드는 두 참조변수에 저장된 값(주소 값)이 같은지를 판단하는 기능 밖에 할 수 없기 때문에
- **equals메서드를 오버라이딩하여 주소가 아닌 객체에 저장된 내용을 비교하도록 변경하면 된다.**

```java
class Value {
	int value;

	Value(int value) {
		this.value = value;
	}
}

class Person {
	long id;

	public boolean equals(Object obj) {
		if(obj != null && obj instanceof Person) {
			return id ==((Person)obj).id; // obj가 Object타입이므로 id값을 참조하기 위해서는 Person타입으로 형변환이 필요하다.
		} else {
			return false; // Person타입이 아니면 값을 비교할 필요도 없다.
		}
	}

	Person(long id) {
		this.id = id;
	}
}

class EqualsEx2 {
	public static void main(String[] args) {
		Person p1 = new Person(8011081111222L);
		Person p2 = new Person(8011081111222L);

		if(p1==p2)
			System.out.println("p1과 p2는 같은 사람입니다.");
		else
			System.out.println("p1과 p2는 다른 사람입니다.");

		if(p1.equals(p2))
			System.out.println("p1과 p2는 같은 사람입니다.");
		else
			System.out.println("p1과 p2는 다른 사람입니다.");

        Value v1 = new Value(10);
		Value v2 = new Value(10);		

		if (v1.equals(v2)) {
			System.out.println("v1과 v2는 같습니다.");
		} else {
			System.out.println("v1과 v2는 다릅니다.");		
		}

		v2 = v1;

		if (v1.equals(v2)) {
			System.out.println("v1과 v2는 같습니다.");
		} else {
			System.out.println("v1과 v2는 다릅니다.");		
		}
	
	}
}
```

```json
p1과 p2는 다른 사람입니다.
p1과 p2는 같은 사람입니다.

v1과 v2는 다릅니다.
v1과 v2는 같습니다.
```

- String클래스 역시 Object클래스의 equals메서드를 **오버리이딩** 을 통해서 **String인스턴스가 갖는 문자열 값을 비교하도록 되어 있는 것이다.**
    - String클래스 뿐만 아니라, Data, File, Wrapper클래스(Integer, Double등)의 equals메서드도 주소값이 아닌 내용을 비교하도록 **오버라이딩** 되어 있다.
    - 의외로 **StringBuffer클래스는 오버라이딩 되어 있지 않다.**

### hashCode()

- `hashCode()` : **해싱 기법** 에 사용되는 `해시함수(hash function)` 를 구현한 것이다.
  - `해싱` : 데이터관리기법중의 하나, 다량의 데이터를 저장하고 검색하는 데 유용하다.
  - `해시함수` 는 찾고자 하는 값을 입력하면, **그 값이 저장된 위치** 를 알려주는 `해시코드` 를 반환한다.  

- 일반적으로 해시코드가 같은 두 객체가 존재하는 것은 가능하다.
- **Object클래스에 정의된 hashCode()** 는 **객체의 주소값** 을 이용해서 해시코드를 만들어 반환하기 때문에 **서로 다른 두 객체는 결코 같은 해시코드를 가질 수 없다.**

클래스의 **인스턴스변수 값** 으로 객체의 **같고 다름을 판단** 하는 경우라면, **hashCode()도 오버라이딩 해야한다.**
- 같은 객체라면 hashCode()를 호출했을 때의 결과값인 **해시코드도 같아야 하기 때문이다.**

- 오버라이딩 하지 않는다면 Object클래스에 정의된 대로 모든 객체가 서로 다른 해시코드 값을 가질 것이다.
- **해싱기법** 을 사용하는 `HashMap, HashSet` 과 같은 클래스에 **저장할 객체** 라면 **반드시 hashCode()를 오버라이딩 해야한다.**  

```java
class HashCodeEx1 {
	public static void main(String[] args) {
		String str1 = new String("abc");
		String str2 = new String("abc");

		System.out.println(str1.equals(str2));
		System.out.println(str1.hashCode());
		System.out.println(str2.hashCode());
		System.out.println(System.identityHashCode(str1));
		System.out.println(System.identityHashCode(str2));
	}
}
```

```json
true
96354
96354
27134973
1284693
```
- `String클래스` 는 **문자열의 내용이 같으면 동일한 해시코드** 를 반환하도록 **hashCode()가 오버라이딩 되어있다.**
- `System.identityHashCode(Object x)` 는 Object클래스의 hashCode메서드처럼 **객체의 주소값** 으로 해시코드를 생성하기 때문에, **모든 객체에 대해 항상 다른 해시코드값을 반환할 것을 보장한다.**
  - System.identityHashCode(Object x)의 호출결과는 실행 할때마다 달라질 수 있다.

### toString()

`toString()` : 인스턴스에 대한 정보를 문자열(String)로 제공한다.

- 대부분의 경우 인스턴스 변수에 저장된 값들을 문자열로 표현한다는 뜻이다.

- 클래스를 작성할 때, **toString()을 오버라이딩하지 않는다면** , **클래스이름에 16진수의 해시코드** 를 얻게 된다. 

```java
public String toString() {
	return getClass().getName()+"@"+Integer.toHexString(hashCode());
}
```

- toString()은 일반적으로 **인스턴스나 클래스에 대한 정보 또는 인스턴스 변수들의 값** 을 **문자열로 변환하여 반환** 하도록 **오버라이딩되는 것이 보통이다.**

### clone()

`clone()` : 자신을 복제하여 **새로운 인스턴스를 생성하는 일** 을 한다.

- 원래의 인스턴스는 보존하고, clone() 메서드를 이용하여 새로운 인스턴스를 생성하여 작업을 하면, 
- 작업이전의 값이 보존되므로 작업에 실패해서 원래의 상태로 되돌리거나 변경되기 전의 값을 참고하는데 도움이 된다.

Object클래스에 정의된 clone()은 **단순히 인스턴스 변수의 값만을 복사** 하기 때문에 **참조 타입의 인스턴스 변수가 있는 클래스는 완전한 인스턴스 복제가 이루어지지 않는다.**

```java
class Point implements Cloneable {
	int x;
	int y;

	Point(int x, int y) {
		this.x = x;
		this.y = y;
	}

	public String toString() {
		return "x="+x +", y="+y;
	}

	public Object clone() {
		Object obj = null;
		try {
			obj = super.clone();  // clone()은 반드시 예외처리를 해주어야 한다.
		} catch(CloneNotSupportedException e) {}
		
        return obj;
	}
}

class CloneEx1 {
	public static void main(String[] args){
		Point original = new Point(3, 5);
		Point copy = (Point) original.clone(); // 복제(clone)해서 새로운 객체를 생성
		System.out.println(original);
		System.out.println(copy);
	}
}
```

- `Cloneable` 인터페이스를 구현한 클래스에서만 clone()을 호출할 수 있다. **이 인터페이스를 구현하지 않고 clone()을 호출하면 예외가 발생한다.**
- clone()을 호출하려면,
  - cloneable 인터페이스를 구현해야 하고,
  - clone()을 오버라이딩하면서 
  - 접근제어자를 protected에서 public으로 변경해야 한다.
  - 그래야만 상속관계가 없는 다른 클래스에서 clone()을 호출할 수 있다.

```java
public class Object {
    ...
    protected native Object clone() throws CloneNotSupportedException;
}
```

- Cloneable 인터페이스를 구현한 클래스의 인스턴스만 clone()을 통해 복제가 가능한 이유 : **인스턴스의 데이터를 보호하기 위해서이다.**
- Cloneable 인터페이스가 구현되어 있다는 의미는 **클래스 작성자가 복제를 허용한다는 의미**

### 공변 반환타입

`JDk 1.5` 부터 `공변 반환타입(covariant return type)` 이라는 것이 추가되었다.

`공변 반환타입(covariant return type)` : **`오버라이딩`** 할 때 **조상메서드의 반환타입** 을 **자손 클래스의 타입** 으로 **변경을 허용** 한다는 뜻이다.

```java
@Override
public Point clone() { // 반환타입을 Object에서 Point로 변경
    Object obj = null;

    try {
        obj = super.clone();
    } catch (CloneNotSupportedException e) {}

    return (Point) obj; // Point타입으로 형변환한다.
}
```

- 공변 반환타입을 사용하면, 조상의 타입이 아닌, **실제로 반환되는 자손 객체의 타입으로 반환** 할 수 있어서 **번거로운 형변환이 줄어든다는 장점** 이 있다. 

- **배열(Array)도 객체** 이기 때문에, Object클래스를 상속받으며, 동시에 Cloneable, Serialzable 인터페이스가 구현되어 있다.

- 배열(Array) 에서는 clone()을 public으로 **오버라이딩** 하였기 때문에, 직접 호출 가능하며, **원본과 같은 타입을 반환** 하므로 **형변환이 필요 없다.** 

-  배열 뿐만 아니라 java.util 패키지의 Vector, ArrayList, LinkedList, HashSet, TreeSet, HashMap, TreeMap, Calendar, Date와 같은 클래스들이 이와 같은 방식으로 복제가 가능하다.

```java
ArrayList list = new ArrayList<>();
...
ArrayList list2 = (ArrayList) list.clone();
```

### 얕은 복사와 깊은 복사

기본형 배열을 clone()으로 복제하는 경우에는 아무런 문제가 없지만,

객체 배열을 clone()으로 복제하는 경우에느 원본과 복제본이 같은 객체를 공유하므로 완전한 복제라고 보기 어렵다.

- `얕은 복사(shallow copy)` : 원본을 변경하면, 복사본도 영향을 받는다.
- `깊은 복사(deep copy)` : 원본이 참조하고 있는 객체까지 복제하는 것

```java
import java.util.*;

class Circle implements Cloneable {
	Point p;  // 원점
	double r; // 반지름

	Circle(Point p, double r) {
		this.p = p;
		this.r = r;
	}

	public Circle shallowCopy() { // 얕은 복사
		Object obj = null;

		try {
			obj = super.clone();
		} catch (CloneNotSupportedException e) {}

		return (Circle)obj;
	}

	public Circle deepCopy() { // 깊은 복사
		Object obj = null;

		try {
			obj = super.clone();
		} catch (CloneNotSupportedException e) {}

		Circle c = (Circle)obj; 
		c.p = new Point(this.p.x, this.p.y); 

		return c;
	}

	public String toString() {
		return "[p=" + p + ", r="+ r +"]";
	}
}

class Point {
	int x;
	int y;

	Point(int x, int y) {
		this.x = x;
		this.y = y;
	}

	public String toString() {
		return "("+x +", "+y+")";
	}
}

class ShallowCopy {
	public static void main(String[] args) {
		Circle c1 = new Circle(new Point(1, 1), 2.0);
		Circle c2 = c1.shallowCopy();
		Circle c3 = c1.deepCopy();
	
		System.out.println("c1="+c1);
		System.out.println("c2="+c2);
		System.out.println("c3="+c3);
		c1.p.x = 9;
		c1.p.y = 9;
		System.out.println("= c1의 변경 후 =");
		System.out.println("c1="+c1);
		System.out.println("c2="+c2);
		System.out.println("c3="+c3);
	}
}
```

```json
실행결과
c1=[p=(1, 1), r=2.0]
c2=[p=(1, 1), r=2.0]
c3=[p=(1, 1), r=2.0]
= c1의 변경 후 =
c1=[p=(9, 9), r=2.0]
c1=[p=(9, 9), r=2.0]
c1=[p=(1, 1), r=2.0]
```

### getClass()

`getClass()` : **자신이 속한 클래스의 Class 객체를 반환하는 메서드**

- Class 객체는 이름이 `Class` 인 클래스의 객체이다.

`Class객체` 는 
- **클래스의 모든 정보**를 담고 있고, **클래스 당 1개** 만 존재한다.
- 클래스 파일이 클래스 로더(ClassLoader)에 의해서 **메모리에 올라갈 때, 자동으로 생성된다.**

```java
public final class Class implements ... { // Class클래스
    ...
}
```

`클래스 로더(ClassLoader)` 는 
- **실행 시에 필요한 클래스를 동적으로 메모리에 로드하는 역할**
  
- 먼저 기존에 생성된 클래스 객체가 메모리에 존재하는지 확인하고,
  - 있으면 객체의 참조를 반환
  - 없으면 클래스 패스(classpath)에 지정된 경로를 따라서 **클래스 파일(.class)** 을 찾는다.
    - 못 찾으면 ClassNotFoundException이 발생하고, 
    - 찾으면 해당 **클래스 파일을 읽어서 Class 객체로 반환**한다. 

그러니까 **파일 형태(.class)로 저장되어 있는 클래스를 읽어서 Class 클래스에 정의된 형식으로 변환하는 것** 이다.

즉, 클래스파일을 읽어서 사용하기 편한 현태로 저장해 놓은 것이 클래스 객체이다.

#### Class 객체를 얻는 방법

클래스의 정보가 필요할 때, 먼저 Class객체에 대한 참조를 얻어 와야 하는데, 해당 Class 객체에 대한 참조를 얻는 방법은 여러가지가 있다.

```java
// 생성된 객체로부터 얻는 방법
Class cObj = new Card().getClass(); 

// 클래스 리터럴(*.class)로 부터 얻는 방법
Class cObj = Card.class;
     
// 클래스 이름으로 부터 얻는 방법
// 특정 클래스 파일, 예를 들어 데이터베이스 드라이버를 메모리에 올릴 때 주로 사용
Class cObj = Class.forName("Card");
```

- forName() 은 특정 클래스 파일, 예를 들어 데이터베이스 드라이버를 메모리에 올릴 때 주로 사용

```java
Card c = new Card(); // new 연산자를 이용한 객체 생성
Card c = Card.class.newInstance(); // Class객체를 이용한 객체 생성
```
- 클래스 객체를 이용하면 클래스에 정의된 멤버의 이름이나 개수 등, 클래스에 대한 모든 정보를 얻을 수 있기 때문에 

- **Class 객체를 통해서 객체를 생성하고, 메서드를 호출하는 등** 보다 **동적인 코드 작성 가능**
- 동적으로 객체를 생성하고 메서드를 호출하는 방법에 대해 더 알아보려면, `리플렉션 API(reflection API)` 를 검색하면 된다.

## String 클래스

### 변경 불가능한(immutable) 클래스

- String 클래스에는 문자열을 저장하기 위해서 문자형 배열 참조변수(char[] value) 를 인스턴스 변수로 정의한다.
- 인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스 변수(value)에 문자형 배열(char[])로 저장되는 것이다.
- String클래스는 앞에 final이 붙어 있으므로 다른 클래스의 조상이 될 수 없다.

```java
public final class String implements java.io.Serializable, Comparable {
	private char[] value;
}
```

- **한번 생성된 String인스턴스가 갖고 있는 문자열은 읽어 올 수만 있고, 변경할 수는 없다.**

<img width="500" alt="새로운 문자열 ab" src="https://user-images.githubusercontent.com/56071088/124723740-4d266a00-df46-11eb-9c32-f934efabea7d.png">

- `+ 연산자` 를 이용하여 문자열을 결합하는 경우 인스턴스 내의 문자열이 바뀌는 것이 아니라 **새로운 문자열("ab")이 담긴 String인스턴스가 생성** 되는 것이다.

- **매 연산 시 마다 새로운 문자열을 가진 String인스턴스가 생성되어 메모리 공간을 차지하므로 가능한 한 결합횟수를 줄이는 것이 좋다.**

- **문자열간의 결합이나 추출 등 문자열을 다루는 작업** 이 많이 필요한 경우에는 String클래스 대신 **StringBuffer클래스** 를 사용하는 것이 좋다. 
  - StringBuffer 인스턴스에 저장된 문자열은 변경이 가능하므로 하나의 StringBuffer인스턴스만으로도 문자열을 다루는 것이 가능하다.

### 문자열의 비교

문자열을 만들 때는 2가지 방법
1. `문자열 리터럴` 을 지정하는 방법
   - **이미 존재하는 인스턴스를 재사용** 하는 것

2. `String클래스의 생성자` 를 사용해서 만드는 방법
   - new 연산자에 의해서 메모리할당이 이루어지기 때문에 항상 새로운 String인스턴스가 생성된다. 

```java
String str1 = "abc";
String str2 = "abc";

String str3 = new String("abc");
String str4 = new String("abc");
```

![문자열 리터럴, String 생성자](https://user-images.githubusercontent.com/56071088/124725971-4c8ed300-df48-11eb-8c6f-a47cb51a05c2.png)

### 문자열 리터럴

**모든 문자열 리터럴** 은 **컴파일 시** 에 **클래스 파일에 저장된다.** 

- **이 때 같은 내용의 문자열 리터럴은 한번만 저장된다.**
  - 문자열 리터럴도 String인스턴스이고, 한번 생성하면 내용을 변경할 수 없으니, 하나의 인스턴스만 공유하면 되기 때문이다.

```java
class StringEx2 {
	public static void main(String args[]) {
		String s1 = "AAA";
		String s2 = "AAA";
		String s3 = "AAA";
		String s4 = "BBB";
	}
}
```

- 위의 예제를 실행하면 "AAA"라는 문자열을 담고 있는 `String 인스턴스` 가 하나가 생성된 후, **참조변수 s1, s2, s3는 모두 이 String 인스턴스를 참조하게 된다.**

- `클래스 파일(.class)` 에는 **소스파일에 포함된 모든 리터럴 목록** 이 있다. 
- 해당 클래스 파일이 **클래스 로더** 에 의해 **메모리** 에 올라갈 때, 이 **리터럴의 목록** 에 있는 **리터럴들이 JVM 내의 상수 저장소(constant pool)** 에 저장된다.

- 이곳에 **"AAA"와 같은 문자열 리터럴이 자동적으로 생성되어 저장되는 것이다.**

### 빈 문자열(empty string) 

**길이가 0인 배열이 존재할 수 있다!!**

- char형 배열도 길이가 0인 배열을 생성할 수 있고,
- **길이가 0인 char형 배열을 내부적으로 가지고 있는 문자열** 이 바로 `빈 문자열` 이다.

```java
String str = "";
```

참조변수 str가 참조하고 있는 String 인스턴스는 내부에 `new char[0]` 와 같이 **길이가 0인 char형 배열** 을 저장하고 있는 것이다.

- char형 변수는 반드시 하나의 문자를 지정해야 한다.

```java
String str = null;
char c = '\u0000';

->
// 이 형태로 하는 것이 보통이다.
String s = "";
char ch = ' ';
```

[참고] `\u0000` 은 **유니코드의 첫번째 문자** 로써 **아무런 문자도 지정되지 않은 빈 문자** 다.

### String클래스의 생성자와 메서드

![String메서드1](https://user-images.githubusercontent.com/56071088/125237464-b4fefb00-e320-11eb-9594-b878aca6780c.png)

<!-- <img src="https://user-images.githubusercontent.com/56071088/125237464-b4fefb00-e320-11eb-9594-b878aca6780c.png"  width="550" height="700"> -->

- String 생성자
  - 문자열 : new String("String");
  - char[] 배열 : new String(chArr);
  - StringBuffer : new String(sb); 

```java
String s1 = new String("String");
String s11 = new String("");

char[] chArr= { 'S', 't', 'r', 'i', 'n', 'g' };
String s2 = new String(chArr);

StringBuffer sb = new StringBuffer("String");
String s3 = new String(sb);
```

![String메서드2](https://user-images.githubusercontent.com/56071088/125237479-b7f9eb80-e320-11eb-906b-2f41ce00dce4.png)

- replace vs replaceAll 의 차이점?
  
```java
String str = "aaabbbccccabcddddabcdeeee";

String result1 = str.replace("a", "왕").replace("b", "왕").replace("c", "왕");

String result2 = str.replaceAll("[abc]", "왕");
```

```json
왕왕왕왕왕왕왕왕왕왕왕왕왕dddd왕왕왕deeee
왕왕왕왕왕왕왕왕왕왕왕왕왕dddd왕왕왕deeee
```

![String메서드3](https://user-images.githubusercontent.com/56071088/125237483-b92b1880-e320-11eb-9f5d-04f4fa6bba14.png)

- `CharSequence` 는 **JDK 1.4** 부터 추가된 `인터페이스` 로 **String, StringBuffer** 등의 클래스가 구현하였다.

- **contains(CharSequence s)** , **replace(CharSequence old, CharSequence old)** 는 **JDK 1.5** 부터 추가되었다.

### join() 과 StringJoiner

`join()` : **여러 문자열 사이에 구분자를 넣어서 결합**한다.

```java
String animals = "dog,cat,bear";
String[] arr = animals.split(","); // 문자열 ','를 구분자로 나눠서 배열에 저장
String str = Sring.join("-", arr); // 배열의 문자열을 '-'로 구분해서 결합한다.

System.out.println(str); // dog-cat-bear
```

`java.util.StringJoiner클래스` 를 사용해서 문자열을 결합할 수도 있다.

```java
StringJoiner sj = new StringJoiner(",", "[", "]");
String[] strArr = { "aaa", "bbb", "ccc" };

for(String s : strArr) {
	sj.add(s.toUpperCase());
}

System.out.println(sj.toString()); // [AAA,BBB,CCC]
```

- join() 과 java.util.StringJoiner 는 JDK1.8 부터 추가되었다.

### 유니코드의 보충문자

- 스트링 클래스의 메서드 중에 매개변수의 타입이 char, int 인 것들도 있다.

- 문자를 다루는 메서드라서 매개변수의 타입이 char일 것 같은데 왜 int일까?
    - **확장된 유니코드**를 다루기 위해서이다.

- `유니코드` 는 원래 **2 byte, 16비트 문자**인데, **20비트로 확장** 하게 되었다.
- 그래서 하나의 문자를 char타입으로 다루지 못하고, **int 타입** 으로 다룰 수 밖에 없다.
- 확장에 의해 **새로 추가된 문자들** 을 **`보충 문자`** 라고 하는데, **String 클래스의 메서드 중에서는 보충 문자를 지원하는 것이 있고 지원하지 않는 것도 있다.**
  
    - **구별법은 매개변수가 int ch인 것들은 지원하는 것들, char ch인 것들은 지원하지 않는 것들이다.**

### 문자 인코딩 변환

**`getBytes(String charsetName)`** 을 사용하면, **문자열의 문자 인코딩을 다른 인코딩으로 변경할 수 있다.**

- Java가 `utf-16` 을 사용하지만, **문자열 리터럴** 에 포함되는 문자들은 **OS 의 인코딩** 을 사용한다. **한글 윈도우즈** 의 경우 문자 인코딩으로 `CP949` 를 사용하며, UTF-8로 변경하려면 아래와 같이 한다.
  - `utf-8` 은 한글 한글자를 3 byte, `CP949` 는 2 byte로 표현
  
  - 사용가능한 문자 인코딩의 목록은 System.out.println(java.nio.charset.Cahrset.availableCharsets());

```java
byte[] utf8_str = "가".getBytes("UTF-8");    // 문자열을 utf-8로 변환
String str = new String(utf8_str, "UTF-8"); // byte배열을 문자열로 변환
```

```java
import java.util.StringJoiner;

class StringEx5 {
	public static void main(String[] args) throws Exception {
		String str = "가";

		byte[] bArr  = str.getBytes("UTF-8");
		byte[] bArr2 = str.getBytes("CP949");

		System.out.println("UTF-8:" + joinByteArr(bArr));
		System.out.println("CP949:" + joinByteArr(bArr2));

		System.out.println("UTF-8:" + new String(bArr,  "UTF-8"));
		System.out.println("CP949:" + new String(bArr2, "CP949"));
	}

	static String joinByteArr(byte[] bArr) {
		 StringJoiner sj = new StringJoiner(":", "[", "]");

		for(byte b : bArr)
			sj.add(String.format("%02X",b));

		return sj.toString();
	}
}
```

```json
실행결과
UTF-8 : [EA:B0:80]
CP949 : [B0:A1]
UTF-8:가
CP949:가
```

### String.format()

`format()` : **형식화된 문자열** 을 만든다.

- **printf()** 를 사용하는 방법과 똑같다.

```java
String str = String.format("%d 더하기 %d는 %d입니다.", 3, 5, 3+5);
System.out.println(str); // 3더하기 5는 8입니다.
```

### 기본형(primitive type) 값을 String으로 변환

1. **기본형(primitive type) + ""(빈 문자열)**
2. **String.valueOf() 사용**

**성능은 valueOf()가 더 좋지만** , 빈 문자열("")을 더하는 방법이 간단하고 편하기 때문에 더 자주 쓴다.

```java
int i = 100;
String s1 = i + ""; // 100을 "100"으로 변환하는 방법 1
String s2 = String.valueOf(i); // 100을 "100"으로 변환하는 방법 2
```

[참고] **참조변수** 에 **String을 더하면** , 참조변수가 가리키는 인스턴스의 toString()을 호출하여 String을 얻은 다음 결합한다.

### String을 기본형(primitive type)으로 변환

1. **valueOf()** 사용
	- 리턴 타입은 Interger인데 **오토박싱** 에 의해 **int로 자동 변환** 된다.
2. **parseInt()** 사용
   - parserBoolean(), parserByte(), parseShort(), parseInt(), parseLong(), parseFloat(), parserDouble()
  
   - **byte, short** 을 문자열로 변경할 때 : String valueOf(int i)를 사용
   - 문자열 "A"를 문자 'A로 변환 : char ch = "A".charAt(0);
   - Boolean, Byte와 같이 기본형 타입의 이름이 첫 글자가 대문자인 것은 **래퍼 클래스** 이다.
     - **기본형 값을 감싸고 있는 클래스** 라는 뜻에서 붙여진 이름으로 **기본형을 클래스로 표현한 것**

```java
int i = Integer.valueOf("100"); // "100"을 100으로 변환하는 방법 1 // 원래 반환 타입이 Integer // 오토박싱(auto-boxing)
int i = Integer.parseInt("100"); // "100"을 100으로 변환하는 방법 2
```

원래 valueOf()의 반환타입은 Integer인데, **오토박싱(auto-boxing)** 에 의해 **Inetger가 int으로 자동변환된다.**

예전에는 parseInt()와 같은 메서드를 많이 썼는데, **메서드의 이름을 통일** 하기 위해서 **valueOf() 가 나중에 추가되었다.**
- **valueOf(String s) 는 메서드 내부에서 parseInt(String s)를 호출** 할 뿐이므로, 두 메서드는 반환타입만 다르지 완전히 같은 메서드이다.

```java
public static Integer valueOf(String s) throws NumberFormatException {
	return Integer.valueOf(parseInt(s, 10)); // 여기서 10은 10진수를 의미
}
```

```java
class StringEx6 {
	public static void main(String[] args) {
		int iVal = 100;
		String strVal = String.valueOf(iVal);	// int를 String으로 변환한다.
		
		double dVal = 200.0;
		String strVal2 = dVal + "";	// int를 String으로 변환하는 또 다른 방법

		double sum  = Integer.parseInt("+"+strVal)+Double.parseDouble(strVal2);
		double sum2 = Integer.valueOf(strVal) + Double.valueOf(strVal2);

		System.out.println(String.join("",strVal,"+",strVal2,"=")+sum);
		System.out.println(strVal+"+"+strVal2+"="+sum2);
	}
}
```

```json
실행결과
100+200.0=300.0
100+200.0=300.0
```

- **parseInt(), paseFloat() 같은 메서드** 는 **문자열에 공백 또는 문자가 포함되어 있는 경우** 변환 시 **예외(NumberFormatException) 가 발생** 할 수 있으므로 주의해야 한다. 
  - 그래서 **문자열 양끝의 공백을 제거** 해주는 **trim()** 을 습관적으로 같이 사용한다. 

```java
int val = Integer.parseInt(" 123 ".trim()); // 문자열 양 끝의 공백을 제거 후 변환
```

- 부호를 의미하는 '+'나 소수점을 의미하는 '.'와 float형 값을 뜻하는 f와 같은 **자료형 접미사** 는 허용된다.
  - **단, 자료형에 알맞은 변환을 하는 경우에만 허용**
  
  - **1.0f**를 Integer.parsetInt() 사용하면 예외 발생, **Float.parserFlaoat()은 문제 없다.** 

- **+** 가 포함된 문자열이 parseInt() 로 변환 가능하게 된 것은 **JDK1.7** 부터이다.

- Integer클래스의 static int parseInt(String s, int radix) 를 사용하면 16진수 값으로 표현된 문자열도 변환할 수 있기 때문에 대소문자 구별없이 a, b, c, d, e, f 도 사용할 수 있다.
  
```java
int result = Integer.parseInt("a", 16); // result에는 정수값 10이 저장된다. // 16진수 a는 10진수로 10을 의미한다.
```

## StringBuffer클래스와 StringBuilder클래스

**String클래스는 인스턴스를 생성할 때 지정된 문자열을 변경할 수 없다!!**

**StringBuffer클래스는 변경이 가능하다!!**
- **내부적으로 문자열 편집** 을 위해 **버퍼(buffer)** 를 가지고 있으며,
- **StringBuffer인스턴스를 생성할 때 그 크기를 지정할 수 있다.**

StringBuffer클래스는 String클래스와 같이 문자열을 저장하기 위한 char형 배열의 참조변수를 인스턴스 변수로 선언해 놓고 있다.

```java
public final class StringBuffer implements java.io.Serializable {
	private char[] value;
	...
}
```

### StringBuffer의 생성자

StringBuffer클래스의 인스턴스를 생성할 때, **적절한 길이의 char형 배열** 이 생성되고, 이 배열은 **문자열을 저장하고 편집하기 위한 공간(buffer)** 으로 사용된다.
- StringBuffer인스턴스를 생성할 때, 버퍼의 크기를 지정해주지 않으면 16개의 문자를 저장할 수 있는 크기의 버퍼를 생성한다. 

```java
public final class StringBuffer implements java.io.Serializable {
	private char[] value;
	
	public StringBuffer(int length) {
		value = new char[length];
		shared = false;
	}

	public StringBuffer() {
		this(16); // 버퍼의 크기를 지정하지 않으면 버퍼의 크기는 16이 된다.
	}

	public StringBuffer(String str) {
		this(str.length() + 16); // 지정한 문자열의 길이보다 16이 더 크게 버퍼를 생성한다.
		append(str);
	}
}
```

버퍼의 크기가 작업하려는 문자열의 길이보다 작을 때, 내부적으로 버퍼의 크기를 증가시키는 작업이 수행된다.
- 배열의 길이는 변경할 수 없으므로 **새로운 길이의 배열을 생성한 후에 이전 배열의 값을 복사해야 한다.**

```java
...
// 새로운 길이(newCapacity)의 배열을 생성한다. newCapacity는 정수값이다.
char[] newValue = new char[newCapacity];

// 배열 value의 내용을 배열 newValue로 복사한다.
System.arrayCopy(value, 0, newValue, 0, count); // count는 문자열의 길이
value = newValue; // 새로 생성된 배열의 주소를 참조변수 value에 저장
```

### StringBuffer의 변경

String과 달리 **StringBuffer는 내용을 변경할 수 있다.**

![스트링버퍼](https://user-images.githubusercontent.com/56071088/125237079-11154f80-e320-11eb-85d7-45f421daba82.png)

```java
SringBuffer sb = new StringBuffer("abc");

sb.append("123"); // sb의 내용 뒤에 "123"을 추가한다. 

StringBuffer sb2 = sb.append("zz");

System.out.println(sb); // abc123zz
System.out.println(sb2); // abc123zz 
```

- append()는 반환타입이 StringBuffer인데 **자신의 주소를 반환한다.**
- sb 와 sb2 모두 같은 인스턴스를 가리키고 있으므로 같은 내용이 출력된다.
- 하나의 StringBuffer인스턴스에 대해 **연속적으로 append()를 호출하는 것이 가능하다.**
  - StringBuffer클래스에는 append() 말고도 객체 자신을 반환하는 메서드들이 많다. 

```java
StringBuffer sb = new StringBuufer("abc");
sb.append("123");
sb.append("zz");

-->

StringBuffer sb = new StringBuufer("abc");
sb.append("123").append("ZZ");
```

### StringBuffer의 비교

- `String클래스` 에서는 **equals메서드를 오버리이딩** 해서 **문자열의 내용을 비교** 하도록 구현되어 있지만,
- `StringBuffer클래스` 는 **equals메서드를 오버라이딩하지 않아서** StringBuffer클래스의 equals메서드를 사용해도 **등가비교연산자(==)로 비교한 것과 같은 결과** 를 얻는다.

반면에 **toString()** 은 **오버라이딩되어 있어서** StringBuffer인스턴스에 toString()을 호출하면, **담고있는 문자열을 String으로 반환한다.**

StringBuffer 인스턴스에 담긴 문자열을 비교하기 위해서는 StringBuffer인스턴스에 toString()을 호출해서 String인스턴스를 얻은 다음, 여기에 equals메서드를 사용해서 비교해야 한다.

```java
StringBuffer sb  = new StringBuffer("abc");
StringBuffer sb2 = new StringBuffer("abc");

System.out.println("sb == sb2 ? " + (sb == sb2)); // false
System.out.println("sb.equals(sb2) ? " + sb.equals(sb2)); // false
		
// StringBuffer의 내용을 String으로 변환한다.
String s  = sb.toString();	// String s = new String(sb);와 같다.
String s2 = sb2.toString();

System.out.println(s.equals(s2)); // true
```

### StringBuffer클래스의 생성자와 메서드

![StringBuffer메서드1](https://user-images.githubusercontent.com/56071088/125239378-8afb0800-e323-11eb-8139-4556f215f09b.png)

- StringBuffer의 생성자
  - 16개의 문자를 담을 수 있는 버퍼를 가진 StringBuffer인스턴스를 생성 : new StringBuffer(); 
  - 길이(length) : new StringBuffer(int length);
  - 문자열(String str) : new StringBuffer(String str);  

```java
StringBuffer sb1 = new StringBuffer();
StringBuffer sb2 = new StringBuffer(10);
StringBuffer sb3 = new StringBuffer("Hi");
```

![StringBuffer메서드2](https://user-images.githubusercontent.com/56071088/125239379-8c2c3500-e323-11eb-9c92-0b79d02c5da1.png)

## StringBuilder란?

`StringBuffer` 는 **멀티쓰레드(Multi-thread) 에 안전(thread-safe)** 하도록 **동기화** 되어 있다.
- 동기화가 성능을 떨어뜨린다.
- 멀티쓰레드로 작성된 프로그램이 아닌 경우, StringBuffer의 동기화는 불필요하게 성능만 떨어뜨리게 된다.

`StringBuilder` 는 **StringBuffer에서 쓰레드의 동기화만 뺀 것이다.**
- **StringBuilder는 StringBuffer와 완전히 똑같은 기능으로 작성되어 있다.**

지금까지(9장) 예제들은 모두 싱글 쓰레드(single-thread)로 작성된 것이다.

![String,StringBuffer,StringBuilder](https://user-images.githubusercontent.com/56071088/125493566-7032daea-2464-45c1-9135-731da2d634da.png)


## Math클래스

Math클래스의 생성자는 접근제어자가 private이기 때문에 다른 클래스에서 Math인스턴스를 생성하지 못하도록 되어 있다.
- 이유는 클래스 내에 인스턴스 변수가 하나도 없어서 인스턴스를 생성할 필요가 없기 때문이다.
- Math클래스의 메서드는 모두 static이며, 2개의 상수만 정의해 놓았다.

```java
public static final double E = 2.718281828... // 자연로그의 밑
public static final double PI = 3.141592....  // 원주율
```

### 올림, 버림, 반올림

소수점 n번째 자리에서 반올림한 값을 얻기 위해서는 round()를 사용해야 하는데, 
 
**round()** 는 **항상 소수점 첫째자리에서 반올림** 을 해서 **정수값(long)을 결과** 로 돌려준다.

소수점 둘째 자리까지의 값을 원할 때,

  1. 원래 값에 100을 곱한다
       
	- 90.7552 * 100 → 9075.52
  
  2. 결과에 Math.round() 사용
    
	- Math.round(9075.52) → 9076
  
  3. 결과에 다시 100.0으로 나눈다.
    
	- 9076 / 100.0 → 90.76
    - 9076 / 100 → 90

- 반올림된 소수점 셋째자리가 필요하다면 100대신 1000을 곱하고 1000.0으로 나눈다.

![math결과값](https://user-images.githubusercontent.com/56071088/125242305-7b7dbe00-e327-11eb-9b86-19d3ac3e1cce.png)

- **rint()** 도 round()처럼 **소수 첫째 자리에서 반올림** 이지만, **반환 값이 double이다.**
  - **rint()** 는 **두 정수의 정가운데 있는 값** 은 **짝수 정수**를 반환한다.
  - `round(-1.5)` : `-1` vs `rint(-1.5)` : `-2`

### 예외를 발생시키는 메서드

메서드 이름에 `Exact` 가 붙은 메서드들이 **JDK 1.8** 부터 새로 추가되었다.
- **정수형 간에 오버플로우(overflow)** 를 감지하기 위한 것이다.

```java
int addExact(int x, int y)       // x + y
int subtractExac(int x, int y)   // x - y
int multiplyExact(int x, int y)  // x * y
int incrementExact(int a)        // a++
int decrementExact(int a)        // a--
int negateExact(int a)           // -a
int toIntExact(long value)       // (int)value
```

위의 메서드들은 오버플로우(overflow) 가 발생하면, **예외(ArithmeticException)** 를 발생시킨다.

negateExact(int a)는 부호를 반대로 바꿔주는 메서드인데 어떤 예외가 발생할까?
- 부호를 반대로 바꾸는 식은 '~a+1'이라 오버플로우가 발생할 수 있다.

```java
import static java.lang.Math.*;
import static java.lang.System.*;

class MathEx2 {
	public static void main(String args[]) {
		int i = Integer.MIN_VALUE;

		out.println("i ="+i);
		out.println("-i="+(-i));

		try {
			out.printf("negateExact(%d)= %d%n",  10, negateExact(10));
			out.printf("negateExact(%d)= %d%n", -10, negateExact(-10));

			out.printf("negateExact(%d)= %d%n", i, negateExact(i)); // 예외발생
		} catch(ArithmeticException e) {
			// i를 long타입으로 형변환 다음에 negateExact(long a)를 호출
		     out.printf("negateExact(%d)= %d%n",(long)i,negateExact((long)i));
		}
	} // main의 끝
}
```

```json
실행결과
i = -2147483648
-i = -2147483648
negateExact(10) = - 10
negateExact(-10) = 10
negateExact(-2147483648) = 2147483648
```

변수 i에 부호연산자로 부호를 반대로 바꾸어도 값이 그대로인 이유
  - **정수형의 최소값에 비트전환연산자 '~'를 적용하면, 최대값이 되는데 여기에 1을 더하니까 오버플로우가 발생하는 것이다.**
  
  - 1000000 00000000 00000000 0000000 → int의 최소값
  - ~ 연산 → 01111111 11111111 11111111 11111111 → int의 최대값
  - +1 → 1000000 00000000 00000000 0000000 → 다시 int의 최소값

### 삼각함수와 지수, 로그

<!-- $\sqrt{}$ -->

- 두 점 (x1, y1) (x2, y2)간의 거리 c는 $\sqrt{(x2-x1)^2+(y2-y1)^2}$
  - 제곱근을 계산해주는 sqrt()와 n제곱을 계산해주는 pow()를 사용

```java
double c = sqrt(pow(x2-x1, 2) + pow(y2-y1, 2));
```

- 삼각함수

```java
Math.sin(Math.PI/4);  // 45 degree
Math.cos(Math.PI/4);
Math.cos(Math.toRadians(45));
```

- 상용로그

```java
Math.log10(x);
```

### StrictMath클래스

**Math클래스** 는 최대한의 성능을 얻기 위해 **JVM이 설치된** **`OS`** 의 **메서드를 호출** 해서 사용한다.
- OS에 의존적인 계산을 한다.
- 컴퓨터의 OS가 다를 경우, 컴퓨터마다 결과가 다를 수 있다.

이러한 차리를 없애기 위해 **성능을 포기하는 대신** , **어떤 OS에서 실행되어도 항상 같은 결과** 를 얻도록 Math클래스를 새로 작성한 것이 `StrictMath클래스` 이다.

### Math클래스의 메서드

![Math클래스 메서드](https://user-images.githubusercontent.com/56071088/125245734-df09ea80-e32b-11eb-8d4b-54178251ab63.png)

## 래퍼(Wrapper) 클래스

객체지향 개념에서 모든 것은 객체로 다루어져야 한다.

- **그러나 Java 에서는 8개의 기본형을 객체로 다루지 않는데** 이것이 바로 **Java가 완전한 객체지향 언어가 아니라는 얘기를 듣는 이유** 이다.
- 그 대신 보다 높은 성능을 얻을 수 있었다.

**`래퍼(Wrapper) 클래스`** : 기본형 값을 객체로 다룰 수 있다. 8개의 기본형을 대표하는 8개의 래퍼 클래스.

- 래퍼 클래스의 생성자는 매개변수로 문자열이나 각 자료형의 값들을 인자로 받는다. 
  - **매개변수로 문자열** 을 제공할 때, 각 자료형에 알맞은 문자열을 사용해야 한다는 것이다.
  - 'new Integer("1.0");' -> NumberFormatException발생 

```java
public final class Integer extends Number Implements Comparable {
	...
	private int value;
	...
}
```

![Wrapperclass](https://user-images.githubusercontent.com/56071088/125252696-9d7d3d80-e333-11eb-9aef-e4e59680221e.png)


```java
class WrapperEx1 {
	public static void main(String[] args) {
		Integer i  = new Integer(100);
		Integer i2 = new Integer(100);

		System.out.println("i==i2 ? "+(i==i2));
		System.out.println("i.equals(i2) ? "+i.equals(i2));
		System.out.println("i.compareTo(i2)="+i.compareTo(i2));
		System.out.println("i.toString()="+i.toString());

		System.out.println("MAX_VALUE="+Integer.MAX_VALUE);
		System.out.println("MIN_VALUE="+Integer.MIN_VALUE);
		System.out.println("SIZE=" +Integer.SIZE+" bits");
		System.out.println("BYTES="+Integer.BYTES+" bytes");
		System.out.println("TYPE=" +Integer.TYPE);
	}
}
```

```json
실행결과
i==i2 ? false
i.equals(i2) ? true
i.compareTo(i2) = 0
i.toString()=100
MAX_VALUE=2147483647
MIN_VALUE=-2147483648
SIZE=32 bits
BYTE+4 bytes
TYPE=int
```

- 래퍼 클래스들은 **모두 equals()가 오버라이딩 되어 있어서** 주소값이 아닌 객체가 가지고 있는 **값을 비교**
    - 오토박싱이 된다고 해도 **Integer 객체** 에 비교연산자를 사용 x, 대신 **compareTo()를 사용** 해야 한다.
       
- **toString()** 도 **오버라이딩 되어있다.**
- 래퍼클래스들은  **MAX_VALUE, MIN_VALUE, SIZE, BYTES, TYPE** 등의 **static 상수를 공통적** 으로 가지고 있다.

### Number클래스

![Number클래스](https://user-images.githubusercontent.com/56071088/125253704-a7ec0700-e334-11eb-8814-01ae38042046.png)

`Number클래스` : **추상클래스** 로 내부적으로 숫자를 멤버변수로 갖는 **래퍼클래스들의 조상** 이다.
- 기본형 중에서 숫자와 관련된 래퍼 클래스들은 모두 **Number클래스의 자손이다.**

- BigInteger는 long으로도 다룰 수 없는 큰 범위의 정수
- BigDecimal은 decimal로도 다룰 수 없는 큰 범위의 부동 소수좀을 처리하기 위한 것

```java
public abstract class Number implements java.io.Serializable {
	public abstract int intValue();
	public abstract long longValue(); 
	public abstract float floatValue(); 
	public abstract double doubleValue(); 


	public byte byteValue() {
		return (byte) intValue();
	}

	public short shortValue() {
		return (short) intValue();
	}
} 
```

### 문자열을 숫자로 변환하기

문자열을 숫자로 변환하는 다양한 방법

```java
int i = new Integer("100").intValue(); // floatValue(), longValue()..
int i2 = Integer.parseInt("100");      // 주로 이 방법을 많이 사용
Integer i3 = Integer.valueOf("100");
```

- `Integer.parseInt(String s)` 형식의 메서드는 **기본형** 반환
  
- `Integer.valueOf()` 메서드는 **래퍼 클래스** 타입 반환

- **JDK 1.5** 부터 도입된 `오토박싱(auto-boxing)` 기능 때문에 **반환값** 이 **기본형** 일 때와 **래퍼 클래스일 때** 의 **차이점이 없어졌다.**
  - 그래서 그냥 구별 없이 valueOf()를 쓰는 것도 괜찮은 방법이지만,
  - **성능은 valueOf()가 조금 더 느리다.** 

```java
int i1 = Integer.parseInt("100");
long l1 = Long.parseLong("100");

-->

int i2 = Integer.valueOf("100");
long l2 = Long.valueOf("100");
```

- 문자열이 10진수가 아닌 **다른 진법의 숫자** 일 때도 **변환이 가능하다.**

```java
static int parseInt(String s, int radix) // 문자열 s를 radix 진법으로 인식
static Integer valueOf(String s, int radix)
```

```java
class WrapperEx2 {
	public static void main(String[] args) {
		int		 i  = new Integer("100").intValue();
		int		 i2 = Integer.parseInt("100");
		Integer  i3 = Integer.valueOf("100");

		int i4 = Integer.parseInt("100",2);
		int i5 = Integer.parseInt("100",8);
		int i6 = Integer.parseInt("100",16);
		int i7 = Integer.parseInt("FF", 16);
//	int i8 = Integer.parseInt("FF");  // NumberFormatException 발생

		Integer i9 = Integer.valueOf("100",2);
		Integer i10 = Integer.valueOf("100",8);
		Integer i11 = Integer.valueOf("100",16);
		Integer i12 = Integer.valueOf("FF",16);
//  Integer i13 = Integer.valueOf("FF"); // NumberFormatException 발생

		System.out.println(i);
		System.out.println(i2);
		System.out.println(i3);
		System.out.println("100(2) -> "+i4);
		System.out.println("100(8) -> "+i5);
		System.out.println("100(16)-> "+i6);
		System.out.println("FF(16) -> "+i7);

		System.out.println("100(2) -> "+i9);
		System.out.println("100(8) -> "+i10);
		System.out.println("100(16)-> "+i11);
		System.out.println("FF(16) -> "+i12);
	}
}
```

```json
실행결과
100
100
100
100(2) -> 4
100(8) -> 64
100(16) -> 256
FF(16) -> 255
100(2) -> 4
100(8) -> 64
100(16) -> 256
FF(16) -> 255
```

### 오토박싱 & 언박싱(autoboxing & unboxing)

**JDK 1.5** 이전에는 기본형과 참조형 간의 연산이 불가능했기 때문에, 래퍼 클래스로 기본형을 객체로 만들어서 연산해야 했다.

그러나 이제는 **기본형과 참조형 간의 덧셈이 가능!!**
- Java언어의 규칙이 바뀐 것은 아니고, 컴파일러가 자동으로 변환하는 코드를 넣어주기 때문이다.

```java
컴파일 전의 코드

int i = 5;
Integer iObj = new Integer(7);
int sum i + iObj;  // error, jdk 1.5이전

컴파일 후의 코드

int i = 5;
Integer iObj = new Integer(7);
int sum i + iObj.intValue();  
```

**`오토박싱(auto-boxing)`** : 기본형 값을 래퍼 클래스의 객체로 자동 변환해주는 것 : **기본형 -> 래퍼 클래스**

**`언박싱(unboxing)`** : **래퍼 클래스 -> 기본형**

- 내부적으로 객체 배열을 가지고 있는 Vector클래스, ArrayList클래스에 기본형 값을 저장해야할 때나 형변환이 필요할 때도 **컴파일러가 자동적으로 코드를 추가해 준다.**

```java
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(10);  // 오토박싱, 10 -> new Integer(10)

int value = list.get(0); // 언박싱, new Integer(10) -> 10
```

```java
class WrapperEx3 {
	public static void main(String[] args) {
		int i = 10;

        // 기본형을 참조형으로 형변환(형변환 생략가능)
		Integer intg = (Integer)i; // Integer intg = Integer.valueOf(i);
		Object   obj = (Object)i;  // Object obj = (Object)Integer.valueOf(i);

		Long     lng = 100L;  // Long lng = new Long(100L);

		int i2 = intg + 10;   // 참조형과 기본형간의 연산 가능
		long l = intg + lng;  // 참조형 간의 덧셈도 가능

		Integer intg2 = new Integer(20);
		int i3 = (int)intg2;  // 참조형을 기본형으로 형변환도 가능(형변환 생략가능)

		Integer intg3 = intg2 + i3; 

		System.out.println("i     ="+i);
		System.out.println("intg  ="+intg);
		System.out.println("obj   ="+obj);
		System.out.println("lng   ="+lng);
		System.out.println("intg + 10  ="+i2);
		System.out.println("intg + lng ="+l);
		System.out.println("intg2 ="+intg2);
		System.out.println("i3    ="+i3);
		System.out.println("intg2 + i3 ="+intg3);
	}
}

실행결과
i = 10
intg = 10
obj = 10
lng = 10
intg + 10 = 20
intg + lng = 110
intg2 = 20
i3 = 20
intg2 + i3 = 40
```

```java
컴파일러가 자동 코드 변경

Integer intg = (Integer)i; -> Integer intg = Integer.valueOf(i);
Object obj = (Object)i; -> Object obj = (Object)Intger.valueOf(i);
Long lng = 100L; -> Long lng = new Long(100L);
```

## java.util.Objects 클래스

`Objects클래스` : Object클래스의 보조 클래스로 Math클래스처럼 모든 메서드가 **static** 이다.
- 객체의 비교나 null체크에 유용하다.

### isNull(), nonNull(), requireNonNull()

`isNull()` : **해당 객체가 null인지 확인** 해서 null이면 true, 아니면 false를 반환

`nonNull()` : isNUll()의 반대 : !Objects.isNull(obj)

`requireNonNull()` : 해당 객체가 null이 아니어야 할 경우 사용
- 만약 겍체가 null이라면, NullPointerException을 발생시킨다.

```java
// isNull()
static boolean isNull(Object obj)

// nonNull()
static boolean nonNull(Object obj)

// requireNonNull()
static <T> T requireNonNull(T obj)
static <T> T requireNonNull(T obj, String message) 
static <T> T requireNonNull(T obj, Supplier<String> meassageSupplier) 
```

- **T 는 Object 타입** 을 의미한다.

```java
void setName(String name) {
	if(name == null)
		throw new NullPointerException("name must not be null.");
	this.name = name;
}

-->

void setName(String name) {
	this.name = Object.requireNonNull(name, "name must not be null.");
}
```

- 예전 같으면 **매개변수의 유효성 검사** 를 if문으로 해야 하는데, 이제는 **requireNonNUll() 의 호출 만으로 해결할 수 있다.**

### compare(), equals(), deepEquals()

`compare()` : **a와 b 두 객체를 비교, 두 비교대상이 같으면 0, 크면 양수, 작으면 음수 를 반환**
- **비교 기준**이 필요하므로 **Comparator을 사용**한다.

`equals()` : Object클래스의 equals() 메서드와 달리 **null검사를 하지 않아도 된다!!**
- equals() 의 내부에서 null검사를 해주기 때문에 null검사를 위한 조건식이 필요 없다.
- **단, a와 b가 모두 null인 경우 true를 반환한다!!** 

`deepEquals()` : **객체를 재귀적으로 비교** 하기 때문에 **다차원 배열의 비교도 가능** 하다.
- 2개의 2차원 문자열 배열을 비교할 때,
- equals()는 반복문과 함께 사용해야 하는데,
- deepEquals()를 사용하면 간단하다.

```java
// compare()
static int compare(Object a, Object b, Comparator c)

// equals()
public static boolean equals(Object a, Object b) {
		return (a == b) || (a != null && a.equals(b));
}
if (a!=null && a.eqauls(B)) {} // a가 널인지 반드시 확인해야함
if (Objects.equals(a, b)) {} // 매개변수의 값이 null인지 확인할 필요가 없다.

// deepEquals()
String[][] str2D = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};
String[][] str2D2 = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};

System.out.println(Objects.equals(str2D, str2D2));  // false
System.out.println(Objects.deeqEquals(str2D, str2D2));  // true
```

### toString()

`toString()` : **Object클래스의 toSting() 메서드** 에 **내부적으로 null 검사를 더한 것**
- 두 번째 메서드는 o가 null일 때, 대신 사용할 값을 지정할 수 있다.

```java
static String toString(Object o)
static String toString(Object o, String nullDefault)
```

### hashCode()

`hashCode()` : **Object클래스의 hashCode() 메서드** 에 **내부적으로 null 검사를 더한 것**
- **단, null일 때는 0을 반환** 한다.
- 보통은 클래스에 선언된 인스턴스의 변수들의 hashCode())를 조합해서 반환하도록 hashCode()를 오버라이딩 하는데, 그 대신 매개변수의 타입이 가변인자인 두 번째 메서드를 사용하면 편리하다
- 11장에서 다시 설명

```java
static int hashCode(Object o)
static int hashCode(Object... vlaues)
```


```java
import java.util.*;
import static java.util.Objects.*;  // Objects 클래스의 메서드를 static import

class ObjectsTest {
    public static void main(String[] args) {
    	String[][] str2D   = new String[][]{{"aaa","bbb"},{"AAA","BBB"}};
    	String[][] str2D_2 = new String[][]{{"aaa","bbb"},{"AAA","BBB"}};

    	System.out.print("str2D  ={");
    	for(String[] tmp : str2D) 
    			System.out.print(Arrays.toString(tmp));
    	System.out.println("}");

    	System.out.print("str2D_2={");
    	for(String[] tmp : str2D_2) 
    		System.out.print(Arrays.toString(tmp));
    	System.out.println("}");

    	System.out.println("equals(str2D, str2D_2)="+Objects.equals(str2D, str2D_2));
    	System.out.println("deepEquals(str2D, str2D_2)="+Objects.deepEquals(str2D, str2D_2));

    	System.out.println("isNull(null) ="+isNull(null));
    	System.out.println("nonNull(null)="+nonNull(null));
    	System.out.println("hashCode(null)="+Objects.hashCode(null));
    	System.out.println("toString(null)="+Objects.toString(null));
    	System.out.println("toString(null, \"\")="+Objects.toString(null, ""));
    	
    	Comparator c = String.CASE_INSENSITIVE_ORDER;  // 대소문자 구분안하는 비교
    			   
        System.out.println("compare(\"aa\",\"bb\")="+compare("aa","bb",c));
    	System.out.println("compare(\"bb\",\"aa\")="+compare("bb","aa",c));
    	System.out.println("compare(\"ab\",\"AB\")="+compare("ab","AB",c));
    }
}
```

```json
실행결과
str2D = {[aaa, bbb][AAA, BBB]}
str2D_2 = {[aaa, bbb][AAA, BBB]}

equals(str2D, str2D_2)=false
deepEqulas(str2D, str2D_2)=true
    
isNull(null)=true
nonNull(null)=false
    
hashCode(null)=0

toString(null)=null
toString(null, "")=
    
compare("aa", "bb")=-1
compare("bb", "aa")=1
compare("ab", "AB")=0
```

- static import문을 사용해도 Object 클래스의 메서드와 **이름이 같은 것들은 충돌이 난다.**
  - **컴파일러가 구분을 못하는 것**
  
  - **클래스의 이름을 붙여줄 수 밖에 없다.**
  
- String 클래스에 상수로 정의되어 있는 Comparator가 있어서 그것을 사용하여 compare()를 호출했다.
- 이 Comparator는 문자열을 대소문자 구분하지 않고 비교할 때 사용하기 위한 것
  - "ab"와 "AB"를 비교한 결과가 0, 즉 두 문자열이 같다는 결과가 나온다. 

```java
Comparator c = String.CASE_INSENSITIVE_ORDER;  // 대소문자 구분안하는 비교
...
System.out.println("compare(\"ab\",\"AB\")=" + compare("ab","AB",c));
```

## java.util.Random클래스

**난수(random number)** 를 얻는 방법

1. **Math.random()**
2. **Random클래스**

- **Math.random()** 은 **내부적으로 Random클래스의 인스턴스를 생성해서 사용** 하는 것으로 둘 중에서 편한 것을 사용하면 된다.

```java
double randNum = Math.random();
double randNum = new Random().nextDouble(); // 위 문장과 동일

// 1~6사이의 정수를 난수로 얻고자 할 때
int num = (int)(Math.random() * 6) + 1;
int num = new Random().nextInt(6) + 1; // nextInt(6): 0~6
```

#### Math.random()과 Random클래스의 차이점

Random클래스는 종자값(seed)를 설정할 수 있다.
- 종자값(seed)이 같은 Random인스턴스들은 항상 같은 난수를 같은 순서대로 반환한다.
- **`종자값(seed)`** : 난수를 만드는 공식에 사용되는 값
- **같은 종자값 == 같은 난수**

### Random클래스의 생성자와 메서드

- **생성자 Random()** 은 `종자값(seed)`을 `System.currentTimeMillis()` 로 하기 때문에 실행할 때마다 얻는 난수가 달라진다.
  - currentTimeMillis() : 현재시간을 천분의 1초단위로 변환해서 반환한다.   

```java
public Random() {
	this(System.currentTimeMillis()); // Random(long seed)를 호출
}
```

- nextBytes()는 BigInteger(int signum, byte[] magnitude)와 함께 사용하면 int의 범위인 '-2^31 ~ 2^31 - 1'보다 넓은 범위의 난수를 얻을 수 있다.

![Random클래스 메서드](https://user-images.githubusercontent.com/56071088/125324726-1f448980-e37b-11eb-9a9e-6e4c9efc71db.png)

자주 사용되는 코드는 메서드로 만들어 놓으면 여러모로 도움이 된다.

**int[] fillRand(int[] arr, int from, int to)**
- 배열 arr을 from과  to범위의 값들로 채워서 반환한다.
  
**int[] fillRand(int[] arr, int[] data)**
- 배열 arr을 배열 data에 있는 값들로 채워서 반환한다.

**int getRand(int from, int to)**
- from과 to를 포함하는 범위의 정수값을 반환한다.

```java
import java.util.*;

class RandomEx3 {
	public static void main(String[] args) {
		for(int i=0; i < 10;i++)
			System.out.print(getRand(5, 10)+",");
		System.out.println();

		int[] result = fillRand(new int[10], new int[]{ 2, 3, 7, 5});

		System.out.println(Arrays.toString(result));
	}

	public static int[] fillRand(int[] arr, int from, int to) {
		for(int i=0; i < arr.length; ++i) {
			arr[i] = getRand(from, to);
		}
 
		return arr;
	}

	public static int[] fillRand(int[] arr, int[] data) {
		for(int i = 0; i < arr.length; ++i) {
			arr[i] = data[getRand(0, data.length-1)];
		}

		return arr;
	}

	public static int getRand(int from, int to) {
		return (int)(Math.random() * (Math.abs(to-from)+1)) + Math.min(from, to);
	}
}
```

## 정규식(Regular Expression) - java.util.regex패키지

`정규식` : 텍스트 데이터 중에서 원하는 조건(패턴, pattern)과 일치하는 문자열을 찾아내기 위해 사용하는 것
- 미리 정의된 기호와 문자를 이용해서 작성한 문자열

장점 : 
1. 많은 양의 텍스트 파일 중에서 원하는 데이터를 손쉽게 뽑아낼 수 있다.
2. 입력된 데이터가 형식에 맞는지 체크할 수 있다.
   - ex)
   - html문서에서 전화번호나 이메일 주소만을 따로 추출한다.
   - 입력한 비밀번호가 숫자와 영문자의 조합으로 되어있는지 확인할 수도 있다.

Java API문서에서 java.util.regex.Pattern을 찾아보면 정규식에 사용되는 기호와 작성방법이 모두 설명되어 있다.

정규식을 자세히 설명하자면 책 한 권 분량이 될 정도로 광범위하기 때문에 깊이 있게 학습하는 것보다는 자주 쓰이는 정규식의 작성 예를 보고 응용할 수 있을 정도까지만 학습하자.

```java
import java.util.regex.*;	// Pattern과 Matcher가 속한 패키지

class RegularEx1 {
	public static void main(String[] args) 
	{
		String[] data = {"bat", "baby", "bonus",
				    "cA","ca", "co", "c.", "c0", "car","combat","count",
				    "date", "disc"};		
		Pattern p = Pattern.compile("c[a-z]*");	// c로 시작하는 소문자영단어

		for(int i=0; i < data.length; i++) {
			Matcher m = p.matcher(data[i]);
			if(m.matches())
				System.out.print(data[i] + ",");
		}
	}
}
```
```json
실행결과
ca,co,car,combat,count,
```

`Pattern` : **정규식을 정의**

`Matcher` : **정규식(pattern)을 데이터와 비교하는 역할**

1. 정규식을 매개변수로 Pattern클래스의 static메서드인 **Pattern compile(String regex)** 을 호출하여 **Pattern인스턴스** 를 얻는다.
   - **Pattren p = Pattern.compile("c[a-z]*");**

2. 정규식을 비교할 대상을 매개변수로 **Pattern클래스의 Matcher matcher(CharSequence input)** 를 호출해서 **Matcher인스턴스** 를 얻는다.
   - **Matcher m = p.matcher(data[i]);**

3. Matcher인스턴스에 **boolean matches()** 를 호출해서 **정규식에 부합하는지 확인** 한다.
   - **if(m.matches())**

- CharSequence 는 **인터페이스** 로, 이를 구현한 **클래스** 는 **CharBuffer, String, StringBuffer** 가 있다.

```java
import java.util.regex.*;	// Pattern과 Matcher가 속한 패키지

class RegularEx2 {
	public static void main(String[] args) {
		String[] data = {"bat", "baby", "bonus", "c", "cA",
				        "ca", "co", "c.", "c0", "c#",
					"car","combat","count", "date", "disc"
						};		
		String[] pattern = {".*","c[a-z]*","c[a-z]", "c[a-zA-Z]",
                                          "c[a-zA-Z0-9]","c.","c.*","c\\.","c\\w",
                                          "c\\d","c.*t", "[b|c].*", ".*a.*", ".*a.+",
                                          "[b|c].{2}"
                                         };

		for(int x=0; x < pattern.length; x++) {
			Pattern p = Pattern.compile(pattern[x]);
			System.out.print("Pattern : " + pattern[x] + " 결과: ");
			for(int i=0; i < data.length; i++) {
				Matcher m = p.matcher(data[i]);
				if(m.matches())
					System.out.print(data[i] + ",");
			}
			System.out.println();
		}
	}  
}
```

```json
실행결과
Pattern : .*  결과: bat,baby,bonus,c,cA,ca,co,c.,c0,c#,car,combat,count,date,disc,
Pattern : c[a-z]*  결과: c,ca,co,car,combat,count,
Pattern : c[a-z]  결과: ca,co,
Pattern : c[a-zA-Z]  결과: cA,ca,co,
Pattern : c[a-zA-Z0-9]  결과: cA,ca,co,c0,
Pattern : c.  결과: cA,ca,co,c.,c0,c#,
Pattern : c.*  결과: c,cA,ca,co,c.,c0,c#,car,combat,count,
Pattern : c\.  결과: c.,
Pattern : c\w  결과: cA,ca,co,c0,
Pattern : c\d  결과: c0,
Pattern : c.*t  결과: combat,count,
Pattern : [b|c].*  결과: bat,baby,bonus,c,cA,ca,co,c.,c0,c#,car,combat,count,
Pattern : .*a.*  결과: bat,baby,ca,car,combat,date,
Pattern : .*a.+  결과: bat,baby,car,combat,date,
Pattern : [b|c].{2}  결과: bat,car,
```

![regex패턴](https://user-images.githubusercontent.com/56071088/125405449-b862b700-e3f2-11eb-8ace-55d03fd16f5f.png)

![정규식1](https://user-images.githubusercontent.com/56071088/125419004-7136dcc3-a89d-4217-973f-c8f81b33db26.png)

![정규식2](https://user-images.githubusercontent.com/56071088/125419007-6ca19588-78c3-4669-bf9d-51ca3b2db69e.png)

![정규식3](https://user-images.githubusercontent.com/56071088/125419010-25775641-c007-44be-bf8e-e67e742a7c38.png)


### 그룹화(grouping)

**정규식의 일부** 를 **괄호** 로 나누어 묶어서 **그룹화(grouping)** 할 수 있다.

- **그룹화된 부분** 은 하나의 단위로 묶이는 셈이 되어서 한 번 또는 그 이상의 반복을 의미하는 **'+' 나 '*'** 가 뒤에 오면 그룹화된 부분이 적용대상이 된다. 
- 그룹화된 부분은 **group(int i)** 를 이용해서 나누어 얻을 수 있다.
- **group(), group(0)** : 그룹으로 매칭된 문자열을 **전체를 나누어지지 않은 채로 반환** 한다.
  - group(int i)를 호출할 때 i가 실제 그룹의 수보다 많으면 java.lang.IndexOutOfBoundException이 발생한다. 

|정규식 패턴|설명|
|---|---|
|0\\\\d{1,2}|0으로 시작하는 최소 2자리 최대 3자리 숫자(0포함)|
|\\\\d{3,4}|최소 3자리, 최대 4자리의 숫자|
|\\\\d{4}|4자리의 숫자|

```java
import java.util.regex.*;	//

class RegularEx3{
	public static void main(String[] args) {
		String source  = "HP:011-1111-1111, HOME:02-999-9999 ";
		String pattern = "(0\\d{1,2})-(\\d{3,4})-(\\d{4})";

		Pattern p = Pattern.compile(pattern);
		Matcher m = p.matcher(source);

		int i=0;

		while(m.find()) {
			System.out.println( ++i + ": " + m.group() + " -> "+ m.group(1) +", "+ m.group(2)+", "+ m.group(3));		
		}
	} // main()
}
```

```json
실행결과
1: 011-1111-1111 -> 011, 1111, 1111
2: 02-999-9999 -> 02, 999, 9999
```

### find()

`find()` : **주어진 소스 내에서 패턴과 일치하는 부분**을 **찾아내면 true** 를 반환하고, **찾지 못하면 false를 반환**한다.
- find()를 호출해서 패턴과 일치하는 부분을 찾아낸 다음, 다시 find()를 호출하면 이전에 발견한 패턴과 **일치하는 부분의 다음부터 다시 패턴매칭** 을 시작한다.

```java
Matcher m = p.matcher(source);

while(m.find()) { // find()는 일치하는 패턴이 없으면 false를 반환한다.
	System.out.println(m.group());
}
```

Matcher의 find()로 정규식과 일치하는 부분을 찾으면, 
-  그 **위치**를 **start()** 와 **end()** 로 알아낼 수 있다. : m.start(), m.end() 
-  **appendReplacement(StringBuffer, sb, String replacement)** 를 이용해서 **원하는 문자열(String replacement)로 치환** 할 수 있다 : m.appendReplacement(sb, "drunken");
-  **치환된 결과** 는 **StringBuffer인 sb 에 저장된다.**

```java
import java.util.regex.*;	// Pattern과 Matcher가 속한 패키지

class RegularEx4 {
	public static void main(String[] args) {
		String source  = "A broken hand works, but not a broken heart.";
		String pattern = "broken";

		StringBuffer sb = new StringBuffer();

		Pattern p = Pattern.compile(pattern);
		Matcher m = p.matcher(source);
		System.out.println("source:"+source);

		int i=0;

		while(m.find()) {
			System.out.println(++i + "번째 매칭:" + m.start() + "~"+ m.end());

            // broken을 drunken으로 치환하여 sb에 저장한다.
			m.appendReplacement(sb, "drunken");  
		}

		m.appendTail(sb);
		System.out.println("Replacement count : " + i);
		System.out.println("result:"+sb.toString());
	}
}
```

```json
실행결과
source:A broken hand works, but not a broken heart.
1번째 매칭:2~8
2번째 매칭:31~37
Replacement count : 2
result: A drunken hand works, but not a drunken heart.
```

1. 문자열 source에서 broken을 m.find()로 찾은 후 m.appendReplacement()가 호출되면 source의 시작부터 broken을 찾은 위치까지의 내용에 drunken을 더해서 저장한다.
    - sb에 저장된 내용: "A drunken"
2. m.find()는 첫 번째로 발견된 위치의 끝에서부터 다시 검색을 시작하여 두 번째 "broken"을 찾게 된다. 다시 m.appendReplacement()가 호출
    - sb에 저장된 내용: "A drunken hand works, but not a drunken"
3. m.appendTail(sb);이 호출되면 마지막으로 치환된 이후의 부분을 sb에 덧붙인다.
    - sb에 저장된 내용: "A drunken hand works, but not a drunken heart"

## java.util.Scanner클래스

`Scanner` : **화면, 파일, 문자열** 과 같은 **입력소스** 로부터 **문자데이터**를 읽어오는데 도움을 줄 목적으로 **JDK 1.5** 부터 추가되었다.
- 다양한 생성자가 있기 때문에 **다양한 입력소스로부터 데이터를 읽어올 수 있다.**

```java
Scanner(String source)
Scanner(File source)
Scanner(InputStream source)
Scanner(Readable source)
Scanner(ReadableByteChannel source)
Scanner(Path source)  // jdk 1.7부터 추가
```

- Scanner는 **정규식 표현(Regular expression)** 을 이용한 **라인단위의 검색을 지원**
- **구분자(delimiter)** 에도 **정규식 표현** 을 사용할 수 있어서 **복잡한 형태의 구분자도 처리가 가능** 하다.

```java
Scanner useDelimiter(Pattern pattern)
Scanner useDelimiter(String pattern)
```

**JDK 1.6** 부터는 **화면 입출력만 전문적으로 담당** 하는 **java.io.Console** 이 새로 추가되었다.
- **Console클래스, Scanner클래스** 는 **사용법이나 성능 측면에서 거의 같기** 때문에 어떤 것을 사용해도 상관없다.
- Console은 Eclipse와 같은 IDE에서 잘 동작하지 않기 때문에 잘 사용하지 않았다.

```java
// jdk 1.5 이전
BufferReader br = new BufferReader(new InputStreamReader(System.in));
String input = br.readline();

// jdk 1.5이후 (java.util.Scanner)
Scanner s = new Scanner(System.in);
String input = s.nextLine();  // nextInt(), nextLong(), ...

// jdk 1.6 이후 (java.io.Console) - Eclipse에서 동작하지 않는다.
Console console = System.console();
String input = console.readLine();
```

```java
import java.util.Scanner;
import java.io.File;

class ScannerEx3 {
	public static void main(String[] args) throws Exception {
		Scanner sc = new Scanner(new File("data3.txt"));
		int cnt = 0;
		int totalSum = 0;

		while (sc.hasNextLine()) {
			String line = sc.nextLine();
			Scanner sc2 = new Scanner(line).useDelimiter(","); // 구분자
			int sum = 0;

			while(sc2.hasNextInt()) {
				sum += sc2.nextInt();
			}
			System.out.println(line + ", sum = "+ sum);
			totalSum += sum;
			cnt++;
		}
		System.out.println("Line: " + cnt + ", Total: " + totalSum);
	}
}
```

## java.util.StringTokenizer클래스

`StringTokenizer` : **긴 문자열**을 **지정된 구분자(delimiter)** 를 기준으로 **토큰(token)** 이라는 **여러 개의 문자열** 로 잘라내는데 사용된다.

StringTokenizer를 사용하는 방법 외에도 
- String의 split(String regex)
- Scanner의 useDelimiter(String pattern)

이 2가지 방법은 정규식 표현(regular expression)을 사용해야하므로, 

정규식 표현에 익숙하지 않은 경우 : StringTokenizer를 사용하는 것이 간단, 명확하다.
- **StringTokenizer** 는 **구분자** 로 **단 하나의 문자** 밖에 사용하지 못하기 때문에 
- **복잡한 형태의 구분자** 로 문자열을 나누어야 할 때는 **어쩔 수 없이 정규식을 사용하는 메서드를 사용** 해야 할 것이다.

```java
StringTokenizer st = new StringTokenizer("100,200,300,400", ",");

String[] result = "100,200,300,400".split(",");
Scanner sc2 = new Scanner("100,200,300,400").useDelimiter(",");
```

### StringTokenizer의 생성자와 메서드

![StringTokenizer메서드](https://user-images.githubusercontent.com/56071088/125433847-2cc70a13-219d-4441-a5d9-347ba324da3c.png)

```java
import java.util.*; 

class StringTokenizerEx2 { 
	public static void main(String[] args) { 
		String expression = "x=100*(200+300)/2";
		StringTokenizer st = new StringTokenizer(expression, "+-*/=()", true);

		while(st.hasMoreTokens()){
			System.out.println(st.nextToken());
		}
	} // mainÀÇ ³¡
}
```
```json
실행결과
x
-
100
*
(
200
+
300
)
/
2
```

```java
StringTokenizer st = new StringTokenizer(expression, "=-*/=()", true);
```

- "+-*/=()" 전체가 하나의 구분자가 아니라 **각각의 문자가 모두 구분자** 이다.

```java
import java.util.*;

class StringTokenizerEx5 {
	public static void main(String[] args) {
		String data = "100,,,200,300";

		String[] result = data.split(",");
		StringTokenizer st = new StringTokenizer(data, ",");

		for(int i=0; i < result.length;i++)
			System.out.print(result[i]+"|");

		System.out.println("개수:"+result.length);

		int i=0;
		for(;st.hasMoreTokens();i++)
			System.out.print(st.nextToken()+"|");

		System.out.println("개수:"+i);
	} 
}
```

```json
실행결과
100|||200|300|개수:5 <- split()
100|200|300|개수:3 <- StringTokenizer
```

- **split()** 은 **빈 문자열도 토큰으로 인식** 하는 
- 반면 **StringTokenizer** 는 **빈 문자열을 토큰으로 인식하지 않는다.**
  - 인식하는 토큰의 개수가 서로 다르다. 
  
- **split()** 은 데이터를 토큰으로 잘라낸 **결과를 배열에 담아서 반환** 하기 때문에 데이터를 토큰으로 **바로바로 잘라서 반환** 하는 **StringTokenizer보다 성능이 떨어진다.**
  - 그러나, **데이터의 양이 많은 경우가 아니라면 별 문제가 되지 않으므로** 크게 신경 쓸 부분은 아니다.


## java.math.BigInteger클래스

정수형으로 표현할 수 있는 값의 한계가 있다. 

가장 큰 정수형 타입인 `long` 으로도 표현할 수 있는 값은 **10진수로 19자리**  정도이다. 상당히 큰 값이지만 , 과학적 계산에서는 더 큰 값을 다뤄야 할 때가 있다.

`BigInteger` : 내부적으로 int배열을 사용해서 값을 다룬다. 
- 그래서 long타입보다 훨씬 큰 값을 다룰 수 있지만, **성능은 long보다 떨어진다.**
- BigInteger는 String처럼 불변(immutable)이다.
- 모든 정수형이 그렇듯이 값을 `2의 보수` 형태로 표현한다.
  - 부호를 따로 저장, 배열에 값 자체만 저장
  - signum의 값이 -1, 즉 음수인 경우, 2의 보수법에 맞게 mag의 값을 변환해서 처리한다.
  - 부호만 다른 두 값의 mag는 같고 signum은 다르다.

```java
final int signum; // 부호, 1(양수), 0, -1(음수) 셋 중의 하나
final int[] mag; // 값(magnitude)
```

### BigInteger의 생성

`문자열` 로 숫자를 표현하는 것이 가장 일반적이다.
- 정수형 리터럴로는 표현할 수 있는 값의 한계가 있기 때문이다.

```java
BigInteger val;
val = new BigInteger("123456789");    // 문자열로 생성
val = new BigInteger("FFFF", 16);     // n진수의 문자열로 생성
val = BigInteger.valueOf(123456789L); // 숫자로 생성
```

### 다른 타입으로의 변환

- **BigInteger -> 문자열, byte배열**

```java
String toString()           // 문자열로 변환
String toString(int radix)  // 지정된 진법의 문자열로 변환
byte[] toByteArray()        // 숫자로 생성
```

- BigInteger도 `Number`로부터 **상속받은 기본형으로 변환하는 메서드들** 을 갖고 있다.
- `Exact` 가 붙은 것들은 변환한 결과가 변환한 타입의 범위에 속하지 않으면 ArithmeticException 발생

```java
int intValue()
long longValue()
float floatValue()
double doubleValue()

byte byteValueExact()
int intValueExact()
long longValueExact()
```

### BigInteger의 연산

BigInteger는 **불변(Immutable)** 이다. : String과 같다.
- **반환타입이 BigInteger -> 새로운 인스턴스가 반환**
- remainder와 mod는 둘 다 나머지를 구하는 메서드지만, mod는 나누는 값이 음수면 ArithmeticException 발생

```java
BigInteger add(BigInteger val)
BigInteger subtract(BigInteger val)
BigInteger muliply(BigInteger val)
BigInteger divide(BigInteger val)
BigInteger remainder(BigInteger val)
```

### 비트 연산 메서드

성능을 향상시키기 위해 비트 단위로 연산을 수행하는 메서드들을 많이 가지고 있다.

```java
int bitCount()                 // 2진수로 표현했을 때, 1의 개수(음수는 0의 개수)를 반환
int bitLength()                // 2진수로 표현했을 때, 값을 표현하는데 필요한 비트수
boolean testBit(int n)         // 우측에서 n+1번째 비트가 1이면 true, 0이면 false
BigInteger setBit(int n)       // 우측에서 n+1번째 비트를 1로 변경
BigInteger clearBit(int n)     // 우측에서 n+1번째 비트를 0로 변경
BigInteger flipBit(int n)      // 우측에서 n+1번째 비트를 전환 (1->0, 0->1) 
```

- n의 값은 배열의 index처럼 0부터 시작이므로, 우측에서 첫번째 비트는 n이 0이다.

```java
BigInteger bi = new BigInteger();

if(bi.remainder(new BigInteger("2")).equals(BigInteger.ZERO)) {}

-->


```

- **짝수는 제일 오른쪽 bit가 0** 이므로, **testBit(0)** 으로 **마지막 비트를 확인하는 것이 더 효율적** 이다.
- **가능하면 산술연산 대신 비트연산으로 처리하는 것이 더 효울적이다.**

## java.math.BigDecimal클래스

double타입으로 표현할 수 있는 값은 상당히 범위가 넓지만, **정밀도가 최대 13자리** 밖에 되지 않고 **실수형의 특성상 오차를 피할 수 없다.**

`BigDecimal`은 실수형과 달리 **정수를 이용** 해서 **실수를 표현** 한다.
  - 마찬가지로 **불변(Immutable)** 이다.
- 실수의 오차는 10진 실수를 2진 실수로 정확히 변환할 수 없는 경우가 있기 때문에 발생하는 것이므로, **오차가 없는 2진 정수로 변환하여 다루는 것이다.**
  
- 실수를 **정수와 10의 제곱의 곱으로 표현** 한다.
  - 정수 x 10^-scale
  - scale은 0부터 Integer.MAX_VALUE 사이의의 범위에 있는 값이다. 그리고 `BigDecimal` 은 **정수를 저장** 하는데 **BigInteger를 사용** 한다.

```java
private final BigInteger intVal;    // 정수 unscaled value
private final int scale;            // 지수 scale
private transient int precision;    // 정밀도 precision - 정수의 자릿수
```

- 123.45는 12345 x 10^-2로 표현 가능, 
  - 이를 BigDecimal에 저장되면, **intVal : 12345, scale : 2**
- `scale` : **소수점 이하의 자리수**
- `precision` : **정수의 전체 자리수** , 여기서는 5가 된다.

```java
BigDecimal val = new BigDecimal("123.45");
val.unscaledValue(); // 12345
val.scale();         // 2
val.precision();     // 5
```

### BigDecimal의 생성

BigDecimal 역시 BigInteger처럼 **문자열로 숫자를 표현하는 것이 일반적**
- 기본형 리터럴로는 **표현할 수 있는 값의 한계**가 있기 때문이다.

```java
BigDecimal val;
val = new BigDecimal("123,45567");   // 문자열로 생성      
val = new BigDecimal(123.456);       // double 타입의 리터럴로 생성
val = new BigDecimal(123456);        // int, long 타입의 리터럴로 생성가능
val = BigDecimal.valueOf(123.456);   // 생성자 대신 valueOf(double) 사용
val = BigDecimal.valueOf(123456);    // 생성자 대신 valueOf(int) 사용
```

- **double 타입의 값** 을 매개변수로 갖는 생성자를 사용하면 **오차가 발생할 수 있다.**

```java
new BigDecimal(0.1);   // 0.100000000000000000555111...
new BigDecimal("0.1"); // 0.1
```

### 다른 타입으로의 변환

**BigDecimal -> 문자열**

```java
String toPlainString()    // 어떤 경우에도 다른 기호없이 숫자로마 표현
String toString()         // 필요하면 지수형태로 표한할 수도 있음
```

- 대부분의 경우 이 두 메서드의 반환결과가 같지만, BigDecimal을 생성할 때 **지수형태의 리터럴**을 사용했을 때 **다른 결과를 얻는 경우**가 있다.

```java
BigDecimal val = new BigDecimal(1.0e-22);
val.toPlainString()     // 0.00....0010..
val.toString()          // 1.00000...48..5E-22
```

- BigDecimal도 **Number로 부터 상속**받은 **기본형으로 변환하는 메서드들**을 가지고 있다.
  - `Exact`가 붙은 것들은 변환한 결과가 변환한 타입의 범위에 속하지 않으면 **ArithmenticException 발생**

```java
int intValue()
long longValue()
float floatValue()
double doubleValue()

byte byteValueExact()
short shortValueExact()
int intValueExact()
long longValueExact()
BigInteger toBigIntegerExact()
```

### BigDecimal의 연산

- BigDecimal에는 실수형에 사용할 수 있는 모든 연산자와 수학적인 계산을 쉽게 해주는 메서드들이 정의되어 있다.

```java
BigDecimal add(BigDecimal val)
BigDecimal subtract(BigDecimal val)
BigDecimal muliply(BigDecimal val)
BigDecimal divide(BigDecimal val)
BigDecimal remainder(BigDecimal val)
```

- 마찬가지로 불변이므로 반환타입이 BigDecimal인 경우 새로운 인스턴스가 반환된다.
- 한 가지 알아둬야 할 것은 연산결과의 정수, 지수, 정밀도가 달라진다는 것이다.
  - 곱셈에서는 두 피연산자의 scale을 더하고, 나눗셈에서는 뺀다.
  - 덧셈과 뺄셈에서는 둘 중에서 자리수가 높은 쪽으로 맞추기 위해서 두 scale 중에서 큰 쪽이 결과가 된다.

```java
// 123456(value), 3(scale), 6(precision)
BigDecimal bd1 = new BigDecimal("123.456");
// 10(value), 1(scale), 2(precision)
BigDecimal bd2 = new BigDecimal("1.0");
// 1234560(value), 4(scale), 7(precision)
BigDecimal bd3 = bd1.multiply(bd2);
```

### 반올림 모드 - divide(), setScale()

- 다른 연산과 달리 나눗셈을 처리하기 위한 메서드는 다음과 같이 다양한 버젼 존재
- 나눗셈의 결과를 어떻게 반올림(roundingMode)처리할 것인가와, 몇 번째 자리(scale)에서 반올림할 것인지를 지정할 수 있따.
- BigDecimal이 아무리 오차없이 실수를 저장한다해도 나눗셈에서 발생하는 오차는 어쩔 수 없다.

```java
BigDecimal divide(BigDecimal divisor)
BigDecimal divide(BigDecimal divisor, int roundingMode)
BigDecimal divide(BigDecimal divisor, RoundingMode roundingMode)
BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)
BigDecimal divide(BigDecimal divisor, int scale, RoundingMode roundingMode)
BigDecimal divide(BigDecimal divisor, MathContext mc)
```

- roundingMode는 반올림 처리방법에 대한 것으로 BigDecimal에 정의된 'ROUND_'로 시작하는 상수들 중에 하나를 선택해서 사용하면 된다.
- RoundingMode는 이 상수들을 열거형으로 정의한 것으로 나중에 추가된 것이다. 열거형을 사용하자
  - CEILING - 올림
  - FLOOR - 내림
  - UP - 양수일 때는 올림, 음수일 때는 내림
  - DOWN - 양수일 때는 내림, 음수일 때는 올림
  - HALF_UP - 반올림(5이상 올림, 5미만 버림)
  - HALF_EVEN - 반올림(반올림 자리의 값이 짝수면 HALF_DOWN, 홀수면 HALF_UP)
  - HALF_DOWN - 반올림(6이상 올림, 6미만 버림)
  - UNNECESSARY - 나눗셈의 결과가 딱 떨어지는 수가 아니면, ArithmenticException 발생
- 주의할 점은 1.0/3.0 처럼 divide()로 나눗셈한 결과가 무한소수인 경우, 반올림 모드를 지정해주지 않으면 ArithmenticException이 발생

```java
BigDecimal bigd = new BigDecimal("1.0");
BigDecimal bigd2 = new BigDecimal("3.0");

bigd.divide(bigd2); // ArithmenticException 발생
bigd.divide(bigd2, 3, RoundingMode.HALF_UP); // 0.333
```

### java.math.MathContext

- 이 클래스는 반올림 모드와 정밀도를 하나로 묶어 놓은 것일 뿐 별다른 것은 없다.
- 한 가지 주의할 점은 divide()에서는 scale이 소수점 이하의 자리수를 의미하는데, MathContext에서는 precision이 정수와 소수점 이하를 모두 포함한 자리수를 의미한다.

```java
BigDecimal bd1 = new BigDecimal("123.456");
BigDecimal bd2 = new BigDecimal("1.0");

bd1.divide(bd2, 2, HALF_UP); // 123.46
bd1.divide(bd2, new MathContext(2, HALF_UP)); // 1.2E+2
```

### scale의 변경

- BigDecimal을 10으로 곱하거나 나누는 대신 scale의 값을 변경함으로써 같은 결과를 얻을 수 있다.
- BigDecimal의 scale을 변경하려면, setScale()을 이용

```java
BigDecimal setScale(int newScale)
BigDecimal setScale(int newScale, int roundingMode)
BigDecimal setScale(int newScale, RoundingMode mode)
```

- setScale()로 scale 값을 줄이는 것은 10의 n제곱으로 나누는 것과 같으므로 divide() 호출할 때처럼 오차 발생 가능하고 반올림 모드를 지정해주어야 한다.



