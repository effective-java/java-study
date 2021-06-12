# 배열(Array)

## 배열(Array) 이란?

`배열(Array)의 정의` : **같은 타입의 여러 변수를 하나의 묶음으로 다루는 것**

중요한 것은 **같은 타입** 이어야 한다.

```java
int[] score = new int[5]; // 5개의 int 값을 저장할 수 있는 배열을 생성한다.
```

![메모리에 생성된 배열](https://user-images.githubusercontent.com/56071088/120654254-7887e600-c4bc-11eb-8100-05308ebbaf98.gif)

- 변수 score 는  배열을 다루는데 필요한 **참조 변수** 일 뿐, 값을 저장하기 위한 공간이 아니다.
- 배열은 각 **저장공간이 연속적으로 배치** 되어 있다.

### 배열의 선언과 생성

[배열을 선언방법과 예시]

|선언방법|선언 예|
|---|---|
| **타입[] 변수이름** | **String[] args** |
| 타입 변수이름[] | String args[] |

- 대괄호[] 를 붙일 때, 타입에 붙이는 쪽을 선호하자. 대괄호가 변수 이름의 일부 보다는, **타입의 일부** 라고 보기 때문이다.

```java
int[] score; // 배열을 선언 (배열을 다루기 위한 참조 변수 선언)

score = new int[5]; // 배열을 생성 (실제 저장공간을 생성)
```

- **배열을 선언** 한 다음에는 **배열을 생성** 해야 한다.
  - `배열을 선언` : **참조 변수 선언**
  - `배열을 생성` : **실제 저장공간을 생성** 

- 배열을 생성하기 위해서는 **연산자 'new'** 와 함께 배열의 타입과 길이를 지정해주어야 한다.

각 배열요소는 자동적으로 지정된 **타입의 기본값(default)** 으로 **초기화** 된다.

### 배열의 인덱스(index) 와 길이(length)

`배열의 요소(element)` : **배열의 각 저장공간**

`배열의 인덱스(index)` : **배열의 요소마다 붙여진 일련번호** 

- 배열을 다룰 때 주의할 점!!
  - index 의 범위를 벗어난 값을 index 으로 사용하지 않아야 한다는 것
  - index 변수의 값은 **실행 시에 대입** 되기 때문에 **컴파일러가 이 값의 범위를 확인할 수 없다.**
  - **실행 시에 에러(ArrayIndexOutOfBoundsException)** 이 발생한다.

```java
int[] score = new int[0]; // 길이가 0인 배열도 생성 가능하다!!
```

- 배열의 **길이가 0인 배열** 도 생성 가능하다!

```java
int[] score = new int[5]; // 길이가 5인 배열을 생성
int temp = score.length; // score.length == 5
```

- Java 에서는 **JVM** 이 **모든 배열의 길이를 별도로 관리** 한다. 

- **배열이름.length** 를 통해 배열의 길이 정보를 가져올 수 있다.

- **배열은 한번 생성되면 길이를 변경할 수 없다.**

### 배열의 초기화

- 배열은 생성과 동시에 자동적으로 **자신의 타입에 맞는 기본값(default) 으로 초기화** 된다.

[원하는 값으로 초기화 하고 싶을 경우]

```java
int[] arr = new int[]{ 1, 2, 3, 4};

int[] arr = { 1, 2, 3, 4 }; // new int[] 를 생략할 수 있다.

int[] score;
score = new int[]{ 1, 2, 3, 4}; // ok
score = { 1, 2, 3, 4}; // Error, new int[] 를 생략할 수 없음!

int[] args = new int[0];  // 길이가 0인 배열 생성
int[] args = new int[]{}; // 길이가 0인 배열 생성
int[] args = {};          // 길이가 0인 배열 생성, new int[] 가 생략됨.
```

### 배열의 출력

```java
int[] iArr = { 1, 2, 3, 4 };

System.ou.println(Arrays.toString(iArr));

// 배열 iArr의 모든 요소를 출력한다. 
// [1, 2, 3, 4] 이 출력된다.
```

- **Arrays.toString(배열이름)**
  - 배열의 모든 요소를 **'[첫번째 요소, 두번째 요소, ...]'** 의 형식의 **문자열** 로 만들어서 반환한다.

```java
int[] iArr = { 1, 2, 3, 4 };

System.out.println(iArr); // [I@14318bb 와 같은 형식으로 출력된다.
```
- iArr 는 **참조 변수** 이다.
- **'타입@주소'** 의 형식으로 출력된다.
  - **'[ I '** 는 1차원 int 배열이라는 의미.
  - **'@'** 뒤에 나오는 **16진수** 는 배열의 주소인데, 실제 주소가 아닌 **내부 주소** 이다.

```java
char[] chArr = { 'a', 'b', 'c', 'd' };
System.out.println(chArr); // abcd 가 출력된다.
```

예외적으로

- **char 배열** 은 **print(), println()** 메서드로 출력하면 **각 요소가 구분자없이 그대로** 출력된다.
- **print(), println() 메서드** 가 **char 배열일 때만** 이렇게 동작하도록 작성되었기 때문이다.

### 배열의 복사

1. **for 문을 이용해서 복사**

```java
int[] arr = new int[5];

int[] tmp = new int[arr.length * 2]; // 기존 배열보다 길이가 2인 배열을 생성

for(int i = 0; i < arr.length; ++i) {
    tmp[i] = arr[i]; // arr[i] 의 값을 tmp[i] 에 저장
}

arr = tmp; // 참조 변수 arr이 새로운 배열을 가리키게 한다.  
```

- 참조 변수 arr 와 tmp 는 같은 배열을 가리키게 된다.
- 배열 arr 과 tmp 는 이름만 다를 뿐 동일한 배열이다.
- **자신을 가리키는 참조변수가 없는 배열은 사용할 수 없다.**
  - 쓸모 없게 된 배열은 **JVM 의 Garbage Collector** 에 의해서 **자동적으로 메모리에서 제거된다.**

2. **System.arraycopy()** 를 이용한 **배열의 복사**

for 문은 배열의 요소 하나하나에 접근해서 복사하지만,

**arraycopy** 는 **지정된 범위의 값들을 한 번에 통째로 복사한다.**
- 각 요소들이 연속적으로 저장된 배열의 특성을 이용한 것이다.

**배열의 복사는 for 문 보다 System.arraycopy()를 사용하는 것이 효율적이다!**

```java 
System.arraycopy(num, 0, newNum, 0, num.length);

// num[0] 에서 newNum[0] 으로 num.length 개의 데이터를 복사
```

- 복사하려는 배열의 위치가 적절하지 못하여 복사하려는 내용보다 여유 공간이 적으면 에러(ArrayIndexOutOfBoundsException) 가 발생한다.

```java
class ArrayEx4 {
	public static void main(String[] args) {
		char[] abc = { 'A', 'B', 'C', 'D'};
		char[] num = { '0', '1', '2', '3', '4', '5', '6', '7', '8', '9'};
		System.out.println(abc);
		System.out.println(num);

		// 배열 abc와 num을 붙여서 하나의 배열(result)로 만든다.
		char[] result = new char[abc.length+num.length];
		System.arraycopy(abc, 0, result, 0, abc.length);
		System.arraycopy(num, 0, result, abc.length, num.length);
		System.out.println(result);

		// 배열 abc을 배열 num의 첫 번째 위치부터 배열 abc의 길이만큼 복사
		System.arraycopy(abc, 0, num, 0, abc.length);	
		System.out.println(num);

	     // number의 인덱스6 위치에 3개를 복사
		System.arraycopy(abc, 0, num, 6, 3);
		System.out.println(num);
	}
}

// 실행결과
// ABCD
// 0123456789
// ABCD456789
// ABCD45ABC9 
```

## 배열의 활용

### 로또번호를 생성하는 예제

```java
class ArrayEx8 { 
	public static void main(String[] args) { 
		// 45개의 정수값을 저장하기 위한 배열 생성. 
		int[] ball = new int[45];       

		// 배열의 각 요소에 1~45의 값을 저장한다. 
		for(int i=0; i < ball.length; i++)       
			ball[i] = i+1;    // ball[0]에 1이 저장된다.

		int temp = 0;  // 두 값을 바꾸는데 사용할 임시변수 
		int j = 0;     // 임의의 값을 얻어서 저장할 변수 

		// 배열의 i번째 요소와 임의의 요소에 저장된 값을 서로 바꿔서 값을 섞는다. 
		// 0번째 부터 5번째 요소까지 모두 6개만 바꾼다.
		for(int i=0; i < 6; i++) {       
			j = (int)(Math.random() * 45); // 0~44범위의 임의의 값을 얻는다. 

            // ball[i] 와 ball[j] 의 값을 서로 바꾼다.
			temp     = ball[i]; 
			ball[i] = ball[j]; 
			ball[j] = temp; 
		} 

		// 배열 ball의 앞에서 부터 6개의 요소를 출력한다.
		for(int i=0; i < 6; i++) 
			System.out.printf("ball[%d]=%d%n", i, ball[i]); 
	} 
} 
```

- 마치 1 부터 45 까지의 번호가 쓰인 카드를 잘 섞은 다음 맨 위의 6장을 꺼내는 것과 같다.

### 버블 정렬(Bubble Sort) 알고리즘으로 크기 순으로 정렬

```java
class ArrayEx10 {
	public static void main(String[] args) {
		int[] numArr = new int[10];

		for (int i=0; i < numArr.length ; i++ ) {
			System.out.print(numArr[i] = (int)(Math.random() * 10));
		}
		System.out.println();

		for (int i=0; i < numArr.length-1 ; i++ ) {
			boolean changed = false;	// 자리바꿈이 발생했는지를 체크한다.

			for (int j=0; j < numArr.length-1-i ;j++) {
				if(numArr[j] > numArr[j+1]) { // 옆의 값이 작으면 서로 바꾼다.
					int temp = numArr[j];
					numArr[j] = numArr[j+1];
					numArr[j+1] = temp;
					changed = true;	// 자리바꿈이 발생했으니 changed를 true로.
				}
			} // end for j

			if (!changed) break;	// 자리바꿈이 없으면 반복문을 벗어난다.

			for(int k=0; k<numArr.length;k++)
				System.out.print(numArr[k]); // 정렬된 결과를 출력한다.
			System.out.println();
		} // end for i
	} // main의 끝
}
```

![버블정렬](https://user-images.githubusercontent.com/56071088/120677139-be9b7480-c4d1-11eb-9056-79f403781afd.jpg)


안쪽 for 문

1. 맨 처음에는 비교 횟수가 4이다.
   - 이 값은 배열의 길이보다 1이 작은 값 **(numArr.length - 1)**
2. 두번째는 비교횟수가 3이다. 
   - 오름차순 정렬이므로 배열의 마지막 요소가 최대값이므로, 비교할 필요가 없기 때문이다.
3. 비교 작업(안쪽의 for 문) 을 반복할수록 **비교해야 하는 범위가 1씩 줄어든다.**
   - 매 반복마다 비교 횟수가 1씩 줄어들기 때문에 **바깥쪽 for 문의 제어변수 i 를 빼주는 것이다.**
   - **numArr.length -1 -i**

바깥쪽 for 문

- 안쪽 for 문을 총 4번, 즉, **배열의 길이 - 1** 번 만큼 반복해서 비교해야 한다.
- **numArr.length - 1**

### 빈도수 구하기

```java
class ArrayEx11 {
	public static void main(String[] args) {
		int[] numArr  = new int[10];
		int[] counter = new int[10];

		for (int i=0; i < numArr.length ; i++ ) {
			numArr[i] = (int)(Math.random() * 10); // 0~9의 임의의 수를 배열에 저장
			System.out.print(numArr[i]);
		}
		System.out.println();

		for (int i=0; i < numArr.length ; i++ ) {
			counter[numArr[i]]++;
		}

		for (int i=0; i < numArr.length ; i++ ) {
			System.out.println( i +"의 개수 :"+ counter[i]);
		}
	} // main의 끝
}
// 실행결과
// 0의 개수: 0
// 1의 개수: 0
// 2의 개수: 0
// 3의 개수: 1
// 4의 개수: 3
// 5의 개수: 2
// 6의 개수: 1
// 7의 개수: 2
// 8의 개수: 0
// 9의 개수: 1
```

## String 배열

```java
String[] args = new String[5]; // 5개의 문자열을 담을 수 있는 배열을 생성한다.
```

![image](https://user-images.githubusercontent.com/56071088/120685875-5487cd00-c4db-11eb-8b1c-8788709fb2f0.png)

3개의 String타입의 참조변수를 저장하기 위한 공간이 마련되고 **참조형 변수의 기본값은 null** 이므로 **각 요소의 값은 null 로 초기화된다.**

#### 참고 : 변수의 타입에 따른 기본값

|자료형|기본값|
|---|---|
|boolean|false|
|char|'\u0000'|
|byte, short, int|0|
|long|0L|
|float|0.0f|
|double|0.0d 또는 0.0|
|**참조형 변수**|**null**|


#### About '\u0000'

`유니코드(16진수)문자` : `\u유니코드` (EX) char a = '\u0041';

```java
public class CharTest {
	public static void main(String[] args) {
        char char1 = '\u0000';
        char char2 = '\u0001';
        char char3 = '\u0002';
        char char4 = '\u0041';
        char char5 = '\uAC00';
        char char6 = 0xAC00;
        
        System.out.println("char1 : " + char1);
        System.out.println("char2 : " + char2);
        System.out.println("char3 : " + char3);
        System.out.println("char4 : " + char4);
        System.out.println("char5 : " + char5);
        System.out.println("char6 : " + char6);
    }
	// 실행결과
	
	// char1 :  
	// char2 : 
	// char3 : 
	// char4 : A
	// char5 : 가
	// char6 : 가
}

```

### String 배열의 초기화

```java
String[] name = new String[]{ "Lee", "Kim", "Park" };
String[] name = { "Lee", "Kim", "Park" }; // new String[]을 생략할 수 있음
```

![String 배열](https://user-images.githubusercontent.com/56071088/120775904-9c9c0380-c55e-11eb-898a-2a718f0d7b29.png)

- 배열에 실제 객체가 아닌 **객체의 주소** 가 저장되어 있다.
- 기본형 배열이 아닌 경우, **참조형 배열** 의 경우 배열에 저장되는 것은 **개체의 주소** 이다.
- `참조형 배열` 을 **객체 배열** 이라고도 한다.
- `참조형 변수(참조 변수)` : 객체가 메모리에 저장된 주소 = **4byte의 정수값(0x0 ~ 0xffffffff)** 또는 **null** 이 저장된다.

[16진수를 2진수로 변환하는 예제]

```java 
class ArrayEx13 {
	public static void main(String[] args) {
		char[] hex = { 'C', 'A', 'F', 'E'};

		String[] binary = {   "0000", "0001", "0010", "0011"
						    , "0100", "0101", "0110", "0111"
						    , "1000", "1001", "1010", "1011"
						    , "1100", "1101", "1110", "1111" };

		String result="";

		for (int i=0; i < hex.length ; i++ ) {		
			if(hex[i] >='0' && hex[i] <='9') {
				result +=binary[hex[i]-'0'];	   // '8'-'0'의 결과는 8이다.
			} else {	// A~F이면
				result +=binary[hex[i]-'A'+10]; // 'C'-'A'의 결과는 2
			}
		}

		System.out.println("hex:"+ new String(hex)); // String(char[] value)
		System.out.println("binary:"+result);
	}
}
```

- 변환하고자 하는 16진수를 배열 hex에 나열
- 16진수에는 A~F 까지 6개의 문자가 포함되므로 char 배열로 처리하였다.
- 문자열 binary 에는 이진수 '0000' ~ '1111'(16진수로 0~F) 까지 모두 16개의 값을 문자열로 저장하였다.

## char 배열과 String 클래스

**String 클래스가 char 배열에 메서드를 추가한 것이다.**

- 객체지향 언어인 Java 에서는 char 배열과 그에 관련된 기능들을 함께 묶어서 String 클래스에 정의한다.

char 배열과 String 클래스의 한 가지 중요한 차이
- **String 객체(문자열)은 읽을 수만 있을 뿐** 내용을 변경할 수 없다.
- **변경 가능한 문자열** 을 다루려면, **StringBuffer 클래스** 를 사용하면 된다.

### String 클래스의 주요 메서드
|메서드|설명|
|---|---|
|char charAt(int index)|문자열에서 해당 위치(index)에 있는 문자를 반환한다.|
|int length()|문자열의 길이를 반환한다.|
|String substring(int from, int to)|문자열에서 해당 범위(from ~ to)에 있는 문자열을 반환한다. **(to는 범위에 포함되지 않음)** |
|boolean equals(Object obj)|문자열의 내용이 obj와 같은지 확인|
|char[] toCharArray()|문자열을 **문자배열(char[])로 변환해서 반환한다.** |

- equals() 메서드는 대소문자를 구분한다.
- equalsIgnoreCase()는 대소문자를 구분하지 않는다.

### char 배열과 String 클래스의 변환

```java
char[] chArr = new char[]{ 'A', 'B', 'C', 'D' };
String str = new String(chaArr); // char 배열 -> String 
char[] tmp = str.toCharArray();  // String -> char 배열 
```

[문자열(String)을 모르스(morse) 부호로 변환하는 예제] 

```java
class ArrayEx15 {
	public static void main(String[] args) {
		String source = "SOSHELP";
		String[] morse = {".-", "-...", "-.-.","-..", "."
						,"..-.", "--.", "....","..",".---"
						, "-.-", ".-..", "--", "-.", "---"
						, ".--.", "--.-",".-.","...","-"
						, "..-", "...-", ".--", "-..-","-.--"
						, "--.." };

		String result="";

		for (int i=0; i < source.length() ; i++ ) {
			result+=morse[source.charAt(i)-'A'];
		}
		System.out.println("source:"+ source);
		System.out.println("morse:"+result);
	}
}
```

### 커맨드 라인을 통해 입력받기

`커맨드 라인을 이용한 방법` : **화면을 통해 사용자로부터 값을 입력 받을 수 있다.**

- **프로그램을 실행할 때 클래스 이름 뒤에 공백 문자로 구분하여 여러 개의 문자열을 프로그램에 전달 할 수 있다.**

EX) c:\jdk1.8\work\ch5\java MainTest hello hi 123

- 커맨드 라인을 통해 입력된 두 문자열은 String 배열에 담겨져서 MainTest 클래스의 **main 메서드의 매개변수(args)에 전달된다.**

```java
class ArrayEx16 {
	public static void main(String[] args) {
		System.out.println("매개변수의 개수 :"+args.length);

		for(int i=0;i< args.length;i++) {
			System.out.println("args[" + i + "] = \""+ args[i] + "\"");
		}
	}
}

// 실행결과
// c:\jdk1.8\work\ch5\java MainTest hello hi "hello world" 123 
// 매개변수의 개수 : 4
// args[0] = "hello"
// args[1] = "hi"
// args[2] = "hello world"
// args[3] = "123"

// c:\jdk1.8\work\ch5\java MainTest  <- 매개변수를 입력하지 않을 때
// 매개변수의 개수 : 0
```

- 커맨드 라인에 입력된 매개변수는 공백 문자로 구분한다.
- 입력된 값에 **공백이 있을 경우 큰따옴표("") 로 감싸주어야 한다.**
- 숫자를 입력해도 문자열로 취급한다.
  - int num = `Integer.parseInt("123")` ; // 변수 num에 숫자 123이 저장된다. 
- 커맨드 라인에 매개변수를 입력하지 않으면 **크기가 0인 배열을 생성** 한다. args.length 의 값은 0이 된다.
  - 이렇게 **크기가 0인 배열도 생성 가능하다.**

- `System.exit(0)` : 프로그램을 종료한다.

## 다차원 배열

|선언 방법|선언 예|
|---|---|
|**타입[][] 변수이름;**|**int[][] nums;**|
|타입 변수이름[][]|int nums[][]|
|타입[] 변수이름[]| int[] nums[]|

```java
int[][] nums = new int[4][3]; // 4행 3열의 2차원 배열을 생성한다.

int[][] arr = new int[][]{ {1,2,3}, {4,5,6} };
int[][] iArr = { {2,3,4}, {5,6,7} }; // new int[][] 가 생략됨.

int[][] iArr = { 
                    {2,3,4}, 
                    {5,6,7} 
                };
// 웬만하면 행별로 줄 바꿈을 해주는 것이 보기에 훨씬 편하다.

```

![2차원 배열](https://user-images.githubusercontent.com/56071088/120833113-5feefd00-c59c-11eb-9efe-fb173b8f9a92.PNG)


### 가변 배열

2차원 이상의 다차원 배열을 생성할 때 전체 배열 차수 중 **마지막 차수의 길이를 지정하지 않고,** 추후에 각기 다른 길이의 배열을 생성할 수 있다. **고정된 형태가 아닌 보다 유동적인 가변 배열을 구성할 수 있다.**

```java
int[][] arr = new int[5][]; // 두 번째 차원의 길이는 지정하지 않는다.
arr[0] = new int[4];
arr[1] = new int[2];
arr[2] = new int[3];
arr[3] = new int[1];
arr[4] = new int[5];
```

![가변 배열](https://user-images.githubusercontent.com/56071088/120833764-24a0fe00-c59d-11eb-895a-85a23e4eefd4.png)


- **1차원 배열의 각 요소는 2차원 배열의 참조변수 역할을 한다.**

[행렬의 곱셈 예제]

```java
class MultiArrEx3 {
	public static void main(String[] args) {
		int[][] m1 = {
			{1, 2, 3},
			{4, 5, 6}
		};

		int[][] m2 = {
			{1, 2},
			{3, 4},
			{5, 6}
		};

		final int ROW    = m1.length;      // m1의 행길이
		final int COL    = m2[0].length;  // m2의 열길이
		final int M2_ROW = m2.length;	    // m2의 행길이

		int[][] m3 = new int[ROW][COL];

	   // 행렬곱 m1 x m2의 결과를 m3에 저장
		for(int i=0;i<ROW;i++)
			for(int j=0;j<COL;j++)
				for(int k=0;k<M2_ROW;k++)
					m3[i][j] += m1[i][k] * m2[k][j]; 

	   // 행렬 m3를 출력 
		for(int i=0;i<ROW;i++) {
			for(int j=0;j<COL;j++) {
				System.out.printf("%3d ", m3[i][j]);
			}
			System.out.println();
		}
	} // main의 끝
}
```

- `m3의 행index`가 행렬 **m1의 행 index와 일치** 하고, 
- `m3의 열index`가 **m2의 열index와 일치** 한다는 것을 알 수 있다.




 


