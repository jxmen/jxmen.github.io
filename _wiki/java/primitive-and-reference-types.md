---
layout  : wiki
title   : 프리미티브 타입과 레퍼런스 타입
summary : 
date    : 2024-02-09 23:18:39 +0900
updated : 2024-02-10 11:59:19 +0900
tag     : java 
toc     : true
public  : true
parent  : [[/java]] 
latex   : false
resource: FE59C026-63F3-44EF-8C86-A6C00E5ADB0B
---
* TOC
{:toc}

### 프리미티브 타입

프리미티브(원시) 타입은 메모리 상에 **실제 값**을 저장하고 데이터의 크기가 정해져 있는 타입들을 의미한다.

대표적으로 `int`, `char`, `double`, `float`등이 있을 것이다.

```java
int a = 10; // 4바이트
```

### 레퍼런스 타입

레퍼런스 타입은 데이터의 값을 저장하는 것이 아닌 **데이터가 가리키는 주소값**을 저장하는 데이터 타입이다.

new로 생성된 데이터 타입, 프리미티브 타입의 wrapper 클래스 등이 해당된다.

```java
class Person {
	int age = 0;
	
	constructor(int age) {
		this.age = age;
	}
}

new Person(25); // 8바이트(64비트 운영체제시) + int(4바이트)의 공간 차지
```

### 프리미티브 타입과 레퍼런스 타입의 크기

프리미티브 타입이 차지하는 데이터 타입의 크기는 다음과 같다. [^1]

|        | 1byte   | 2byte | 4byte   | 8byte  |
|--------|---------|-------|---------|--------|
| 논리형 | boolean |       |         |        |
| 문자형 |         | char  |         |        |
| 정수형 | byte    | short | int     | long   |
| 실수형 |         |       | float   | double |

레퍼런스 타입이 차지하는 데이터 타입의 크기는 운영체제가 메모리를 얼마나 차지하느냐에 따라 달라진다.

32비트 운영체제에서는 4바이트, 64비트 운영체제에서는 8바이트를 차지할 것이다.


### (부록) IntegerCache

앞서 말했듯 Integer도 레퍼런스 타입이다. 

하지만, Java에서 127을 Integer를 서로 동치성 비교를 하면, 같은 값이 나오는 것을 확인할 수 있다. 아래의 테스트 코드를 보자.

```java
@ParameterizedTest
@ValueSource(ints = { 127, -127 })
void _127_Integer는_비교시_true를_반환한다(int value) {
	Integer a = value;
	Integer b = value;

	assert a == b;
}
```

- Integer로 변환함에 따라 `Integer.valueOf(value)`로 오토박싱이 일어났다는 것을 유의하자.

위의 테스트 코드는 통과한다. 하지만 여기서 해당 값을 벗어나는 값에서는 `a == b`가 faslse를 리턴한다.

해당 현상이 발생하는 이유는, Integer 내부 클래스를 들여다보면 `IntegerCache`라는 Inner Class가 자주 쓰는 int 값들을 캐싱하고 있기 때문이다.

해당 캐시 지정값은 JVM 옵션을 통해 수정이 가능하다.

### 참고 문서
- [Java Integer.valueOf(127) == Integer.valueOf(127) 는 참일까요?](https://meetup.nhncloud.com/posts/185)

### 주석

[^1]: 자바의 정석 p29
