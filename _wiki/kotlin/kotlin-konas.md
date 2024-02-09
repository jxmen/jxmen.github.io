---
layout  : wiki
title   : Kotlin Koans 필요한 내용 요약
summary : Intellij에서 제공하는 Kotlin 튜토리얼 플러그인 
date    : 2024-01-29 11:02:01 +0900
updated : 2024-02-10 03:44:56 +0900
tag     : kotlin
toc     : true
public  : true
parent  : [[Kotlin]]
latex   : false
resource: EB558039-AC80-4BDF-9913-DDDE2CAC3D3D
---
* TOC
{:toc}

### Strings
- 정규식을 선언할때도 큰따옴표 3개를 사용한다 `"""\d{2}"""`

### Null 다루기
- let 구문과 옵셔널 체이닝(`?.`)을 조합하여, null이 아닐때만 괄호 안의 구문이 실행되도록 할 수 있다.
	```kotlin
	message?.let { mailer.sendMessage(email, message)} // message를 it으로도 가능
	```
 
### Smart casts

- 어떤 타입인지 체크할 경우 알아서 해당 타입에 대해 캐스팅이 된다. [링크](https://kotlinlang.org/docs/typecasts.html#smart-casts)
	
	```kotlin
	when (expr) {
		is Num -> expr.value
		is Sum -> eval(expr.left) + eval(expr.right)
		else -> throw IllegalArgumentException("Unknown expression")
	}

	// ... 중략
	interface Expr
	class Num(val value: Int) : Expr
	class Sum(val left: Expr, val right: Expr) : Expr
	```

### `sealed` 키워드

- sealed로 정의될 경우, 다른 패키지나 모듈에서 접근할 수 없다. 패턴 매칭/when 등에서 유용하게 사용할 수 있다. [링크](https://kotlinlang.org/docs/sealed-classes.html)

	```kotlin
	sealed interface Expr

	class Num(val value: Int) : Expr
	class Sum(val left: Expr, val right: Expr) : Expr
	```

### Associate

- Map 형태의 자료구조를 만들 때 유용하게 사용한다. [링크](https://kotlinlang.org/docs/collection-transformations.html#associate)
 
	```kotlin
	val list = listOf("abc", "cdef")

	list.associateBy { it.first() } // == mapOf('a' to "abc", 'c' to "cdef")

	list.associateWith { it.length } // == mapOf("abc" to 3, "cdef" to 4)

	list.associate { it.first() to it.length } // == mapOf('a' to 3, 'c' to 4)
	```				

### Fold And Reduce

- [링크](https://kotlinlang.org/docs/collection-aggregate.html#fold-and-reduce)
	- 초기값을 지정하고 싶을때는 `Fold`를 사용한다.
	- 모든 축소 작업은 빈 컬렉션일때는 Exception을 발생한다. 이에 대응하려면 orNull을 사용하면 null로 받을 수 있다.

