# 11장 컬렉션 프레임워크(Collections Framework)

![컬렉션프레임워크](https://user-images.githubusercontent.com/56071088/125505546-8aead4c5-d932-4e33-aff0-aefc05e5dcfb.png)

**`컬렉션 프레임워크(Collections Framework)`** : **데이터 군(무리 군 :群)을 저장하는 클래스들을 표준화한 설계**
- `Java API`에서는 `컬렉션 프레임워크`를 **데이터 군을 다루고 표현하기 위한 단일화된 구조(architecture)** 라고 정의하고 있다. 

**`컬렉션(Collection)`** : **다수의 데이터 == 데이터 그룹** 

**`프레임워크(Framework)`** : 표준화된 프로그래밍 방식

JDK 1.2부터 컬렉션 프레임워크가 등장하면서 다양한 종류의 컬렉션 클래스가 추가되고 모든 컬렉션 클래스를 표준화된 방식으로 다룰 수 있도록 체계화되었다.
- 컬렉션 클래스 : Vector클래스와 같이 다수의 데이터를 저장할 수 있는 클래스

## 컬렉션 프레임워크의 핵심 인터페이스

`컬렉션 프레임워크` 에서는 **컬렉션데이터 그룹**을 **크게 3가지 타입** 이 존재한다고 인식하고 컬렉션을 다루는데 필요한 기능을 가진 **3개의 인터페이스** 를 정의하였다.
- **인터페이스 List, Set** 의 **공통된 부분** 을 다시 뽑아서 **새로운 인터페이스 Collection** 을 추가로 정의하였다.
- **Map인터페이스는 이들과는 전혀 다른 형태로 컬렉션을 다루기 때문에** 같은 상속계층도에 포함되지 못했다.
- **JDK 1.5** 부터 **Iterable 인터페이스**가 추가되고 **이를 Collection 인터페이스가 상속받도록 변경**되었으나 
  - 이것은 단지 **인터페이스들의 공통적인 메서드인 iterator()** 를 뽑아서 **중복을 제거**하기 위한 것에 불과하므로 상속계층도에서 별 의미가 없다.

![컬렉션프레임워크의 핵심 인터페이스](https://user-images.githubusercontent.com/56071088/125505550-89e69b97-d710-4f5d-8ef0-72a21aaa4b66.png)

![컬렉션 프레임워크의 핵심 인터페이스 3가지의 특징](https://user-images.githubusercontent.com/56071088/125549830-7297d592-a8c7-4709-bd98-a309c15f822e.png)


컬렉션 프레임워크의 모든 컬렉션 클래스들은 **List, Set, Map 중의 하나를 구현** 하고 있고, **구현한 인터페이스의 이름이 클래스의 이름에 포함** 되어 있어서 이름만으로도 클래스의 특징을 쉽게 알 수 있도록 되어있다.
- **Vector, Stack, HashTable, Properties**와 같은 클래스들은 **컬렉션 프레임워크가 만들어지기 이전부터 존재하던 것이기 때문에 컬렉션 프레임워크의 명명법을 따르지 않는다.**
  - Vector, HashTable과 같은 기존의 컬렉션 클래스들은 호환을 위해, 설계를 변경해서 남겨두었지만 가능하면 사용하지 않는 것이 좋다.
    - **그 대신 새로 추가된 ArrayList, HashMap을 사용하자!!**

### Collection인터페이스

![collection인터페이스](https://user-images.githubusercontent.com/56071088/125550587-7e10a331-bd9b-47d6-9330-54c631ab7795.png)

`Collection인터페이스`는 `컬렉션 클래스`에 저장된 데이터를 읽고, 추가하고 삭제하는 등 컬렉션을 다루는데 가장 기본적인 메서드들을 정의하고 있다.
- **JDK 1.8**부터 추가된 **parallelStream, removeIf, stream, forEach** 등은 **lambda와 stream**에 관한 메서드들이다.
- 반환 타입이 boolean인 메서드들 : 작업을 성공하거나 사실이면 true, 그렇지 않으면 false를 반환
- `Object`가 아닌 `E`로 표기되어있는데, `E`는 **특정 타입을 의미하는 것**으로 **JDk 1.5**부터 추가된 **Generics**에 의한 표기이다.
  - `T`, `K`, `V`도 있다. 일단은 모두 Object타입이라고 이해해두자.

### List인터페이스

`List인터페이스` : **`중복을 허용`**, **`저장순서가 유지`** 되는 컬렉션을 구현하는데 사용된다.

![List의 상속 계층도](https://user-images.githubusercontent.com/56071088/125552778-3aaed456-c0d0-4932-ac21-37bf03ff8e59.png)

- Collection인터페이스로부터 상속받은 메서드들은 제외한 List인터페이스의 메서드

![List인터페이스 메서드](https://user-images.githubusercontent.com/56071088/125552779-5858cfd8-fe7b-48e8-9af5-ba10e60d470c.png)

### Set인터페이스

`Set인터페이스` : **`중복을 허용 X`**, **`저장순서가 유지 X`** 이러한 컬렉션 클래스들을 구현하는데 사용된다.
- Set인터페이스를 구현한 클래스로는 **HashSet, TreeSet**등이 있다.

![Set상속 계층도](https://user-images.githubusercontent.com/56071088/125552812-07199165-126e-42f6-bec8-cb5f33130b0d.png)

### Map인터페이스

![Map상속계층도](https://user-images.githubusercontent.com/56071088/125552816-23179744-51af-439c-aa50-61b0a08b1187.png)

`Map인터페이스` : **`key와 value`** 를 **하나의 쌍으로 묶어서 저장**하는 컬렉션 클래스를 구현하는 데 사용된다.
- **key는 중복될 수 없다. value는 중복을 허용한다.**
- 기존에 저장된 데이터와 **중복된 키와 값을 저장**하면 **기존의 값은 없어지고 마지막에 저장된 값이 남게된다.**
- Map인터페이스를 구현한 클래스로는 HashTable, HashMap, LinkedHashMap, SoretedMap, TreeMap등이 있다.
- `Map` : **어떤 두 값을 연결한다**는 의미에서 붙여진 이름이다.

![Map인터페이스 메서드](https://user-images.githubusercontent.com/56071088/125552804-3e00d834-d8e6-4ec6-9634-a20c9264517e.png)

- **values()** 에서는 **반환타입이 Collection**
- **keySet()** 에서는 **반환타입이 Set**
  - **Map인터페이스**에서 **value는 중복을 허용하기 때문에** `Collection타입`으로 반환
  - **key는 중복을 허용하지 않기 때문에** `Set타입`으로 반환한다.

### Map.Entry인터페이스

`Map.Entry인터페이스`는 **Map 인터페이스**의 **내부 인터페이스**이다.

- 내부 클래스와 같이 인터페이스도 인터페이스 안에 **인터페이스를 정의하는 내부 인터페이스(inner interface)** 를 정의하는 것이 가능하다.
- Map에 저장되는 **key-value 쌍을 다루기 위해 내부적으로 Entry인터페이스를 정의해 놓았다.**
  - 이것은 보다 객체지향적으로 설계하도록 유도하기 위한 것
  - Map 인터페이스를 구현하는 클래스에서는 **Map.Entry인터페이스도 함께 구현**해야 한다.

```java
public interface Map {
  public static interface Entry {
    Object getKey();
    Object getValue();
    Object setValue(Object value);
    boolean equals(Object o);
    int hashCode();
    ... // JDK 8.0 부터 추가된 메서드는 생략
  }
}
```

![Map Entry인터페이스 메서드](https://user-images.githubusercontent.com/56071088/125554452-d13412bb-4a04-4c79-beab-a137f9131c95.png)


## ArrayList

`ArrayList`

- List인터페이스를 구현하기 때문에, **데이터의 순서가 유지, 중복을 허용**한다는 특징
- 기존의 Vector를 개선, 구현 원리와 기능적인 측면에서 Vector와 동일
  - **가능하면 Vector 대신, ArrayList를 사용**하자.

ArrayList는 Object배열을 이용해서 데이터를 순차적으로 저장한다.

- `transient Object[] elementData;`
- 배열에 순서대로 객체를 저장한다.
- **크기가 10인 배열을 생성**
  - 배열에 더 이상 저장할 공간이 없으면 **보다 큰 새로운 배열을 생성해서 기존의 배열에 저장된 내용을 새로운 배열로 복사한 다음에 저장**된다.
- 선언된 배열의 타입이 모든 객체의 최고조상인 Object이기 때문에 모든 종류의 객체를 담을 수 있다.

```java
public class ArrayList extends AbstractList implements List, RandomAccess, Cloneable, java.io.Serializable {
  ...
  transient Object[] elementData; // Object배열
  ...
}
```

[참고] `transient` : 직렬화(serialization)와 관련한 제어자. 15장에서 다룬다.

<img src="https://user-images.githubusercontent.com/56071088/126200453-3af0d768-8285-40d9-9523-88452d2ee574.png"  width="550" height="650">

```java
import java.util.*;

class ArrayListEx1{
	public static void main(String[] args) {
		ArrayList list1 = new ArrayList(10);
		list1.add(new Integer(5));
		list1.add(new Integer(4));
		list1.add(new Integer(2));
		list1.add(new Integer(0));
		list1.add(new Integer(1));
		list1.add(new Integer(3));

		ArrayList list2 = new ArrayList(list1.subList(1,4)); 
		print(list1, list2);

		Collections.sort(list1);	// list1과 list2를 정렬한다.
		Collections.sort(list2);	// Collections.sort(List l)
		print(list1, list2);

		System.out.println("list1.containsAll(list2):" + list1.containsAll(list2));

		list2.add("B");
		list2.add("C");
		list2.add(3, "A");
		print(list1, list2);

		list2.set(3, "AA");
		print(list1, list2);
		
		// list1에서 list2와 겹치는 부분만 남기고 나머지는 삭제한다.
		System.out.println("list1.retainAll(list2):" + list1.retainAll(list2));	
		print(list1, list2);
		
		//  list2에서 list1에 포함된 객체들을 삭제한다.
		for(int i= list2.size()-1; i >= 0; i--) {
			if(list1.contains(list2.get(i)))
				list2.remove(i);
		}
		print(list1, list2);
	} // main의 끝

	static void print(ArrayList list1, ArrayList list2) {
		System.out.println("list1:"+list1);
		System.out.println("list2:"+list2);
		System.out.println();		
	}
} // class
```

```json
실행결과
list1[5, 4, 2, 0, 1, 3]
list2[4, 2, 0]

list1[0, 1, 2, 3, 4, 5] // Collections.sort(List l)
list2[0, 2, 4]          // // Collections.sort(List l)

list1.containsAll(list2) : true
list1[0, 1, 2, 3, 4, 5]
list2[0, 2, 4, A, B, C]  // add(Object obj)

list1[0, 1, 2, 3, 4, 5]
list2[0, 2, 4, AA, B, C]  // set(int index, Object obj)

list1.retainAll(list2) : true 
list1[0, 2, 4]
list2[0, 2, 4, AA, B, C]

list1[0, 2, 4]
list2[AA, B, C]      // if contains(): remove()
```

- `ArrayList정렬` : **Collections.sort(List l), Collections클래스의 sort메서드**
  - Collection은 인터페이스, Collections는 클래스 
- `boolean containAll(Collection c)` : Collection c의 모든 요소를 포함하고 있을 때만 true, 아닐 때는 false  
- `boolean removeAll(Collection c)` : 지정한 Collection에 저장된 것과 동일한 객체들을 ArrayList에서
- `boolean retainAll(Collection c)` : ArrayList에 저장된 객체 중에서 **주어진 Collection과 공통된 것들만을 남기고 나머지는 삭제한다.**
- `boolean addAll(Collection c)` : 주어진 Collection의 모든 객체를 저장한다.
- `boolean addAll(int index, Collection c)` : 지정된 위치부터 주어진 Collection의 모든 객체를 저장한다.
- `void sort(Comparator c)` : 지정된 정렬기준(c)으로 ArrayList를 정렬

```java
//  list2에서 list1에 포함된 객체들을 삭제한다.
for(int i = list2.size()-1; i >= 0; i--) {
  if(list1.contains(list2.get(i))) {
    list2.remove(i);
  }
}
```

- list2에서 list1과 공통되는 요소들을 찾아서 삭제하는 부분
- 변수 i를 증가시키면서 삭제하면, 한 요소가 삭제될 때마다 빈 공간을 채우기 위해 나머지 요소들이 자리이동을 하기 때문에 올바른 결과를 얻을 수 없다.
  - 제어변수를 감소시켜가면서 삭제를 해야 자리이동이 발생해도 영향을 받지 않고 작업이 가능하다.
```java
import java.util.*; 

class ArrayListEx2 { 
	public static void main(String[] args) { 
		final int LIMIT = 10;	// 자르고자 하는 글자의 개수를 지정한다.
		String source = "0123456789abcdefghijABCDEFGHIJ!@#$%^&*()ZZZ"; 
		int length = source.length(); 

		List list = new ArrayList(length/LIMIT + 10); // 크기를 약간 여유 있게 잡는다.

		for(int i=0; i < length; i += LIMIT) { 
			if(i+LIMIT < length ) 
				list.add(source.substring(i, i+LIMIT)); 
			else 
				list.add(source.substring(i)); 
		} 

		for(int i=0; i < list.size(); i++) { 
			System.out.println(list.get(i)); 
		} 
	} // main()
}
```

```json
실행결과
0123456789
abcdefghij
ABCDEFGHIJ
!@#$%^&*()
ZZZ
```

- ArrayList를 생성할 때, 저장할 요소의 개수를 고려해서 실제 저장할 개수보다 **약간 여유있는 크기로 하는 것이 좋다.**
  - 생성할 때 지정한 크기보다 더 많은 객체를 저장하면 **자동적으로 크기가 늘어나기는 하지만 이 과정에서 처리시간이 많이 소요**되기 때문이다. 

```java
import java.util.*;

class VectorEx1 {
	public static void main(String[] args) {
		Vector v = new Vector(5);	// 용량(capacity)이 5인 Vector를 생성한다.
		v.add("1");
		v.add("2");
		v.add("3");
		print(v);

		v.trimToSize();	// 빈 공간을 없앤다.(용량과 크기가 같아진다.)
		System.out.println("=== After trimToSize() ===");
		print(v);

		v.ensureCapacity(6);
		System.out.println("=== After ensureCapacity(6) ===");
		print(v);

		v.setSize(7);
		System.out.println("=== After setSize(7) ===");
		print(v);
		
		v.clear();
		System.out.println("=== After clear() ===");
		print(v);
	}

	public static void print(Vector v) {
		System.out.println(v);
		System.out.println("size :" + v.size());
		System.out.println("capacity :" + v.capacity());
	}
}
```

```json
실행결과
[1, 2, 3]
size: 3
capacity: 5
=== after trimToSize() ===
[1, 2, 3]
size: 3
capacity: 3
=== after ensureCapacity(6) ===
size: 3
capacity: 6
=== after setSize(7) ===
size: 7
capacity: 12  // capacity가 부족한 경우 size를 지정하지 않으면 2배증가
=== after clear() ===
size: 0
capacity: 12
```

- Vector의 용량과 크기에 관한 예시
- v.trimToSize(), v.ensureCapcaity(), v.setSize()와 같은 Vector의 용량(capacity)과 크기(size)에 관한 메서드들은 capacity의 크기에 따라서 크기가 작다면 새로운 인스턴스를 생성해야 한다.
  - `배열의 크기(protected Object[] elementData;)` 를 변경할 수 없기 때문에, **새로운 배열을 생성해서 그 주소값을 변수 v에 할당한다.** 
  - **기존의 Vector인스턴스는 더 이상 사용할 수 없으므로, 가비지 콜렉터(GC)에 의해서 메모리에서 제거된다.**
  - 기존의 인스턴스를 다시 사용하는 것이 아닌 새로운 인스턴스를 생성하는 것이다.
- `Vector, ArrayList`같이 `배열을 이용한 자료구조`는 **데이터를 읽어오고 저장하는 데는 효율이 좋지만**, 
  - **용량을 변경할 때**는 **새로운 배열을 생성한 후 기존의 배열로부터 새로 생성된 배열로 데이터를 복사**해야하기 때문에 **상당히 효율이 떨어진다는 단점**이 있다.
- **처음에 인스턴스를 생성할 때**, 저장할 데이터의 개수를 잘 고려하여 **충분한 용량의 인스턴스를 생성하는 것이 좋다.**

```java
import java.util.*;

public class MyVector implements List {
	Object[] data = null;	// 객체를 담기 위한 객체배열을 선언한다.
	int capacity = 0;		// 용량
	int size = 0;			// 크기

	public MyVector(int capacity) {
		if (capacity < 0)
		   throw new IllegalArgumentException("유효하지 않은 값입니다. :"+ capacity);

		this.capacity = capacity;
		data = new Object[capacity];		
	}

	public MyVector() {
		this(10);		// 크기를 지정하지 않으면 크기를 10으로 한다.
	}

    // 최소한의 저장공간(capacity)를 확보하는 메서드
	public void ensureCapacity(int minCapacity) {
		if (minCapacity - data.length > 0)
			setCapacity(minCapacity);
	}

	public boolean add(Object obj) {
		// 새로운 객체를 저장하기 전에 저장할 공간을 확보한다.
		ensureCapacity(size+1);
		data[size++] = obj; 
		return true;
	}

	public Object get(int index) {
		if(index < 0 || index >= size) 
			throw new IndexOutOfBoundsException("범위를 벗어났습니다.");

		return data[index];
	}

	public Object remove(int index) { 
		Object oldObj = null;

		if(index < 0 || index >= size) 
			throw new IndexOutOfBoundsException("범위를 벗어났습니다.");

		oldObj = data[index];

		// 삭제하고자 하는 객체가 마지막 객체가 아니라면, 배열복사를 통해 빈자리를 채워줘야 한다.
		if(index != size-1) {
			System.arraycopy(data, index+1, data, index, size-index-1);
		}

        // 마지막 데이터를 null로 한다. 배열은 0 부터 시작하므로 마지막 요소는 index가 size-1이다.
		data[size-1] = null;	
		--size;

		return oldObj;
	}

	public boolean remove(Object obj) {
		for(int i=0; i< size; i++) {
			if(obj.equals(data[i])) {
				remove(i);
				return true;
			}
		}
		return false;
	}

	public void trimToSize() {
		setCapacity(size);
	}

	private void setCapacity(int capacity) {
		if(this.capacity==capacity) return; // 크기가 같으면 변경하지 않는다.

		Object[] tmp = new Object[capacity];
		System.arraycopy(data,0, tmp, 0, size);
		data = tmp;
		this.capacity = capacity;
	}

	public void clear(){ 
		for (int i = 0; i < size; i++)
			data[i] = null;
		size = 0;
	}

	public Object[] toArray(){ 
		Object[] result = new Object[size];
		System.arraycopy(data, 0, result, 0, size);

		return result;
	}

	public boolean isEmpty() { return size==0;}
	public int capacity() { return capacity; }
	public int size() { return size; }
/****************************************/
/*       List인터페이스로부터 상속받은 메서드들          */
/****************************************/
//  public int size();
//  public boolean isEmpty();
	public boolean contains(Object o){ return false;}
	public Iterator iterator(){ return null; }
//	public Object[] toArray();
	public Object[] toArray(Object a[]){ return null;}
//  public boolean add(Object o);
//  public boolean remove(Object o);
	public boolean containsAll(Collection c){ return false; }
	public boolean addAll(Collection c){ return false; }
	public boolean addAll(int index, Collection c){ return false; }
	public boolean removeAll(Collection c){ return false; }
	public boolean retainAll(Collection c){ return false; }
//	public void clear();
	public boolean equals(Object o){ return false; }
//	public int hashCode();
//  public Object get(int index);
	public Object set(int index, Object element){ return null;}
	public void add(int index, Object element){}
//  public Object remove(int index);
	public int indexOf(Object o){ return -1;}
	public int lastIndexOf(Object o){ return -1;}
	public ListIterator listIterator(){ return null; }
	public ListIterator listIterator(int index){ return null; }
	public List subList(int fromIndex, int toIndex){ return null; }

	default void sort(Comparator c) { /* 내용생략 */ }                     // JDK1.8부터
	default Spliterator spliterator() { /* 내용생략 */ }                  // JDK1.8부터
	default void replaceAll(UnaryOperator operator){/* 내용생략 */} //JDK1.8부터
}
```

- Vector클래스의 실제 코드를 바탕으로 이해하기 쉽게 재구성한 것이다.
- 주석처리 한 것은 코드를 정상적으로 작동하도록 구현한 것
- 주석처리 하지 않은 것은 컴파일만 가능하도록 최소한 구현한 것
- 인터페이스를 구현할 때 인터페이스에 정의된 모든 메서드를 구현해야 한다. **일부 메서드만 구현했다면 추상클래스로 선언해야 한다.**
  - 그러나 JDK 1.8부터 List인터페이스에 3개의 default method가 추가되었으며, 이 들은 구현하지 않아도 된다.

```java
public Object remove(int index) { 
  Object oldObj = null;

  if(index < 0 || index >= size) 
    throw new IndexOutOfBoundsException("범위를 벗어났습니다.");
  
  oldObj = data[index];
  
  // 삭제하고자 하는 객체가 마지막 객체가 아니라면, 배열복사를 통해 빈자리를 채워줘야 한다.
  if(index != size-1) {
    System.arraycopy(data, index+1, data, index, size-index-1);
  }
  
  // 마지막 데이터를 null로 한다. 배열은 0 부터 시작하므로 마지막 요소는 index가 size-1이다.
  
  data[size-1] = null;	
  --size;
		
  return oldObj;
}
```

`Object remove(int index)` 메서드는 지정된 위치(index)에 있는 객체를 삭제하고 삭제한 객체를 반환하도록 작성하였다.
- 삭제할 객체의 바로 뒤에 있는 데이터를 한 칸씩 앞으로 복사해서 삭제할 객체를 덮어쓰는 방식으로 처리한다.
- 만일 삭제할 객체가 마지막 데이터라면, 복사할 필요 없이 단순히 null로 변경해준다. 
  
`배열`에 **객체를 순차적으로 저장할 때**와 **객체를 마지막에 저장된 것부터 삭제**하면 System.arrayCopy()를 호출하지 않기 때문에 **작업시간이 짧지만**,

`배열`의 **중간에 위치한 객체를 추가하거나 삭제하는 경우 System.arrayCopy()를 호출**해서 다른 데이터의 위치를 이동시켜 줘야 하기 때문에 다루는 데이터의 개수가 많을수록 **작업시간이 오래 걸린다는 것이다.**

1. ArrayList에 저장된 첫 번째 객체부터 삭제하는 경우 **(배열 복사 발생)**

```java
for(int i = 0; i < list.size(); ++i) {
  list.remove(i);
}
```
![remove1](https://user-images.githubusercontent.com/56071088/126263982-2ede1c88-c202-4568-88ca-40936df8060e.PNG)


1. ArrayList에 저장된 마지막 객체부터 삭제하는 경우 **(배열 복사 발생 안함)**

```java
for(int i = list.size()-1; i >= 0; --i) {
  list.remove(i);
}
```

![remove2](https://user-images.githubusercontent.com/56071088/126263987-12f35bfc-80d1-44ec-90dd-c3a879f40d6a.PNG)

#### 예시) MyVector클래스에 0~4의 값이 저장되어 있는 상태에서 세 번째 데이터를 삭제하기 위해 remove(2)를 호출

![a](https://user-images.githubusercontent.com/56071088/126263019-39975162-1997-4381-8b15-c9fd82d3fe9d.PNG)

1. 삭제할 데이터의 뒤에 있는 데이터를 한 칸씩 앞으로 복사해서 삭제할 데이터를 덮어쓴다.

```java
System.arrayCopy(data, index+1, data, index, size-index-1);
-->
System.arrayCopy(data, 3, data, 2, 2);
```

- data[3]에서 data[2]로 2개의 데이터를 복사하라는 의미

2. 데이터가 모두 한 칸씩 앞으로 이동하였으므로 마지막 데이터는 null로 변경해야 한다.

```java
data[size-1] = null;
```

3. 데이터가 삭제되어 데이터의 개수가 줄었으므로 size의 값을 1 감소시킨다.

```java
--size;
```

## LinkedList

`배열의 장점`

- 가장 기본적인 형태의 자료구조로 구조가 간단하며 사용하기 쉽고 데이터를 읽어오는데 걸리는 시간(접근시간: access time)이 가장 빠르다.

`배열의 단점`

1. **크기를 변경할 수 없다.**
  - 크기를 변경할 수 없으므로 새로운 배열을 생성해서 데이터를 복사해야 한다.
  - 실행속도를 향상시키기 위해서는 충분히 큰 크기의 배열을 생성해야 하므로 메모리가 낭비된다.
2. **비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.**
  - 차례대로 데이터를 추가하고 마지막에서부터 데이터를 삭제하는 것은 빠르지만,
  - 배열의 중간에 데이터를 추가하려면, 빈자리를 만들기 위해 다른 데이터들을 복사해서 이동해야 한다.

### linked list

![linked list](https://user-images.githubusercontent.com/56071088/126271884-d6bcc108-0695-4dcd-80b1-488ed1d05a0a.PNG)

**이러한 배열의 단점을 보완하기 위해서 LinkedList라는 자료구조가 고안되었다.**
- 배열은 모든 데이터가 연속적으로 존재하지만 linked list는 불연속적으로 존재하는 데이터를 서로 연결(link)한 형태로 구성되어 있다.

![배열과 링크드 리스트](https://user-images.githubusercontent.com/56071088/126265517-a3c4392f-3775-4a63-a11f-cb3be1c0461a.png)

`linked list`의 각 **요소(node)** 들은 **자신과 연결된 다은 요소에 대한 참조(주소값)** 와 **데이터**로 구성되어 있다.

```java
class Node {
  Node next; // 다음 요소의 주소를 저장
  Object obj; // 데이터를 저장
}
```

#### linked list에서의 데이터 삭제

삭제하고자 하는 요소의 이전 요소가 삭제하고자 하는 요소의 다음 요소를 참조하도록 변경하면 된다.
- 배열처럼 데이터를 이동하기 위해 복사하는 과정이 없기 때문에 처리속도가 매우 빠르다.

![linked list에서의 데이터 삭제](https://user-images.githubusercontent.com/56071088/126270806-1d61a119-1dd4-477b-bc4d-752590da7530.png)

#### linked list에서의 데이터 추가

새로운 요소를 생성한 다음 추가하고자 하는 위치의 이전 요소의 참조를 새로운 요소에 대한 참조로 변경해주고, 새로운 요소가 그 다음 요소를 참조하도록 변경하기만 하면 되므로 처리속도가 매우 빠르다.

![linked list에서의 데이터 추가](https://user-images.githubusercontent.com/56071088/126270804-35847461-f474-4c42-967e-15591cd49c5a.PNG)


### double linked list

![double linked list](https://user-images.githubusercontent.com/56071088/126271887-e0b49a3b-d175-4dcc-b741-aa044e56d3ac.PNG)

linked list는 이동방향이 단방향이기 때문에 다음 요소에 대한 접근은 쉽지만 이전요소에 대한 접근은 어렵다. 이 점을 보완한 것이 `double linked list(이중 연결 리스트)` 이다.
- 단순히 linked list에 이전 요소 참조를 위한 참조변수 하나를 추가한 것을 제외하고는 linekd list와 똑같다.
- linked list보다 각 요소에 대한 접근과 이동이 용이하므로 linked list보다 더 많이 사용된다.

```java
class Node {
  Node next; // 다음 요서의 주소를 저장
  Node previous; // 이전 요소의 주소를 저장
  Obkect obj; // 데이터를 저장
}
```

### double circular linked list

![double circular linked list](https://user-images.githubusercontent.com/56071088/126271881-52a8c391-b433-4b89-b49a-0bdd721c04c5.PNG)

double linked list의 접근성을 보다 향상시킨 것이 `double circular linked list(이중 원형 연결 리스트)` 이다.
- double linked list에서 첫 번째 요소와 마지막 요소를 서로 연결시킨 것

### LinkedList의 메서드

실제로 `LinkedList클래스`는 이름과 달리 `double linked list`로 구현되어 있다.
- linked list의 단점인 낮은 접근성(accessibility)을 높이기 위한 것이다.

LinkedList 역시 List인터페이스를 구현했기 때문에 ArrayList와 내부구현 방법만 다를 뿐 제공하는 메서드의 종류와 기능은 거의 같다.

- LinkedList는 Queue인터페이스와 Deque 인터페이스를 구현하도록 변경되었는데, 아래의 22개의 메서드는 Queue인터페이스와 Deque 인터페이스를 구현하면서 추가된 메소드이다.
  - element(), offer(), peek(), poll(), remove() → Queue 인터페이스
  - 나머지 → Deque 인터페이스

![linked list메서드](https://user-images.githubusercontent.com/56071088/126272268-4a902967-91e3-482a-820a-a8dc2fbf8244.png)

### ArrayList vs LinkedList 성능 차이

#### 데이터의 추가/삭제 성능 비교

```java
import java.util.*; 

public class ArrayListLinkedListTest {

  public static void main(String args[]) { 
    // 추가할 데이터의 개수를 고려하여 충분히 잡아야한다.
    ArrayList al = new ArrayList(2000000);
    LinkedList ll = new LinkedList(); 

    System.out.println("= 순차적으로 추가하기 ="); 
    System.out.println("ArrayList :"+add1(al)); 
    System.out.println("LinkedList :"+add1(ll)); 
    System.out.println(); 
    System.out.println("= 중간에 추가하기 ="); 
    System.out.println("ArrayList :"+add2(al)); 
    System.out.println("LinkedList :"+add2(ll)); 
    System.out.println(); 
    System.out.println("= 중간에서 삭제하기 ="); 
    System.out.println("ArrayList :"+remove2(al)); 
    System.out.println("LinkedList :"+remove2(ll)); 
    System.out.println(); 
    System.out.println("= 순차적으로 삭제하기 ="); 
    System.out.println("ArrayList :"+remove1(al)); 
    System.out.println("LinkedList :"+remove1(ll));
  } 

  public static long add1(List list) { 
    long start = System.currentTimeMillis(); 
    for(int i=0; i<1000000;i++) list.add(i+"");
    long end = System.currentTimeMillis();
    return end - start; 
  } 

  public static long add2(List list) { 
    long start = System.currentTimeMillis(); 
    for(int i=0; i<10000;i++) list.add(500, "X"); 
    long end = System.currentTimeMillis(); 
    return end - start; 
  } 

  public static long remove1(List list) { 
    long start = System.currentTimeMillis(); 
    for(int i=list.size()-1; i >= 0;i--) list.remove(i); 
    long end = System.currentTimeMillis(); 
    return end - start; 
  } 

  public static long remove2(List list) { 
    long start = System.currentTimeMillis(); 
    for(int i=0; i<10000;i++) list.remove(i); 
    long end = System.currentTimeMillis(); 
    return end - start; 
  } 
}
```

```json
실행결과

순차적으로 추가하기
ArrayList : 284
LinkedList : 406

중간에서 추가하기
ArrayList : 3453
LinkedList : 16

중간에서 삭제하기
ArrayList : 2641
LinkedList : 234

순차적으로 삭제하기
ArrayList : 0
LinkedList : 31
```

```java
// 추가할 데이터의 개수를 고려하여 충분히 잡아야한다.
ArrayList al = new ArrayList(2000000);
// LinkedList는 상관없다.
LinkedList ll = new LinkedList(); 
```

**단순히 저장하는 시간만을 비교하기 위해** ArrayList를 생성할 때, **저장할 데이터의 개수만큼 충분한 초기용량 확보**해서, **저장공간이 부족해서 새로운 ArrayList를 생성해야 하는 상황이 일어나지 않도록 했다.**

1. **순차적으로 추가/삭제 하는 경우 : ArrayList가 LinkedList보다 빠르다.**
- 만일 ArrayList의 크기가 충분하지 않으면, 새로운 크기의 ArrayList를 생성하고 데이터를 복사하는 일이 발생하게 되므로 순차적으로 데이터를 추가해도 ArrayList보다 LinkedList가 더 빠를 수 있다.
  
- 순차적으로 데이터를 삭제한다는 것은 배열의 마지막 요소부터 차례대로 삭제한다는 것을 의미
- ArrayList는 마지막 데이터부터 삭제할 경우 각 요소들의 재배치가 필요하지 않기 때문에 상당히 빠르다. 
  - 단지 마지막 요소의 값을 null로만 바꾸면 된다.   

2. **중간 데이터를 추가/삭제하는 경우 : LinkedList가 ArrayList보다 빠르다.**
  - LinkedList는 각 요소간의 연결만 변경해주면 되기 때문에 처리속도가 상당히 빠르다.
  
  - ArrayList는 각 요소들을 재배치하여 추가할 공간을 확보하거나 빈 공간을 채워야하기 때문에 처리속도가 늦다.

#### 데이터의 조회 성능 비교

```java
import java.util.*; 

public class ArrayListLinkedListTest2 { 
      public static void main(String args[]) { 
            ArrayList  al = new ArrayList(1000000); 
            LinkedList ll = new LinkedList(); 
            add(al);
            add(ll);

            System.out.println("= 접근시간 테스트 ="); 
            System.out.println("ArrayList :"+access(al)); 
            System.out.println("LinkedList :"+access(ll)); 
	  }

      public static void add(List list) { 
            for(int i=0; i<100000;i++) list.add(i+""); 
      } 

      public static long access(List list) { 
            long start = System.currentTimeMillis(); 

            for(int i=0; i<10000;i++) list.get(i); 

            long end = System.currentTimeMillis(); 

            return end - start; 
      } 
}
```

```json
실행결과

= 접근시간 테스트 =
ArrayList: 1
LinkedList: 317
```

1. `배열`의 경우, 만일 인덱스가 n인 원소의 값을 얻어 오고자 한다면 단순히 수식을 계산하면 된다.
  - **인덱스가 n인 데이어의 주소 = 배열의 주소 + n * 데이터 타입의 크기**
  - `배열`은 각 요소들이 연속적으로 메모리상에 존재하기 때문에 간단한 계산으로 **원하는 요소의 주소를 얻어서 저장된 데이터를 바로 읽어올 수 있다.**

Objet배열의 arr[2]의 요소를 읽으려 한다면
- n은 2, 모든 참조형 변수의 크기는 4byte, 생성된 배열의 주소는 0x100이므로 
- 3번째 데이터가 저장되어 있는 주소는 0x100 + 2*4 = 0x108

```java
Object[] arr = new Object[5];
```
![인덱스가 n인 데이터의 주소](https://user-images.githubusercontent.com/56071088/126276466-2369a521-5d79-47ca-8c97-f2670a0257c6.PNG)

2. LinkedList의 경우, 불연속적으로 위치한 각 요소들이 서로 연결된 것이라 **처음부터 n번째 데이터까지 차례대로 따라가야만 원하는 값을 얻을 수 있다.**
  - LinkedList는 저장해야 하는 데이터의 개수가 많아질수록 데이터를 읽어 오는 시간, 즉 접근시간(access time)이 길어진다는 단점이 있다.

#### 결론

![ArrayList, LinkedList 성능 비교](https://user-images.githubusercontent.com/56071088/126276992-e278bd80-ffb6-4f3e-9b3d-d4f74916c6a4.png)

- 다루고자 하는 데이터의 개수가 변하지 않는 경우라면, ArrayList가 최상의 선택

- 데이터 개수의 변경이 자주 일어난다면, LinkedList가 더 나은 선택

두 클래스의 장점을 이용해서 적절히 혼합해서 사용하는 방법도 해볼 수 있다.

- 처음에 작업하기 전에 데이터를 저장할 때는 ArrayList를 사용한 다음, 작업할 때는 LinkedList로 데이터를 옮겨서 작업하면 좋은 효율을 얻을 수 있다.

- Collection Framework는 이처럼 **서로 변환이 가능한 생성자**를 제공하므로, **간단히 다른 컬렉션 클래스로 데이터을 옮길 수 있다.**

```java
ArrayList al = new ArrayList(1000000);

for(int i=0; i<1000000; i++)  {
  al.add(i+"");
}

LinkedList ll = new LinkedList(al);

for(int i=0; i<1000000; i++) {
  ll.add(500, "X");
}
```

## Stack과 Queue

### Stack과 Queue의 기본 개념

`Stack`은 마지막에 저장한 데이터를 가장 먼저 꺼내게 되는 **LIFO(Last In First Out) 구조**

`Queue`는 처음에 저장한 데이터를 가장 먼저 꺼내게 되는 **FIFO(First In First Out) 구조**

![스택과 큐 구조](https://user-images.githubusercontent.com/56071088/126280876-9fdaec38-c222-43d4-ace8-2663a5d3810f.png)

Stack과 Queue를 구현하기 위해서는 어떤 Collection Class를 사용하는 것이 좋을까?

- 순차적으로 Data를 추가하고 삭제하는 `Stack`에는 **ArrayList와 같은 배열기반의 Collection Class가 적합**
- `Queue`는 **Data를 꺼낼 때 항상 첫 번째 Data를 삭제**하므로, ArrayList보다 **데이터의 추가/삭제가 쉬운 LinkedList로 구현하는 것이 더 적합**하다.
  - ArrayList와 같은 배열기반의 컬렉션 클래스를 사용한다면 **데이터를 꺼낼 때 마다 빈 공간을 채우기 위해 데이터의 복사가 발생하므로 비효율적**

### Stack의 메서드
- 추가 : push
- 삭제 : pop
- 맨 위 조회: peek
- 조회: search

![Stack메서드](https://user-images.githubusercontent.com/56071088/126281458-4d2988be-8eda-454c-8b85-52e2c0e45e5b.png)

### Queue의 메서드

- 추가 : add(exception발생) vs offer
- 삭제 : remove(exception발생) vs poll
- 맨 앞 조회 : element(exception발생) vs peek

![Queue메서드](https://user-images.githubusercontent.com/56071088/126281768-5e96cc2d-e378-41d7-86d2-bd6bea1a40a4.png)

#### Stack과 Queue 사용 예시

```java
import java.util.*;

class StackQueueEx {
	public static void main(String[] args) {
		Stack st = new Stack();
		Queue q = new LinkedList();	 // Queue인터페이스의 구현체인 LinkedList를 사용
		
		st.push("0");
		st.push("1");
		st.push("2");

		q.offer("0");
		q.offer("1");
		q.offer("2");

		System.out.println("= Stack =");
		while(!st.empty()) {
			System.out.println(st.pop());
		}

		System.out.println("= Queue =");
		while(!q.isEmpty()) {
			System.out.println(q.poll());
		}
	}
}
```

```json
실행결과
stack
2
1
0
queue
0
1
2
```

```java
Stack st = new Stack();
Queue q = new LinkedList();	 // Queue인터페이스의 구현체인 LinkedList를 사용
```

- Java에서는 스택을 **Stack클래스로 구현하여 제공**하고 있지만 
- 큐는 **Queue인터페이스로만 정의해 놓았을 뿐 별도의 클래스를 제공하고 있지 않다.** 
  - 대신 Queue인터페이스를 구현한 class들이 있어서 이 들 중의 하나를 선택해서 사용하면 된다.

### Stack 직접 구현하기

```java
import java.util.*;

class MyStack extends Vector {
  public Object push(Object item) {
		addElement(item);
		return item;
  }

  public Object pop() {
		Object obj = peek();	 // Stack에 저장된 마지막 요소를 읽어온다.
		// 만일 Stack이 비어있으면 peek()메서드가 EmptyStackException을 발생시킨다.
	  // 마지막 요소를 삭제한다. 배열의 index가 0 부터 시작하므로 1을 빼준다.
		removeElementAt(size() - 1); 
		return obj;
  }

  public Object peek() {
		int	len = size();
	
		if (len == 0) {
			throw new EmptyStackException();
    }

    // 마지막 요소를 반환한다. 배열의 index가 0 부터 시작하므로 1을 빼준다.
		return elementAt(len - 1);	
  }

  public boolean empty() {
		return size() == 0;
	}
    
  public int search(Object o) {
		int i = lastIndexOf(o);	// 끝에서부터 객체를 찾는다. 
		// 반환값은 저장된 위치(배열의 index)이다.
		if (i >= 0) { // 객체를 찾은 경우
			return size() - i; // Stack은 맨 위에 저장된 객체의 index를 1로 정의하기 때문에 계산을 통해서 구한다.
	  }
    
    return - 1;		// 해당 객체를 찾지 못하면 -1를 반환한다.
	}

}
```

### Stack과 Queue의 활용

- **Stack의 활용 예시**
  - 수식 계산, 수식괄호검사. 워드프로세서의 undo/redo, 웹브라우저의 뒤로/앞으로
- **Queue의 활용 예시**
  - 최근사용문서, 인쇄작업 대기목록, 버퍼(buffer)

1. `웹 브라우저의 뒤로/앞으로` 버튼의 기능을 구현한 예시
   - **2개의 Stack을 사용해야 한다.**
   - 워드 프로세서의 되돌리기 기능(undo/redo)도 동일한 방식으로 구현 가능

```java
import java.util.*;

public class StackEx1 {
	public static Stack back    = new Stack();
	public static Stack forward = new Stack();  

	public static void main(String[] args) {
		goURL("1.네이트");
		goURL("2.야후");
		goURL("3.네이버");
		goURL("4.다음");

		printStatus();

		goBack();
		System.out.println("= 뒤로가기 버튼을 누른 후 =");  
		printStatus();

		goBack();
		System.out.println("= '뒤로' 버튼을 누른 후 =");  
		printStatus();

		goForward();
		System.out.println("= '앞으로' 버튼을 누른 후 =");  
		printStatus();

		goURL("codechobo.com");
		System.out.println("= 새로운 주소로 이동 후 =");  
		printStatus();
	}

	public static void printStatus() {
		System.out.println("back:"+back);
		System.out.println("forward:"+forward);
		System.out.println("현재화면은 '" + back.peek()+"' 입니다.");  
		System.out.println();
	}

	public static void goURL(String url){
		back.push(url);
		if(!forward.empty()) 
			forward.clear();
	}

	public static void goForward(){
		if(!forward.empty())
			back.push(forward.pop());
	}

	public static void goBack(){
		if(!back.empty())
			forward.push(back.pop());
	}
}
```

```json
실행결과
back:[1.네이트, 2.야후, 3.네이버, 4.다음]
foward:[]
현재화면은 '4.다음' 입니다.

= 뒤로 버튼 누른 후 =
back:[1.네이트, 2.야후, 3.네이버]
foward:[4.다음]
현재화면은 '3.네이버' 입니다.

= 뒤로 버튼 누른 후 =
back:[1.네이트, 2.야후]
foward:[4.다음, 3.네이버]
현재화면은 '2.야후' 입니다.

= 앞으로 버튼 누른 후 =
back:[1.네이트, 2.야후, 3.네이버]
foward:[4.다음]
현재화면은 '3.네이버' 입니다.

= 새로운 주소로 이동후 =
back:[1.네이트, 2.야후, 3.네이버, codechobo.com]
foward:[]
현재화면은 'codechobo.com' 입니다.
```

### PriorityQueue

`PriorityQueue` : Queue인터페이스의 구현체 중 하나

- 저장한 순서에 관계없이 우선순위(priority)가 높은 것 부터 꺼내게 된다.
- null은 저장할 수 없다.
  - null을 저장하면 NullPointerException이 발생한다.
- 저장공간으로 배열을 사용
- 각 요소를 `힙(heap)` 이라는 자료구조의 형태로 저장한다.
  - `힙(heap)` : 이진트리의 한 종류, 가장 큰 값이나 가장 작은 값을 빠르게 찾을 수 있다.

```java
import java.util.*;

class PriorityQueueEx {
	public static void main(String[] args) {
		Queue pq = new PriorityQueue();
		pq.offer(3);  // pq.offer(new Integer(3)); 오토박싱
		pq.offer(1);
		pq.offer(5);
		pq.offer(2);
		pq.offer(4);

		System.out.println(pq); // pq의 내부 배열을 출력

		Object obj = null;

		// PriorityQueue에 저장된 요소를 하나씩 꺼낸다.
		while((obj = pq.poll())!=null) 
			System.out.println(obj);
	}

}
```

```
[1, 2, 5, 3, 4]
1
2
3
4
5
```

- 숫자 뿐만 아니라 객체를 저장할 수도 있다.
  - 객체를 저장하는 경우, 각 객체의 크기를 비교할 수 있는 방법을 제공해야 한다.
  - pq.offer(new Integer(3)); // 오토박싱
  - 예제에서는 정수를 사용했는데, Compiler가 Integer를 auto-boxing해준다.
    - Integer와 같은 Number의 자손들은 숫자를 비교하는 방법을 정의하고 있기 때문에 비교 방법을 지정해주지 않아도 된다.

### Deque(Double-Ended Queue)

Queue의 변형, `Deque`은 양쪽 끝에 추가/삭제가 가능하다.
- Deque의 조성은 Queue이며, 구현체로는 ArrayList, LinkedList 등이 있다.
- Deque은 Stack과 Queue를 합쳐놓은 것과 같으며 스택으로도 사용할 수도 있고, Queue로 사용할 수도 있다. 

|Deque|Queue|Stack|
|---|---|---|
|offerFirst()|-|-|
|offerLast()|offer()|push()|
|pollLast()|-|pop()|
|pollFirst()|poll()|-|
|peekFirst()|peek()||
|peekLast()|-|peek()|

## Iterator, ListIterator, Enumeration

`Iterator, ListIterator, Enumeration` 모두 Collection에 저장된 요소를 접근하는데 사용되는 인터페이스이다.
- Enumeration은 Iterator의 구버젼이며,
- ListIterator는 Iterator의 기능을 향상 시킨 것이다.

### Iterator

`Collection Framework`에서는 **Collection에 저장된 요소들을 읽어오는 방법을 표준화하였다.**

- Collection에 저장된 각 요소에 접근하는 기능을 가진 **Iterator인터페이스를 정의**하고, 
- `Collection인터페이스`에는 **Iterator(Iterator를 구현한 클래스의 인스턴스)** 를 **반환하는 iterator()를 정의** 하고 있다.

```java
public interface Iterator {
  boolean hasNext();
  Object next();
  void remove();
}

public interface Collection {
  ...
  public Iterator iterator();
  ...
}
```

iterator()는 Collection인터페이스에 정의돤 메서드이므로 Collection인터페이스의 자손인 List와 Set에도 포함되어 있다.
- 그래서 List, Set인터페이스를 구현하는 컬렉션은 iterator()가 각 컬렉션의 특징에 알맞게 작성되어 있다.

컬렉션 클래스에 대해 iterator()를 호출하여 Iterator를 얻은 다음 반복문, 주로 while문을 사용해서 컬렉션 클래스의 요소들을 읽어 올 수 있다.

#### Iterator인터페이스의 메서드

![iterator메서드](https://user-images.githubusercontent.com/56071088/126301982-f1ad8f28-d872-4b66-abaf-b7638954a0ce.png)

#### ArrayList Iterator 예시

```java
List list = new ArrayList(); // 다른 컬렉션으로 변경할 때는 이 부분만 고치면 된다.
Iterator it = list.iterator();

while(it.hasNext()) {
  System.out.println(it.next());
}
```

ArrayList대신 List인터페이스를 구현한 다른 컬렉션 클래스에 대해서도 이와 동일한 코드를 사용할 수 있다. 
- 첫 줄에서 ArrayList대신 **List인터페이스를 구현한 다른 컬렉션 클래스의 객체를 생성하도록 변경하기만 하면 된다.**

**Iterator를 이용해서 컬렉션의 요소를 읽어오는 방법을 표준화**했기 때문에 **코드의 재사용성을 높이는 것이 가능**
- **공통 인터페이스를 정의**해서 **표준을 정의하고 구현**하여 **표준을 따르도록 함**으로써
  - 코드의 일관성 유지, 재사용성 극대화  

#### Map인터페이스 Iterator 예시

Map인터페이스를 구현한 컬렉션 클래스는 **key, value을 쌍(pair)으로 저장**하고 있기 때문에 **iterator()를 직접 호출할 수 없다.**
- 그 대신 keySet(), entrySet() 과 같은 메서드를 사용하여
- key와 value를 각각 따로 Set의 형태로 얻어 온 후에 다시 iterator()를 호출해야 Iterator를 얻을 수 있다.

```java
Map map = new HashMap();
...
// 아래의 두 문장을 하나로 합친 것
Iterator it = map.entrySet().iterator();

-->
Set eSet = map.entrySet();
Iterator it = eSet.iterator();
```

map.entrySet()의 실행결과가 Set이므로 map.entrySet()을 통해 얻은 Set인스턴스의 iterator()를 호출해서 Iterator인스턴스를 얻는다. 마지막으로 Iterator인스턴스의 참조가 it에 저장된다.

```java
import java.util.*;

class IteratorEx1 {
	public static void main(String[] args) {
		ArrayList list = new ArrayList();
		list.add("1");
		list.add("2");
		list.add("3");
		list.add("4");
		list.add("5");

		Iterator it = list.iterator();

		while(it.hasNext()) {
			Object obj = it.next();
			System.out.println(obj);
		}
	} 
}
```

```json
실행결과
1
2
3
4
5
```

- `List클래스들`은 **저장순서를 유지** -> Iterator를 이용해서 읽어 온 결과 역시 **저장순서와 동일**
- `Set클래스들`은 **각 요소간의 순서가 유지 되지 않는다.** -> Iterator를 이용해서 저장된 요소들을 읽어 와도 **처음에 저장된 순서와 같지 않다.**

### ListIterator와 Enumeration

`Enumeration` : Iterator의 구버전

`ListIterator` : Iterator의 양방향 조회기능 추가(List를 구현한 경우만 사용가능)
### Enumeration

`Enumeration`은 **Collection Framework이 만들어지기 이전에 사용하던 것**으로 `Iterator의 구버젼`이라고 생각하면 된다.
- 이전 버젼으로 작성된 소스와의 호환을 위해서 남겨 두고 있을 뿐이므로 **Enumeration 대신 Iterator를 사용하도록 하자.**

### ListIterator

`ListIterator` : Iterator를 상속받아서 기능을 추가한 것
- 컬렉션의 요소에 접근할 때
  - Iterator는 단방향으로 이동
  - **ListIterator는 양방향으로의 이동이 가능** (다만, ArrayList나 LinkedList와 같이 List인터페이스를 구현한 컬렉션에서만 사용 가능)

### Enumeration메서드, ListIterator메서드

- Enumeration과 Iterator는 메서드이름만 다를 뿐 기능은 같다.

[Enumeration메서드]

![Enumeration메서드](https://user-images.githubusercontent.com/56071088/126354945-3f5707bf-6659-4348-8e2a-3caca5523ffc.png)

- ListIterator는 Iterator에 이전방향으로의 접근 기능을 추가한 것

[ListIterator메서드]

![ListIterator메서드](https://user-images.githubusercontent.com/56071088/126354955-f7d1a931-afdd-442f-bf87-bd0b58c960b7.png)

```java
import java.util.*;

class ListIteratorEx1 {
	public static void main(String[] args) {
		ArrayList list = new ArrayList();
		list.add("1");
		list.add("2");
		list.add("3");
		list.add("4");
		list.add("5");

		ListIterator it = list.listIterator();

		while(it.hasNext()) {
			System.out.print(it.next()); // 순방향으로 진행하면서 읽어온다.
		}
		System.out.println();

		while(it.hasPrevious()) {
			System.out.print(it.previous()); // 역방향으로 진행하면서 읽어온다.
		}
		System.out.println();
	}
}
```

```json
실행결과
12345
54321
```

```java
ListIterator it = list.listIterator();
```

- `Iterator`는 단방향으로만 이동, 컬렉션의 마지막 요소에 다다르면 더 이상 사용할 수 없다.
- `ListIterator`는 양방향으로 이동 가능, 각 요소간의 이동이 자유롭다.
  - 이동하기 전에 반드시 hasNext(), hasPrevious()를 호출해서 이동할 수 있는지 확인하자!

**ListIterator**에 있는 메서드 중 `선택적 기능(Optional Operation)`이라고 표시된 것들은 **반드시 구현하지 않아도 된다.**
- **인터페이스로부터 상속받은 메서드**는 **추상메서드**라 **메서드의 body를 반드시 구현**해야 한다.
- 이때 **예외(UnsupportedOpeationException)를 던져서 구현되지 않은 기능**이라는 것을 **메서드 호출하는 쪽에 알리는 것이 좋다.**
  - 그렇지 않으면 호출하는 쪽에서 소스를 구해보기 전까지는 **이 기능이 바르게 동작하는지 알 방법이 없다.** 

```java
public void remove() {
  throw new UnsupportedOpeationException();
}
```

- **remove()메서드의 선언부에 예외처리를 하지 않은 이유**는 **UnsupportedOpeationException이 RuntimeException의 자손**이기 때문이다.

Iterator의 remove()는 단독으로 쓰일 수 없고, next()와 같이 써야 한다.
- 특정위치의 요소를 삭제하는 것이 아니라 **읽어 온 것을 삭제**
- **next()의 호출 없이 remove()를 호출**하면, **IllegalStateException이 발생**

```java
ArrayList original = new ArrayList(10);
ArrayList copy2 = new ArrayList(10);

it = original.iterator(); // Iterator는 재사용이 안되므로, 처음부터 다시 돌리려면 다시 얻어와야 한다.

while(!it.hasNext()) {
  copy2.add(it.next());
  it.remove(); // Iterator의 remove()는 next()를 호출해야만 remove()를 호출 가능
}
```

#### Iterator구현 예제

```java
import java.util.*;

public class MyVector2 extends MyVector implements Iterator {
	int cursor  = 0;
	int lastRet = -1;
	
	public MyVector2(int capacity) {
		super(capacity);		
	}
	
	public MyVector2() {
		this(10);		
	}

	public String toString() {
		String tmp = "";
		Iterator it = iterator();

		for(int i=0; it.hasNext();i++) {
			if(i!=0) tmp+=", ";
			tmp += it.next(); 	// tmp += next().toString();
		}

		return "["+ tmp +"]";		
	}

	public Iterator iterator() {
		cursor=0;		// cursor와 lastRet를 초기화 한다.
		lastRet = -1;
		return this;		
	}	
	
	public boolean hasNext() {
	  return cursor != size();
	}
	
  public Object next(){
		Object next = get(cursor);
		lastRet = cursor++;
		return next;
  }
	
	public void remove() {
    // 더이상 삭제할 것이 없으면 IllegalStateException를 발생시킨다.
		if(lastRet == -1) {  
			throw new IllegalStateException();
		} 
    else {
			remove(lastRet);
			cursor--;           // 삭제 후에 cursor의 위치를 감소시킨다.
			lastRet = -1;		// lastRet의 값을 초기화 한다.	
		}
	}		
} // class
```

`cursor` : **앞으로 읽어 올 요소의 위치를 저장**

`lastRet` : **마지막으로 읽어 온 요소의 위치(index)를 저장**


- `lastRet`은 `cursor`보다 **항상 1이 작은 값이 저장**되고,
- remove()를 호출하면 이미 next()를 통해서 읽은 위치의 요소, 즉 lastRet에 저장된 값의 위치에 있는 요소를 삭제,
- lastRet의 값을 -1로 초기화
- 만일 next()를 호출하지 않고 remove()를 호출하면, lastRet의 값은 -1이 되어 IllegalStateException발생
- remove()는 next()로 읽어 온 객체를 삭제하는 것이기 때문에 remove()를 호출하기 전에 반드시 next()를 호출해야 한다.

```java
public Object next(){
	Object next = get(cursor);
	lastRet = cursor++;
	return next;
}

public void remove() {
  // 더이상 삭제할 것이 없으면 IllegalStateException를 발생시킨다.
	if(lastRet == -1) {  
		throw new IllegalStateException();
	} 
  else {
		remove(lastRet);
		cursor--;           // 삭제 후에 cursor의 위치를 감소시킨다.
		lastRet = -1;		// lastRet의 값을 초기화 한다.	
	}
}
```

- remove(lastRet)을 호출하여 lastRet의 위치에 있는 객체를 삭제한 다음, **cursor의 값을 감소, lastRet의 값을 -1로 초기화**
  - remove()메서드를 호출해서 객체를 삭제하고 나면, **삭제된 위치 이후의 객체들이 빈 공간을 채우기 위해 자동적으로 이동**되기 때문에 **cursor의 위치도 같이 이동시켜줘여 한다.**
  - **읽어온 요소가 삭제되었으므로 읽어온 요소의 값이 없어졌다는 의미**로 **lastRet을 -1로 초기화**

**lastRet의 값이 -1 : 읽어온 값이 없다는 것을 의미**

```java
class MyVector2Test {
	public static void main(String args[]) {
		MyVector2 v = new MyVector2();
		v.add("0");
		v.add("1");
		v.add("2");
		v.add("3");
		v.add("4");

		System.out.println("삭제 전 : " + v);
		Iterator it = v.iterator();
		it.next();
		it.remove();
		it.next();
		it.remove();

		System.out.println("삭제 후 : " + v);

    MyVector2 v2 = new MyVector2();
		v2.add("0");
		v2.add("1");
		v2.add("2");
		v2.add("3");
		v2.add("4");

		System.out.println("삭제 전 : " + v2);
		Iterator it = v2.iterator();
		
    it.next();
		it.remove();
		it.next();
		it.remove();
    it.next();
		it.remove();

		System.out.println("remove() 주석 처리 후 : " + v2);
	}
}

/*
public void remove() {
  if(lastRet == -1) {
    throw new IllegalStateException();
  }
  else {
    remove(lastRet);
    // cursor--; // 주석처리한다.
    lastRet = -1;
  }
}
*/
```

```json
실행결과
삭제 전 : [0, 1, 2, 3, 4]
삭제 후 : [2, 3, 4]

삭제 전 : [0, 1, 2, 3, 4]
remove() 주석 처리 후 : [1, 3]
```

- remove()에서 cursor--; 에 대한 코드를 주석처리한다면

  - Data 0을 삭제한 후, 1이었던 cursor가 다시 Index 0이 되어서 next()메서드를 호출했을 때, lastRet을 다시 첫번째 요소(Index 0)인 Data 1을 가리키도록 만들어야 하지만
  
  - 주석처리를 하면, 그대로 cursor가 Index 1이므로 next()메서드 호출 시 Index 2가 되고, lastRet이 Index 1이 되므로
  
    - remove()메서드 호출 시, 2번째 요소(Index 1)인 Data 2를 삭제하게 된다.
- 같은 방식으로 요소를 하나 더 삭제하면 Data 4를 삭제하게 될 것이다.

## Arrays

`Arrays`  
- class이다. 
- **같은 기능의 메서드**가 **배열의 타입만 다르게 오버로딩(Overloading)** 되어 있다. 
  - **모든 기본형 배열**과 **참조형 배열 별로 하나씩 정의**
- **Arrays클래스에 정의된 모든 메서드는 static 메서드**

[Arrays클래스 함수 예시]

```java
static String toString(boolean[] a);
static String toString(byte[] a);
static String toString(char[] a);
static String toString(short[] a);
static String toString(int[] a);
static String toString(long[] a);
static String toString(float[] a);
static String toString(double[] a);
static String toString(Object[] a);
```

### 배열의 복사 - cpoyOf(), copyOfRange()

`copyOf()` : **배열 전체를 복사 후, 새로운 배열을 만들어 반환**

`copyOfRange()` : **배열 일부를 복사 후, 새로운 배열을 만들어 반환**
- copyOfRange()의 지정된 범위의 끝은 포함되지 않는다.

```java
int[] arr = {0, 1, 2, 3, 4};
int[] arr2 = Arrays.copyOf(arr, arr.length); // arr2 = [0,1,2,3,4]
int[] arr3 = Arrays.copyOf(arr, 3); // arr3 = [0,1,2]
int[] arr4 = Arrays.copyOf(arr, 7); // arr4 = [0,1,2,3,4,0,0]
int[] arr5 = Arrays.copyOfRange(arr, 2, 4); // arr5 = [2, 3] <-- 4는 불포함
int[] arr6 = Arrays.copyOfRange(arr, 0, 7); // arr6 = [0,1,2,3,4,0,0]
```

### 배열 채우기 - fill(), setAll()

`fill()` : 배열의 모든 요소를 지정된 값으로 채운다.

`setAll()` : **배열을 채우는데** 사용할 **함수형 인터페이스를 매개변수로 받는다.** 이 함수형 인터페이스를 구현한 객체 혹은 람다식이 반환한 값들로 배열을 채운다.
- 메서드를 호출할 때, 
  - **함수형 인터페이스를 구현한 객체**를 **매개변수로 지정**
  - **람다식을 매개변수로 지정**

```java
int[] arr = new int[5];
Arrays.fill(arr, 9); // arr = [9,9,9,9,9]
Arrays.setAll(arr, i -> (int)(Math.random()*5)+1); // arr = [1,5,2,1,1]
```

- `i -> (int)(Math.random()*5)+1`
  - 람다식(lambda expression)

### 배열의 정렬과 검색 - sort(), binarySearch()

`sort()` : 배열을 정렬한다.

`binarySearch()` : 배열에 저장된 요소를 검색 후, 지정된 값이 **저장된 위치(index)를 찾아서 반환**
- **반드시 배열이 정렬된 상태**이어야 올바른 결과를 반환한다.
- 만일 **검색한 값과 일치하는 요소들이 여러 개 있다면, 이 중에서 어떤 것의 위치가 반환될지는 알 수 없다!!**
- 배열을 검색할 때, 범위를 절반씩 줄여서 검색하기 때문에, 검색 속도가 상당히 빠르다.
- 큰 배열의 검색에 유리 

```java
int[] arr = {3,2,0,1,4};
int idx = Arrays.binarySearch(arr, 2); // -5, 잘못된 결과

Arrays.sort(arr);
int idx = Arrays.binarySearch(arr, 2); // 2, 올바른 결과
```

### 문자열의 비교와 출력 - equals(), toString()

**모든 요소를 문자열로 편하게 출력**

`toString()` : **일차원 배열**에만 사용 가능

`deepToString()` : **다차원 배열**에서 사용 가능
- **배열의 모든 요소를 재귀적으로 접근해서 문자열을 구성**
- 2차원 뿐만 아니라 **3차원 이상의 배열에도 동작**한다.

```java
int[] arr = {0,1,2,3,4};
int[][] arr2D = {{11,22}, {33,44}};

Arrays.toString(arr); // [0, 1, 2, 3, 4]
Arrays.deepToString(arr2D); // [[11, 22], [33, 44]]
```

`equals()` : **일차원 배열**에만 사용 가능, **두 배열에 저장된 모든 요소를 비교해서 같으면 true, 다르면 false를 반환**
- 다차원 배열은 배열의 배열로 구성되어 있다.
- 다차원 배열을 equals()메서드로 비교하면
  - **'배열에 저장된 배열의 주소'를 비교**, **서로 다른 배열은 항상 주소가 다르므로 false를 결과로 얻는다.**

`deepEquals()` : **다차원 배열**에서 사용 가능

```java
String[][] str2D = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};
String[][] str2D2 = new String[][]{{"aaa", "bbb"}, {"AAA", "BBB"}};

Arrays.equals(str2D, str2D2); // false
Arrays.equals(str2D, str2D2); // true
```

### 배열을 List로 변환 - asList(Object... a) 

`asList()` : 배열을 List에 담아서 반환
- 매개변수의 타입이 가변인수, 배열 생성 없이 저장할 요소들만 나열하는 것도 가능하다.
- asList()가 반환한 List의 크기를 변경할 수 없다.
  - 추가/삭제가 불가능
  - 이미 저장된 내용은 변경 가능
  - 크기를 변경할 수 있는 List가 필요하다면 
  - `List list = new ArrayList(Arrays.asList(1,2,3,4,5));
` 

```java
List list = Arrays.asList(new Integer[]{1,2,3,4,5}); // list = [1,2,3,4,5]
List list = Arrays.asList(1,2,3,4,5);  // list = [1,2,3,4,5]
list.add(6); // UnsupportedOperationException 예외 발생


List list = new ArrayList(Arrays.asList(1,2,3,4,5));
```

### parallelxxx(), spliterator(), stream()

`parallel로 시작하는 이름의 메서드들` : 보다 빠른 결과를 위해, 여러 쓰레드가 작업을 나누어 처리하도록 한다.

`spliterator()` : 여러 쓰레드가 처리할 수 있게 하나의 작업을 여러 작업으로 나누는 Spliterator를 반환

`stream()` : 컬렉션을 스트림으로 반환


### 전체 메서드 사용 예시

```java
import java.util.*;

class ArraysEx {
	public static void main(String[] args) {
		int[]	arr   =  {0,1,2,3,4};
		int[][] arr2D =  {{11,12,13}, {21,22,23}};

		System.out.println("arr="+Arrays.toString(arr));
		System.out.println("arr2D="+Arrays.deepToString(arr2D));

		int[] arr2 = Arrays.copyOf(arr, arr.length);
		int[] arr3 = Arrays.copyOf(arr, 3);          
		int[] arr4 = Arrays.copyOf(arr, 7);          
		int[] arr5 = Arrays.copyOfRange(arr, 2, 4);  
		int[] arr6 = Arrays.copyOfRange(arr, 0, 7);  

		System.out.println("arr2="+ Arrays.toString(arr2));
		System.out.println("arr3="+ Arrays.toString(arr3));
		System.out.println("arr4="+ Arrays.toString(arr4));
		System.out.println("arr5="+ Arrays.toString(arr5));
		System.out.println("arr6="+ Arrays.toString(arr6));

		int[] arr7 =  new int[5];
		Arrays.fill(arr7, 9);  // arr=[9,9,9,9,9]
		System.out.println("arr7="+Arrays.toString(arr7));

		Arrays.setAll(arr7, i -> (int)(Math.random()*6)+1);
		System.out.println("arr7="+Arrays.toString(arr7));

		for(int i : arr7) {
			char[] graph = new char[i];
			Arrays.fill(graph, '*');
			System.out.println(new String(graph)+i);
		}

		String[][] str2D  = new String[][]{{"aaa","bbb"},{"AAA","BBB"}};
		String[][] str2D2 = new String[][]{{"aaa","bbb"},{"AAA","BBB"}};

		System.out.println(Arrays.equals(str2D, str2D2));      // false
		System.out.println(Arrays.deepEquals(str2D, str2D2));  // true

		char[] chArr = { 'A', 'D', 'C',  'B', 'E' };

		int idx = Arrays.binarySearch(chArr, 'B');
		System.out.println("chArr="+Arrays.toString(chArr));
		System.out.println("index of B ="+Arrays.binarySearch(chArr, 'B'));
		System.out.println("= After sorting =");
		Arrays.sort(chArr);
		System.out.println("chArr="+Arrays.toString(chArr));
		System.out.println("index of B ="+Arrays.binarySearch(chArr, 'B'));
	}
}
```

### Comparable와 Comparator

`Comparable와 Comparator`는 **모두 인터페이스**로 **컬렉션을 정렬**하는데 필요한 메서드를 정의

[Comparable와 Comparator]

```java
public interface Comparable {
	public int compareTo(Object o);
}

public interface Comparator {
	int compare(Object o1, Object o2);
	boolean equals(Object obj);
}
```

`Comparable` : **기본 정렬기준**을 구현하는데 사용
- Comparable을 구현하고 있는 클래스들은 **같은 타입의 인스터스끼리 서로 비교**할 수 있는 클래스들
- 주로 Integer와 같은 wrapper클래스와 String, Date, File과 같은 클래스들
  - 기본적으로 오름차순 정렬
- **Comparable을 구현한 클래스**는 **정렬이 가능하다는 것을 의미** 
- java.lang패키지

`Comparator` : 기본 정렬기준 외에 다른 기준으로 정렬하고자할 때 사용
- java.util패키지

`compareTo(), compare()` : 선언형태와 이름이 약간 다를 뿐 두 객체를 비교한다는 같은 기능을 목적으로 고안
- 반환값은 int, 비교하는 두 객체가 같으면 0, 비교하는 값 보다 작으면 음수, 크다면 양수를 반환하도록 구현

`equals()` : Comparator를 구현하는 클래스는 오버라이딩이 필요할 수도 있다는 것을 알리기 위해서 정의한 것 뿐이다.
- 그냥 compare(Object o1, Object o2)만 구현하면 된다.

[실제 Integer클래스의 일부]

```java
public final Integer extends Number implements Comaprable {
		public int compareTo(Object o) {
				return compareTo((Integer)o);
		}
		public int compareTo(Integer anotherInteger) {
				int thisVal = this.value;
				int anotherVal = anotherInteger.value;
				return thisVal < anotherVal ? -1 : (thisVal == anotherVal ? 0 : 1)
		}
}
```

[Comparator구현 예시 - 내림차순(Descending)]

```java
import java.util.*;

class ComparatorEx {
	public static void main(String[] args) {
		String[] strArr = {"cat", "Dog", "lion", "tiger"};

		Arrays.sort(strArr); // String의 Comparable구현에 의한 정렬
		System.out.println("strArr=" + Arrays.toString(strArr));

		Arrays.sort(strArr, String.CASE_INSENSITIVE_ORDER); // 대소문자 구분안함
		System.out.println("strArr=" + Arrays.toString(strArr));

		Arrays.sort(strArr, new Descending()); // 역순 정렬
		System.out.println("strArr=" + Arrays.toString(strArr));
	}
}

class Descending implements Comparator { 
	public int compare(Object o1, Object o2){
		if( o1 instanceof Comparable && o2 instanceof Comparable) {
			Comparable c1 = (Comparable)o1;
			Comparable c2 = (Comparable)o2;
			return c1.compareTo(c2) * -1 ; // -1을 곱해서 기본 정렬방식의 역으로 변경한다.
			
			// 또는 c2.compareTo(c1)와 같이 순서를 바꿔도 된다.
		}
		return -1;
	} 
}
```

```json
실행결과

strArr=[Dog, cat, lion, tiger]
strArr=[cat, Dog, lion, tiger]
strArr=[tiger, lion, cat, Dog]
```

`Arrays.sort()` : 배열을 정렬할 때, 따로 Comparator을 지정해주지 않으면 저장하는 객체(주로 Comparable을 구현한 객체)에 구현된 내용에 따라 정렬

```java
// 객체 배열에 저장된 객체가 구현한 Comparable에 의한 정렬
static void sort(Object[] a) 

// 지정한 Compartor 의한 정렬
static void sort(Object[] a, Comaprator c)
```

String의 Comparable구현은 **문자열을 사전 순으로 정렬**
- **문자열의 오름차순 정렬 : 공백, 숫자, 대문자, 소문자 순으로 정렬**
  - **문자의 unicode 순서**가 **작은 값부터 큰 값으로 정렬**

`String.CASE_INSENSITIVE_ORDER` : **문자열의 대소문자를 구분하지 않고** 비교하는 **Comparator를 상수의 형태**로 정의

```java
public static final Comparator CASE_INSENSITIVE_ORDER;

Arrays.sort(strArr, String.CASE_INSENSITIVE_ORDER); // 대소문자 구분없이 정렬
```

문자열 내림차순(Descending) 정렬 - Comparator인터페이스 구현

1. String에 구현된 **compareTo()의 결과에 -1 을 곱한다.**
2. 비교하는 **객체의 위치를 바꿔서 c2.compareTo(c1)** 의 형태로 만든다.

- **compare의 매개변수가 Object타입이기 때문에** int compareTo(Object o)를 바로 호출할 수 없으므로 **Comparable타입으로 형변환**해야 한다!!

```java
class Descending implements Comparator { 
	public int compare(Object o1, Object o2){
		if( o1 instanceof Comparable && o2 instanceof Comparable) {
			Comparable c1 = (Comparable)o1;
			Comparable c2 = (Comparable)o2;
			return c1.compareTo(c2) * -1 ; // -1을 곱해서 기본 정렬방식의 역으로 변경한다.
			
			// 또는 c2.compareTo(c1)와 같이 순서를 바꿔도 된다.
		}
		return -1;
	} 
}
```

## HashSet

`HashSet` : `Set인터페이스`를 구현한 가장 대표적인 컬렉션, Set인터페이스의 특징대로 **HashSet은 중복된 요소를 저장하지 않는다.**
- `add(), addAll()`을 사용하여 새로운 요소를 추가한다.
  - **중복된 요소를 추가할 시 false를 반환함**으로써 추가에 실패했다는 것을 알려준다. 
- 중복을 허용하지 않는 Set의 특징을 이용하여 **컬렉션 내의 중복된 요소들을 쉽게 제거 가능**
- **HashSet은 내부적으로 HashMap을 이용해서 만들어졌으며, HashSet이란 이름은 해싱(hashing)을 이용해서 구현했기 때문에 붙여진 이름이다.**

![hashSet메서드](https://user-images.githubusercontent.com/56071088/126446153-aad60ea6-ad09-4fbe-b0bf-4fc9ab4e889d.png)

```java
HashSet(int initialCapacity, float loadFactor) // 초기용량과 loadFactor를 지정하는 생성자 
```

- `loadFactor` : **컬렉션 클래스에 저장공간이 가득 차기 전에 미리 용량 확보하기 위한 것**
  - **기본 값 : 0.75 : 즉, 75%**
  - 이 값을 0.8로 지정하면 저장공간의 80%가 채워졌을 때 용량이 2배로 늘어난다. 

- `addAll(Collection c)` : **합집합**, 주어진 컬렉션에 저장된 모든 객체들을 추가
- `retainAll(Collection c)` : **교집합**, 주어진 컬렉션에 저장된 모든 객체와 동일한 것만 남기고 삭제
- `removeAll(Collection c)` : **차집합**, 주어진 컬렉션에 저장된 객체와 동일한 것들을 HashSet에서 모두 삭제


[HashSet 사용 예시]

```java
import java.util.*;

class HashSetEx1 {
	public static void main(String[] args) {
		Object[] objArr = {"1",new Integer(1),"2","2","3","3","4","4","4"};
		Set set = new HashSet();

		for(int i=0; i < objArr.length; i++) {
			set.add(objArr[i]);	// HashSet에 objArr의 요소들을 저장한다.
		}
    // HashSet에 저장된 요소들을 출력한다.
		System.out.println(set);	
	}
}
```

```json
실행결과
[1, 1, 2, 3, 4]
```

중복을 허용하지 않는다고 했는데, 1이 2번 출력되었다.

- 하나는 **String인스턴스**, 다른 하나는 **Integer인스턴스**로 **서로 다른 객체이므로 중복으로 간주하지 않는다.**

`LinkedHashSet` : **중복을 제거하는 동시에 저장한 순서를 유지한다.**

`HashSet` : **중복을 제거하고, 저장한 순서는 유지하지 않는다.**
- **저장된 순서를 보장하지 않고 자체적인 저장방식에 따라 순서가 결정된다.**

[HashSet 사용 예시 - 정렬(Collections.sort())]

```java
import java.util.*;

class HashSetLotto {
	public static void main(String[] args) {
		Set set = new HashSet();
		
		for (int i = 0; set.size() < 6 ; i++) {
			int num = (int)(Math.random()*45) + 1;
			set.add(new Integer(num));
		}

		List list = new LinkedList(set);	// LinkedList(Collection c)
		Collections.sort(list);				// Collections.sort(List list)
		System.out.println(list);
	}
}
```

- 정렬을 위해 `Collections.sort(List list)`를 사용
  - 인자로 List인터페이스 타입을 필요로 하므로, `LinkedList(Collection c) 생성자` 를 사용해서 LinkedList를 생성
- 컬렉션에 저장된 객체가 Integer이기 때문에 Integer클래스에 정의된 기본정렬이 사용
  - 정렬기준을 변경하는 방법은 Collections클래스에서 다룬다.

```java
import java.util.*;

class HashSetEx3 {
	public static void main(String[] args) {
		HashSet set = new HashSet();

		set.add("abc");
		set.add("abc");
		set.add(new Person("David",10));
		set.add(new Person("David",10));

		System.out.println(set);
	}
}

class Person {
	String name;
	int age;

	Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public String toString() {
		return name +":"+ age;
	}
}
```

```json
실행결과
[abc, David:10, David:10]
```

- **name과 age가 같음**에도 불구하고 **서로 다른 것으로 인식**하여 **두번 출력되었다.**

같은 것으로 인식하게 하려면 어떻게 해야 할까?

[HashSet 사용을 위한 Cutomize Class 예시 - equals(), hashCode() 오버라이딩(Overriding)]

```java
import java.util.*;

class HashSetEx4 {
	public static void main(String[] args) {
		HashSet set = new HashSet();

		set.add(new String("abc"));
		set.add(new String("abc"));
		set.add(new Person2("David",10));
		set.add(new Person2("David",10));

		System.out.println(set);
	}
}

class Person2 {
	String name;
	int age;

	Person2(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public boolean equals(Object obj) {
		if(obj instanceof Person2) {
			Person2 tmp = (Person2)obj;
			return name.equals(tmp.name) && age==tmp.age;
		}

		return false;
	}

	public int hashCode() {
		return (name+age).hashCode();
	}

	public String toString() {
		return name +":"+ age;
	}
}
```

```json
실행결과
[abc, David:10]
```

`HashSet의 add()메서드` 는 **새로운 요소를 추가하기 전에 기존에 저장된 요소와 같은 것인지 판별**하기 위해 추가하려는 요소의 `equals(), hashCode()` 를 호출한다.
- **equals(), hashCode()를 목적에 맞게 오버라이딩(Overriding)** 해야 한다.

```java
public boolean equals(Object obj) {
	if(obj instanceof Person2) {
		Person2 tmp = (Person2)obj;
		return name.equals(tmp.name) && age==tmp.age;
	}

	return false;
}

public int hashCode() {
	return (name+age).hashCode();
}

-->

public int hashCode() {
	return Objects.hash(name, age); // int hash(Object... values) 
}
```

`String클래스의 hashCode()` : **문자열의 내용으로 해시코드를 생성** , 내용이 같은 문자열에 대한 hashCode()호출은 항상 같은 값을 반환

`Object클래스의 hashCode()` : **객체의 주소로 해시코드를 생성** , 실행할 때마다 해시코드값이 달라질 수 있다.

- **equals()** : 두 인스턴스의 name, age가 서로 같으면 true를 반환하도록 **오버라이딩**
- **hashCode()** : String클래스의 hashCode()를 이용하여 구현하였다.
  - String클래스의 hashCode()메서드가 잘 구현되어 있기 때문에 이를 활용하면 간단하게 처리 가능하다.
- **hashCode() 오버라이딩**을 JDK 1.8부터 추가된 **java.util.Objects클래스의 hash()** 를 이용하자 
   - **메서드의 괄호 안**에 **클래스의 인스턴스 변수들**을 넣으면 된다.

오버라이딩을 통해 작성된 hashCode()는 다음의 3가지 조건을 만족해야 한다.

1. `실행 중인 애플리케이션 내의 동일한 객체` 에 대해서 **여러 번 hashCode()를 호출해도 동일한 int 값을 반환** 해야 한다.
- 하지만 `실행시마다` **동일한 int 값을 반환할 필요는 없다** (eqauls 메서드의 구현에 사용된 멤버변수의 값이 바뀌지 않았다고 가정한다.)

```java
Person2 p = new Person2("david", 10);
int hashCode1 = p.hashCode();
int hashCode2 = p.hashCode();

p.age = 20;
int hashCode3 = p.hashCode();
```

- hashCode1, hashCode2 는 동일한 int값, hashCode3은 1,2 와 달라도 상관 없다.
- hashCode1, hashCode2 도 실행할 때마다 반드시 같은 값이 아니어도 된다.

2. equals 메서드를 이용한 비교에 의해서 true를 얻은 두 객체에 대해 각각 hashCode()호출해서 얻은 결과는 반드시 같아야 한다.
- p1 p2의 equals 메서드의 결과가 true라면 hashCode1과 2의 값은 같아야 한다는 뜻

```java
Person2 p1 = new Person2("david", 10);
Person2 p2 = new Person2("david", 10);

boolean b = p1.equals(p2);

int hashCode1 = p1.hashCode();
int hashCode2 = p2.hashCode();
```

3. **equals 메서드를 호출했을 때 false를 반환하는 두 객체** 는 hashCode()호출에 대해 같은 int 값을 반환하는 경우가 있더도 괜찮지만, **해싱을 사용하는 컬렉션의 성능을 향상시키기** 위해서는 **다른 int 값을 반환하는 것이 좋다.**

- **hashCode**를 사용하는 **Hashtable, HashMap** 과 같은 **컬렉션의 성능을 높이기 위해서는** 가능한 한 **서로 다른 값을 반환하도록 hashCode()를 잘 작성**해야 한다.
- **서로 다른 객체**에 대해서 **해시코드값이 중복**되는 경우가 많아질수록 **해싱을 사용하는 컬렉션의 검색속도가 떨어진다.**

[hashSet 사용 예시 - 합집합(addAll), 차집합(removeAll), 교집합(retainAll)]

```java
import java.util.*;

class HashSetEx5 {
	public static void main(String args[]) {
		HashSet setA    = new HashSet();
		HashSet setB    = new HashSet();
		HashSet setHab = new HashSet();
		HashSet setKyo = new HashSet();
		HashSet setCha = new HashSet();

		setA.add("1");		setA.add("2");
		setA.add("3");		setA.add("4");
		setA.add("5");
		System.out.println("A = "+setA);

		setB.add("4");		setB.add("5");
		setB.add("6");		setB.add("7");
		setB.add("8");
		System.out.println("B = "+setB);

		Iterator it = setB.iterator();
		while(it.hasNext()) {
			Object tmp = it.next();
			if(setA.contains(tmp))
				setKyo.add(tmp);
		}

		it = setA.iterator();
		while(it.hasNext()) {
			Object tmp = it.next();
			if(!setB.contains(tmp))
				setCha.add(tmp);
		}

		it = setA.iterator();
		while(it.hasNext())
			setHab.add(it.next());

		it = setB.iterator();
		while(it.hasNext())
			setHab.add(it.next());

		System.out.println("A ∩ B = "+setKyo);  // 한글 ㄷ을 누르고 한자키
		System.out.println("A ∪ B = "+setHab);  // 한글 ㄷ을 누르고 한자키
		System.out.println("A - B = "+setCha); 
	}
}
```

```json
실행결과
A = [1,2,3,4,5]
B = [4,5,6,7,8]
A 교 B = [4, 5]
A 합 B = [1,2,3,4,5,6,7,8]
A - B = [1, 2, 3]
```

## TreeSet

`TreeSet` : **이진 검색 트리(binary search tree)라는 자료구조의 형태로 데이터를 저장하는 컬렉션 클래스**
- `TreeSet`은 **이진 검색 트리의 성능을 향상**시킨 `레드-블랙-트리(Red-Black tree)` 로 구현되어 있다.
- **중복된 데이터의 저장을 허용 X, 정렬된 위치에 저장, 저장순서를 유지 X**

### binary tree

`binary tree` : **linked list처럼 여러 개의 노드(Node)가 서로 연결된 구조**
- 각 노드에 **최대 2개의 노드를 연결**
- **root(루트)** 라고 불리는 하나의 노드에서부터 시작해서 계속 확장해 나갈 수 있다.
- **왼쪽 자식 노드의 값 < 부모노드의 값 < 오른쪽 자식 노드의 값** : left < parent < right
- **노드의 추가/삭제에 시간이 걸린다.(순차적으로 저장하지 않으므로)**
- **검색(범위 검색)** 과 **정렬** 에 **유리**하다.
- **중복된 값을 저장하지 못한다.**

![트리 구조](https://user-images.githubusercontent.com/56071088/126454343-6a0aa998-d6f3-47a0-a56b-b0b7689eb5d8.png)

![이진 트리 예시](https://user-images.githubusercontent.com/56071088/126454345-88c66c0a-7114-4221-befd-07931230771f.png)

```java
class TreeNode {
	TreeNode left;
	Object element;
	TreeNode right;
}
```

- 데이터를 저장하기 위한 Object타입의 참조변수
- 두 개의 노드를 참조하기 위한 두 개의 참조변수를 선언
- 부모 노드의 왼쪽에는 부모 노드보다 작은 값의 자식 노드, 오른쪽에는 부모노드보다 큰 값의 자식 노드를 저장하는 이진 트리

![TreeSet데이터 저장 과정](https://user-images.githubusercontent.com/56071088/126454333-200d36fc-ffd3-4f2b-a8cd-710713184b9c.png)

앞서 살펴본 것 처럼 컴퓨터는 알아서 값을 비교하지 못한다.
- **TreeSet에 저장되는 객체**가 **Comparable을 구현**하던가
- **Comparator를 제공**해서 **두 객체를 비교하는 방법**을 알려줘야 한다.
- **그렇지 않으면 TreeSet에 객체를 저장할 때 예외가 발생한다.**

### TreeSet메서드

![TreeSet메서드1](https://user-images.githubusercontent.com/56071088/126456894-dd67fac4-5764-46d6-9d68-5befd79063d6.png)

![TreeSet메서드 2](https://user-images.githubusercontent.com/56071088/126456900-41b7d302-58f1-4f12-baf1-10985fa411a6.png)

[TreeSet 사용 예시 - Collections.sort()를 사용하지 않아도 정렬되어 있는 상태]

```java
import java.util.*;

class TreeSetLotto {
	public static void main(String[] args) {
		Set set = new TreeSet();
		
		for (int i = 0; set.size() < 6 ; i++) {
			int num = (int)(Math.random()*45) + 1;
			set.add(num);  // set.add(new Integer(num));
		}

		System.out.println(set);
	}
}
```

```json
실행결과
[5,12,24,26,33,45]
```

- hashSet에서 보여준 예시와 달리 Collections.sort()를 사용하지 않아도 삽입할 때 정렬을 수행한다.

[TreeSet 사용 예시 - subSet()을 이용한 범위검색(range search)]

```java
import java.util.*;

class TreeSetEx1 {
	public static void main(String[] args) {
		TreeSet set = new TreeSet();

		String from = "b";
		String to	= "d";

		set.add("abc");
		set.add("alien");
		set.add("bat");
		set.add("car");
		set.add("Car");
		set.add("disc");
		set.add("dance");
		set.add("dZZZZ");
		set.add("dzzzz");
		set.add("elephant");
		set.add("elevator");
		set.add("fan");
		set.add("flower");

		System.out.println(set);
		System.out.println("range search : from " + from  +" to "+ to);
		System.out.println("result1 : " + set.subSet(from, to));
		System.out.println("result2 : " + set.subSet(from, to + "zzz"));
	}
}
```

```json
실행결과
[Car, abc, alien, bat, dZZZZ, dance, disc, dzzzz, elephant, elevator, 
fan, flower]
range search: from b to d
result1: [bat, cat]
result2: [bat, car, dZZZ, dance, disc]
```

**subSet()을 이용해서 범위검색(range search)**

- **시작범위는 포함**되지만 **끝 범위는 포함되지 않는다.**
  - result1에는 c로 시작하는 단어까지만 검색결과에 포함
  - **끝 범위인 d로 시갖하는 단어까지 포함시키려고 한다면, 끝범위에 'zzz' 문자열을 붙인다.**
- 대문자가 소문자보다 우선하기 때문에 대소문자가 섞여 있는 경우 의도한 것과는 다른 범위검색결과를 얻을 수 있다.
  - 가능하면 대문자 또는 소문자로 통일해서 저장하는 것이 좋다.
- **반드시 대소문자가 섞여 있어야** 하거나 **다른 방식으로 정렬해야 하는 경우** , **Comparator을 이용하면 된다.**
  - **문자열의 정렬 순서 : 문자의 unicode값을 기준 : 오름차순의 경우, 공백, 숫자, 대문자, 소문자 순으로 정렬**

[TreeSet 사용 예시 - headSet(), tailSet()]

```java
import java.util.*;

class TreeSetEx2 {
	public static void main(String[] args) {
		TreeSet set = new TreeSet();
		int[] score = {80, 95, 50, 35, 45, 65, 10, 100};

		for(int i=0; i < score.length; i++)
			set.add(new Integer(score[i]));

		System.out.println("50보다 작은 값 :"	+ set.headSet(new Integer(50)));
		System.out.println("50보다 큰 값 :"	+ set.tailSet(new Integer(50)));
	}
}
```

```json
실행결과
50보다 작은 값: [10, 35, 45]
50보다 큰 값: [50, 65, 80, 95, 100]
```

- `SortedSet headSet(Object toElement)` : 지정된 객체보다 작은 값의 객체들을 반환
- `NavigableSet headSet(Object toElement, boolean inclusive)` : 지정된 객체보다 작은 값의 객체들을 반환, inclusive가 true이면 같은 값의 객체도 포함
- `SortedSet tailSet(Object fromElement)` : 지정된 객체보다 큰 값의 객체들을 반환한다.

![headSet, tailSet](https://user-images.githubusercontent.com/56071088/126458796-b2e17624-7f7e-4087-8ec8-36029b75be6e.png)


## HashMap과 HashTable

**`HashMap`** : HashTable과 HashMap의 관계는 Vector와 ArrayList의 관계와 같아서 **HashTable보다 새로운 버전인 HashMap을 사용하는 것을 권한다.**

**`HashTable`** : `Map인터페이스` 를 구현, Map의 특징인 `key` , `value` 를 묶어서 `하나의 데이터(entry)` 로 저장한다는 특징을 갖는다. **`해싱(hashing)`** 을 사용하기 때문에 **많은 양의 데이터**를 **`검색`** 하는데 있어서 **뛰어난 성능**을 보인다.

**[HashMap의 실제 소스]**

```java
public class HashMap extends AbstractMap implements Map, Cloneable, Serializable {
	transient Entry[] table;
	static calss Entry implements Map.Entry {
		final Object key;
		Object value;
	}
}
```

|비객체지향적인 코드|객체지향적인 코드|
|---|---|
|Object[] key; Object[] value; |Entry[] table;|
||class Entry { Object key; Object value; }|

[참고] Map.Entry는 Map인터페이스에 정의된 `static inner interface` 이다.
 
HashMap은 **Entry** 라는 **내부 클래스**를 정의하고, 다시 Entry타입의 배열을 선언한다.
- **key와 value**는 별개의 값이 아니라 **서로 관련된 값**이기 때문에,
- 각각의 배열로 선언하기 보다 **하나의 클래스로 정의**해서 **하나의 배열**로 다루는 것이 **데이터의 무결성(Integrity)** 적인 측면에서 더 바람직하다. 

**HashMap**은 key와 value를 **각각 Object타입으로 저장**한다. 
- `(Object, Object)` 의 형태로 저장
- **어떠한 객체도 저장**할 수 있다. **key는 주로 String**을 대문자 또는 소문자로 통일해서 사용

**`key`** : 컬렉션 내의 **key중에서 유일**해야 한다.

**`value`** : key와 달리 **데이터의 중복을 허용**한다.


### HashMap메서드 (책에는 Object clone()도 포함)

![hashMap메서드](https://user-images.githubusercontent.com/56071088/126461547-f6a7d19b-8541-4b51-8bab-85b5e5359ccd.png)

- 람다와 스트림에 관련된 것들은 제외

[자주 사용하는 HashMap 메서드]

- **void clear()** : HashMap에 저장된 모든 객체를 제거

- **boolean containsKey(Object key)** : HashMap에 지정된 Key가 포함되어 있는지 알려준다. **(포함되어 있다면 true)**
- **boolean containsValue(Object value)** : HashMap에 지정된 value가 포함되어 있는지 알려준다. **(포함되어 있다면 true)**
- **Set entrySet()** : HashMap에 저장된 key와 value를 **Entry(key와 value의 결합)의 형태**로 **Set에 저장해서 반환**
- **Set keySet()** : HashMap에 저장된 모든 key가 저장된 Set을 반환
- **Collection values()** : HashMap에 저장된 **모든 value**를 **Collection의 형태로 반환**
- **Object get(Obejct key)** : 지정된 key의 값(객체)을 반환, **못 찾으면 null 반환**
- **Object getOrDefault(Object key, Object defaultValue)** : 지정된 key의 값(객체)을 반환, **key를 못 찾으면, 기본값(defaultValue)로 지정된 객체를 반환**
- **Object put(Object key, Object value)** : 지정된 key와 value을 HashMap에 저장
- **void putAll()** : map에 저장된 모든 요소를 HashMap에 저장
- **Object replace(Object key, Object value)** : 지정된 Key의 값을 지정된 객체(value)로 대체
- **boolean replace(Object key, Object oldValue, Object newValue)** : 지정된 key와 객체(oldValue)가 모두 일치하는 경우에만 세로운 객체(newValue)로 대체
- **default V putIfAbsent(K key, V value)** : **Key 값이 존재하는 경우 Map의 Value의 값을 반환하고, Key값이 존재하지 않는 경우 Key와 Value를 Map에 저장하고 Null을 반환합니다.**

[hashMap 사용 예시 - 중복된 key 사용시 value의 값은 덮어씌워진다]

```java
import java.util.*;

class HashMapEx1 {
	public static void main(String[] args) {
		HashMap map = new HashMap();
		map.put("myId", "1234");
		map.put("asdf", "1111");
		map.put("asdf", "1234");

		Scanner s = new Scanner(System.in);	// 화면으로부터 라인단위로 입력받는다.

		while(true) {
			System.out.println("id와 password를 입력해주세요.");
			System.out.print("id :");
			String id = s.nextLine().trim();

			System.out.print("password :");
			String password = s.nextLine().trim();
			System.out.println();

			if(!map.containsKey(id)) {
				System.out.println("입력하신 id는 존재하지 않습니다. 다시 입력해주세요.");
				continue;
			} else {
				if(!(map.get(id)).equals(password)) {
					System.out.println("비밀번호가 일치하지 않습니다. 다시 입력해주세요.");
				} else {
					System.out.println("id와 비밀번호가 일치합니다.");						
					break;
				}
			}
		} // while
	} // main의 끝
}
```

```json
실행결과
id:asdf
pw: 1111
비밀번호 불일치

id:asdf
pw:1234
id와 pw일치
```

- key 'asdf'에 연결된 값은 **기존의 값이 덮어씌어져** '1234'가 된다.
- Hashtable과 달리 **HashMap은 key나 value으로 null 허용**

[hashMap 사용 예시 - entrySert(), keySet(), values()]

```java
import java.util.*;

class HashMapEx2 {
	public static void main(String[] args) {
		HashMap map = new HashMap();
		map.put("김자바", new Integer(90));
		map.put("김자바", new Integer(100));
		map.put("이자바", new Integer(100));
		map.put("강자바", new Integer(80));
		map.put("안자바", new Integer(90));

		Set set = map.entrySet();
		Iterator it = set.iterator();

		while(it.hasNext()) {
			Map.Entry e = (Map.Entry)it.next();
			System.out.println("이름 : "+ e.getKey() + ", 점수 : " + e.getValue());
		}

		set = map.keySet();
		System.out.println("참가자 명단 : " + set);

		Collection values = map.values();
		it = values.iterator();

		int total = 0;

		while(it.hasNext()) {
			Integer i = (Integer)it.next();
			total += i.intValue();
		}

		System.out.println("총점 : " + total);
		System.out.println("평균 : " + (float)total/set.size());
		System.out.println("최고점수 : " + Collections.max(values));
		System.out.println("최저점수 : " + Collections.min(values));
	}
}
```

- `enrtySet()` 을 이용해서 **key와 value을 함께 읽어** 올 수도 있고,
- `keySet()` 이나 `values()` 를 이용해서 **key와 value을 따로 읽어 올 수 있다.**

[HashMap 사용 예시]

```java
import java.util.*;

class HashMapEx3 {
	static HashMap phoneBook = new HashMap();

	public static void main(String[] args) {
		addPhoneNo("친구", "이자바", "010-111-1111");
		addPhoneNo("친구", "김자바", "010-222-2222");
		addPhoneNo("친구", "김자바", "010-333-3333");
		addPhoneNo("회사", "김대리", "010-444-4444");
		addPhoneNo("회사", "김대리", "010-555-5555");
		addPhoneNo("회사", "박대리", "010-666-6666");
		addPhoneNo("회사", "이과장", "010-777-7777");
		addPhoneNo("세탁", "010-888-8888");

		printList();
	} // main

	// 그룹을 추가하는 메서드
	static void addGroup(String groupName) {
		if(!phoneBook.containsKey(groupName))
			phoneBook.put(groupName, new HashMap());
	}

	// 그룹에 전화번호를 추가하는 메서드
	static void addPhoneNo(String groupName, String name, String tel) {
		addGroup(groupName);
		HashMap group = (HashMap)phoneBook.get(groupName);
		group.put(tel, name);	// 이름은 중복될 수 있으니 전화번호를 key로 저장한다.
	}

	static void addPhoneNo(String name, String tel) {
		addPhoneNo("기타", name, tel);
	}

	// 전화번호부 전체를 출력하는 메서드
	static void printList() {
		Set set = phoneBook.entrySet();
		Iterator it = set.iterator();	

		while(it.hasNext()) {
			Map.Entry e = (Map.Entry)it.next();

			Set subSet = ((HashMap)e.getValue()).entrySet();
			Iterator subIt = subSet.iterator();	

			System.out.println(" * "+e.getKey()+"["+subSet.size()+"]");

			while(subIt.hasNext()) {
				Map.Entry subE = (Map.Entry)subIt.next();
				String telNo = (String)subE.getKey();
				String name = (String)subE.getValue();
				System.out.println(name + " " + telNo );
			}
			System.out.println();
		}
	} // printList()
} // class
```

```json
실행결과
* 기타[1]
세탁 010-888-8888

* 친구[3]
이자바 010-111-1111
김자바 010-222-2222
김자바 010-333-3333

* 회사[4]
이과장 010-777-7777
김대리 010-444-4444
김대리 010-555-5555
박대리 010-666-6666
```

- HashMap은 데이터를 key와 value을 모두 Object타입으로 저장하기 때문에 
- **HashMap의 값으로 HashMap을 다시 저장**할 수 있다.

[hashMap 사용 예시 - 한정되지 않은 범위의 비순차적인 값들의 빈도수를 구할 때]

```java
import java.util.*;

class HashMapEx4 {
	public static void main(String[] args) {
		String[] data = { "A","K","A","K","D","K","A","K","K","K","Z","D" };

		HashMap map = new HashMap();

		for(int i=0; i < data.length; i++) {
			if(map.containsKey(data[i])) {
				Integer value = (Integer)map.get(data[i]);
				map.put(data[i], new Integer(value.intValue() + 1));
			} else {
				map.put(data[i], new Integer(1));			
			}
		}

		Iterator it = map.entrySet().iterator();

		while(it.hasNext()) {
			Map.Entry entry = (Map.Entry)it.next();
			int value = ((Integer)entry.getValue()).intValue();
			System.out.println(entry.getKey() + " : " + printBar('#', value) + " " + value );
		}
	} // main

	public static String printBar(char ch, int value) { 
		char[] bar = new char[value]; 

		for(int i=0; i < bar.length; i++) { 
			bar[i] = ch; 
		} 

		return new String(bar); 	// String(char[] chArr)
	}
}
```

```json
실행결과
A : ### 3
B : ## 2
Z : # 1
K : ###### 6
```

**문자열 배열에 담긴 문자열들의 빈도수를 구하는 예제**
- 한정된 범위 내에 있는 순차적인 값들의 빈도수는 배열을 이용하지만, 
- **한정되지 않은 범위**의 **비순차적인 값들의 빈도수**는 **HashMap을 이용**해서 구할 수 있다. 

### Map의 putIFAbsent(), computeIfAbsent() 사용 방법

```java
public class ComputeIfAbsentTest {

    public static void main(String[] args) {
        // Map 선언
        Map<String, Set<Integer>> map = new HashMap<>();

        // 키 'a'에 대한 Set 추가
        map.put("a", new HashSet<>());

        // 키 'a'의 Set에 값 추가
        map.get("a").add(1);
        map.get("a").add(2);
        map.get("a").add(3);

        // map.computeIfAbsent
        map.computeIfAbsent("b", k -> new HashSet<>()).add(4);
        map.computeIfAbsent("b", k -> new HashSet<>()).add(5);
        map.computeIfAbsent("b", k -> new HashSet<>(){{ add(7); }});
        map.computeIfAbsent("b", k -> new HashSet<>(){{ add(8); }});
        map.computeIfAbsent("b", k -> new HashSet<>()).add(6);

        // map.putIfAbsent
        map.putIfAbsent("c", new HashSet<>(){{ add(1); }});
        map.putIfAbsent("c", new HashSet<>(){{ add(2); }});
        map.putIfAbsent("b", new HashSet<>(){{ add(3); }});

        System.out.println(map); // 출력 결과: {a=[1, 2, 3], b=[4, 5, 6], c=[1]}
    }
}
```


### Map의 entrySet() 사용 방법

키-값 쌍 접근하기 Map.Entry 인터페이스를 사용하여 각 키-값 쌍에 접근할 수 있습니다. 다음과 같이 코드를 작성할 수 있습니다:

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 3);
map.put("banana", 5);
map.put("cherry", 7);

for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer value = entry.getValue();
    System.out.println("Key: " + key + ", Value: " + value);
}
```

이 코드는 다음과 같은 출력 결과를 보여줍니다:

```
Key: apple, Value: 3
Key: banana, Value: 5
Key: cherry, Value: 7
```

키와 값을 별도로 접근하기 entrySet()을 사용하지 않고 keySet()과 values()를 사용하여 키와 값을 별도로 접근할 수도 있습니다:

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 3);
map.put("banana", 5);
map.put("cherry", 7);

for (String key : map.keySet()) {
    Integer value = map.get(key);
    System.out.println("Key: " + key + ", Value: " + value);
}
```

이 코드의 출력 결과는 위의 코드와 동일합니다.

Map 정렬하기 entrySet()을 사용하면 Map의 키-값 쌍을 정렬할 수 있습니다. 다음은 키를 기준으로 오름차순 정렬하는 예시입니다:

```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 3);
map.put("banana", 5);
map.put("cherry", 7);

List<Map.Entry<String, Integer>> entries = new ArrayList<>(map.entrySet());
entries.sort(Map.Entry.comparingByKey());

for (Map.Entry<String, Integer> entry : entries) {
    System.out.println("Key: " + entry.getKey() + ", Value: " + entry.getValue());
}
```

이 코드의 출력 결과는 다음과 같습니다:

```
Key: apple, Value: 3
Key: banana, Value: 5
Key: cherry, Value: 7
```

이처럼 entrySet()을 사용하면 Map의 모든 키-값 쌍을 효과적으로 처리할 수 있습니다. 필요에 따라 키와 값을 별도로 접근하거나, 정렬 기준을 변경할 수 있습니다.


### 해싱과 해시함수

`해싱(hashing)`이란 `해시함수(hash function)`을 이용해서 **데이터**를 **해시테이블(hash table)** 에 **저장하고 검색하는 기법**을 말한다.

해싱을 구현한 컬렉션 클래스로는 HashSet, HashMap, Hashtable 등이 있다. Hashtable은 Collection Framework이 도입되면서 HashMap으로 대체되었고, 호환성의 문제로 남겨두고 있다.

**해싱에서 사용하는 자료구조**는 **`배열과 링크드 리스트의 조합`** 으로 되어 있다.

![배열과 링크드 리스트의 조합](https://user-images.githubusercontent.com/56071088/126956600-6b0f7ea8-48cf-4c3e-8420-9e2e9efbe228.png)

저장할 데이터의 key를 hash function에 넣으면 배열의 한 요소를 얻게 되고, 다시 그 곳에 연결되어 있는 링크드 리스트에 저장한다.
- 배열의 각 요소에는 링크드 리스트가 저장되어 있어서 **실제 데이터는 링크드 리스트에 담겨지게 된다.**

![해싱](https://user-images.githubusercontent.com/56071088/126463882-e11b2a14-006b-4b8f-b196-bc66070c992f.png)

1. 검색하고자 하는 값을 **key**로 **해시함수(hash function)** 를 호출한다.
2. **해시함수의 계산결과(hashCode)** 로 **해당 값이 저장되어 있는 linked list를 찾는다.**
3. **linked list**에서 **검색한 key와 일치하는 데이터를 찾는다.**

linked list는 검색에 불리한 자료구조, linked list의 크기가 커질수록 검색속도가 떨어지게 된다. 

반면에, 배열(array)은 배열의 크기가 커져도, 원하는 요소가 몇 번째에 있는지만 알면 공식을 통해 빠르게 원하는 값을 얻을 수 있다.

- **배열의 인덱스가 n인 요소의 주소** : **배열의 시작주소 + type의 size * n**

따라서 하나의 요소에 많은 데이터가 저장되어 있는 형태보다는 **많은 요소에 하나의 데이터씩만 저장되어 있는 형태**가 **더 빠른 겸색 결과**를 얻을 수 있다.

**하나의 linked list**에 **최소한의 데이터만 저장**되려면, 
- **저장될 데이터의 크기**를 고려해서 **HashMap의 크기를 적절하게 지정**
- **해시함수(hash function)** 가 **서로 다른 key**에 대해서 **중복된 해시코드(hashCode)의 반환을 최소화**

그래야 **HashMap에 빠른 검색시간**을 얻을 수 있다.

**`HashMap과 같이 해싱(hashing)을 구현한 컬렉션 클래스`** 에서는 **Object클래스에 정의된 hashCode()를 해시함수로 이용**한다.
- `Object클래스의 hashCode()` 는 **객체의 주소를 이용하는 알고리즘**으로 hashCode를 만들어 낸다.
- **모든 객체에 대해 hashCode()를 호출한 결과가 서로 유일한 훌륭한 방법**

`String클래스의 경우` : Object클래스로부터 상속받는 hashCode()를 오버라이딩(overriding)하여 **문자열의 내용으로 hashCode**를 만들어 낸다.
- 서로 다른 인스턴스일지라도 **문자열의 내용이 같다면 hashCode()를 호출하면 같은 hashCode를 얻는다.**

서로 다른 두 객체에 대해 **equals()로 비교한 결과가 true**인 동시에 **hashCode()의 반환값이 같아야 같은 객체로 인식**
- HashMap에서도 같은 방법으로 객체를 구별
- 이미 존재하는 key에 대한 값을 저장하면 기존의 값을 새로운 값으로 덮어쓴다.

**`새로운 클래스를 정의`** : equals()를 오버라이딩(overriding) 해야 한다면, hashCode()도 같이 오버라이딩(overriding) 해야 한다.
- **두 객체의 equals()의 결과가 true -> 두 객체의 hashCode()의 결과 값이 항상 같도록 해야 한다.** 

**두 객체의 equals()의 결과가 false, 두 객체의 hashCode()의 결과가 같은 경우** : **같은 linked list**에 저장된 **서로 다른 data**가 된다.

## TreeMap

`TreeMap` : **이진검색트리의 형태로 key와 value의 쌍으로 이루어진 데이터를 저장한다.**
- **검색과 정렬에 적합한 컬렉션 클래스**이다.
- **검색** 성능 : **HashMap** > TreeMap
- **범위검색**이나 **정렬** : HashMap < **TreeMap**

### TreeMap 메서드

![TreeMap메서드 1](https://user-images.githubusercontent.com/56071088/126464053-c27d5583-83c1-4e88-a814-bc3055c33e34.png)

![TreeMap 메서드2](https://user-images.githubusercontent.com/56071088/126464044-c937682a-2015-449a-abc0-13a41f3e1b2e.png)

[TreeMap 사용 예시]

```java
import java.util.*;

class TreeMapEx1 {
	public static void main(String[] args) {
		String[] data = { "A","K","A","K","D","K","A","K","K","K","Z","D" };

		TreeMap map = new TreeMap();

		for(int i=0; i < data.length; i++) {
			if(map.containsKey(data[i])) {
				Integer value = (Integer)map.get(data[i]);
				map.put(data[i], new Integer(value.intValue() + 1));
			} else {
				map.put(data[i], new Integer(1));			
			}
		}

		Iterator it = map.entrySet().iterator();

		System.out.println("= 기본정렬 =");
		while(it.hasNext()) {
			Map.Entry entry = (Map.Entry)it.next();
			int value = ((Integer)entry.getValue()).intValue();
			System.out.println(entry.getKey() + " : " + printBar('#', value) + " " + value );
		}
		System.out.println();

		// map을 ArrayList로 변환한 다음에 Collectons.sort()로 정렬
		Set set = map.entrySet();
		List list = new ArrayList(set);	// ArrayList(Collection c) 
		
		// static void sort(List list, Comparator c)  
		Collections.sort(list, new ValueComparator());

		it = list.iterator();

		System.out.println("= 값의 크기가 큰 순서로 정렬 =");		
		while(it.hasNext()) {
			Map.Entry entry = (Map.Entry)it.next();
			int value = ((Integer)entry.getValue()).intValue();
			System.out.println(entry.getKey() + " : " + printBar('#', value) + " " + value );
		}

	} // public static void main(String[] args) 

	static class ValueComparator implements Comparator {
		public int compare(Object o1, Object o2) {
			if(o1 instanceof Map.Entry && o2 instanceof Map.Entry) {
				Map.Entry e1 = (Map.Entry)o1;
				Map.Entry e2 = (Map.Entry)o2;

				int v1 = ((Integer)e1.getValue()).intValue();
				int v2 = ((Integer)e2.getValue()).intValue();

				return  v2 - v1;
			} 
			return -1;
		}
	}	// static class ValueComparator implements Comparator {

	public static String printBar(char ch, int value) { 
		char[] bar = new char[value]; 

		for(int i=0; i < bar.length; i++) { 
			bar[i] = ch; 
		} 

		return new String(bar); 
	} 
}
```

```json
실행결과
== 기본정렬 ==
A: ### 3
D: ## 2
K: ###### 6
Z: # 1

= 값의 크기가 큰 순서로 정렬 =
K: ###### 6
A: ### 3
D: ## 2
Z: # 1
```

- **TreeMap을 사용**했기 때문에 **key가 오름차순으로 정렬**되어 있다.
  - **key가 String 인스턴스**이기 때문에 **String클래스에 정의된 정렬 기준에 의해서 정렬**
- **Comparator를 구현한 클래스**와 **Collections.sort(List list, Comparator c)** 를 이용해서 **value에 대한 내림차순으로 정렬**

## Properies

**`Properties`** 
- HashTable을 상속받아 구현
- Properties는 **(String, String)** 의 형태로 저장하는 **보다 단순화된 컬렉션 클래스**
  - HashMap은 key와 value를 (Object, Object)의 형태로 저장
- **애플리케이션의 환경설정과 관련된 속성(Property)을 저장**하는데 사용되며 **데이터를 파일로부터 읽고 쓰는 편리한 기능을 제공**
- **간단한 입출력**은 **Properties를 활용**하면 몇 줄의 코드로 쉽게 해결 
### Propertirs 메서드

![properties메서드](https://user-images.githubusercontent.com/56071088/126464974-894ffb9a-fb52-411a-8ca5-1b085f5c9324.png)

[Properties의 메서드 사용 예제]

```java
import java.util.*;

class PropertiesEx1 {
	public static void main(String[] args) {
		Properties prop = new Properties();

		// prop에 키와 값(key, value)을 저장한다.
		prop.setProperty("timeout","30");
		prop.setProperty("language","kr");
		prop.setProperty("size","10");
		prop.setProperty("capacity","10");

		// prop에 저장된 요소들을 Enumeration을 이용해서 출력한다.
		Enumeration e = prop.propertyNames();

		while(e.hasMoreElements()) {
			String element = (String)e.nextElement();
			System.out.println(element + "="+ prop.getProperty(element));
		}

		System.out.println();
		prop.setProperty("size","20");	// size의 값을 20으로 변경한다.
		System.out.println("size="       + prop.getProperty("size"));
		System.out.println("capacity="   + prop.getProperty("capacity", "20"));
		System.out.println("loadfactor=" + prop.getProperty("loadfactor", "0.75"));

		System.out.println(prop);	// prop에 저장된 요소들을 출력한다.
		prop.list(System.out);      // prop에 저장된 요소들을 화면(System.out)에 출력한다.
	}
}
```

```json
실행결과
capacity=10
size=10
timeout=30
language=kr

size=20
capacity=10
loadfactor=0.75 // 키가 없으므로 디폴트값으로 지정한 0.75
{capacity=10, size=20, language=kr} // System.out.println(prop);
-- listing properties -- // prop.list(System.out);
capacity=10
size=20
timeout=30
language=kr
```

**Object setProperty(String key, String value)** : Hashtable의 put()메서드와 똑같다.
- 지정된 key와 값을 저장
- 이미 존재하는 키(key)이면 새로운 값(value)로 바꾼다.
  
**String getProperty(String key)**

**String getProperty(String key, String defaultValue)** 
- Properies에 저장된 값을 읽어온다.
- 만약 키(key)가 존재하지 않으면 지정된 기본값(defaultValue) 반환
  
**Properies**는 Hashtable을 상속받아 구현한 것이라 **Map의 특성상 저장순서 유지하지 않는다.**
- Properies는 Collection framework 이전의 구버전이므로 Iterator가 아닌 **Enumeration을 사용**


**void list(PrintStream out)**

**void list(PrintWriter out)**

- Properies에 저장된 모든 데이터를 화면 또는 파일에 편리하게 출력할 수 있다.
- System.out은 화면과 연결된 표준출력으로 System클래스에 정의된 PrintStream타입의 static 변수이다.

**Set stringPropertyNames()** : Properies에 저장되어 있는 모든 key를 Set에 담아서 반환한다.

[외부파일(input.txt)로부터 데이터를 입력받아서 계산결과를 보여주는 예제]

```java
import java.io.*;
import java.util.*;

class PropertiesEx2 {
	public static void main(String[] args) {
		// commandline에서 inputfile을 지정해주지 않으면 프로그램을 종료한다.
		if(args.length != 1) {
			System.out.println("USAGE: java PropertiesEx2 INPUTFILENAME");
			System.exit(0);
		}

		Properties prop = new Properties();

		String inputFile = args[0];

		try {
			prop.load(new FileInputStream(inputFile));
		} catch(IOException e) {
			System.out.println("지정된 파일을 찾을 수 없습니다.");
			System.exit(0);
		}

		String   name = prop.getProperty("name");
		String[] data = prop.getProperty("data").split(",");
		int max = 0;
		int min = 0;
		int sum = 0;

		for(int i=0; i < data.length; i++) {
			int intValue = Integer.parseInt(data[i]);
			if (i==0) max = min = intValue;

			if (max < intValue) {
				max = intValue;
			} else if (min > intValue) {
				min = intValue;
			}

			sum += intValue;
		}

		System.out.println("이름 :"  + name);		
		System.out.println("최대값 :" + max);
		System.out.println("최소값 :" + min);
		System.out.println("합계 :"  + sum);
		System.out.println("평균 :"  + (sum*100.0/data.length)/100);
	}
}
```

```json
실행결과
이름 :Seong Namkung
최대값 :35
최소값 :1
합계 :111
평균 :11.1

input.txt
# 이것은 주석
# 여러줄 가능
name=Seong Namkung
data=9,1,5,2,8,13,26,11,35,1
```

- **외부파일(input.text) 의 형식**은 **라인 단위**로 **키(key)와 값(value)** 가 **'=' 로 연결된 형태**이어야 한다. 
- **주석라인**은 **첫 번째 문자가 #**이어야 한다.
- 정해진 규칙대로만 파일을 작성하면 **load()** 를 호출하는 것만으로 쉽게 데이터를 읽어 올 수 있다.
  - 다만, **인코딩(encoding)** 문제로 **한글이 깨질 수 있기 때문에** 한글을 입력받으려면 아래와 같이 코드를 변경해야 한다.

```java
String name = prop.getProperty("name");

try {
	name = new String(name.getBytes("8859_1"), "EUC-KR");
} catch(Exception e) {
}
```

- **읽어온 데이터의 인코딩**을 **라틴문자집합(8859_1)에서 한글완성형(EUC-KR 또는 KSC5601)으로 변환**해주는 과정을 추가한 것이다.
  - **우리가 사용하는 OS의 기본 인코딩(encoding)** 이 **유니코드(unicode)가 아니라서** 이런 변환이 필요한 것 

[Properties에 저장된 데이터를 store(), storeToXML() 를 이용해서 파일로 저장하는 예제]

```java
import java.util.*;
import java.io.*;

class PropertiesEx3 {
	public static void main(String[] args) {
		Properties prop = new Properties();

		prop.setProperty("timeout","30");
		prop.setProperty("language","ÇŃąŰ");
		prop.setProperty("size","10");
		prop.setProperty("capacity","10");

		try {
			prop.store(new FileOutputStream("output.txt"), "Properties Example");
			prop.storeToXML(new FileOutputStream("output.xml"),  "Properties Example");
		} catch(IOException e) {
			e.printStackTrace();		
		}
	}
}
```

- 반대로 **Properties에 저장된 데이터** 를 **store(), storeToXML()** 를 이용해서 **파일로 저장**하는 방법
- 한글 문제가 발생
  - **storeToXml()을 이용하여 저장한 XML**은 **Eclipse, Editplus**에서 **한글 편집이 가능**
  - 데이터에 한글이 포함된 경우, storeToXML()을 이용하여 XML로 작성하는 것이 좋다. 

[시스템 속성을 가져오는 방법을 보여주는 예제]

```java
import java.util.*;

class PropertiesEx4{
	public static void main(String[] args) {
		Properties sysProp = System.getProperties();
		System.out.println("java.version :" + sysProp.getProperty("java.version"));
		System.out.println("user.languag :" + sysProp.getProperty("user.language"));
		sysProp.list(System.out);
	}
}
```

```json
실행결과
java.version :1.8.0_51
user_language:ko
-- listing properties --
java.runtime.name=~~
sun.boot.library~
```

- **System 클래스의 getProperties()** 를 호출하면 **시스템과 관련된 속성이 저장된 Properties**를 가져올 수 있다.
- **getProperty()** 를 통해 **원하는 속성**을 얻을 수 있다.

## Collections

**`Collections`**

- Collections는 컬렉션과 관련된 메서드를 제공한다.
  - Arrays는 배열과 관련된 메서드를 제공
- fill(), copy(), sort(), binarySearch()와 같은 메서드들은 이미 설명했으므로 여기서는 설명을 생략
  - Arrays의 메서드들과 같은 기능을 한다.
- `java.util.Collection` : 인터페이스(interface)
- `java.util.Collections` : 클래스(class)

### 컬렉션의 동기화 - synchronized

**멀티 쓰레드(Multi-Thread) 프로그래밍**에서는 **하나의 객체**를 **여러 쓰레드가 동시에 접근**할 수 있기 때문에 **데이터의 일관성(Consistency)** 을 유지하기 위해서는 **공유되는 객체**에 **동기화(Synchronous)가 필요**하다.

- **Vector, HashTable**와 같은 **구버젼(JDK 1.2 이전)의 클래스들**은 **자체적으로 동기화 처리가 됨**
  - Multi-Thread Programming이 아닌 경우엔 불필요한 기능이 되어버렸다. (동기화, 비동기화 선택 불가)

- **새로 추가된 ArrayList, HashMap과 같은 컬렉션 클래스들**은 **동기화를 자체적으로 하지 않고** 필요한 경우에만 **`java.util.Collections클래스의 동기화 메서드`** 를 이용하여 처리가 가능 (동기화, 비동기화 선택 가능)

**Collections클래스에는 동기화 메서드를 제공, 동기화가 필요한 경우에 사용하면 된다.**

**[Collections클래스의 동기화 메서드]**

```java
static Collections synchronizedCollection(Collection c)
static List synchroziedList(List list)
static Set synchroziedSet(Set s)
static Map synchroziedMap(Map m)
static SortedSet synchroziedSortedSet(SortedSet s)
static SortedMap synchroziedSortedMap(SortedMap m)
```

**[동기화 메서드를 사용하는 방법]**

```java
List syncList = Collections.synchroziedList(new ArrayList(...));
```

### 변경불가 컬렉션 만들기 - unmodifiable

**컬렉션에 저장된 데이터를 보호**하기 위해서 **컬렉션을 변경할 수 없게, 즉 읽기 전용**으로 만들어야 할 때가 있다.
- 주로 **Multi-Thread Programming**에서 **여러 쓰레드가 하나의 컬렉션을 공유**하다보면 **데이터가 손상**될 수 있다.

이를 **방지**하기 위해서 **Collections클래스**에서 **데이터를 변경할 수 없게 읽기 전용으로 만드는 메서드들을 제공**한다.

**[읽기 전용(변경불가)으로 만드는 메서드들]**

```java
static Collections unmodifiableCollection(Collection c)
static List unmodifiableList(List list)
static Set unmodifiableSet(Set s)
static Map unmodifiableMap(Map m)
static NavigableSet unmodifiableNavigableSet(NavigableSet s)
static SortedSet unmodifiableSortedSet(SortedSet s)
static NavigableMap unmodifiableNavigableMap(NavigableMap m)
static SortedMap unmodifiableSortedMap(SortedMap m)
```

### 싱글톤(SingleTon) 컬렉션 만들기 - 단 하나의 객체만을 저장하는 컬렉션 - singleTon

**단 하나의 객체만을 저장하는 컬렉션**을 만들고 싶은 경우, **싱글톤(SingleTon) 컬렉션 메서드를 사용**한다.
- **매개변수로 저장할 요소를 지정** , **해당 요소를 저장하는 컬렉션을 반환**
- **반환된 컬렉션은 변경 불가**

**[단 하나의 객체만을 저장하는 컬렉션을 만드는 메서드들]**

```java
static List singletonList(Object o)
static Set singleton(Object o) // singletonSet이 아님
static Map singletonMap(Object key, Object value)
```

### 한 종류의 객체만 저장하는 컬렉션 만들기 - checked

컬렉션에 모든 종류의 객체를 저장할 수 있다는 것은 장점이자 단점

대부분의 경우, **한 종류의 객체를 저장** , **컬렉션에 지정된 종류의 객체만 저장할 수 있도록 제한**할 때 **checked종류 메서드를 사용**

**[컬렉션에 지정된 종류의 객체만 저장할 수 있도록 제한하는 메서드들]**

```java
static Collections checkedCollection(Collection c, Class type)
static List checkedList(List list, Class type)
static Set checkedSet(Set s, Class type)
static Map checkedMap(Map m, Class keytype, Class valueType)
static Queue checkedQueue(Queue queue, Class type)
static NavigableSet checkedNavigableSet(NavigableSet s, Class type)
static SortedSet checkedSortedSet(SortedSet s, Class type)
static NavigableMap checkedNavigableMap(NavigableMap m, Class keytype, Class valueType)
static SortedMap checkedSortedMap(SortedMap m, Class keytype, Class valueType)
```

[사용방법]

```java
List list = new ArrayList();
List checkedList = checkedList(list, String.class);
checkedList.add("abc"); //ok
checkedList.add(new Integer(3)); // 에러, ClassCastException 발생
```

**두 번째 매개변수**에 **저장할 객체의 클래스를 지정**
- **컬렉션에 저장할 요소의 타입을 제한**하는 것 : **지네릭스(Generics)로 처리 가능**
  - 그럼에도 이런 메서드들을 제공하는 이유 : **호환성** 


### addAll(), rotate(), swap(), shuffle(), sort(), binarySearch(), max(), min(), fill(), nCopies(), disjoint(), copy(), replaceAll()

```java
import java.util.*;
import static java.util.Collections.*;

class CollectionsEx {
	public static void main(String[] args) {
		List list = new ArrayList();
		System.out.println(list);
		
		addAll(list, 1,2,3,4,5);
		System.out.println(list);
		
		rotate(list, 2);	// 오른쪽으로 두 칸씩 이동
		System.out.println(list);
		
		swap(list, 0, 2);	// 첫 번째와 세 번째를 교환(swap)
		System.out.println(list);
		
		shuffle(list);		// 저장된 요소의 위치를 임의로 변경
		System.out.println(list);
		
		sort(list);
		System.out.println(list);
		
		sort(list, reverseOrder());	// 역순 정렬 reverse(list);와 동일
		System.out.println(list);
		
		int idx = binarySearch(list, 3);	// 3이 저장된 위치(index)를 반환
		System.out.println("index of 3 = " + idx);
		
		System.out.println("max = " + max(list));
		System.out.println("min = " + min(list));
		System.out.println("min = " + max(list, reverseOrder()));
		
		fill(list, 9);	// list를 9로 채움
		System.out.println("list = " + list);
		
		// list와 같은 크기의 새로운 list를 생성하고 2로 채운다. 단, 결과는 변경 불가
		List newList = nCopies(list.size(), 2);
		System.out.println("newList = " + newList);
		
		System.out.println(disjoint(list, newList));		// 공통요소가 없으면 true
		
		copy(list, newList);
		System.out.println("newList = " + newList);
		System.out.println("list = " + list);
		
		replaceAll(list, 2, 1);
		System.out.println("list = " + list);
		
		Enumeration e = enumeration(list);
		ArrayList list2 = list(e);
		
		System.out.println("list2 = " + list2);
	}
}
```

```json
실행결과
[]
[1,2,3,4,5]
[4,5,1,2,3]
[1,5,4,2,3]
[4,1,2,3,5]
[5,4,3,2,1]
[1,2,3,4,5]
index of 3 = 2
max=5
min=1
min=1
list=[9,9,9,9,9]
newList=[2,2,2,2,2]
true
newList=[2,2,2,2,2]
list=[2,2,2,2,2]
list=[1,1,1,1,1]
list2=[1,1,1,1,1]
```

## 컬렉션 클래스 정리 & 요약

![컬렉션 클래스간의 관계](https://user-images.githubusercontent.com/56071088/127686159-c7626481-0ad1-472b-af0e-874aa47e64c2.png)

|컬렉션|특징|
|---|---|
|ArrayList|**배열기반, 데이터의 추가와 삭제에 불리, 순차적인 추가,삭제는 제일 빠름, 임의의 요소에 대한 접근성(accessibility)가 뛰어남**|
|LinkedList|**연결기반, 데이터의 추가와 삭제에 유리, 임의의 요소에 대한 접근성이 좋지 않다.**|
|HashedMap|**배열과 연결리스트가 결합된 형태, 추가, 삭제, 검색, 접근성이 모두 뛰어남, 검색에는 최고 성능**|
|TreeMap|**연결기반, 정렬과 검색(특히 범위 검색)에 적합, 검색 성능은 HashedMap보다 떨어짐**|
|Stack|**Vector**를 **상속**받아 구현|
|Queue|**LinkedList**가 **Queue인터페이스를 구현**|
|Properties|HashTable을 상속받아 구현|
|HashSet|HashMap을 이용해서 구현|
|TreeSet|TreeMap을 이용해서 구현|
|LinkedHashMap, LinkedHashSet| HashMap과 HashSet에 **저장순서유지기능**을 추가|





