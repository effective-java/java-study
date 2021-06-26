# 8장 예외 처리(exception handling)

## 프로그램 오류

`프로그램 오류` : 프로그램이 실행 중 오작동을 하거나 비정상적으로 종료되는 결과를 초래하는 원인

- `컴파일 에러(Compile Error)` : 컴파일 시에 발생하는 에러

- `런타임 에러(Runtime Error)` : 런타임에 발생하는 에러

- `논리적 에러(Logical Error)` : 실행은 되지만, 의도와는 다르게 작동하는 것
  - Ex) 창고의 재고가 음수가 되는 것, 게임 프로그램에서 비행기가 총을 맞아도 부서지지 않는 것 

컴파일러가 실행도중에 발생할 수 있는 잠재적인 오류까지 검사할 수 없기 때문에, 컴파일은 잘 되었어도 실행 중에 오류에 의해서 잘못된 결과를 얻거나 프로그램이  비정상적으로 종료될 수 있다.

Java에서는 실행 시(runtime) 발생할 수 있는 프로그램 오류를 **예외(Exception)** 와 **에러(Error)** 라고 한다.

- `에러(Error)` : **프로그램 코드** 에 의해서 수습될 수 없는 **심각한 오류**
  - Ex) **메모리 부족(OutOfMemoryError), 스택오버플로우(StackOverFlowError)** 

- `예외(Exception)` : **프로그램 코드** 에 의해서 수습될 수 있는 **다소 미약한 오류** 

에러가 발생하면, 프로그램의 비정상적인 종료를 막을 길이 없지만, **예외는 발생하더라도 프로그래머가 이에 대한 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료를 막을 수 있다!!**

## 예외 클래스의 계층 구조

<img src="https://user-images.githubusercontent.com/56071088/123503534-79143680-d68e-11eb-9127-3a9586f59bb0.png"  width="400" height="250">

Java 에서는 실행 시 발생할 수 있는 오류(Exception과 Error)를 클래스로 정의하였다.

모든 클래스의 조상은 `Object 클래스` 이므로 `Exception` 과 `Error 클래스` **역시 Object 클래스의 자손들이다.**

<img src="https://user-images.githubusercontent.com/56071088/123503475-29356f80-d68e-11eb-99e0-0e927c46205b.jpg"  width="550" height="350">

모든 예외의 최고 조상은 Exception 클래스

## 예외처리하기 - try-catch문

프로그램의 실행도중에 발생하는 에러는 어쩔 수 없지만, 예외는 프로그래머가 이에 대한 예외처리를 미리 해주어야 한다.

`예외처리(exception handling)` : 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것

- 프로그램의 비정상적인 종료를 막고, 정상적인 실행 상태를 유지하는 것

[Tips] 에러(Error)와 예외(Exception)는 모두 프로그램 실행 시(Runtime) 발생하는 오류이다.

발생한 예외를 처리하지 못한다면, 프로그램은 비정상적으로 종료되며, 처리하지 못한 오류(Uncaught Exception)는 `JVM` 의 `예외처리기(UncaughtExceptionHandler)` 가 받아서 **예외의 원인을 화면에 출력한다.**

예외를 처리하기 위해선 `try-catch` 문을 사용한다.

```java
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch(Exception1 e1) {
    // Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch(Exception2 e2) {
    // Exception2이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch(ExceptionN eN) {
    // Exception3이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
}
```

이 중 발생한 예외와 **일치하는 단 한 개의 catch블럭만 수행된다.**

[참고] if문과 달리, try블럭이나 catch블럭 내에 포함된 문장이 하나뿐이어도 괄호{}를 생략할 수 없다.

```java
class ExceptionTest {
    public static void main(Stringp[] args) {
        try {
            try {

            } catch(Exception e) {

            }
        } catch(Exception e) {
            try {

            } catch(Exception e) { // Error. 변수 e가 중복 선언되었다.

            }
        }

        try {

        } catch(Exception e) {

        }
    }
}
```

- try블럭 또는 catch블럭에 또 다른 try-catch문이 포함될 수 있다.

- **catch블럭의 괄호 내에 사용된 변수** 는 catch블력 내에서만 유효하기 때문에, 위의 모든 catch블럭에 `참조변수 e` 하나 만을 사용해도 된다. 

- 그러나, **catch블럭 내에 또 하나의 try-catch문이 포함된 경우** , 같은 이름의 참조변수를 사용해서는 안된다. 
  
  - 각 catch블럭에 선언된 두 참조변수의 영역이 서로 겹치므로 다른 이름을 사용해야만 서로 구별되기 때문이다.
  
  - catch블럭 내의 try-catch문에 선언되어 있는 참조변수의 이름을 `e` 가 아닌 다른 것으로 바꿔야 한다.

```java
class ExceptionEx3 {
	public static void main(String args[]) {
		int number = 100;
		int result = 0;

		for(int i=0; i < 10; i++) {
			try {
				result = number / (int)(Math.random() * 10);
				System.out.println(result);
			} catch (ArithmeticException e)	{
				System.out.println("0"); // ArithmeticException이 발생하는 경우 실행되는 코드
			} 
		} 
	} 
}
```

- ArithmeticException이 발생하면 해당하는 catch 블럭을 찾아내어 그 catch블럭 내의 문장들을 실행한 다음 try-catch문을 벗어나 for문의 다음 반복을 계속 수행하여 작업을 모두 마치고 정상적으로 종료한다.

- 만일 예외처리를 하지 않았다면, ArithmeticException가 발생하면 프로그램이 비정상적으로 종료되었을 것이다.

## try-catch문에서의 흐름

- try블럭 내에서 예외가 발생한 경우
  1. 발생한 예외와 일치하는 catch블럭이 있는지 확인한다.
  2. 일치하는 catch블럭을 찾게 되면, 그 catch블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 문장을 계속해서 수행한다. 만일 일치하는 catch 블럭을 찾지 못하면, 예외는 처리되지 못한다.

- try블럭 내에서 예외가 발생되지 않은 경우
  1. catch블럭을 거치지 않고, 전체 try-catch문을 빠져나가서 수행을 계속한다.

```java
class ExceptionEx5 {
	public static void main(String args[]) {
			System.out.println(1);			
			System.out.println(2);
			try {
				System.out.println(3);
				System.out.println(0/0);   // 0으로 나눠서 고의로 ArithmeticException을 발생시킨다.
				System.out.println(4);  // 실행되지 않는다!!
			} catch (ArithmeticException ae)	{
				System.out.println(5);
			}	// try-catch의 끝
			System.out.println(6);
	}	// main메서드의 끝
}
```

```json
실행결과
1
2
3
5
6
```

## 예외의 발생과 catch 블럭

catch블록은 `괄호()` 와 `블럭{}` 두 부분으로 나눠져 있는데, **괄호() 내에는 처리하고자 하는 예외와 같은 타입의 참조변수 하나를 선언** 해야 한다.

- 예외가 발생하면, **발생한 예외에 해당하는 클래스의 인스턴스가 만들어진다.**
  
- catch블럭의 괄호() 내에 선언된 참조변수의 종류와 생성된 예외클래스의 인스턴스에 **instanceof연산자** 를 이용해서 검사하게 되는데, **검사결과가 true인 catch블럭을 만날 때까지 계속 검사하게 된다.**

- **모든 예외 클래스는 Exception 클래스의 자손** 이므로, catch 블럭의 괄호()에 Exception클래스 타입의 참조변수를 선언해 놓으면 **어떤 종류의 예외가 발생하더라도 이 catch 블럭에 의해서 처리된다.**

```java
class ExceptionEx7 {
	public static void main(String args[]) {
		System.out.println(1);			
		System.out.println(2);
		try {
			System.out.println(3);
			System.out.println(0/0);
			System.out.println(4); 		// 실행되지 않는다.
		} catch (ArithmeticException ae)	{
			if (ae instanceof ArithmeticException) 
				System.out.println("true");	
			System.out.println("ArithmeticException");
		} catch (Exception e)	{ // ArithmeticException을 제외한 모든 예외가 처리된다.
			System.out.println("Exception");
		}	// try-catch의 끝
		System.out.println(6);
	}	// main메서드의 끝
}
```

```json
실행결과
1
2
3
true
ArithmeticException
6
```

- 첫 번째 검사에서 일치하는 catch 블럭을 찾아냈기 때문에 두 번째 catch 블럭은 검사하지 않게 된다.

- 이처럼, try-catch문의 마지막에 Exception클래스 타입의 참조변수를 선언한 catch블럭을 사용하면, 어떤 종류의 예외가 발생하더라도 이 catch블럭에 의해 처리되도록 할 수 있다.

### printStackTrace()와 getMessage()

예외가 발생했을 때 생성되는 예외 클래스의 인스턴스에는 발생한 예외에 대한 정보가 담겨 있으며, getMessage(), printStackTrace()를 통해서 이 정보들을 얻을 수 있다.

catch 블럭의 괄호() 안에 선언된 참조변수를 통해 이 인스턴스에 접근할 수 있다.

- `printStackTrace()` : 예외발생 당시의 호출스택(Call Stack)에 있었던 **메서드의 정보와 에외 메시지** 를 화면에 출력한다.

- `getMessage()` : **발생한 예외클래스의 인스턴스에 저장된 메시지** 를 얻을 수 있다. 

```java
class ExceptionEx8 {
	public static void main(String args[]) {
		System.out.println(1);			
		System.out.println(2);
		try {
			System.out.println(3);
			System.out.println(0/0); // 예외발생!!!
			System.out.println(4); 	 // 실행되지 않는다.
		} catch (ArithmeticException ae)	{
			ae.printStackTrace();
			System.out.println("예외메시지 : " + ae.getMessage());
		}	// try-catch의 끝
		System.out.println(6);
	}	// main메서드의 끝
}
```

```json
실행결과
1
2
3
java.lang.ArithmeticException : / by zero
    at ExceptionEx8.main(ExceptionEx8.java: 7)
예외메시지 : / by zero
6
```

[참고] printStackTrace(PrintStream s) 또는 printStackTrace(PrintWriter s)를 사용하면 발생한 예외에 대한 정보를 파일에 저장할 수도 있다.

### 멀티 catch 블럭

`멀티 catch 블럭` : **JDK 1.7** 부터 여러 catch블럭을 `'|' 기호` 를 이용해서, 하나의 catch 블럭으로 합칠 수 있게 되었다.

- 중복된 코드를 줄일 수 있다.

- '|' 기호로 연결할 수 있는 예외 클래스의 개수에는 제한이 없다.

- **멀티 catch 블럭에 상용되는 '|' 는 논리 연산자가 아니라 기호** 이다.

```java
try {
} catch (ParentException | ChildException e) { // 에러!!
	e.printStackTrace();
}

-> 

try {
} catch (ParentException e) { 
	e.printStackTrace();
}
```

- 멀티 catch블럭의 '|'기호로 연결된 예외 클래스가 **조상과 자손의 관계** 에 있다면 **컴파일 에러** 가 발생한다.

- 조상 클래스 하나만 써주는 것과 같은 효과이기 때문이다. **불필요한 코드는 제거하라는 의미에서 에러가 발생하는 것이다.**

```java
try {
    ...
    // Exception e = new ExceptionA(); 이었을 것이다.
} catch (ExceptionA | ExceptionB e) {

    e.methodA(); // error. ExceptionA에 선언된 methodA()는 호출불가
    
    if (e instanceof ExceptionA) {
        ExceptionA e1 = (ExceptionA) e; // 다운 캐스팅.
        e1.methodA(); // ok. ExceptionA에 선언된 메서드 호출 가능
	} else if(e instanceof ExceptionB){
        ...
	}

    e.printStackTrace();
} 
```

- **멀티 catch블럭** 내에서는 **실제로 어떤 예외가 발생한 것인지 알 수 없다.**

- 그래서 참조변수 e로 멀티 catch블럭에 '|'기호로 연결된 예외 클래스들의 **공통 분모인 조상 예외 클래스**에 **선언된 멤버만 사용할 수 있다.**

- 멀티 catch블럭에 선언된 **참조변수 e는 상수** 이므로, 값을 변경할 수 없다는 제약이 있다.
  - 이것은 여러 catch블럭이 하나의 참조변수를 공유하기 때문에 생기는 제약으로 실제로 참조변수의 값을 바꿀 일은 없을 것이다.

## 예외 발생시키기 : throw

`키워드 throw` 를 사용하여 **프로그래머가 고의로 예외를 발생시킬 수 있다.**

1. 먼저, 연산자 new를 이용해서 발생시키려는 에외 클래스의 객체를 만든 다음
   - Exception e = new Exception("고의로 예외 발생!!");
  
2. `키워드 throw` 를 이용해서 예외를 발생시킨다.


```java
class ExceptionEx9 {
	public static void main(String args[]) {
		try {
			Exception e = new Exception("고의로 발생시켰음.");
			throw e;	 // 예외를 발생시킴
		//  throw new Exception("고의로 발생시켰음.");  

		} catch (Exception e)	{
			System.out.println("에러 메시지 : " + e.getMessage());
		     e.printStackTrace();
		}
		System.out.println("프로그램이 정상 종료되었음.");
	}
}
```

```json
실행결과
에러 메시지 : 고의로 발생시켰음
java.lang.Exception : 고의로 발생시켰음.
    at ExceptionEx9.main(ExceptionEx9.java: 4)
프로그램이 정상 종료되었음.
```

- Exception 인스턴스를 생성할 때, 생성자에 String을 넣어주면, **이 String 이 Exception 인스턴스에 메시지로 저장된다.** 

- 이 메시지는 **getMessage()** 를 이용해서 얻을 수 있다.

**[Checked Exception : 컴파일 에러]**

```java
class ExceptionEx10 {
	public static void main(String[] args) {
		throw new Exception();		// Exception을 고의로 발생시킴.
	}
}
```

**[UnChecked Exception : RuntimeException : 런타임 에러]**

- 컴파일 에러는 발생하지 않으나(성공적으로 컴파일 됨), 런타임 에러가 발생한다. 

```java
class ExceptionEx11 {
	public static void main(String[] args) {
		throw new RuntimeException();	// RuntimeException을 고의로 발생시킴.
	}
}
```

### 미확인(Unchecked) 예외를 사용하라

||Checked Exception|UnChecked Exception|
|---|---|---|
|확인 단계|컴파일 시점|런타임 시점|
|처리 여부|반드시 처리|명시적인 처리를 강제하지 않음|
|트랜잭션 처리|roll-back X|roll-back O|
|예시|Exception의 상속받는 하위 클래스 중 **Runtime Exception을 제외한 모든 예외** => FileNotFoundException, ClassNotFoundException, DataFormatException, IOException, SQLException|**Runtime Exception 하위 예외** => ArrayIndexOutOfBoundException, ClassCastException, ArithmeticException, NullPointerException, IllegalArgumentException, IndexOutOfBoundException, SystemException|

확인된 예외(Checked Exception)은 **OCP(Open Closed Principle)를 위반한다.**

* 메서드에서 확인된 예외를 던졌는데 catch 블록이 3단계 위에 있다면 그 사이의 메서드 모두가 선언부에 해당 예외를 정의해야 한다.
* ***하위 단계에서 코드를 변경하면 상위 단계 메서드 선언부를 전부 고쳐야 한다는 의미**

## 메서드에 예외 선언하기

















