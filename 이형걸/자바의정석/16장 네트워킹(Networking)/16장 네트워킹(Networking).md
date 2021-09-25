# 16장 네트워킹(Networking)

**`네트워킹`** : **2대 이상의 컴퓨터를 케이블로 연결하여 네트워크(network)를 구성하는 것**
- 네트워킹의 개념은 컴퓨터들을 서로 연결하여 데이터를 손쉽게 주고받거나 또는 자원프린터와 같은 주변기기를 함께 공유하고자 하는 노력에서 시작되었다.

`java.net패키지`를 사용하면 이러한 Network Application의 데이터 통신 부분을 쉽게 작성할 수 있으며, 간단한 Network Application은 단 몇 줄의 Java Code만으로도 작성이 가능하다.

### 클라이언트/서버 (Client/Server)

**`클라이언트/서버 (Client/Server)`**
- 컴퓨터간의 관계를 역할로 구분하는 개념

**`서버(Server)`** : **서비스를 제공하는 컴퓨터(service provider)**
- 하드웨어의 사양에 상관없이 서비스를 제공하는 software가 실행되는 컴퓨터를 서버라 한다.

**`클라이언트(Client)`** : **서비스를 사용하는 컴퓨터(service user)**

**`서비스(Service)`** : 서버가 클라이언트로부터 **요청받은 작업을 처리하여 그 결과를 제공하는 것**
- 서버가 제공하는 서비스의 종류에 따라
- 파일서버(file server)
- 메일서버(mail server)
- 어플리케이션 서버(application server)

서버가 서비스를 제공하기 위해서는 서버프로그램이 있어야 하고, 클라이언트가 서비스를 제공받기 위해서는 서버프로그램과 연결할 수 있는 클라이언트 프로그램이 있어야 한다.
- 웹서버에 접속하여 정보를 얻기 위해서는 웹브라우져(클라이언트 프로그램)이 있어야 한다.
- FTP서버에 접속해서 파일을 전송받기 위해서는 알FTP와 같은 FTP 클라이언트 프로그램이 필요하다.

일반 PC의 경우, **주로 서버에 접속하는 클라이언트 역할을 수행**하지만, `FTP Serv-U`(`FTP 서버 프로그램`)이나 `Tomcat`(`웹서버프로그램`)을 설치하면 **서버역할도 수행**할 수 있다.

**`서버 기반 모델(Server-Based model)`** : 네트워크를 구성할 때 전용서버를 두는 것

**`P2P모델(Peer-to-Peer model)`** : 별도의 전용서버없이 각 클라이언트가 서버역할을 동시에 수행하는 것

|**서버 기반 모델(Server-Based model)**|**P2P모델(Peer-to-Peer model)**|
|---|---|
|안정적인 서비스의 제공이 가능하다.|서버구축 및 운용비용을 절감할 수 있다.|
|공유 데이터의 관리와 보안이 용이하다.|자원의 활용을 극대화 할 수 있다.|
|서버구축비용과 관리비용이 든다.|자원의 관리가 어렵다.|
||보안이 취약하다.|

### IP 주소(IP address)

**`IP 주소(IP address)`** : **컴퓨터(호스트, host)를 구별하는데 사용되는 고유한 값**
- **인터넷에 연결된 모든 컴퓨터는 IP 주소를 갖는다.**
- 4 Byte(32 bit)의 정수로 구성되어 있다.
- 4개의 정수가 마침표(.)를 구분자로 `a.b.c.d`의 형식으로 표현
- 각각의 정수는 부호없는 1 Byte값, 0~255 사이의 정수

**IP주소**는 `네트워크 주소`, `호스트 주소`로 나뉘어 진다.
- 32bit(4 Byte)의 IP 주소 중에서 네트워크 주소와 호스트주소가 각각 몇 bit를 차지하는 지는 네트워크를 어떻게 구성하였는지에 따라 달라진다.
  - 서로 다른 두 호스트의 IP주소의 네트워크 주소가 같다는 것 == 두 호스트가 같은 네트워크에 포함되어 있다는 것을 의미

![IP주소와 서브넷마스크의 2진법 표기](https://user-images.githubusercontent.com/56071088/134770889-8bb97a0d-01d6-4cfa-bd9f-71f90972aeab.png)

IP주소와 서브넷마스크를 Bit연산자 `&`로 연산하면 IP주소에서 네트워크 주소만을 뽑아낼 수 있다.
- IP주소 `192.168.10.100` 의 네트워크 주소 : `24bit(192.168.10)` 과 호스트 주소는 마지막 8 bit(100)

IP주소에서 네트워크의 주소가 차지하는 자리수가 많을수록 호스트 주소의 범위가 줄어들기 때문에 네트워크의 규모가 작아진다.
- 이 경우 호스트 주소의 자리수가 8자리이기 때문에 256개(2^8)의 호스트만 이 네트워크에 포함될 수 있다.
- 호스트 주소가 0인 것은 네트워크 자신을 나타낸다.
- 255는 브로드캐스트 주소로 사용되기 때문에 실제로는 네트워크에 포함 가능한 호스트 개수는 254개이다.

### InetAddress

**`InetAddress`** : Java에서는 IP주소를 다루기 위한 클래스로 InetAddress 제공

![InetAddress](https://user-images.githubusercontent.com/56071088/134771210-56e3737c-a9c0-4c1b-9675-4ca53d3a98c7.png)

![InteAddress메서드](https://user-images.githubusercontent.com/56071088/134771211-850712d7-f97c-47db-a36d-9e0d561708ce.png)


**[InetAddress의 주요 메서드들을 활용하는 예제]**
- 하나의 도메인명(www.naver.com)에 여러 IP주소가 매핑될 수도 있고, 또 그 반대의 경우도 가능하기 때문에 전자의 경우 getAllByName()을 통해 모든 IP주소를 얻을 수 있다.
- getLocalHost() : 호스트명과 IP주소를 알 수 있다.

```java
import java.net.*;
import java.util.*;

class NetworkEx1 {
	public static void main(String[] args) 
	{
		InetAddress ip = null;
		InetAddress[] ipArr = null;

		try {
			ip = InetAddress.getByName("www.naver.com");
			System.out.println("getHostName() :"   +ip.getHostName());
			System.out.println("getHostAddress() :"+ip.getHostAddress());
			System.out.println("toString() :"      +ip.toString());

			byte[] ipAddr = ip.getAddress();
			System.out.println("getAddress() :"+Arrays.toString(ipAddr));

			String result = "";
			for(int i=0; i < ipAddr.length;i++) {
				result += (ipAddr[i] < 0) ? ipAddr[i] + 256 : ipAddr[i];
				result += ".";
			}
			System.out.println("getAddress()+256 :"+result);
			System.out.println();
		} catch (UnknownHostException e) {
			e.printStackTrace();
		}

		try {
			ip = InetAddress.getLocalHost();
			System.out.println("getHostName() :"   +ip.getHostName());
			System.out.println("getHostAddress() :"+ip.getHostAddress());
			System.out.println();
		} catch (UnknownHostException e) {
			e.printStackTrace();
		}
		

		try {
			ipArr = InetAddress.getAllByName("www.naver.com");

			for(int i=0; i < ipArr.length; i++) {
				System.out.println("ipArr["+i+"] :" + ipArr[i]);
			}			
		} catch (UnknownHostException e) {
			e.printStackTrace();
		}

	} // main
}
```

### URL(Uniform Resiurce Loactor)

**`URL(Uniform Resiurce Loactor)`** : 인터넷에 존재하는 여러 서버들이 제공하는 **자원**에 접근할 수 있는 **주소**를 표현
- `프로토콜://호스트명:포트번호/경로명/파일명?쿼리스트링#참조`
- URL에서 포트번호, 쿼리, 참조는 생략할 수 있다.

```java
http://www.codecohbo.com:80/sample/hello.html?referer=codechobo#index1

프로토콜 : 자원에 접근하기 위해 서버와 통신한는데 사용되는 통신규약(http)
호스트명 : 자원을 제공하는 서버의 이름(www.codechobo.com)
포트번호 : 통신에 사용되는 서버의 포트번호(80)
경로명 : 접근하려는 자원이 저장된 서버상의 위치(/sample/)
파일명 : 접근하려는 자원의 이름 (hello.html)
쿼리(query) : URL 에서 '?' 이후의 부분(referer=codechobo)
참조(referer) : URL 에서 '#' 이후의 부분(index1) 
```

[참고] http프로토콜에서는 80번 포트를 사용하기 때문에 URL에서 포트번호를 생략하는 경우 80으로 간주한다.
- 각 프로토콜에 따라 통신에 사용하는 포트번호가 다르며 생략되면 각 프로토콜의 기본 포트가 사용된다.

Java에서는 URL을 다루기 위한 클래스로 `URL클래스`를 제공한다.

![URL의 메서드](https://user-images.githubusercontent.com/56071088/134771713-7f8066e8-4c6c-46ed-9d06-faf0606308ac.jpg)

**[URL클래스를 활용한 예제]**

```java
import java.net.*;

class NetworkEx2 {
	public static void main(String args[]) throws Exception {
		URL url = new URL("http://www.codechobo.com:80/sample/"+"hello.html?referer=javachobo#index1");

		System.out.println("url.getAuthority():"+ url.getAuthority());
		System.out.println("url.getContent():"+ url.getContent());
		System.out.println("url.getDefaultPort():"+ url.getDefaultPort());
		System.out.println("url.getPort():"+ url.getPort());
		System.out.println("url.getFile():"+ url.getFile());
		System.out.println("url.getHost():"+ url.getHost());
		System.out.println("url.getPath():"+ url.getPath());
		System.out.println("url.getProtocol():"+ url.getProtocol());
		System.out.println("url.getQuery():"+ url.getQuery());
		System.out.println("url.getRef():"+ url.getRef());
		System.out.println("url.getUserInfo():"+ url.getUserInfo());
		System.out.println("url.toExternalForm():"+ url.toExternalForm());
		System.out.println("url.toURI():"+ url.toURI());
	}
}
```

### URLConnection

**`URLConnection`** : **Application과 URL간의 통신연결을 나타내는 클래스의 최상위 클래스로 추상클래스다.**
- URLConnextion을 상속받아 구현한 클래스는 `HttpURLConnection, JarURLConnection`이 있다.
- URL의 protocol이 http : openConnection()은 HttpURLConnection을 반환한다.

**URLConnection을 사용해서 연결하고자하는 자원에 접근하고 읽고 쓰기를 할 수 있다.**

![URLConnection메서드 1](https://user-images.githubusercontent.com/56071088/134772213-e043c527-ffaf-4c23-b0f5-aed90ab83368.jpg)

![URLConnection메서드 2](https://user-images.githubusercontent.com/56071088/134772216-1169f8a9-951f-4921-9711-452a5a16a435.jpg)

**[URLConnection 추상클래스를 활용한 예시]**

```java
import java.net.*;
import java.io.*;

public class NetworkEx3 {
	public static void  main(String args[]) {
		URL url = null;
		String address = "http://www.codechobo.com/sample/hello.html";
		String line = "";

		try {
			url = new URL(address);
			URLConnection conn = url.openConnection();

			System.out.println("conn.toString():"+conn);
			System.out.println("getAllowUserInteraction():"+conn.getAllowUserInteraction());
			System.out.println("getConnectTimeout():"+conn.getConnectTimeout());
			System.out.println("getContent():"+conn.getContent());
			System.out.println("getContentEncoding():"+conn.getContentEncoding());
			System.out.println("getContentLength():"+conn.getContentLength());
			System.out.println("getContentType():"+conn.getContentType());
			System.out.println("getDate():"+conn.getDate());
			System.out.println("getDefaultAllowUserInteraction():"+conn.getDefaultAllowUserInteraction());
			System.out.println("getDefaultUseCaches():"+conn.getDefaultUseCaches());
			System.out.println("getDoInput():"+conn.getDoInput());
			System.out.println("getDoOutput():"+conn.getDoOutput());
			System.out.println("getExpiration():"+conn.getExpiration());
			System.out.println("getHeaderFields():"+conn.getHeaderFields());
			System.out.println("getIfModifiedSince():"+conn.getIfModifiedSince());
			System.out.println("getLastModified():"+conn.getLastModified());
			System.out.println("getReadTimeout():"+conn.getReadTimeout());
			System.out.println("getURL():"+conn.getURL());
			System.out.println("getUseCaches():"+conn.getUseCaches());
		} catch(Exception e) {
			e.printStackTrace();
		}
	} // main
}
```

**[URL에 연결하여 그 내용을 읽어오는 예제]**

```java
import java.net.*;
import java.io.*;

public class NetworkEx4 {
	public static void  main(String args[]) {
		URL url = null;
		BufferedReader input = null;
		String address = "http://www.codechobo.com/sample/hello.html";
		String line = "";

		try {
			url = new URL(address);

		    input = new BufferedReader(new InputStreamReader(url.openStream()));

			while((line=input.readLine()) !=null) {
				System.out.println(line);
			}
			input.close();
		} catch(Exception e) {
			e.printStackTrace();
		}
	}
}
```

- 만일 URL이 유효하지 않다면 `Malformed-URLException`이 발생한다.
- 읽어 올 데이터가 문자데이터이기 때문에 BufferedReader를 사용하였다.
- openStream()을 호출해서 URL의 InputStream을 얻은 이후로는 파일로부터 데이터를 읽는 것과 다르지 않다.

openStream()은 openConnection()을 호출해서 URLConnection을 얻고, getInputStream()을 호출한 것과 같다.
- 즉, URL에 연결해서 InputStream을 얻은 것

```java
InputStream in = url.openStream();

-->

URLConnection conn = url.openConnection();
InputStream in = conn.getInputStream();
```

## 소켓 프로그래밍
(1) 소켓이란 프로세스간의 통신에 사용되는 양쪽 끝단을 의미한다. 프로세스간 통신을 위해서는 소켓이 필요하다.

![TCP와 UDP의 비교](https://user-images.githubusercontent.com/56071088/134772442-3233ece9-085d-4805-9357-5ee4428fb2c7.JPG)


(2) TCP는 데이터를 전송하기 전에 먼저 상대편과 연결을 한 후 데이터를 전송하며 잘 전송되었는지 확인하고 실패했다면 데이터를 재전송한다. 신뢰 있는 데이터의 전송이 요구되는 통신에 적합하다. (ex - 파일전송)

(3) UDP는 상대편과 연결하지 않고 데이터를 전송한다. 데이터를 전송하지만 데이터가 바르게 수신되었는지 확인하지 않기 때문에 데이터가 전송되었는지 확인할 길이 없다. (데이터를 보낸 순서대로 수신한다는 보장x)

게임이나 동영상 같이 데이터가 중간에 손실되더라도 빠른 전송이 필요할 때 적합하다.

### TCP 소켓 프로그래밍

(1) TCP 소켓 프로그래밍은 먼저 서버 프로그램이 실행되어 클라이언트 프로그램의 연결요청을 기다리고 있어야한다.

(2) 서버 소켓은 포트와 결합되어 포트를 통해 원격 사용자의 연결요청을 기다리다가 새로운 소켓을 생성하여 상대편 소켓과 통신할 수 있도록 연결하는 역할을 한다. 실제적인 데이터의 통신은 서버소켓과 관계없이 소켓간에 이루어진다.

(3) 소켓들이 데이터를 주고받는 연결통로는 입출력스트림이다. 소켓은 입력스트림과 출력스트림을 가지고 있으며 상대편 소켓의 스트림들과 교차연결된다.

(4) Socket클래스는 프로세스간 통신을 담당한다. InputStream과 OutputStream을 가지고 있으며 두 스트림을 통해 프로세스간의 통신이 이루어진다.

(5) ServerSocket클래스는 포트와 연결되어 외부의 연결요청을 기다리다 연결요청이 들어오면 Socket을 생성해서 소켓간 통신이 이루어지도록 한다. 한 포트에 하나의 ServerSocket만 연결할 수 있다.

```java
//서버 프로그램을 실행한다.
ServerSocket serverSocket = null;
try{
	//1. 서버 소켓을 생성한다.
    serverSocket = new ServerSocket(7777);
}catch(IOException e) { e.printStackTrace(); }
while(true){
    try{
    	//2.서버 소켓이 연결요청을 처리할 수 있도록 대기상태로 만든다.
        //클라이언트 프로그램의 연결요청이 오면 새로운 소켓을 생성해서 클라이언트 프로그램의 소켓과 연결한다.
    	Socket socket = serverSocket.accept();
        //4. 소켓의 출력스트림을 얻는다.
        OutputStream out = socket.getOutputStream();
        DataOutputStream dos = new DataOutputStream(out);
        //5. 출력스트림을 이용해 데이터를 전송한다.
        dos.writeUTF("Message from Server");
        //7.출력스트림을 닫는다.
       	dos.close();
        //8.소켓을 닫는다.
        socket.close();
    } catch(IOException e) { e.printStackTrace(); }
}

//클라이언트 프로그램
try{
    //3. 클라이언트 프로그램에서 소켓을 생성하여 서버 소켓에 연결을 한다.
    Socket socket = new Socket("127.0.0.1", 7777);
    //4. 소켓의 입력스트림을 얻는다.
    InputStream in = Socket.getInputStream();
    DataInputStream dis = new DataInputStream(in);
    //6. 입력스트림을 이용해 데이터를 얻는다.
    String result = dis.readUTF();
    //7. 입력스트림을 닫는다.
    dis.close();
    //8.소켓을 닫는다.
    socket.close();
} catch(ConnectException ce) {
    ce.printStackTrace();
} catch(IOException ie){
    ie.printStackTrace();
} catch(Exception e){
    e.printStackTrace();
}
```

(6) ServerSocket클래스의 setSoTimeOut(int timeout)을 사용해서 서버소켓의 대기시간을 지정할 수 있다. timeout 값이 0일경우 제한시간 없이 대기한다. 지정한 대기시간이 지나면 accept( )에서 SocketTimeoutException이 발생한다.

(7) 서버에 접속하는 클라이언트의 수가 많을 때는 쓰레드를 이용해서 클라이언트의 요청을 병렬적으로 처리하는 것이 좋다. (서버가 접속을 요청한 순서대로 처리하면 늦게 접속을 요청한 클라이언트는 오랜 시간을 기다릴 수 밖에 없기 때문)

### UDP 소켓 프로그래밍
(1) UDP 소켓 프로그래밍에서는 DatagramSocket과 DatagramPacket을 사용한다.

(2) UDP는 ServerSocket을 필요로 하지 않는다. UDP통신에서 사용하는 소켓은 DatagramSocket이며 데이터를 DatagramPacket에 담아서 전송한다.

(3) DatagramPacket은 헤더와 데이터로 구성되어 잇으며 헤더에는 DatagramPacket을 수신할 호스트의 정보(호스트의 주소, 포트)가 저장되어 있다. DatagramPacket을 전송하면 지정된 주소의 DatagramSocket에 도착한다.

```java
//서버 프로그램
try{
    //소켓 생성
    DatagramSocket Socket = new DatagramSocket(7777);
    DatagramPacket inPacket, outPacket;
    //데이터 공간을 생성
    byte[] inMsg = new byte[10];
    byte[] outMsg;
    while(true){
        //데이터를 수신하기 위한 패킷 생성
        inPacket = new DatagramPacket(inMsg, inMsg.length);
        //패킷을 통해 데이터를 수신
        socket.receive(inPacket);
        //수신한 패킷으로부터 client의 IP주소를 얻는다.
        InetAddress address = inPacket.getAddress();
        int port = inPacket.getPort();
        //패킷을 생성해서 client에게 전송한다.
        String msg = "ServerMsg";
        outMsg = msg.getBytes();
        //해당 프로그램은 패킷을 송신한 클라이언트에게 메시지를 전송한다.
        outPakcet = new DatagramPakcet(outMsg, outMsg.length, address, port);
        socket.send(outPacket);
    }
} catch(IOException e) { e.printStackTrace(); }

//클라이언트 프로그램
try{
    //소켓생성
    DatagramSocket datagramSocket = new DatagramSocket();
    //미리 알고있는 서버의 IP주소로 InetAddress 생성
    InetAddress serverAddress = InetAddress.getByName("127.0.0.1");
    //데이터가 저장될 공간을 byte배열로 생성한다.
    byte[] msg = new byte[100];
    //DatagramPacket을 수신할 상대편의 주소를 IP주소와 포트번호로 설정
    DatagramPacket outPacket = new DatagramPacket(msg, 1, serverAddress, 7777);
    //소켓을 통해 패킷을 전송
    datagramSocket.send(outPacket);
    //데이터를 수신할 DatagramPakcet 생성
    DatagramPakcet inPacket = new DatagramPacket(msg, msg.length);
    //소켓을 통해 패킷을 수신
    datagramSocket.receive(inPacket);
    //소켓을 종료
    datagramSocket.close();
} catch(Exception e){ e.printStackTrace(); }
```