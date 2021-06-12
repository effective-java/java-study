# chap4 조건문과 반복문

## 조건문

### if 문

```java
if (조건식) {

} else if(조건식) {

} else {

}
```

- 제약조건
    - 조건식의 결과는 반드시 true/false 이어야 한다.

### switch 문

```java
switch(조건식) {
		case 값1 :
				break
		case 값2 :
				break
		default :
}
```

- break문이 각 case문의 영역 구분 역할
- 제약조건
    - 조건식의 결과값이 반드시 정수 or 문자열(jdk 1.7 부터) 이어야 한다.
    - case문의 값은 정수 상수만 가능하며, 중복되지 않아야 한다.
        - 변수, 실수, 문자열 불가
- if문 처럼 중첩 가능

## 반복문

### for문

```java
//   (초기화;조건식;증감식)
for(int i=1; i<5; i++) {

}
```

- 초기화
    - 반복에 사용될 변수를 초기화하는 부분
    - 처음 한번 수행
    - 둘 이상의 변수 사용은 콤마로 구분, 단 변수의 타입은 같아야 한다.
- 조건식
    - 조건식이 참인동안 반복
- 증감식
    - 반복문을 제어하는 변수의 값을 증가 또는 감소시키는 식
    - 쉼표를 이용해서 두 문장 이상을 하나로 연결해서 쓸 수 있다.
- 세가지 요소는 생략 가능하며, 모두 생략하면 무한 반복문
- 중첩 가능

### 향상된 for문(enhanced for statement)

- jdk 1.5부터 배열과 컬렉션에 저장된 요소에 접근할 때 기존보다 편리한 방법으로 처리 가능

```java
for (타입 변수명: 배열 또는 컬렉션) {

}
------------------------------
int[] arr = {10,20,30,40,50};
for (int tmp: arr) {
		System.out.println(tmp)
}
```

### while문

```java
while(조건식) {

}
```

- 조건식 생략 불가

```java
while(i--!=0) {
	System.out.println(i);
}

위 아래 같다.

while(i!=0) {
	i--;
	System.out.println(i);
}
```

```java
while(--i!=0) {
	System.out.println(i);
}

무한반복

i--;
while(i!=0) {
	System.out.println(i);
}
```

### do-while문

```java
do {

} while(조건식);
```

### 이름 붙은 반복문

- break문은 근접한 하나의 반복문만 벗어날 수 있기 때문에, 중첩 반복문 앞에 이름을 붙이고 break문과 continue문에 이름을 지정해 줌으로써 하나 이상의 반복문을 벗어나거나 반복을 건너뛸 수 있다.

```java
Loop1 : for(int i=2; i<=9; i++) {
				for(int j=1; j<9; j++) {
						if(j==5) {
								break Loop1;
								// break;
								// continue Loop1;
								// continue;
						}
				}
}
```