# 13장 쓰레드(thread)

![프로그램과 프로세스](https://user-images.githubusercontent.com/56071088/128971301-a2325a18-17bd-4709-967f-1b54dffb08cd.jpg)

**`프로세스(process)`** : **실행 중인 프로그램(program)**
- 프로그램을 실행하면 OS로부터 실행에 필요한 자원(메모리)을 할당 받아 프로세스가 된다.
- 프로세스는 프로그램을 수행하는데 필요한 데이터와 메모리 등의 자원 그리고 쓰레드로 구성되어 있다.

**`쓰레드(thread)`** : **프로세스의 자원을 이용해서 실제로 작업을 수행하는 것**
- **쓰레드를 가벼운 프로세스,** 즉 **경량 프로세스(LWP, Light-Weight Process)** 라고 부르기도 한다.

**`멀티쓰레드 프로세스(multi-threaded process)`** : 둘 이상의 쓰레드를 가진 프로세스
- 모든 프로세스는 최소 하나 이상의 쓰레드가 존재한다.

![싱글쓰레드와 멀티 쓰레드](https://user-images.githubusercontent.com/56071088/128971297-1d3ad18d-5808-4f4a-9bf5-67ed944db936.png)

**쓰레드를 프로세스라는 작업공간(공장)에서 작업을 처리하는 일꾼(worker)으로 생각하면 편할 것이다.**

**하나의 프로세스가 가질 수 있는 쓰레드의 개수는 제한되어 있지 않다.**

**쓰레드가 작업을 수행**하는데 **개별적인 메모리 공간(호출스택)을 필요**하기 때문에 **프로세스의 메모리 한계**에 따라 **생성할 수 있는 쓰레드의 수가 결정**된다.
- 실제로는 프로세스의 메모리 한계에 다다를 정도로 많은 쓰레드를 생성하는 일은 없을 것이니 걱정 안해도 된다.

### 멀티태스킹과 멀티쓰레딩

**`멀티태스킹(multi-tasking)`** : **다중 작업, 여러 개의 프로세스를 동시에 실행한다.**

**`멀티쓰레딩(multi-threading)`** : **하나의 프로세스 내에서 여러 쓰레드가 동시에 작업을 수행한다.**
- **CPU의 코어(core)** 가 **한 번**에 **단 하나의 작업만을 수행**
- **실제로 동시에 처리되는 작업의 개수**는 **코어의 개수**와 **일치**한다.

**처리해야 되는 쓰레드의 수**는 **언제나 코어의 개수**보다 **훨씬 많기** 때문에 **각 코어**가 **아주 짧은 시간** 동안 **여러 작업**을 **번갈아 가며 수행함**으로써 **여러 작업들이 모두 동시에 수행하는 것** 처럼 **'보이게 한다'**

**프로세스의 성능**이 **단순히 쓰레드의 개수에 비례**하는 것은 **아니며**, **하나의 쓰레드를 가진 프로세스**보다 **두 개의 쓰레드를 가진 프로세스**가 **오히려 더 낮은 성능을 보일 수도 있다.**

### 멀티쓰레딩의 장점

**`도스(DOS)`** 와 같이 **한 번에 한 가지 작업**만 할 수 있는 **OS**

**`윈도우`** 와 같이 **멀티태스킹이 가능한 OS**

이 둘의 차이는 **싱글쓰레드 프로그램**과 **멀티쓰레드 프로그램의 차이**와 같다고 보면 된다.

**멀티쓰레딩의 장점**
- **CPU의 사용률**을 향상시킨다.
- **자원**을 보다 **효율적으로 사용**할 수 있다.
- 사용자에 대한 **응답성**이 향상된다.
- **작업이 분리**되어 **코드가 간결**해진다.

1. 메신저로 채팅, 2. 파일을 다운로드, 3. 음성 대화를 나누기 를 한번에 할 수 있는 이유 : **멀티쓰레드로 작성되어 있기 떄문이다.**
- 만일 싱글쓰레드로 작성되어 있디면 파일을 다운로드 받는 동안에는 다른 일(채팅)을 전혀 할 수 없다.

**여러 사용자에게 서비스를 해주는 서버 프로그램**의 경우 : **멀티쓰레드로 작성하는 것은 필수적**이어서 **하나의 서버 프로세스**가 **여러 개의 쓰레드를 생성**해서 **쓰레드**와 **사용자의 요청**이 **일대일로 처리**되도록 프로그래밍 해야 한다.
- 만일 싱글쓰레드로 서버 프로그램을 작성한다면 사용자의 요청마다 **새로운 프로세스를 생성**해야 하는데
- **프로세스를 생성하는 것**은 **쓰레드를 생성하는 것**이 비해 **더 많은 시간과 메모리 공간이 필요**하기 때문에 **많은 수의 사용자 요청을 서비스하기 어렵다.**

멀티쓰레딩이 장점만 있는 것은 아니다!!
- 멀티쓰레드 프로세스는 여러 쓰레드가 같은 프로세스 내에서 자원을 공유하면서 작업을 하기 때문에 
- 발생할 수 있는 동기화(synchronization), 교착상태(deadlock)와 같은 문제들을 고려해서 신중히 프로그래밍 해야 한다.

**`교착상태(deadlock)`** : **두 쓰레드가 자원을 점유한 상태에서 서로 상대편이 점유한 자원을 사용하려고 기다리느라 진행이 멈춰있는 상태를 말한다.**

## 쓰레드의 구현과 실행

**쓰레드를 구현하는 방법** 
1. Thread클래스를 상속받는 방법
2. Runnable인터페이스를 구현하는 방법

어느 쪽을 선택해도 별 차이는 없지만 **Thread클래스를 상속**받으면 **다른 클래스를 상속 받을 수 없기 때문에**, **Runnable인터페이스를 구현하는 것이 일반적!!**

![Runnable인터페이스](https://user-images.githubusercontent.com/56071088/128974394-c033e0fb-3f43-499e-b991-dc5d1d0cbf0c.png)

Runnable인터페이스를 구현하는 방법의 장점
1. 재사용성(reusablilty)이 높다.
2. 코드의 일관성(Consistency)을 유지
3. 보다 객체지향적인 방법이다.

**[1. Thread 클래스를 상속]**

```java
class MyThread extends Thread {
    public void run() { // Thread클래스의 run()을 오버라이딩
        /* 작업 내용 */ 
    }
}
```

**[2. Runnable 인터페이스를 구현]**

```java
class MyThread implements Runnable {
    public void run() { // Runnable인터페이스의 run()을 오버라이딩
        /* 작업 내용 */ 
    }
}
```

Runnable인터페이스는 오로지 run()만 정의되어 있는 간단한 인터페이스이다.
- Runnable인터페이스를 구현하기 위해서 해야 할 일은 추상 메서드인 run()의 몸통(body){}을 만들어 주는 것 뿐이다.

```java
public interface Runnable {
    public abstract void run();
}
```

**쓰레드를 구현한다는 것** : 위의 2가지 방법 중 어떤 것을 선택하든지, 그저 **쓰레드를 통해 작업하고자 하는 내용**으로 **run()의 몸통{}을 채우는 것 뿐**이다.

[쓰레드를 구현하는 2가지 방법의 차이점을 보여주는 예제]

```java
class ThreadEx1 {
	public static void main(String args[]) {
		ThreadEx1_1 t1 = new ThreadEx1_1();

		Runnable r  = new ThreadEx1_2();
		Thread   t2 = new Thread(r);	  // 생성자 Thread(Runnable target)

		t1.start();
		t2.start();
	}
}

class ThreadEx1_1 extends Thread {
	public void run() {
		for(int i=0; i < 5; i++) {
			System.out.println(getName()); // 조상인 Thread의 getName()을 호출
		}
	}
}

class ThreadEx1_2 implements Runnable {
	public void run() {
		for(int i=0; i < 5; i++) {
		    System.out.println(Thread.currentThread().getName()); // Thread.currentThread() - 현재 실행중인 Thread를 반환한다.
		}
	}
}
```

Thread클래스를 상속받은 경우, Runnable인터페이스를 구현한 경우의 **인스턴스 생성 방법**이 **다르다.**

```java
ThreadEx1_1 t1 = new ThreadEx1_1();

Runnable r  = new ThreadEx1_2();
Thread   t2 = new Thread(r);	  // 생성자 Thread(Runnable target)

Thread t2 = new Thread(new ThreadEx1_2());
```

Runnable인터페이스를 구현한 경우
- Runnable인터페이스를 구현한 클래스의 인스턴스를 생성한 다음,
- 이 인스턴스를 Thread클래스의 생성자의 매개변수로 제공해야 한다.

[실제 Thread클래스의 소스코드(Thread.java)를 이해하기 쉽게 수정한 것]

```java
public class Thread {
    private Runnable r; // Runnable인터페이스를 구현한 클래스의 인스턴스를 참조하기 위한 변수

    public Thread(Runnable r) {
        this.r = r;
    }

    public void run() {
        if(r != null) {
            r.run(); // Runnable인터페이스를 구현한 인스턴스의 run()을 호출
        }
    }
}
```

```json
실행결과
Thread-0
Thread-0
Thread-0
Thread-0
Thread-0
Thread-1
Thread-1
Thread-1
Thread-1
Thread-1
```

- **인스턴스변수**로 **Runnable타입의 변수 r**을 선언
- **생성자**를 통해서 **Runnable인터페이스를 구현한 인스턴스를 참조**하도록 되어 있는 것
- **run()을 호출**하면, **참조변수 r**을 통해서 **Runnable인터페이스를 구현한 인스턴스의 run()** 이 호출된다.
  - 이렇게 함으로써 **상속을 통해 run()을 오버라이딩하지 않고도 외부로부터 run()을 제공**받을 수 있게 된다.

**Thread클래스 메서드를 호출하는 방법**

- **Thread클래스를 상속**받으면, 자손 클래스에서 **조상인 Thread클래스의 메서드를 직접 호출**할 수 있지만, 
- **Runnable인터페이스를 구현**하면 **Thread클래스의 static메서드인 currentThread()를 호출**하여 **쓰레드에 대한 참조**를 얻어 와야만 **호출이 가능**하다.

```java
static Thread currentThread(); // 현재 진행중인 쓰레드의 탐조를 반한다.
String getName(); // 쓰레드의 이름을 반환한다.
```

- Thread를 상속받은 ThreadEx_1은 간단히 getName()을 호출
- Runnable인터페이스를 구현한 ThreadEx_2의 멤버는 run() 밖에 없다.
  - Thread클래스의 getName()을 호출하고 싶다면, `Thread.currentThread().getName()` 으로 호출해야만 한다.

```java
class ThreadEx1_1 extends Thread {
	public void run() {
		System.out.println(getName()); // 조상인 Thread의 getName()을 호출
	}
}

class ThreadEx1_2 implements Runnable {
	public void run() {
        System.out.println(Thread.currentThread().getName()); // Thread.currentThread() - 현재 실행중인 Thread를 반환한다.
	}
}

System.out.println(Thread.currentThread().getName()); 

-->

Thread t = Thread.currentThread();
String name = t.getName();
System.out.println(name);
```

**쓰레드의 이름을 지정하는 방법**

- **생성자나 메서드**를 통해서 **지정** 또는 **변경이 가능**하다.

```java
Thread(Runnable target, String name)
Thread(String name)
void setName(String name)
```

**쓰레드의 이름을 지정하지 않는다면,** `Thread-번호`의 형식으로 이름이 정해진다.

### 쓰레드의 실행 - start()

쓰레드를 생성했다고 해서 자동으로 실행되는 것이 아니다. **start()를 호출해야만 쓰레드가 실행**된다. 
- start()가 호출되었다고 바로 실행되는 것도 아니다.
- **start()가 호출**되면, **실행대기 상태**에 있다가 **자신의 차례가 되어야 실행된다.**
  - 물론 실행대기중인 쓰레드가 하나도 없으면 곧바로 실행상태가 된다. 

```java
t1.start();
t2.start();
```

**한번 실행이 종료된 쓰레드는 다시 실행할 수 없다.**
- **하나의 쓰레드**가 **한 번만 호출될 수 있다**는 뜻이다.

만일 쓰레드의 작업을 한번 더 수행해야 한다면, 
- **새로운 쓰레드를 생성한 다음에 start()를 호출**해야 한다.
- **하나의 쓰레드**에 **2번 이상 start()를 호출**한다면, **IllegalThreadStateException**이 발생

```java
ThreadEx1_1 t1 = new ThreadEx1_1();
t1.start();
t1.start(); // 예외 발생

ThreadEx1_1 t1 = new ThreadEx1_1();
t1.start();
t1 = new ThreadEx1_1();
t1.start(); // ok
```

## start()와 run()

**start()와 run()의 차이점**

![main메서드에서 run()을 호출](https://user-images.githubusercontent.com/56071088/128971684-9b017f2e-cdad-4410-90b2-8c1d2d1d9627.png)

**`run()`** : main메서드에서 run()을 호출하는 것은 생성된 쓰레드를 실행시키는 것이 아니라 **단순히 클래스에 선언된 run()메서드를 호출**하는 것 뿐이다.

![main메서드에서 start()를 호출](https://user-images.githubusercontent.com/56071088/128974391-34a3acc3-d918-4c30-8d8d-e1b2ee9e03df.png)


**`start()`** : **새로운 쓰레드**가 **작업을 실행**하는데 **필요한 호출스택(call stack)을 생성**한 다음에 **run()을 호출**해서, **생성된 호출스택**에 **run()이 첫 번째**로 올라가게 한다.

**모든 쓰레드**는 **독립적인 작업을 수행**하기 위해 **자신만의 호출스택**을 필요로 하기 때문에, 
- **새로운 쓰레드를 생성하고 실행**시킬 때마다 **새로운 호출 스택이 생성**되고,
-  **쓰레드가 종료**되면 **작업에 사용된 호출스택은 소멸**된다.

![새로운 쓰레드를 생성하고 start()를 호출](https://user-images.githubusercontent.com/56071088/128971307-486b61e1-e559-4fd2-8c7f-e45881087dba.png)

1. main()메서드에서 쓰레드의 start()를 호출한다.
2. start()는 새로운 쓰레드를 생성하고, 쓰레드가 작업하는데 사용될 호출스택을 생선한다.
3. 새로 생성된 호출스택에 run()이 호출되어, **쓰레드**가 **독립된 공간**에서 **작업을 수행**한다.
4. 이제는 **호출스택이 2개**이므로 **스케줄러(scheduler)** 가 **정한 순서**에 의해서 **번갈아 가면서 실행**된다.
 
호출스택에서는 가장 위에 있는 메서드가 현재 실행중인 메서드이고 나머지 메서드들은 대기 상태에 있다.

쓰레드가 2개 이상일 때는 호출스택의 최상위에 있는 메서드 일지라도 대기 상태에 있을 수 있다.
- 스케줄러(scheduler)는 실행대기중인 쓰레드들의 우선순위를 고려하여 실행순서와 실행시간을 결정
- 각 쓰레드들은 작성된 스케쥴에 따라 자신의 순서가 되면 지정된 시간동안 작업을 수행

주어진 시간동안 작업을 마치지 못한 쓰레드는 다시 자신의 차례가 돌아올 때까지 대기상태로 있게 된다.
- 작업을 마친 쓰레드(run()의 수행이 종료된 쓰레드)는 호출스택이 모두 비워지면서 이 쓰레드가 사용하던 호출스택은 사라진다.

마치 Java프로그램을 실행하면 호출스택이 생성되고 main메서드가 처음으로 호출되고, main메서드가 종료되면 호출스택이 비워지면서 프로그램도 종료되는 것과 같다.

### main쓰레드

**`main쓰레드`** : main메서드의 작업을 수행하는 쓰레드

프로그램을 실행하면 기본적으로 하나의 쓰레드(일꾼)을 생성하고, 그 쓰레드가 main메서드를 호출해서 작업이 수행되도록 하는 것

[main메서드가 종료된 후의 호출 스택]

![run()](https://user-images.githubusercontent.com/56071088/128973309-9675e93e-d237-4d72-99e3-98d2ced6d747.jpg)


- **main메서드가 수행을 마쳤다**하더라도, **다른 쓰레드가 아직 작업을 마치지 않은 상태**라면 **프로그램은 종료되지 않는다.**

쓰레드의 종류
1. `사용자 쓰레드(user-thread)(non-daemon thread)`
2. `데몬 쓰레드(daemon-thread)`

***실행 중인 사용자 쓰레드가 하나도 없을 때 프로그램은 종료된다.***

[새로 생성한 쓰레드엣 고의로 예외를 발생시키고 printStackTrace()를 이용해서 예외가 발생한 당시의 호출스택을 출력하는 예제]

```java
 class ThreadEx2 {
	public static void main(String args[]) throws Exception {
		ThreadEx2_1 t1 = new ThreadEx2_1();
		t1.start();
	}
}

class ThreadEx2_1 extends Thread {
	public void run() {
		throwException();
	}

	public void throwException() {
		try {
			throw new Exception();		
		} catch(Exception e) {
			e.printStackTrace();	
		}
	}
}
```

![run throwException](https://user-images.githubusercontent.com/56071088/128974407-a11503ee-dcf0-4c6f-92e4-15b4f0189cee.png)

- 호출스택의 첫 번째 메서드가 main메서드가 아니라 run메서드이다.
- 한 쓰레드가 예외가 발생해서 종료되어도 다른 쓰레드의 실행에는 영향을 미치지 않는다.
- main쓰레드의 호출 스택이 없는 이유는 main쓰레드가 종료되었기 때문이다.

[쓰레드를 생성하지 않고 run()만 호출한 예제]

```java
class ThreadEx3 {
	public static void main(String args[]) throws Exception {
		ThreadEx3_1 t1 = new ThreadEx3_1();
		t1.run();
	}
}

class ThreadEx3_1 extends Thread {
	public void run() {
		throwException();
	}

	public void throwException() {
		try {
			throw new Exception();		
		} catch(Exception e) {
			e.printStackTrace();	
		}
	}
}
```

![main run throwException](https://user-images.githubusercontent.com/56071088/128974398-bfefe52d-4300-4a65-a0e3-7054e599059b.png)

- 쓰레드가 새로 생성되지 않았다.
- ThreadEx3_1 클래스의 run()만 호출
- main쓰레드의 호출스택에 main메서드가 포함되어 있다.

## 싱글쓰레드와 멀티쓰레드

**싱글쓰레드 프로세스와 멀티쓰레드 프로세스의 차이점**

- 두 개의 작업을 하나의 쓰레드(th1)으로 처리하는 경우
  - 한 작업을 마친 후에, 다른 작업을 수행 
- 두 개의 작업을 두 개의 쓰레드(th1, th2)로 처리하는 경우
  - 짧은 시간 동안 2개의 쓰레드(th1, th2)가 번갈아 가면서 작업을 수행해서 동시에 두 작업이 처리되는 것과 같이 느끼게 된다.

[싱글쓰레드 프로세스와 멀티쓰레드 프로세스의 비교(싱글코어)]

![싱글쓰레드 프로세스와 멀티쓰레드 프로세스의 비교(싱글코어)](https://user-images.githubusercontent.com/56071088/128975061-d8c7d959-f9bf-414e-addd-44fdee1988d6.png)

**두 개의 작업을 수행한 시간은 거의 같다!!**
- 오히려 2개의 쓰레드로 작업한 시간이 싱글 쓰레드로 작업한 시간보다 더 걸리게 되었다.
  - 쓰레드간의 작업전환(context-switching)에 시간이 걸리기 때문이다.
    - **`컨텍스트 스위칭(context-switching)`** : 프로세스 또는 쓰레드간의 작업 전환
  - 작업을 전환할 때는 현재 진행 중인 작업의 상태(다음에 실행해야할 위치(PC, Program Counter))등의 정보를 저장하고 읽어 오느 시간이 소요된다.
  - 쓰레드의 스위칭에 비해 프로세스의 스위칭이 더 많은 전보를 저장해야하므로 더 많은 시간이 소요된다.

싱글 코어에서 단순히 CPU만을 사용하는 계산 작업이라면 오히려 멀티쓰레드보다 싱글쓰레드로 프로그래밍하는 것이 더 효율적이다.

[출력하는 작업을 하나의 쓰레드가 연속적으로 처리하는 시간을 측정하는 예제]

```java
class ThreadEx4 {
	public static void main(String args[]) {
		long startTime = System.currentTimeMillis();

		for(int i=0; i < 500; i++)
			System.out.printf("%s", new String("-"));		

		System.out.print("¼Ò¿ä½Ã°£1:" +(System.currentTimeMillis()- startTime)); 

		for(int i=0; i < 500; i++) 
			System.out.printf("%s", new String("|"));		

		System.out.print("¼Ò¿ä½Ã°£2:"+(System.currentTimeMillis() - startTime));
	}
}
```

```json
실행결과
---------------------------------------------소요시간1:68
|||||||||||||||||||||||||||||||||||||||||||||소요시간2:117
```

[새로운 쓰레드를 하나 생성해서 두 개의 쓰레드가 작업을 하나씩 나누어서 수행하는 예제]

```java
class ThreadEx5 {
	static long startTime = 0;

	public static void main(String args[]) {
		ThreadEx5_1 th1 = new ThreadEx5_1();
		th1.start();
		startTime = System.currentTimeMillis();

		for(int i=0; i < 300; i++) {
			System.out.print("-");
		}

		System.out.print("¼Ò¿ä½Ã°£1:" + (System.currentTimeMillis() - ThreadEx5.startTime));
	}
}

class ThreadEx5_1 extends Thread {
	public void run() {
		for(int i=0; i < 300; i++) {
			System.out.print("|");
		}

		System.out.print("¼Ò¿ä½Ã°£2:" + (System.currentTimeMillis() - ThreadEx5.startTime));
	}
}
```

```json
실행결과

싱글코어
----|||||-----|||||....
소요시간1:25 소요시간2:21

멀티코어
---|||----||-|||||--|||||-----||....
소요시간2:56-------...소요시간1:62
```

두 작업이 아주 짧은 시간동안 번갈아가면서 실행되었으며 거의 동시에 작업이 완료되었다.

하나의 쓰레드로 작업한 시간보다 2개의 쓰레드로 작업하는데 시간이 더 오래 걸렸다.

2가지 이유가 있다.
1. 두 쓰레드가 번갈아가면서 작업을 수행하기 때문에 **쓰레드간의 작업 전환 시간이 소요**된다.
2. 한 쓰레드가 **출력하고 있는 동안** 다른 쓰레드는 **출력이 끝나기를 기다려야 하는데**, 이때 발생하는 **대기시간**이 있다.

위의 결과는 실행할 때 마다 다른 결과를 얻을 수 있다.
->

**실행 중인 예제프로그램(프로세스)** 가 **OS의 프로세스 스케줄러**의 영향을 받기 때문이다.
- **JVM**의 **쓰레드 스케줄러**에 의해서 어떤 쓰레드가 얼마동안 실행될 것인지 결정된다.
- 마찬가지로, **프로세스**도 **프로세스 스케줄러**에 의해서 **실행순서**와 **실행시간**이 결정된다.
  - **매 순간 상황에 따라 프로세스에게 할당되는 시간 역시 일정하지 않게 된다.**

**쓰레드**가 이러한 **불확실성**을 가지고 있다는 것을 염두하자.

**Java**가 **OS(플랫폼) 독립적**이라고 하지만 **실제로는 OS종속적인 부분**이 몇가지 있는데 **쓰레드도 그 중의 하나**이다.

**JVM의 종류**에 따라 **쓰레드 스케줄러의 구현방법**이 **다를 수 있기** 때문에 **멀티쓰레드**로 작성된 프로그램을 **다른 종류의 OS**에서도 **충분히 테스트해 볼 필요가 있다.**

### 싱글코어와 멀티코어의 비교

<img src="https://user-images.githubusercontent.com/56071088/128988923-bc2c1749-1f94-4c43-9fdf-7b01eac9aab1.png"  width="500" height="600">

싱글 코어의 경우
- 멀티쓰레드라도 하나의 코어가 번갈아가면서 작업을 수행한다.
- 두 작업이 서로 겹치지 않는다.

멀티 코어의 경우
- 멀티쓰레드로 두 작업을 수행하면, 동시에 두 쓰레드가 수행된다.
- 두 작업 A와 B가 겹치는 부분이 발생
  - **화면(console)이라는 자원**을 놓고 **두 쓰레드가 경쟁**하게 된다.

**`병행(concurrent)`** : **여러 쓰레드**가 **여러 작업을 동시에** 진행하는 것

**`병렬(parallel)`** : **하나의 작업**을 **여러 쓰레드**가 나눠서 처리하는 것

[두 쓰레드가 서로 다른 자원을 사용하는 작업의 경우 - 싱글쓰레드, 멀티쓰레드 비교]

![blocking](https://user-images.githubusercontent.com/56071088/128988932-52ba17da-fd5c-4034-930f-90ec3703951c.png)

**두 쓰레드**가 **서로 다른 자원**을 사용하는 작업의 경우 : **멀티쓰레드가 싱글쓰레드보다 훨씬 효율적이다.**

싱글쓰레드의 경우 :
- 사용자가 입력을 마칠 때까지 다른 쓰레드는 아무 일도 하지 못하고 기다리기만 해야 한다.
  
멀티쓰레드의 경우 :
- **사용자의 입력을 기다리는 동안 다른 쓰레드가 작업을 처리할 수 있기 때문에 보다 효율적인 CPU의 사용**이 가능하다.
- 멀티쓰레드 프로세스의 경우가 작업을 더 빨리 마치는 것을 알 수 있다.

[싱글 쓰레드로 두 작업을 수행하는 경우]

```java
import javax.swing.JOptionPane;

class ThreadEx6 {
	public static void main(String[] args) throws Exception
	{
		String input = JOptionPane.showInputDialog("아무 값이나 입력하세요."); 
		System.out.println("입력하신 값은 " + input + "입니다.");

		for(int i=10; i > 0; i--) {
			System.out.println(i);
			try {
				Thread.sleep(1000);
			} catch(Exception e ) {}
		}
	}
}
```

- 하나의 쓰레드로 2개의 작업을 수행하기 때문에 입력을 마치지 전까지 화면에 숫자 출력을 하지 않는다.

[멀티 쓰레드로 두 작업을 수행하는 경우]

```java
import javax.swing.JOptionPane;

class ThreadEx7 {
	public static void main(String[] args) throws Exception 	{
		ThreadEx7_1 th1 = new ThreadEx7_1();
		th1.start();

		String input = JOptionPane.showInputDialog("아무 값이나 입력하세요."); 
		System.out.println("입력하신 값은 " + input + "입니다.");
	}
}

class ThreadEx7_1 extends Thread {
	public void run() {
		for(int i=10; i > 0; i--) {
			System.out.println(i);
			try {
				sleep(1000);
			} catch(Exception e ) {}
		}
	}
}
```

- 2개의 쓰레드로나누어서 처리했기 때문에 입력을 마치지 않았어도 화면에 숫자가 출력된다.


## 쓰레드의 우선순위

**쓰레드(Thread)** 는 **우선순위(priority)** 라는 **속성(멤버변수)** 를 가지고 있는데, **이 우선순위의 값에 따라** 쓰레드가 얻는 **실행시간**이 달라진다.
- 쓰레드가 수행하는 **작업의 중요도**에 따라 **쓰레드의 우선순위를 서로 다르게 지정**하여
- **특정 쓰레드**가 **더 많은 작업시간**을 갖도록 할 수 있다.

EX) 파일 전송 기능이 있는 메신저의 경우
- 파일다운로드 하는 쓰레드보다 채팅내용을 전송하는 쓰레드의 우선순위를 높게 해야 사용자가 채팅을 하는데 불편함을 느끼지 않을 것이다.
- 대신 파일 다운로드하는 작업에 걸리는 시간은 더 걸릴 것이다.
- **시각적인 부분이나 사용자에게 빠르게 반응해야 하는 작업**을 하는 **쓰레드의 우선순위**는 다른 작업을 수행하는 쓰레드에 비해 **더 높아야 한다.**

### 쓰레드의 우선순위 지정하기

**쓰레드의 우선순위를 지정하는 메서드와 상수**

```java
public final void setPriority(int newPriority)
public final int getPriority() { return priority; }

public static final int MIN_PRIORITY = 1; // 최소
public static final int NORM_PRIORITY = 5; 
public static final int MAX_PRIORITY = 10; // 최대 우선순위
```

- 쓰레드가 가질 수 있는 우선순위의 범위는 `1~10`이며, 숫자가 높을수록 우선순위가 높다.
- **쓰레드의 우선순위**는 **쓰레드를 생성한 쓰레드로부터 상속**받는다.
  - **main메서드**를 **수행하는** 쓰레드의 우선순위는 **우선순위가 5**
  - **main메서드**에서 **생성한** 쓰레드의 **우선순위도 자동적으로 5가 된다.**

[쓰레드의 우선순위를 지정하는 예제]

```java
class ThreadEx8 {
	public static void main(String args[]) {
		ThreadEx8_1 th1 = new ThreadEx8_1();
		ThreadEx8_2 th2 = new ThreadEx8_2();

		th2.setPriority(7);

		System.out.println("Priority of th1(-) : " + th1.getPriority() );
		System.out.println("Priority of th2(|) : " + th2.getPriority() );
		th1.start();
		th2.start();
	}
}

class ThreadEx8_1 extends Thread {
	public void run() {
		for(int i=0; i < 300; i++) {
			System.out.print("-");
			for(int x=0; x < 10000000; x++);
		}
	}
}

class ThreadEx8_2 extends Thread {
	public void run() {
		for(int i=0; i < 300; i++) {
			System.out.print("|");
			for(int x=0; x < 10000000; x++);
		}
	}
}
```

쓰레드th1, th2 모두 main메서드에서 생성했기 때문에 자동적으로 우선순위가 5를 상속받았다.

```java
th2.setPriority(7);
```

th2의 우선순위를 7로 변경한 다음에 start()를 호출해서 쓰레드를 실행시켰다.

쓰레드를 **실행하기 전에만 우선순위를 변경할 수 있다!!**

[쓰레드의 우선순위에 따라 할당되는 시간의 차이]

![쓰레드의 우선순위](https://user-images.githubusercontent.com/56071088/128995146-5551d0b0-a5f9-4e86-8682-dc482f61192e.png)

이전의 예제와 달리 **우선순위가 높은** th2의 **실행시간**이 th1에 비해 **상당히 늘어났다.**

**싱글코어의 경우**

두 개의 쓰레드로 두개의 작업을 실행했을 때
- 우선순위가 같은 경우
  - 각 쓰레드에게 거의 같은 양의 실행시간이 주어진
- 우선순위가 다른 경우
  - **우선순위가 높은 th1에게 상대적으로 th2보다 더 많은 양의 실행시간이 주어지고**, 
  - 결과적으로 작업A가 작업B보다 **더 빨리 완료**될 수 있다.

**멀티코어의 경우**

두 개의 쓰레드로 2개의 작업을 실행했을 때
- **쓰레드의 우선순위에 따른 차이가 전혀 없었다.**
- 결국 **쓰레드의 우선순위에 차등을 두어 쓰레드를 실행시키는 것이 별 효과가 없었다.**

**쓰레드에 높은 우선순위를 주면 더 많은 실행시간과 실행기회를 갖게 될 것이라고 기대할 수 없는 것**

**멀티코어**라고 해도 **OS마다 다른 방식으로 스케쥴링**하기 때문에, **어떤 OS에서 실행**하느냐에 따라 **다른 결과**를 얻을 수 있다.

굳이 우선순위에 차등을 두어 쓰레드를 실행하려면
- **OS의 스케쥴링 정책, JVM의 구현**을 직접 확인해봐야한다.
- Java는 쓰레드가 우선순위에 따라 어떻게 다르게 처리되어야 하는지 **강제하지 않으므로**
- **쓰레드의 우선순위와 관련된 구현**이 **JVM마다 다를** 수 있기 때문이다.
- 만일 확인하더라도 **OS의 스케쥴러에 종속적**이므로 **어느 정도 예측만 가능한 정도일 뿐 정확히 알 수 없다.**

쓰레드에 우선순위를 부여하는 대신 **작업에 우선순위를 두어 PrioriryQueue에 저장**해놓고, **우선순위가 높은 작업이 먼저 처리되도록 하는 것이 나을 수 있다!!**

## 쓰레드 그룹(Thread Group)

**`쓰레드 그룹(Thread Group)`** : **서로 관련된 쓰레드를 그룹으로 다루기 위한 것**
- 쓰레드 그룹을 생성해서 쓰레드를 그룹으로 묶어서 관리할 수 있다.
- 쓰레드 그룹에 다른 쓰레드 그룹을 포함 시킬 수 있다.

쓰레드 그룹은 보안상의 이유로 도입된 개념
- 자신이 속한 쓰레드 그룹이나 하위 쓰레드 그룹은 변경할 수 있지만 다른 쓰레드 그룹의 쓰레드를 변경할 수는 없다.

[ThreadGroup의 주요 생성자와 메서드]

![ThreadGroup의 생성자와 메서드](https://user-images.githubusercontent.com/56071088/128998469-93b63de8-5378-49d6-ba74-0e29417528e3.jpg)

**쓰레드를 쓰레드 그룹에 포함**시키려면 **Thread의 생성자**를 이용

```java
Thread(ThreadGroup group, String name)
Thread(ThreadGroup group, Runnable target)
Thread(ThreadGroup group, Runnable target, String name)
Thread(ThreadGroup group, Runnable target, String name, long stackSize)
```

모든 쓰레드는 **반드시 쓰레드 그룹에 포함되어 있어야 한다.**
- 쓰레드 그룹을 지정하는 생성자를 사용하지 않은 쓰레드는 기본적으로 자신을 생성한 쓰레드와 같은 쓰레드 그룹에 속하게 된다.
  
Java어플리케이션이 실행되면, 
- **JVM**은 **main**과 **system**이라는 **쓰레드 그룹(ThreadGroup)** 을 만들고,
- **JVM 운영에 필요한 쓰레드들을 생성**해서 이 **쓰레드 그룹에 포함**시킨다.
  - **main메서드**를 수행하는 **main이라는 이름의 쓰레드**는 **main쓰레드 그룹**에 속하고,
  - **Garbage Collection**을 수행하는 **Finalizer쓰레드**는 **system쓰레드 그룹**에 속한다.

**우리가 생성하는 모든 쓰레드 그룹**은 **main쓰레드 그룹의 하위 쓰레드 그룹**이 되며, **쓰레드 그룹을 지정하지 않고 생성한 쓰레드**는 **자동적으로 main쓰레드 그룹**에 속하게 된다.

[Thread의 ThreadGroup와 관련된 메서드]

```java
ThreadGroup getThreadGroup() // 쓰레드 자신이 속한 쓰레드 그룹을 반환한다.
void uncaughtException(Thread t, Throwable e) // 쓰레드 그룹의 쓰레드가 처리되지 않은 예외에 의해 실행이 종료되었을 때, JVM에 의해 이 메서드가 자동적으로 호출된다.
```

[쓰레드그불과 쓰레드를 생성하고 main.list()를 호출해서 main쓰레드 그룹의 정보를 출력하는 예제]

```java
class ThreadEx9 {
	public static void main(String args[]) throws Exception {
		ThreadGroup main = Thread.currentThread().getThreadGroup();
		ThreadGroup grp1 = new ThreadGroup("Group1");
		ThreadGroup grp2 = new ThreadGroup("Group2");

		// ThreadGroup(ThreadGroup parent, String name) 
		ThreadGroup subGrp1 = new ThreadGroup(grp1,"SubGroup1"); 

		grp1.setMaxPriority(3);	// 쓰레드 그룹 grp1의 최대우선순위를 3으로 변경.
		
		Runnable r = new Runnable() {
			public void run() {
				try { 
					Thread.sleep(1000); // 쓰레드를 1초간 멈추게 한다.
				} catch(InterruptedException e) {}
			}	
		};

         // Thread(ThreadGroup tg, Runnable r, String name)
		new Thread(grp1,     r, "th1").start();
		new Thread(subGrp1,  r, "th2").start();
		new Thread(grp2,     r, "th3").start();   

		System.out.println(">>List of ThreadGroup : "+ main.getName() 
                           +", Active ThreadGroup: " + main.activeGroupCount()
                           +", Active Thread: "      + main.activeCount());
		main.list();
	}
}
```

```json
실행결과
>>List of ThreadGroup : main, Active ThreadGroup: 3, Active Thread: 4
java.lang.ThreadGroup[name=main,maxpri=10]
    Thread[main,5,main]
    java.lang.ThreadGroup[name=Group1,maxpri=3]
        Thread[th1,3,Group1]
        java.lang.ThreadGroup[name=SubGroup1,maxpri=3]
            Thread[th2,3,SubGroup1]
    java.lang.ThreadGroup[name=Group2,maxpri=10]
        Thread[th3,5,Group2]
```

- **새로 생성한 모든 쓰레드 그룹**은 **main 쓰레드 그룹의 하위 쓰레드 그룹으로 포함**된다.
- `setMaxPriority()` 는 쓰레드가 **쓰레드 그룹에 추가되기 이전에 호출**되어야 한다.
- 쓰레드 그룹 grp1의 최대우선순위를 3으로 지정했기 때문에, 후에 **여기에 속하게 된 쓰레드 그룹과 쓰레드가 영향을 받았다.**

**참조변수 없이 쓰레드를 생성해서 바로 실행**
- 이 쓰레드가 **Garbage Collecter의 제거 대상이 되지는 않는다.**
- 이 쓰레드의 **참조**가 **ThreadGroup에 저장**되어 있기 때문이다.

## 데몬 쓰레드(Daemon Thread)

**`데몬 쓰레드(Daemon Thread)`** : **다른 일반 쓰레드(데몬 쓰레드가 아닌 쓰레드)의 작업을 돕는 보조적인 역할을 수행하는 쓰레드**
- **일반 쓰레드가 모두 종료**되면 데몬 쓰레드는 **강제적으로 자동 종료**된다.
- 데몬 쓰레드는 일반 쓰레드의 보조역할을 수행, 일반 쓰레드가 모두 종료되고 나면 **데몬 쓰레드의 존재의 의미가 없기 때문이다.**

**이 점을 제외하고는 데몬 쓰레드와 일반 쓰레드는 다르지 않다.**
- 데몬 쓰레드의 예 : Garbage Collecter, 워드프로세서의 자동저장, 화면자동갱신

**데몬 쓰레드**는 **무한루프와 조건문**을 이용해서 실행 후 **대기**하고 있다가 **특정 조건이 만족되면 작업을 수행**하고 **다시 대기하도록 작성한다.**
- 일반 쓰레드의 작성방법과 실행방법이 같으며 다만 쓰레드를 생성한 다음 실행하기 전에 `setDaemon(true)` 를 호출하면 된다.
- **데몬 쓰레드가 생성한 쓰레드**는 **자동적으로 데몬 쓰레드가 된다.**

```java
boolean isDaemon() // 쓰레드가 데몬 쓰레드인지 확인한다. 데몬 쓰레드이면 true를 반환.

void setDaemon(boolean on) // 쓰레드를 데몬 쓰레드로 또는 사용자 쓰레드로 변경한다. 매개변수 on의 값을 true로 지정하면 데몬 쓰레드가 된다.

private boolean daemon = false;

public final boolean isDaemon() {
    return daemon;
}

public final void setDaemon(boolean on) {
        checkAccess();
        if (isAlive()) {
            throw new IllegalThreadStateException();
        }
        daemon = on;
    }
```

[데몬 쓰레드 예제]

```java
class ThreadEx10 implements Runnable  {
	static boolean autoSave = false;

	public static void main(String[] args) {
		Thread t = new Thread(new ThreadEx10());
		t.setDaemon(true);		// 이 부분이 없으면 종료되지 않는다.
		t.start();

		for(int i=1; i <= 10; i++) {
			try{
				Thread.sleep(1000);
			} catch(InterruptedException e) {}
			System.out.println(i);
			
			if(i==5)
				autoSave = true;
		}

		System.out.println("프로그램을 종료합니다.");
	}

	public void run() {
		while(true) {
			try { 
				Thread.sleep(3 * 1000);	// 3초마다
			} catch(InterruptedException e) {}	

			// autoSave의 값이 true이면 autoSave()를 호출한다.
			if(autoSave) {
				autoSave();
			}
		}
	}

	public void autoSave() {
		System.out.println("작업파일이 자동저장되었습니다.");
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
작업파일이 자동저장되었습니다.
6
7
8
작업파일이 자동저장되었습니다.
9
10
프로그램을 종료합니다.
```

- 3초마다 변수 autoSave의 값을 확인해서 그 값이 true이면, autoSave()를 호출하는 일을 무한히 반복하도록 쓰레드를 작성하였다. 
- **만일 이 쓰레드를 데몬 쓰레드로 지정하지 않았다면, 이 프로그램은 강제 종료하지 않는 한 영원히 종료되지 않을 것이다.**
- `setDaemon메서드`는 **반드시 start()를 호출하기 이전에 실행**되어야 한다.
  - **그렇지 않으면 IllegalThreadStateException이 발생**

## 쓰레드의 실행제어

**쓰레드의 프로그래밍이 어려운 이유**
1. 동기화(synchronization)
2. 스케쥴링(scheduling)

효율적인 멀티쓰레딩 프로그램을 만들기 위해서는 보다 정교한 스케쥴링을 통해 프로세스에게 주어진 자원과 시간을 여러 쓰레드가 낭비없이 잘 사용하도록 프로그래밍 해야 한다.

쓰레드의 스케쥴링을 잘하기 위해서는 쓰레드의 상태와 관련 메서드를 잘 알아야 한다.

[쓰레드의 스케쥴링과 상태와 관련된 메서드]

![쓰레드의 스케쥴링과 관련된 메서드](https://user-images.githubusercontent.com/56071088/129005234-0e1b3851-786e-43ee-b7f6-7cdf6e385e4f.png)

### 쓰레드의 상태(State)

[쓰레드의 상태 크게 5가지 (State)]

![쓰레드의 상태](https://user-images.githubusercontent.com/56071088/129005236-eeaac488-b3ff-4118-bdb8-258ac01d584e.png)

- **Thread의 상태**는 `Thread클래스`의 `getState()` 메서드를 호출해서 알 수 있다. JDK 1.5부터 추가되었다.

[쓰레드의 생성부터 소멸까지의 모든 과정]

![쓰레드의 상태 그림 예시](https://user-images.githubusercontent.com/56071088/129005225-11647d75-895f-4be3-8003-d8af6b89ec0e.png)

1. 쓰레드를 생성하고 start()를 호출하면, 바로 실행되는 것이 아니라 **실행대기열에 저장되어 기다린다.**
   - **대기열**은 **큐(queue)와 같은 구조**
   - 먼저 실행대기열에 들어온 쓰레드가 먼저 실행된다.
2. 실행대기상태에 있다가 자신의 차례가 되면 실행상태가 된다.
3. **주어진 실행시간이 다되거나**, **yield()** 를 만나면 다시 **실행대기 상태**
4. **실행 중에 suspend(), sleep(), wait(), join(), I/O block**에 의해 **일시정지상태**가 될 수 있다.
    - I/O block은 입출력작업에서 발생하는 지연상태
5. **지정된 일시정지시간이 다되거나(time-out), notifiy(), resume(), interrupt()** 가 호출되면 **다시 실행대기열에 저장**
6. **실행을 모두 마치거나, stop()** 이 호출되면 **쓰레드 소멸**

### sleep(long millis)

**`sleep(long millis)`** : **지정된 시간동안 쓰레드를 멈추게 한다.**

```java
static void sleep(long millis)
static void sleep(long millis, int nanos)
```

- 밀리세컨드(millis, 1000분의 1초), 나노세컨드(nanos, 10억분의 1초)의 시간 단위로 값을 세밀하게 지정 가능
  - 나노세컨드(nanos)로 지정할 수 있는 값의 범위는 0~999999이며, 999999나노 세컨드는 약 1밀리세컨드이다. 
- 어느 정도의 오차가 발생할 수 있다는 것은 염두해야 한다.

sleep()에 의해 일시정지 상태가 된 쓰레드는
- 지정된 시간이 다 되거나 
- interrupt()가 호출되면(InterruptedException이 발생)
- 잠에서 깨어나 실행대기 상태가 된다.
  
그래서, sleep()을 호출할 때는 항상 `try-catch문`으로 **예외 처리**해줘야 한다.
- `try-catch문`까지 포함하는 메서드를 만들어서 사용하기도 한다.(매번 예외처리가 번거롭기 때문에)

```java
void delay(long millis) {
    try {
        Thread.sleep(millis);
    } catch (InterruptedException e) {}
}
```

**sleep()** 은 **항상 현재 실행 중닌 쓰레드에 대해 작동한다!!**
- `th1.sleep(2000)`와 같이 호출했어도 
- **실제로 영향을 받는 것**은 **main메서드를 실행하는 main쓰레드**이다.

그래서, **sleep()** 은 `static`으로 선언되어 있으며, 참조변수를 이용해서 호출하기 보다는 **Thread**.sleep(2000)과 같이 **클래스명(Thread)으로 호출**한다.
- `yield()`가 `static`으로 선언되어 있는 것도 같은 이유이다.

### interrupt()와 interrupted() - 쓰레드의 작업을 취소한다.

**진행 중인 Thread의 작업이 끝나기 전에 취소사켜야 할 때가 있다.**

**`interrupt()`** : **쓰레드에게 작업을 멈추라고 요청한다. 단지 멈추라고 요청만 하는 것일 뿐 쓰레드를 강제로 종료시키지는 못한다.**
- **그저 쓰레드의 interrupted상태(인스턴스 변수)를 바꾸는 것일 뿐이다.**

**`interrupted()`** : **쓰레드에 대해 interrupt()가 호출되었는지 알려준다.**
- interrupt()가 호출되지 않았다면 false를, interrupt()가 호출되옸다면 true를 반환한다.

**`isInterrupted()`** : **쓰레드의 interrupt()가 호출되었는지 확인할 때 사용할 수 있지만, interrupted()와 달리 inInterrupted()는 쓰레드의 interrupted상태를 false로 초기화하지 않는다.**

```java
Thread th = new Thread();
th.start();
...
th.interrupt(); // 쓰레드 th에 interrupt()를 호출한다.

class MyThread extends Thread {
	public void run() {
		while(!interrupted()) { // interrupted()의 결과가 false인 동안 반복
			...
		}
	}
}
```

```java
void interrupt() // 쓰레드의 interrupted상태를 false에서 true로 변경.
boolean isInterrupted() // 쓰레드의 interrupted상태를 반환
static boolean interrupted() // 현재 쓰레드의 interrupted상태를 반환 후, false로 변경
```

- 쓰레드가 **sleep(), wait(), join()** 에 의해 `일시정지 상태(WAITING)` 에 있을 때,
- 해당 쓰레드에 대해 **interrupt()를 호출**하면, 
- **sleep(), wait(), join()** 에서 **InterruptedException이 발생**하고 쓰레드는 `실행대기 상태(RUNNABLE)` 로 바뀐다.
  - 즉, **멈춰있던 쓰레드를 깨워서 실행가능한 상태로 만드는 것**  

[시간지연을 위해 for문 대신 Thread.sleep(1000)을 사용한 interrupt()예제]

```java
import javax.swing.JOptionPane;

class ThreadEx14_1 {
	public static void main(String[] args) throws Exception 	{
		ThreadEx14_2 th1 = new ThreadEx14_2();
		th1.start();

		String input = JOptionPane.showInputDialog("아무 값이나 입력하세요."); 
		System.out.println("입력하신 값은 " + input + "입니다.");
		th1.interrupt();   // interrupt()를 호출하면, interrupted상태가 true가 된다.
		System.out.println("isInterrupted():"+ th1.isInterrupted());
	}
}

class ThreadEx14_2 extends Thread {
	public void run() {
		int i = 10;

		while(i!=0 && !isInterrupted()) {
			System.out.println(i--);

			try {
				Thread.sleep(1000);  // 1초 지연
			} catch(InterruptedException e) {}
		}

		System.out.println("카운트가 종료되었습니다.");
	} // main
}
```

Thread.sleep(1000)에서 InterruptedException이 발생
- sleep()에 의해 쓰레드가 잠시 멈춰있을 때, 
- interrupt()를 호출하면 InterruptedException이 발생되고,
- 쓰레드의 interrupted상태는 false로 자동 초기화된다.

```java
try {
	Thread.sleep(1000);  // 1초 지연
} catch(InterruptedException e) {}

--> 

try {
	Thread.sleep(1000);  // 1초 지연
} catch(InterruptedException e) {
	interrupt(); // 추가
}
```

- catch블럭에 interrupt()를 추가로 넣어줘서 쓰레드의 interrupted상태를 true로 다시 바꿔줘야 한다.

### suspend(), resume(), stop()

**`suspend()`** : **sleep()처럼 쓰레드를 멈추게 한다.**

**`resume()`** : **sleep()에 의해 정지된 쓰레드를 다시 실행대기 상태가 된다.**

**`stop()`** : **호출되는 즉시 쓰레드가 종료된다.**

suspend(), resume(), stop()은 쓰레드를 제어하는 가장 손쉬운 방법이지만, **suspend(), stop()이 교착상태(deadlock)을 일으키기 쉽게 작성되어 있으므로 사용이 권장되지 않는다.**
- 그래서 **이 메서드들은 모두 deprecated되었다.**
- `deprecated메서드`는 하위 호환성을 위해서 삭제하지 않는 것일 뿐이므로 사용해서는 안된다.

[suspend(), resume(), stop()을 보다 객체지향적인 코드로 작성한 예시]

```java
class ThreadEx17 {
	public static void main(String args[]) {
		ThreadEx17_1 th1 = new ThreadEx17_1("*");
		ThreadEx17_1 th2 = new ThreadEx17_1("**");
		ThreadEx17_1 th3 = new ThreadEx17_1("***");
		th1.start();
		th2.start();
		th3.start();

		try {
			Thread.sleep(2000);
			th1.suspend();		
			Thread.sleep(2000);
			th2.suspend();
			Thread.sleep(3000);
			th1.resume();
			Thread.sleep(3000);
			th1.stop();
			th2.stop();
			Thread.sleep(2000);
			th3.stop();
		} catch (InterruptedException e) {}
	}
}

class ThreadEx17_1 implements Runnable {
	boolean suspended = false;
	boolean stopped   = false;

	Thread th;

	ThreadEx17_1(String name) {
		th = new Thread(this, name); // Thread(Runnable r, String name)
	}

	public void run() {
		while(!stopped) {
			if(!suspended) {
                System.out.println(Thread.currentThread().getName());
				try {
					Thread.sleep(1000);
				} catch(InterruptedException e) {}			
			}
		}
		System.out.println(Thread.currentThread().getName() + " - stopped");
	}

	public void suspend() { suspended = true;  }
	public void resume()  { suspended = false; }
	public void stop()    { stopped   = true;  }
	public void start()   { th.start();        }
}
```





























