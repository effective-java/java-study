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
        ... // jdk8.0 부터 추가된 메서드는 생략

		}
}
```

![Map Entry인터페이스 메서드](https://user-images.githubusercontent.com/56071088/125554452-d13412bb-4a04-4c79-beab-a137f9131c95.png)
