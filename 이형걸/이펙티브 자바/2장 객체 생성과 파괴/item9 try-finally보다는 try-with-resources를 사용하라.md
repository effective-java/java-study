# item9 try-finally보다는 try-with-resources를 사용하라

Java Library에는 `close() 메서드`를 호출해 직접 닫아줘야 하는 자원이 많다.
- `InputStream, OutputStream, java.sql.Connection`등이 있다.

자원 닫기는 클라이언트가 놓치기 쉬워서 예측할 수 없는 성능 문제로 이어지기도 한다.
- 상당수가 안전망으로 finalizer를 활용하고는 있지만, **finalizer는 믿을만하지 못하다.**

## 전통적으로 자원을 닫는 수단 - try-finally구문

**[try-finally - 더이상 자원을 회수한ㄴ 최선의 방책이 아니다!]**

```java
static String firstLineOfFile(String path) throws IOException {
    BufferedReader br = new BufferedReader(new FileReader(path));
    try {
        return br.readLine();
    } finally {
        br.close();
    }
}
```

**[자원이 2 이상일 경우 try-finally방법은 너무 지저분하다!]**

```java
static void copy(String src, String dst) throws IOException {
    InputStream in = new FileInputStream(src);

    try {
        OutputStream out = new FileOuputStream(dst);
        try {
            byte[] buf = new byte[BUFFER_SIZE];
            int n;
            while((n = in.read(buf)) >= 0) {
                out.write(buf, 0, n);
            }
        } finally {
            out.close();
        }
    } finally {
        in.close();
    }
}
```

미묘한 결점이 있다.
- 예외는 `try블럭, finally블럭` 모두에서 발생 가능
- firstLineOfFile메서드의 readLine()메서드가 예외를 던지고, close()메서드도 예외를 던지게 되면
- **2번째 예외가 1번째 예외를 완전히 덮어 버린다.**
- stack추적 내역에 1번째 예외에 관한 내용은 남지 않게 되어, 실제 시스템에서의 디버깅을 매우 어렵게 한다.
- 2번째 예외 대신 1번째 예외를 기록하도록 코딩할 수 있지만, 코드가 더러워지므로 하지 않을 때가 더 많다.

## 해결책 - Java7 : try-with-resources

- 이 구조를 사용하려면 **해당 자원이 AutoCloseable인터페이스를 구현해야 한다.**
- 단순히 void를 반환하는 close() 메서드 하나만 덩그러니 정의한 interface이다.
- Java Library와 수 많은 Third-Party Library들의 수 많은 클래스와 인터페이스가 이미 AutoCloseable을 구현하거나 확장해뒀다.

```java
static String firstLineOfFile(String path) throws IOException {
    try (BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    }
}
```

```java
static void copy(String src, String dst) throws IOException {
    try (InputStream in = new FileInputStream(src); 
        OutputStream out = new FileOuputStream(dst)) {
        byte[] buf = new byte[BUFFER_SIZE];
        int n;
        
        while((n = in.read(buf)) >= 0) {
            out.write(buf, 0, n);
        }
    }
}
```

- `try-with-resources`버젼이 짧고 읽기 수월, 문제를 진단하기에도 용이
- firstLineOfFile메서드에서 readLine, close 양쪽에서 예외가 발생하면
  - close에서 발생한 예외는 숨겨지고, readLine에서 발생한 예외가 기록된다.
- 이처럼 실전에서는 프로그래머에게 보여줄 예외 하나만 보존되고 여러 개의 다른 예외가 숨겨질 수도 있다.
  - 숨겨진 예외들도 stack 추적 내용에 `suppressed(숨겨졌다)` 라는 꼬리표를 달고 출력된다.
  - Java7 에서는 Throwable에 추가된 getSuppressed메서드를 이용하면 프로그램 코드에서 가져올 수도 있다.

`try-with-resources` 에서도 `catch절`을 사용할 수 있다.
- catch절 덕분에 try문을 더 중첩하지 않고도 다수의 예외를 처리할 수 있다.

```java
static String firstLineOfFile(String path, String defaultVal) throws IOException {
    try (BufferedReader br = new BufferedReader(new FileReader(path))) {
        return br.readLine();
    } catch (IOException e) {
        return defaultVal;
    }
}
```

## 결론
- **꼭 회수해야 하는 자원을 다룰 때** : `try-with-resources`를 사용하자.
- 예외는 없다.
- 코드는 더 짧고 분명, 만들어지는 예외 정보도 훨씬 유용
- 정확하고 쉽게 자원을 회수
