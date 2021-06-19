# chap8 예외처리

### 프로그램 오류

- 프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 결과를 초래하는 원인
- 발생시점에 따라 분류
    - 컴파일 에러 : 컴파일 시에 발생하는 에러
    - 런타임 에러: 실행 시에 발생하는 에러
    - 논리적 에러: 실행은 되지만, 의도와 다르게 동작하는 것
- 따라서 런타임 에러를 방지하기 위해서는 프로그램의 실행도중 발생할 수 있는 모든 경우의 수를 고려하여 대비 필요
- 자바에서는 런타임에서 발생할 수 있는 오류를 에러와 예외로 구분
    - 에러: 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
    - 예외: 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

### 예외 클래스의 계층구조

- 자바에서는 실행 시 발생할 수 있는 오류를 클래스로 정의

![image_0](../image/chap8_예외처리_1.png)

- 예외의 최고 조상은 Exception 클래스, 두 그룹으로 나눠질 수 있다
    1. Exception 클래스와 그 자손들(윗 부분)
        - checked 예외
    2. RuntimeException 클래스와 그 자손들(아랫 부분)
        - unchecked 예외

![image_0](../image/chap8_예외처리_2.png)

- Exception 클래스들: 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외
- RuntimeException 클래스들: 프로그래머의 실수로 발생하는 예외

### 예외 처리하기

- 정의 : 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것
- 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것
- 하나의 try 블럭 다음에는 여러 종류의 예외를 처리할 수 있도록 하나 이상의 catch 블럭
- 발생한 예외의 종류와 일치하는 단 한 개의 catch 블럭만 수행된다.
- catch 문은 블럭 생략 불가
- 중첩 try-catch문 가능
    - catch 블럭 내에 또 하나의 try - catch문이 포함된 경우, 같은 이름의 참조변수 사용 불가
    - 각 catch 블럭에 선언된 두 참조변수의 영역이 서로 겹치므로 다른 이름을 사용해야 구별 가능

```java
try {
		try { } catch (Exception2 e2) { } // ok
} catch (Exception1 e1) {

} catch (Exception2 e2) {
		try { } catch (Exception2 e2) { } // 에러

} ...
```

### try-catch문에서의 흐름

- try 블럭 내에서 예외가 발생한 경우
    1. 발생한 예외와 일치하는 catch 블럭이 있는지 확인
    2. 일치하는 catch 블럭을 찾게 되면, 해당 catch블럭 내의 문장 수행하고 전체 try - catch문을 빠져나가서 그 다음 문장 계속해서 수행, 만일 일치하는 catch블럭을 찾지 못하면, 예외는 처리되지 못한다.
- try 블럭 내에서 예외가 발생하지 않는 경우
    1. catch 블럭을 거치지 않고 전체 try - catch문을 빠져나가서 수행 진행

### 예외의 발생과 catch 블럭

- 예외가 발생하면, 발생한 예외에 해당하는 클래스의 인스턴스가 생성
- 모든 예외 클래스는 Exception 클래스의 자손이므로, Exception 클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 해당 catch 블럭에 의해서 처리된다.

### printStackTrace()와 getMessage()

- printStackTrace() : 예외발생 당시의 호출스택(call stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
    - 파일에 저장할 수도 있다. 파일입출력 챕터에서
- getMessage(): 발생화 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

### 멀티 catch블럭

- jdk 1.7부터 여러 catch 블럭을 '|'이용해서, 하나의 catch 블럭으로 합칠 수 있다.
- 이를 멀티 catch 블럭
- 중복 코드 줄일 수 있다.
- 연결할 수 있는 예외 클래스의 개수에는 제한이 없다.
- 연결된 예외 클래스가 조상과 자손의 관계에 있다면 컴파일 에러
    - 조상 클래스만 써주는 것과 똑같기 때문

```java
try {
} catch (ExceptionA | ExceptionB e) {
		e.printStackTrace();
} 
```

- 발생한 예외를 멀티 catch 블럭으로 처리하게 되었을 때, 실제로 어떤 예외가 발생한 것인지 알 수 없다.
    - 따라서 연결된 예외 클래스들의 공통 분모인 조상 예외 클래스에 선언된 멤버만 사용 가능
    - 필요하다면 아래처럼 instanceof 연산자로 개별 처리 가능
    - 하지만 이렇게까지 catch 블럭을 합칠 일은 거의 없다
    - 참조변수 e는 공유해야 하기 때문에 상수

    ```java
    try {
    } catch (ExceptionA | ExceptionB e) {
    		e.methodA(); // error

    		if(e instanceof ExceptionA) {
    					ExceptionA e1 = (ExceptionA)e
    					e1.methodA(); // ok
    		} else {

    		}
    } 
    ```

### 예외 발생시키기

- 'throw'를 사용해서 프로그래머가 고의로 예외를 발생시킬 수 있다
    1. 먼저 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든다
    2. 키워드 throw를 이용해서 발생시킨다
- 생성자에 String을 넣어주면 인스턴스에 메시지로 저장, getMessage()를 이용해서 얻을 수 있다.
- RunTimeException 클래스와 그 자손들에 해당하는 예외들은 예외처리를 강제하지 않는다.

```java
Exception e = new Exception("고의로 발생시켰음");
throw e;

==

throw new Exception("고의로 발생시켰음");
```

### 메서드에 예외 선언하기

- 메서드에 선언부에 키워드 throws를 사용해서 메서드 내에서 발생할 수 있는 예외를 적는다.
- 여러 개일 경우에는 쉼표로 구분
- 상속관계까지 고려해야 한다(오버라이딩)
- 메서드의 선언부에 예외를 선언함으로 메서드를 사용하는 사람이 메서드의 선언부를 보았을 때, 어떠한 예외들이 처리되어져야 하는지 파악 가능
- 메서드의 throws에 명시하는 것은 예외를 처리하는 것이 아니라, 자신을 호출한 메서드에게 예외를 전달하여 예외처리를 떠맡기는 것이다.
    - 따라서 어느 한 곳에서는 반드시 try - catch문으로 예외처리를 해주어야 한다.
    1. 예외가 발생한 메서드 자체에서 try - catch로 예외 처리하느냐
    2. 메서드를 호출한 메서드에서 try - catch로 예외 처리하느냐

```java
void method() throws Exception1, Exception2, ... {

}
```

### finally 블럭

- 예외의 발생여부에 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용
- try - catch - finally의 순서
    - 예외 발생: try - catch - finally
    - 예외 발생 x: try - finally
- try/catch 블럭에서 return문이 실행되는 경우에도 finally 블럭의 문장들이 실행된 후에, 실행 중인 메서드를 종료한다.

```java
try {
		System.out.println("1");
		return;
} catch (ExceptionA e) {
		
} finally {
		System.out.println("2");
}

---------------------
1
2
```

### 자동 자원 반환 try - with - resources문

- jdk 1.7부터 try - with - resources문 추가
- 입출력과 관련된 클래스를 사용할 때 유용
- 사용한 후에 꼭 닫아서 자원을 반환해야 하는 경우 사용

```java
try {
		fis = new FileInputStream("score.daat");
		dis = new DataInputStream(fis);
} catch (IOException ie) {
		ie.printStackTrace();
} finally {
		dis.close();
}
-------------------------------------------------
try {
		fis = new FileInputStream("score.daat");
		dis = new DataInputStream(fis);
} catch (IOException ie) {
		ie.printStackTrace();
} finally {
		try {
				if(dis != null)
						dis.close();
		} catch(IOException ie) {
					ie.printStackTrace();
		}
}
```

- close가 예외를 발생 시킬 수 있음, 따라서 아래처럼 해야함
    - 가독성 구림
    - try 예외, finally 예외이면 try 예외 무시됨

```java
// 괄호 () 안에 두 문장 이상 넣을 경우 ';'로 구분
try (FileInputStream fis = new FileInputStream("score.daat");
		 DataInputStream dis = new DataInputStream(fis);) {

} catch (IOException ie) {
} finally {

}
```

- try 블럭을 벗어나는 순간 자동적으로 close() 호출, 그 이후 catch 또는 finally 수행
- 이처럼 try - with - resources문에 의해 자동으로 객체의 close()가 호출될 수 있으려면 클래스가 AutoCloseable이라는 인터페이스를 구현한 것이어야만 한다.

    ```java
    public interface AutoCloseable {
    		void close() throws Exception;
    }
    ```

- close()도 예외를 발생시킬 수 있는데 자동 호출된 close()에서 예외가 발생하면??

    ```java
    class TryWithResourceEx {
    	public static void main(String args[]) {

    		try (CloseableResource cr = new CloseableResource()) {
    			cr.exceptionWork(false); // 예외가 발생하지 않는다.
     		} catch(WorkException e) {
    			e.printStackTrace();
    		} catch(CloseException e) {
    			e.printStackTrace();
    		}
    		System.out.println();
    	
    		try (CloseableResource cr = new CloseableResource()) {
    			cr.exceptionWork(true); // 예외가 발생한다.
     		} catch(WorkException e) {
    			e.printStackTrace();
    		} catch(CloseException e) {
    			e.printStackTrace();
    		}	
    	} // main의 끝
    }

    class CloseableResource implements AutoCloseable {
    	public void exceptionWork(boolean exception) throws WorkException {
    		System.out.println("exceptionWork("+exception+")가 호출됨");

    		if(exception)
    			throw new WorkException("WorkException발생!!!");
    	}

    	public void close() throws CloseException {
    		System.out.println("close()가 호출됨");
    		throw new CloseException("CloseException발생!!!");
    	}
    }

    class WorkException extends Exception {
    	WorkException(String msg) { super(msg); }
    }

    class CloseException extends Exception {
    	CloseException(String msg) { super(msg); }
    }
    ```

    - 첫 번째 것은 close()에서만 예외, 두 번째 것은 exceptionWork()와 close() 모두 예외 발생
        - 첫 번째는 close()에서 발생한 예외 출력
        - 두 번째는 exceptionWork()에서 발생한 예외 출력, close()에서 발생한 예외는 억제된(suppressed) 머리말과 함께 출력
        - 두 예외가 동시에 발생할 수는 없기 때문에, 실제 발생한 예외를 WorkException 하고, CloseException은 억제된 예외로 다룬다
        - 억제된 예외에 대한 정보는 실제로 발생한 예외에 저장된다.
        - Throwable에는 억제된 예외와 관련된 메서드가 정의되어 있다.

            ```java
            void addSuppressed(Throwable exception) // 억제된 예외를 추가
            Throwable[] getSuppressed()             // 억제된 예외(배열) 반환
            ```

### 사용자정의 예외 만들기

- 프로그래머가 새로운 예외 클래스를 정의하여 사용 가능
- 보통 Exception/RuntimeException 클래스로부터 상속받아 클래스 생성
    - unchecked인 RuntimeException 상속받는게 추세
- 필요에 따라 알맞은 예외 클래스 선택 가능
- 메시지를 저장할 수 있으려면, String을 매개변수로 받는 생성자를 추가해주어야 한다.

```java
class MyException extends Exception {
		MyException(String msg) {
				super(msg);
		}
}
```

### 예외 되던지기(exception re-throwing)

- 한 메서드에서 발생할 수 있는 예외가 여럿인 경우 몇 개는 try - catch를 통한 메서드 내에서 자체처리 & 나머지는 선언부에 지정하여 호출한 메서드에서 처리하도록 함으로써 양쪽에서 나눠서 처리 가능
- 단 하나의 예외에 대해서도 예외가 발생한 메서드와 호출한 메서드, 양쪽에서 처리하도록 할 수 있다.
    - 예외를 처리한 후에 인위적으로 다시 발생시키는 방법을 통해서 가능
    - 이것을 예외 되던지기라고 한다.
    1. 먼저 예외가 발생할 가능성이 있는 메서드에서 try - catch문을 사용해서 예외를 처리
    2. catch문에서 throw문을 사용해서 예외를 다시 발생시킨다
    3. 다시 발생한 예외는 이 메서드를 호출한 메서드에게 전달되고 호출한 메서드의 try - catch문에서 예외를 또다시 처리한다.
- 이 방법은 하나의 예외에 대해서 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두에서 처리해줘야 할 작업이 있을 때 사용된다.
- 주의할 점: 예외가 발생한 메서드에서는 try - catch문 사용해서 예외처리 해줌과 동시에 메서드의 선언부에 발생할 예외를 throws에 지정해줘야 함

```java
class ExceptionEx17 {
	public static void main(String[] args) 
	{
		try  {
			method1();		
		} catch (Exception e)	{
			System.out.println("main메서드에서 예외가 처리되었습니다.");
		}
	}	// main메서드의 끝

	static void method1() throws Exception {
		try {
			throw new Exception();
		} catch (Exception e) {
			System.out.println("method1메서드에서 예외가 처리되었습니다.");
			throw e;			// 다시 예외를 발생시킨다.
		}
	}	// method1메서드의 끝
}

실행결과
method1메서드에서 예외가 처리되었습니다.
main메서드에서 예외가 처리되었습니다.
```

- 반환값이 있는 return문의 경우, catch블럭에도 return문이 있어야 한다.
    - 예외가 발생했을 경우에도 값을 반환해야하기 때문
    - 아니면 예외 되던지기를 해서 호출한 메서드로 예외를 전달하면, return문이 없어도 된다.
    - cf) finally 블럭 내에도 return 사용 가능, try/catch 블럭의 return문 다음에 수행, 최종 return 값은 finally return문의 값

    ```java
    	static int method1() {
    		try {
    			System.out.println("method1()이 호출되었습니다.");
    			return 0;
    		} catch (Exception e) {
    			return 1;         // catch블럭 내에도 return문 필요
    		}
    	}	// method1메서드의 끝
    ```

    ```java
    	static int method1() throws Exception{   // 예외 선언
    		try {
    			System.out.println("method1()이 호출되었습니다.");
    			return 0;
    		} catch (Exception e) {
    			throw new Exception();   // return문 대신 예외를 호출한 메서드로 전달
    		}
    	}	// method1메서드의 끝
    ```

    ### 연결된 예외(chained exception)

    - 한 예외가 다른 예외 발생시킬 수도 있다.
        - 예외 a가 예외 b를 발생시켰다면
        - a를 b의 원인 에러(cause error)

    ```java
    try {

    } catch (SpaceException e) {
    		InstallException ie = new InstallException("설치중 예외발생");
    		ie.initCause(e);  // InstallException의 원인 예외를 SpaceException로 지정
    		throw ie;
    }
    ```

    ```java
    Throwable initCause(Throwable cause) // 지정한 예외를 원인 예외로 등록
    Throwable getCause()                 // 원인 예외를 반환
    ```

    ```java
    public class Throwable implements Serializable {
    		private Throwable cause = this;
    }
    ```

    - 발생한 예외를 그냥 처리하지 않고, 원인 예외를 등록해서 다시 예외를 발생시키는 이유는??
        1. 여러가지 예외를 하나의 큰 분류의 예외로 묶어서 다루기 위해서
            - InstallException을 아래 처럼SpaceException와 MemoryException의 조상으로 해서 catch 작성하면, 실제로 발생한 예외가 어떤 것인지 알 수 없다는 문제 발생
            - 그렇다고 SpaceException와 MemoryException의 상속관계를 변경하는 것도 부담
            - 연결된 예외를 사용하면 두 예외는 상속관계가 아니어도 상관없다.

                ```java
                try {

                // InstallException은 SpaceException와 MemoryException의 조상
                } catch (InstallException e) { 
                		e.printStackTrace();
                }
                ```

        2. checked 예외를 unchecked 예외로 바꿀 수 있도록 하기 위해
            - checked 예외가 발생해도 예외를 처리할 수 없는 상황이 발생
            - 그러면 의미없는 try-catch문 추가
            - checked 를 unchecked로 바꾸면 예외처리가 선택적이므로 억지로 예외처리 안해도 된다.

            ```java
            static void startInstall() throws SpaceException, MemoryException { 
            	if(!enoughSpace()) { 		// 충분한 설치 공간이 없으면...
            		throw new SpaceException("설치할 공간이 부족합니다.");
            	}

            	if (!enoughMemory()) {		// 충분한 메모리가 없으면...
            		throw new MemoryException("메모리가 부족합니다.");
            	}
            } // startInstall메서드의 끝
            ```

            ```java
            static void startInstall() throws SpaceException, MemoryException { 
            	if(!enoughSpace()) { 		// 충분한 설치 공간이 없으면...
            		throw new SpaceException("설치할 공간이 부족합니다.");
            	}

            	if (!enoughMemory()) {		// 충분한 메모리가 없으면...
                throw new RuntimeException(new MemoryException("메모리가 부족합니다."));
            	}
            } // startInstall메서드의 끝
            ```

            - MemoryException은 Exception의 자손이므로 반드시 예외를 처리해야 하는데, RuntimeException으로 감싸버렸기 때문에 unchecked 예외가 되었다.
            - 따라서 더이상 선언부에 MemoryException 선언 x
            - initCause() 대신 생성자 사용했다.