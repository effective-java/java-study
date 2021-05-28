# Java 8 interface의 변화(default method, static method)

java7 까지는 인터페이스에 **상수** , **실행 블록이 없는 추상 메소드** 선언만 가능했다. 하지만 java8부터 인터페이스에 **디폴트메소드와 정적메소드도 추가로 선언** 이 가능하다. 이로인해 java의 인터페이스는 더욱 유연해진 코딩을 할 수 있다.

## java8의 인터페이스 형태

```java
interface 인터페이스명{

  //상수
  타입 상수명 = 값;

  //추상 메소드
  타입 메소드명(매개변수);
  
  //디폴트 메소드
  default 타입 메소드명(매개변수) { ... }

  //정적 메소드
  static 타입 메소드명(매개변수) { ... }
}
```

### Java 8에서 인터페이스의 디폴트메소드와 추상메소드가 추가된 이유가 있다.

어떤 프로그램에서 인터페이스가 있고 이 인터페이스 I를 구현한 C1, C2의 구현 클래스가 있다고 가정하자. 변경 사항이 생겨서 기존 인터페이스에 추가 기능이 필요해서 추상메소드를 한개 추가하고 새롭게 D1 이라는 클래스로 인터페이스 I를 구현하게 된다. **문제는 기존에 사용하던 C1, C2의 구현 객체에서 D1 클래스를 위해 추가된 추상메소드를 추가로 구현해야 한다는 점이다.**

Java 8 에서는 인터페이스의 **디폴트 메소드와, 정적메소드** 를 추가하여 프로그래밍의 유연성을 높여주고 있다.

## 인터페이스의 디폴트메소드(default method)

`디폴트 메소드` :  인터페이스에 선언되지만 사실은 **객체(구현 객체)** 가 가지고 있는 **인스턴스 메소드** 라고 생각해야 된다. **따라서 인터페이스를 구현한 객체를 통해서 호출 해야만 한다.** 

`Java 8 에서 디폴트 메소드를 허용한 이유` : 기존 인터페이스를 확장해서 **새로운 기능을 추가** 하기 위해서이다. 만약 구현 객체에서 인터페이스의 디폴트 메소드가 적절하지 못하다면, **오버라이딩** 하여 수정해서 사용하면 된다.

형태 : [public] default 리턴 타입 메소드명(매개변수){ … }

## 인터페이스의 정적메소드(static method)

`정적 메소드` : Java 8 부터 작성할 수 있는데, 디폴트 메소드와는 달리 객체가 없어도 **인터페이스만으로 호출이 가능하다.**

형태 : [public] static 리턴 타입 메소드명(매개변수, …){ … }

*java8의 인터페이스의 디폴트메소드, 추상메소드 예제*

```java
interface Java8InterfaceTest{
  
  int talkCnt = 0;
  
  void run();
  //디폴트 메소드
  default void talk(String msg){
    
    if(msg != null && msg.equals("")){
      System.out.println(msg);
    }else{
      System.out.println("디폴트 메소드");
    }
    
  }// talk
  //정적 메소드
  static void talk2(String msg){
    if(msg != null && msg.equals("")){
      System.out.println(msg);
    }else{
      System.out.println("정적 메소드다");
    }
    
  }// talk2
}

public class InterfaceTest1 {

  public static void main(String[] args) {
    
    Java8InterfaceTest.talk2(null); // 정적메소드 호출
    
    Java8InterfaceTest test1 = new Java8InterfaceTest(){

      
      public void run() {
        System.out.println("run");
      }
      
    };
    
    test1.run();
    test1.talk(null); // 디폴트 메소드 호출
    
  }
// 실행결과
// 정적 메소드
// run
// 디폴트 메소드
}
```
## 인터페이스의 상속에서 디폴트메소드의 변화

**인터페이스는 인터페이스를 상속 할 수 있다.** 

**자식 인터페이스에서 부모의 인터페이스를 상속할 때** , 부모의 디폴트 메소드를 어떻게 할지 선택 할 수있다.

1. 부모 인터페이스의 디폴트 메소드를 **그냥 상속**한다.

2. 부모 인터페이스의 디폴트 메소드를 **재정의(Override)** 한다.

3. 부모 인터페이스의 디폴트 메소드를 **추상메소드로 재 선언** 한다.

인터페이스의 디폴트메소드는 인터페이스의 강제성을 조금 유연하게 해주는 기능이다. 하지만 좋게 말하면 유연이고 나쁘게 말한다면 느슨한 것이다. 

만약 이 느슨한것을 다시 엄격하게 수정할 필요가 있다면 인터페이스를 상속하여 다시 추상메소드로 재 선언 하는것도 방법이다.

## 인터페이스 상수 선언(public static final)할 때 주의할점

- `일반적인 클래스` : 상수(static final)의 초기화는 **선언과 동시에** 그리고 **정적영역(static block)** 두 곳 모두에서 가능하다. 

- `인터페이스의 상수` : 정적영역(static block)에서 초기화 할수 없기 때문에 **반드시 선언과 동시에 초기화** 해야 한다.

[참고] : https://www.hanumoka.net/2017/09/16/java-20170916-java8-interface/