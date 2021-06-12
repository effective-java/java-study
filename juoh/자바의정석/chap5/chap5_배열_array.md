# chap5 배열 array

## 배열

- **같은 타입**의 여러 변수를 하나의 묶음으로 다루는 것
- 변수와 달리 배열은 각 **저장공간이 연속적으로 배치**되어 있다.

![image_0](../image/chap5_배열_array_1.png)

### 배열 선언

```java
// case1, 선호방법
타입[] 변수이름;

// case2
타입 변수이름[];
```

### 배열 생성

```java
타입[] 변수이름;            // 배열을 선언(배열을 다루기 위한 참조변수 선언)
변수이름 = new 타입[길이];   // 배열을 생성(실제 저장공간을 생성)

int[] socre;            // 메모리에 score 참조변수 공간만 생성
score = new int[5];     // 1. 연산자 'new'에 의해서 5개의 int 형 데이터 저장공간 생성
                        // 2. 각 배열요소는 기본값인 0으로 초기화
                        // 3. 대입 연산자 '='에 의해 배열의 주소가 int형 배열 참조변수 socre에 저장
```

### 길이와 인덱스

- 요소(element): 생성된 배열의 각 저장공간, 배열이름[인덱스]의 형식으로 배열의 요소 접근
- 인덱스: 배열의 요소마다 붙여진 일련번호, 0부터 시작
- 배열이름.length
    - 자바에서는 JVM이 모든 배열의 길이를 별도로 관리하며 .length로 길이값을 구할 수있다.
    - 배열이름.length는 상수다.

### 배열의 길이 변경하기

1. 더 큰 배열을 새로 생성한다.
2. 기존 배열의 내용을 새로운 배열에 복사한다.

### 배열의 초기화

```java
int[] socre = new int[] {50, 60, 79, 80, 90}

// new int[] 생략 가능
int[] socre = {50, 60, 79, 80, 90}       // ok

// 생략 불가능
int[] score;
score = new int[]{1,2,3,4,5}             // ok
socre = {1,2,3,4,5}                      // error

// 생략 불가능2 
int add(int[] arr) { }
int result = add(new int[]{1,2,3,4,5,}) // ok 
int result = add({1,2,3,4,5,})          // error
```

```java
// 모두 길이가 0인 배열
int[] score = new int[0];
int[] score = new int[]{};
int[] scorfe = {};
```

### 배열의 출력

1. 반복문 사용
    - char 배열일 경우 println 메서드를 사용해도 각 요소가 구분자 없이 그대로 출력된다.
2. Arrays.toString(배열이름) 메서드 사용

### 배열의 복사

1. for문을 이용한 배열복사

```java
int[] arr = new int[5];
int[] tmp = new int[arr.length * 2];

for(int i=0; i<arr.length; i++){
		tmp[i] = arri[;]
}
arr = tmp;
```

2. System.arraycopy()를 이용한 배열의 복사

```java
// num[0]에서 newNum[0]으로 num.length개의 데이터를 복사
System.arraycopy(num, 0, newNum, 0, num.length);
```

### String 배열

```java
// 참조형 변수의 기본값은 null이므로 각 요소의 값은 null로 초기화
String[] name = new String[3];
```

- 문자열은 문재별인 char배열과 같은 뜻이지만 String 클래스를 이용해서 문자열을 처리하는 이유는 char배열에 여러 가지 기능(메서드)을 추가하여 확장한 것이기 때문
- String 객체는 읽은 수만 있을 뿐 내용을 변경할 수 없다.
- 주요 메서드
    - char CharAt(int index) : 문자열에서 해당 위치에 있는 문자 반환
    - int length(): 문자열의 길이 반환
    - String substring(int from, int to): 문자열에서 해당범위에 있는 문자열을 반환한다.(to는 범위 포함 x)
    - boolean equals(Object obj): 문자열의 내용이 obj와 같은지 확인
    - char[] toCharArray(): 문자열을 문자배열로 변환해서 반환한다.

### char 배열과 String 클래스의 변환

```java
char[] chArr = {'a', 'b', 'c'};
String str = new String(chArr);  // char array to String
char[] tmp = str.toCharArray();  // String to char array
```

## 다차원배열

### 배열선언 & 생성

```java
// 타입[][] 변수이름;
int[][] socre;

// 타입 변수이름[][];
int score[][];

// 타입[] 변수이름[];
int[] score[];

// 생성
int[][] socre = new int[4][3];
```

### 초기화

```java
int[][] arr = new int[][] { {1,2,3},{4,5,6} };
// new int[][] 생략
int[][] arr = { 
									{1,2,3},
									{4,5,6} 
							};
```

- arr.length : 2
- arr[0] ~ arr[1].length : 3

### 가변 배열

- 2차원 이상의 다차원 배열을 생성할 때 전체 배열 차수 중 마지막 차수의 길이를 지정하지 않고, 추후에 각기 다른 길이의 배열을 생성함으로써 고정된 형태가 아닌 보다 유동적인 가변 배열 구성 가능

```java
int[][] socre = new int[5][3];

// 가변 배열
int[][] socre = new int[5][];
score[0] = new int[4];
score[1] = new int[3];
score[2] = new int[2];
score[3] = new int[1];
score[4] = new int[5];

// 가변 배열2
int[][] socre = {
										{1,2,3},
										{4,5},
										{6,7,8}
								}

```