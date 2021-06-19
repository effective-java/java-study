# chap9 java.lang 패키지와 유용한 클래스

### java.lang 패키지

- 자바프로그래밍의 가장 기본이 되는 클래스들을 포함하고 있다.
- 따라서 import 없이 사용 가능

### Object 클래스

- 모든 클래스의 최고 조상이기 때문에 Object 클래스의 멤버들은 모든 클래스에서 바로 사용 가능
- 멤버변수는 없고 오직 11개의 메서드만 가지고 있다.

[주요 메서드](https://www.notion.so/f14e9a29bb7f4b5a93eca1c901271ae7)

### equals(Object obj)

- 매개변수로 객체의 참조변수를 받아서 그 결과를 boolean값으로 알려 주는 역할
- 실제코드
    - 두 객체의 같고 다름을 참조변수의 값으로 판단
    - 즉 두 참조변수에 저장된 주소값이 같은지를 판단하는 기능

```java
public boolean equals(Object obj) {
		return (this==obj)
}
```

```java
class EqualsEx1 {
	public static void main(String[] args) {
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
	} // main
} 

class Value {
	int value;

	Value(int value) {
		this.value = value;
	}
}
--------------------------------------------------
실행결과
v1과 v2는 다릅니다.
v1과 v2는 같습니다.
```

- 클래스의 인스턴스변수 값으로 객체의 같고 다름을 비교하게 하고 싶다면 오버라이딩을 하면된다
    - String class의 equals 메서드도 오버라이딩을 통해 String 인스턴스가 갖는 문자열 값을 비교하도록 되어 있다.
    - String, Date, File, wrapper 클래스(Integer, Double 등)
    - example

    ```java
    class Person {
    		long id;
    		
    		Person(long id) {
    				this.id = id;
    		}
    		public boolean equals(Object obj) {
    				if(obj != null && obj instanceof Person) { 
    						return id == ((Person)obj).id
    				} else {
    						return falsel;
    				}
    		}
    }
    ```

### hashCode()

- 해싱기법에 사용되는 해시함수를 구현한 것이다.
- 해싱
    - 데이터관리 기법 중 하나
    - 다량의 데이터를 저장하고 검색하는데 유용
- 해시함수
    - 찾고자하는 값을 입력하면, 그 값이 저장된 위치를 알려주는 해시코드를 반환한다.
- 일반적으로 해시코드가 같은 두 객체가 존재하는 것이 가능하지만, Object 클래스에 정의된 hashCode 메서드는 객체의 주소값으로 해시코드를 만들어 반환하기 때문에 32 bit jvm에서는 서로 다른 두 객체는 결코 같은 해시코드를 가질 수 없었다.
- 하지만 64bit jvm에서는 8 byte 주소값으로 해시코드(4byte)를 만들기 때문에 해시코드가 중복될 수 있다.
- 마찬가지고 클래스의 인스턴스변수 값으로 객체의 같고 다름을 판단해야하는 경우라면 equals 메서드 뿐만 아니라 hashCode 메서드도 적절히 오버라이딩해야 한다.
    - 같은 객체라면 hashCode 메서드를 호출했을 때의 결과값인 해시코드도 같아야 하기 때문
    - ex) HashMap, HashSet 클래스
- example
    - String 클래스는 문자열의 내용이 같으면, 동일한 해시코드를 반환하도록 오버라이딩되어 있기 때문에 내용이 같은 경우 항상 동일한 해시코드값
    - identityHashCode(Object obj)는 Object 클래스의 hashCode 메서드처럼 객체의 주소값으로 해시코드를 생성

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
--------------------------------------------------------
실행결과
true
96354
96354
271334973
1284693
```

### toString()

- 인스턴스에 대한 정보를 문자열로 제공할 목적의 정의
- 대부분의 경우 인스턴스 변수에 저장된 값들을 문자열로 표현
- Object class의 toString()

```java
public String toString() {
		return getClass().getName()+"@"+Integer.toHexString(hashCode());
}
```

### Clone()

- 자신을 복제하여 새로운 인스턴스를 생성하는 일을 한다
- 원래의 인스턴스는 보존하고 clone메서드를 통해 새로운 인스턴스를 생성하여 작업을 하면 작업이전의 값이 보존되므로 작업에 실패해서 원래의 상태로 되돌리거나 변경되기 전의 값을 참고하는데 도움이 된다
- Object 클래스에 정의된 clone()은 단순히 인스턴스변수의 값만 복사하기 때문에 참조타입의 인스턴스 변수가 있는 클래스는 완전한 인스턴스 복제가 이루어지지 않는다.
    - ex) 배열의 경우, 복제된 인스턴스도 같은 배열의 주소를 갖기 때문에 복제된 인스턴스의 작업이 원래의 인스턴스에 영향을 미치게 된다.
    - 이런 경우 마찬가지로 오버라이딩해서 새로운 배열을 생성하고 배열의 내용을 복사하도록 해야 한다.
- example
    - 복제할 클래스가 Cloneable 인터페이스를 구현한 클래스에서만 clone() 호출 가능
    - 그리고 접근제어자를 protected에서 public으로 변경
    - 마지막으로 조상클래스의 clone()을 호출하는 코드가 포함된 try - catch문 작성

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
		Point copy = (Point)original.clone(); // 복제(clone)해서 새로운 객체를 생성
		System.out.println(original);
		System.out.println(copy);
	}
}
----------
실행결과
x=3, y=5
x=3, y=5
```

- 공변 반환타입(covariant return type)
    - jdk 1.5부터 추가된 기능
    - 오버라이디할 때 조상 메서드의 반환타입을 자손 클래스의 타입으로 변경을 허용하는 것
    - 이처럼 조상의 타입이 아닌, 실제로 반환되는 자손 객체의 타입으로 반환할 수 있어서 번거로운 형변환이 줄어드는 장점

    ```java
    	public Point clone() {
    		Object obj = null;
    		try {
    			obj = super.clone();  // clone()은 반드시 예외처리를 해주어야 한다.
    		} catch(CloneNotSupportedException e) {}
    		return (Point)obj;
    	}
    ```

    ```java
    // Point copy = (Point)original.clone();
    Point copy = original.clone();
    ```

    ### 배열 복사

    - 배열도 객체이기 때문에 Object 클래스를 상속받으며, 동시에 Cloneable, Serializable 인터페이스가 구현되어 있다.
    - 또한 clone 메서드가  public으로 오버라이딩되어 있다.

    ```java
    import java.util.*;

    class CloneEx2 {
    	public static void main(String[] args){
    		int[] arr = {1,2,3,4,5};

            // 배열 arr을 복제해서 같은 내용의 새로운 배열을 만든다.
    		int[] arrClone = arr.clone(); 
    		arrClone[0]= 6;

    		System.out.println(Arrays.toString(arr));
    		System.out.println(Arrays.toString(arrClone));
    	}
    }
    ------------------
    실행결과
    [1, 2, 3, 4, 5]
    [6, 2, 3, 4, 5]
    ```

    ```java
    int[] arr = {1, 2, 3};

    // case1
    int[] arrClone = arr.clone();
    // case2
    int[] arrClone2 = new int[arr.length];
    System.arraycopy(arr, 0, arrClone2, 0, arr.length);
    ```

    - 일반적으로 배열을 복사할 때, 같은 길이의 새로운 배열을 생성한 다음에 System.arraycopy()를 이용해서 내용을 복사하지만, 이처럼 clone()이용도 가능, 같은 결과

    ```java
    ArrayList list = new ArrayList();
    ArrayList list2 = (ArrayList)list.clone();
    ```

    - 배열 뿐만 아니라 java.util 패키지의 Vector, ArrayList, LinkedList, HashSet, TreeSet, HashMap, TreeMap, Calendar, Date와 같은 클래스들이 이와 같은 방식으로 복제가 가능하다.

    ### 얕은 복사와 깊은 복사

    - clone()은 단순히 객체에 저장된 값을 그대로 복제할 뿐, 객체가 참조하고 있는 객체까지 복제하지는 않는다.
    - 기본형 배열인 경우에는 아무런 문제가 없지만, 객체배열을 clone()으로 복제하는 경우에는 원본과 복제본이 같은 객체를 공유하므로 완전한 복제라고 보기 어렵다.
        - 이러한 복제를 얕은 복사라고 한다, shallow copy
        - 얕은 복사는 원본을 변경하면 복사본도 영향을 받는다.
    - 반면에 원본이 참조하고 있는 객체까지 복제하는 것을 깊은 복사, deep copy
        - 깊은 복사에서는 원본과 복사본이 서로 다른 객체를 참조하기 때문에 원본의 변경이 복사본에 영향을 미치지 않는다.

![image_0](../image/chap9_java_lang_패키지와_유용한_클래스_1.png)

    - example

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
    ---------------------------------
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

    - 자신이 속한 클래스의 Class 객체를 반환하는 메서드
    - Class 객체는 이름이 'Class'인 클래스 객체이다.
    - 다음과 같이 정의

        ```java
        public final class Class implements ... {

        }
        ```

    - 클래스 객체는 클래스의 모든 정보를 담고 있으며, 클래스 당 1개만 존재
    - 클래스 파일이 '클래스 로더(ClassLoader)'에 의해 메모리에 올라갈 때, 자동으로 생성
        - 클래스 로더는 실행 시에 필요한 클래스를 동적으로 메모리에 로드하는 역할
        - 먼저 기존에 생성된 클래스 객체가 메모리에 존재하는지 확인하고
            - 있으면 객체의 참조를 반환
            - 없으면 클래스 패스에 지정된 경로를 따라서 클래스 파일을 찾는다.
                - 못찾으면  ClassNotFoundException 발생
                - 찾으면 해당 클래스 파일을 읽어서 Class 객체로 변환
    - 즉 파일 형태로 저장되어 있는 클래스를 읽어서 Class 클래스에 정의된 형식으로 변환하는 것
    - 따라서 클래스 파일을 읽어서 사용하기 편한 형태로 저장해 놓은 것이 클래스 객체이다.

    ### Class 객체를 얻는 방법

    - 클래스의 정보가 필요할 때, 먼저 Class 객체에 대한 참조를 얻어 와야 하는데, 해당 Class 객체에 대한 참조를 얻는 방법으로 여러 가지가 있다.

        ```java
        // case1: 생성된 객체로부터 얻는 방법
        Class cObj = new Card().getClass(); 

        // case2: 클래스 리터럴(*.class)로 부터 얻는 방법
        Class cObj = Card.class;
         
        // case3: 클래스 이름으로 부터 얻는 방법
        // 특히 클래스 파일, 예를 들어 데이터베이스 드라이버를 메모리에 올릴 때 주로 사용
        Class cObj = Class.forName("Card");
        ```

    - 클래스 객체를 이용하면 클래스에 정의된 멤버의 이름이나 개수 등, 클래스에 대한 모든 정보를 얻을 수 있기 때문에 Class 객체를 통해서 객체를 생성하고 메서드를 호출하는 등 보다 동적인 코드 작성 가능

        ```java
        Card c = new Card();                  // new 연산자를 이용한 객체 생성
        Card c = Card.class.newInstance();    // Class객체를 이용한 객체 생성
        ```

    - example

        ```java
        final class Card {
        	String kind;
        	int num;

        	Card() {
        		this("SPADE", 1);
        	}

        	Card(String kind, int num) {
        		this.kind = kind;
        		this.num  = num;
        	}

        	public String toString() {
        		return kind + ":" + num;
        	}
        }

        class ClassEx1 {
        	public static void main(String[] args) throws Exception {
        		Card c  = new Card("HEART", 3);       // new연산자로 객체 생성
        		Card c2 = Card.class.newInstance();   // Class객체를 통해서 객체 생성

        		Class cObj = c.getClass();

        		System.out.println(c);
        		System.out.println(c2);
        		System.out.println(cObj.getName());
        		System.out.println(cObj.toGenericString());
        		System.out.println(cObj.toString());		
        	}
        }
        --------------------------
        실행결과
        HEART: 3
        SPADE: 1
        Card
        final class Card
        class Card
        ```

    ### String 클래스

    - 문자열을 위한 클래스
    - 문자열을 저장하고 이를 다루는데 필요한 메서드를 함께 제공

    ### 변경 불가능한 클래스

    - String 클래스에는 문자열을 저장하기 위해서 문자형 배열 참조변수(char[]) value를 인스턴스 변수로 정의해놓고 있다.
    - 인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스변수에 문자형 배열로 저장되는 것이다.

    ```java
    public final class String implements java.io.Serializable, Comparable {
    		private char[] value;
    }
    ```

    - 한번 생성된 String 인스턴스가 갖고 있는 문자열은 읽어 올 수만 있고, 변경할 수는 없다.
        - 아래처럼 +로 결합하는 경우 새로운 문자열("ab")이 담긴 String 인스턴스가 생성되는 것이다.

    ```java
    String a = "a";
    String b = "b";

    a = a + b;
    ```

    - 이처럼 문자열을 결합하는 것은 매 연산 시마다 새로운 문자열을 가진 String 인스턴스가 생성되어 메모리공간을 차지하게 되므로 가능한 한 결합횟수를 줄이는 것이 좋다.
    - 문자열간의 결합이나 추출 등 문자열을 다루는 작업이 많이 필요한 경우에는 String 클래스 대신 StringBuffer 클래스를 사용하는 것이 좋다.
        - StringBuffer 인스턴스에 저장된 문자열은 변경이 가능하므로 하나의 StringBuffer 인스턴스만으로도 문자열을 다루는 것이 가능하다.

    ### 문자열의 비교

    - 문자열을 만들때는 두가지 방법이 있다.
        1. 문자열 리터럴을 지정하는 방법
        2. String 클래스의 생성자를 사용해서 만드는 방법
    - String  클래스의 생성자를 이용한 경우에는 new 연산자에 의해서 메모리할당이 이루어지기 때문에 항상 새로운 String 인스턴스가 생성된다.
    - 문자열 리터럴은 이미 존재하는 것을 재사용하는 것이다.
        - cf) 문자열 리터럴은 클래스가 메모리에 로드될 때 자동적으로 미리 생성

    ```java
    String str1 = "abc";
    String str2 = "abc";

    String str3 = new String("abc");
    String str4 = new String("abc");
    ```

![image_0](../image/chap9_java_lang_패키지와_유용한_클래스_2.png)

    - equals()를 사용했을 때는 문자열의 내용을 비교하기 때문에 true이지만 인스턴스의 주소를 ==로 비교했을 때는 결과가 다르다.

### 문자열 리터럴

- 자바 소스파일에 포함된 모든 문자열 리터럴은 컴파일 시에 클래스 파일(*.class)에 저장된다.
- 이 때 같은 내용의 문자열 리터럴은 한번만 저장된다. 문자열 리터럴도 String 인스턴스이고, 한번 생성하면 내용을 변경할 수 없으니 하나의 인스턴스를 공유하면 되기 때문이다.
- 이와 같이 String 리터럴들은 컴파일시에 클래스파일에 저장되며 아래의 예제를 실행하면 "AAA"를 담고 있는 String 인스턴스가 하나 생성된 후, 참조변수 s1, 2, 3, 4가 모두 이 스트링 인스턴스를 참조하게 된다.
- 클래스 파일에는 소스파일에 포함된 모든 리터럴의 목록이 있다.
    - 해당 클래스 파일이 클래스 로더에 의해 메모리에 올라갈 때, 이 리터럴의 목록에 있는 리터럴들이 jvm 내에 있는 '상수 저장소(constant pool)'에 저장된다.
    - 이 때, 이곳에 "AAA"와 같은 문자열 리터럴이 자동적으로 생성되어 저장되는 것이다.

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

### 빈 문자열

- 길이가 0인 배열 존재 가능? >> yes
- char 배열도 길이가 0인 배열을 생성할 수 있고, 이 배열을 내부적으로 가지고 있는 문자열이 바로 빈 문자열이다.
- String s= ""; 과 같은 문장이 있을 때, 참조변수 s가 참조하고 있는 String 인스턴스는 내부에 new char[0]과 같이 길이가 0인 char형 배열을 저장하고 있는 것이다.
- 하지만 char c = '';와 같은 표현도 가능한 것은 아니다. 반드시 하나의 문자를 지정해야 한다.

```java
String s = null;   ==> String s = "";  // 빈 문자열로 초기화
char c = '\u0000'l ==> char c = ' ';   // 공백으로 초기화
```

- String은 빈 문자열로, char은 공백으로 초기화 하는 것이 보통

### String 클래스의 생성자와 메서드

- 자주 사용되는 메서드

![image_0](../image/chap9_java_lang_패키지와_유용한_클래스_3.png)

![image_0](../image/chap9_java_lang_패키지와_유용한_클래스_4.png)

- replace vs replaceAll

    String str = "aaabbbccccabcddddabcdeeee";

    String result1 = str.replace("a", "왕").replace("b", "왕").replace("c", "왕");

    String result2 = str.replaceAll("[abc]", "왕");

![image_0](../image/chap9_java_lang_패키지와_유용한_클래스_5.png)

### join()과 StringJoiner

- join()은 여러 문자열 사이에 구분자를 넣어서 결합한다. 구분자로 문자열을 자르는 split()과 반대의 작업을 한다고 생각하면 이해하기 쉽다.

    ```java
    String animals = "dog,cat,bear";
    String[] arr = animals.split(",");
    String str = String.join("-", arr);
    str: dog-cat-bear
    ```

- java.util.StringJoiner 클래스를 사용해서 문자열을 결합할 수도 있다.

    ```java
    StringJoiner sj = new StringJoiner(",", "[" , "]");
    String[] strArr = {"aaa", "bbb", "ccc"};

    for(String s: strArr)
    	sj.add(s.toUpperCase());

    System.out.println(sj.toString());
    // [AAA,BBB,CCC]
    ```

### 유니코드의 보충문자

- 스트링 클래스의 메서드 중에 매개변수의 타입이 char, int 인 것들도 있다.
- 문자를 다루는 메서드라서 매개변수의 타입이 char일 것 같은데 왜 int일까?
    - 확장된 유니코드를 다루기 위해서
- 유니코드는 원래 2 byte, 16비트 문자인데, 20비트로 확장하게 되었다.
- 그래서 하나의 문자를 char타입으로 다루지 못하고, int 타입으로 다룰 수 밖에 없다.
- 확장에 의해 새로 추가된 문자들을 '보충 문자'라고 하는데, String 클래스의 메서드 중에서는 보충 문자를 지원하는 것이 있고 지원하지 않는 것도 있다.
    - 구별법은 매개변수가 int ch인 것들은 지원하는 것들, char ch인 것들은 지원하지 않는 것들이다.

### 문자 인코딩 변환

- getBytes(String charsetName)을 사용하면, 문자열의 문자 인코딩을 다른 인코딩으로 변경할 수 있다.
- 자바가 utf-16을 사용하지만, 문자열 리터럴에 포함되는 문자들은 os의 인코딩을 사용한다. 한글 윈도우즈의 경우 문자 인코딩으로 CP949를 사용하며, UTF-8로 변경하려면 아래와 같이 한다.
    - utf-8은 한글 한글자르 3 byte, cp949는 2 byte로 표현
    - cf) 사용가능한 문자 인코딩의 목록은 System.out.println(java.nio.charset.Cahrset.availableCharsets());

    ```java
    byte[] utf8_str = "가".getBytes("UTF-8");    // 문자열을 utf-8로 변환
    String str = new String(utf8_str, "UTF-8"); // byte배열을 문자열로 변환
    ```

- example

    ```java
    import java.util.StringJoiner;

    class StringEx5 {
    	public static void main(String[] args) throws Exception {
    		String str = "°¡";

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
    -------------------------------------------
    실행결과
    utf-8:[EA:B0:80]
    cp949:[B0:A1]
    utf-8:가
    cp949:가
    ```

### String.format()

- format()은 형식화된 문자열을 만들어내는 간단한 방법

```java
String str = String.format("%d 더하기 %d는 %d입니다.", 3, 5, 3+5)
Sytem.out.prinln(str); // 3 더하기 5는 8입니다.
```

### 기본형 값을 String으로 변환

- 숫자로 이루어진 문자열을 숫자로, 또는 그 반대로 변환하는 경우가 자주 있다.
- 방법은 간다하다. 숫자에 빈 문자열을 더해주면 된다
- 이외에도 valueOf()를 사용하는 방법도 있다.
    - 성능은 vlaueOf()가 더 좋다.

```java
int i = 100;
String str1 = i + "";
String str2 = String.valueOf(i);
```

### String을 기본형 값으로 변환

- valueOf()를 사용하거나
    - 리턴 타입은 Interger인데 오토박싱에 의해 int로 자동 변환된다.
    - 해당 메서드는 메서드 내부에서 parserInt()를 호출할 뿐이므로 반환 타입만 다르지 같은 메서드이다.
- parserInt() 사용
    - parserBoolean(), parserByte(), ... parserDouble()
    - cf) byte, short을 문자열로 변경할 떄는 String valueOf(int i)를 사용
    - cf) 문자열 "A"를 문자 'A로 변환하려면 char ch = "A".charAt(0);
    - Boolean, Byte와 같이 기본형 타입의 이름이 첫 글자가 대문자인 것은 래퍼 클래스이다.
        - 기본형 값을 감싸고 있는 클래스라는 뜻에서 붙여진 이름으로 기본형을 클래스로 표현한 것

```java
int i = Integer.parserInt("100");
int i2 = Integer.valueOf("100");
```

```java
public static Integer valueOf(String s) throws NumberFormatException {
		return Integer.valueOf(parseInt(s, 10));  // 10진수
}
```

- exmaple

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

    ---------------
    실행결과
    100+200.0=300.0
    100+200.0=300.0
    ```

    - parsetInt()같은 메서드는 문자열에 공백 또는 문자가 포함되어 있는 경우 변환 시 예외(NumberFormatException)가 발생할 수 있으므로 주의해아한다.
    - 그래서 문자열 양끝의 공백을 제거해주는 trim()을 사용하면 좋다.
    - 그러나 부호를 의미하는 '+'나 소수점을 의미하는 '.'와 float형 값을 뜻하는 f와 같은 자료형 접미사는 허용된다.
        - 단 자료형에 알맞은 변환을 하는 경우에만 허용
        - '1.0f'를 Integer.parsetInt() 사용하면 예외, Float.parserFlaoat()은 문제 없다

### StringBuffer 클래스

- StringBuffer 클래스는 스트링 클래스와 달리 지정된 문자열을 변경할 수 있다.
- 내부적으로 문자열 편집을 위한 버퍼를 가지고 있으며 인스턴스를 생성할 때 그 크기를 지정할 수 있다.
    - 버퍼의 길이를 충분히 잡아주는 것이 좋다.
    - 편집 중인 문자열입 버퍼의 길이를 넘어서게 되면 버퍼의 길이를 늘려주는 작업이 추가로 수행되어야하기 때문이다.
- 스트링 클래스처럼 문자열을 저장하기 위한 char 배열의 참조변수를 인스턴스 변수로 선언해놓고 있다.

```java
public final class StringBuffer implements java.io.Serializable {
		private char[] value;
}
```

### StringBuffer의 생성자

- 클래스의 인스턴스를 생성할 때, 적절한 길이의 char형 배열이 생성되고 이 배열은 문자열을 저장하고 편집하기 위한 공간(버퍼)로 사용된다.
- 버퍼의 크기를 지정해주지 않으면 16개의 문자를 저장할 수 있는 크기의 버퍼를 생성한다.

```java
public StringBuffer(int length) {
		value = new char[length];
		shared = false;
}
public StringBuffer() {
		this(16);
}
public StringBuffer(String str) {
		this(str.length() + 16);
		append(str);
}
```

### StringBuffer의 변경

- 스트링 클래스와 달리 내용 변경 가능

![image_0](../image/chap9_java_lang_패키지와_유용한_클래스_6.png)

- append()는 반환타입이 스트링버퍼인데 자신의 주소를 반환
    - 따라서 연속적으로 append() 호출 가능
    - sb.append("123").append("ZZ");

### StringBuffer의 비교

- 스트링 클래스와 달리 equals() 메서드를 오버라이딩하지 않아 ==로 비교한 것과 같은 결과를 얻는다.
- toString()은 오버라이딩되어 있어서 StringBuffer 인스턴스에 toString()을 호출하면, 담고있는 문자열 String으로 반환한다.
- 따라서 문자열을 비교하기 위해서는 인스턴스에 toString()을 호출해서 String 인스턴스를 얻은 다음, 여기에 equals메서드를 사용해서 비교

```java
class StringBufferEx1 {
	public static void main(String[] args) {
		StringBuffer sb  = new StringBuffer("abc");
		StringBuffer sb2 = new StringBuffer("abc");

		System.out.println("sb == sb2 ? " + (sb == sb2));
		System.out.println("sb.equals(sb2) ? " + sb.equals(sb2));
		
		// StringBuffer의 내용을 String으로 변환한다.
		String s  = sb.toString();	// String s = new String(sb);와 같다.
		String s2 = sb2.toString();

		System.out.println("s.equals(s2) ? " + s.equals(s2));
	}
}
----------------------------------------------
실행결과
sb == sb2 ? false
sb.equals(sb2) ? false
s.equals(s2) ? true
```

### StringBuffer 클래스의 생성자와 메서드

![image_0](../image/chap9_java_lang_패키지와_유용한_클래스_7.png)

![image_0](../image/chap9_java_lang_패키지와_유용한_클래스_8.png)

### StringBuilder란?

- 멀티쓰레드에 안전하도록 동기화되어 있다.
- 동기화가 스트링 버퍼 클래스의 성능을 떨어뜨린다.
- 그래서 스트링버퍼에서 쓰레드의 동기화만 뺀 스트링빌더가 새로 추가되었다.
- 스트링 버퍼와 완전히 똑같은 기능으로 작성되어 있다.