---
layout  : wiki
title   : equals와 hashCode
summary : 
date    : 2024-02-02 18:03:56 +0900
updated : 2024-02-02 19:39:05 +0900
tag     : java 
toc     : true
public  : true
parent  : [[java]]
latex   : false
resource: 4C91799A-A0CE-4DDD-B3B2-0575A36AE970
---
* TOC
{:toc}

### equals를 재정의 해야 할 때는 언제일까?

> 객체 식별성이 아니라 **논리적 동치성**을 확인해야 하는데, 상위 클래스의 equals가 논리적 동치성을 비교하도록 재정의하지 않았을 때다. 주로 값 클래스들이 해당한다. [^1]

결론적으로는 "값"이 같을때 같은 취급을 하고 싶은 경우 equals를 재정의하는 것이다.

### equals 메서드를 재정의를 위한 일반 규약 요약

일반 규약을 지키도록 해야한다. 컬렉션 클래스를 포함해 수많은 클래스는 전달받은 객체가 equals 규약을 지킨다고 가정한다.

- 반사성 `x.equals(x) == true`
- 대칭성 `x.equals(y) == y.equals(x)`
- 추이성 `x.equals(y) == y.equals(z) == x.equals(z)`
- 일관성 `x.equals(y) // n번 호출해도 모두 true`
- null-아님 `x.equals(null) // false`

### 설계 오류로 인해 equals를 사용하면 안되는 라이브러리

- `java.sql.Timestamp`: java.util.Date를 확장한 후 nanoseconds 필드를 추가
- `java.net.URL`: 주어진 URL과 매핑된 호스트의 IP 주소를 통해 비교

### hashCode를 잘못 지정하면 어떻게 될까?

HashMap, HashSet 등의 컬렉션 프레임워크 클래스를 사용할 때, 내가 원하는 값을 찾지 못할 수 있다. 

```java
@Nested
@DisplayName("논리적으로 같은 객체가 다른 해시코드를 반환할 때")
class NonHashCodePhoneNumberTest {

	@Test
	@DisplayName("해시 기반 컬렉션에서 null을 리턴한다.")
	void test() {
		Map<PhoneNumber, String> m = new HashMap<>();
		m.put(new PhoneNumber(707, 867, 5309), "제니");

		assert m.get(new PhoneNumber(707, 867, 5309)) == null;
	}
}
```

Map의 경우 key값으로 객체의 해시 값을 저장하는데, 해시코드가 정의되지 않을 경우 다른 해시코드를 찾게 되어 null이 반환될 수 있다.

### 좋은 해시 함수란?

이상적인 해시 함수는 주어진 인스턴스들을 `32비트` 정수 범위에 균일하게 분배하는 것이라고 한다.

## 주석 

[^1]: 이펙티브 자바 3판. 아이템 10. 53쪽.

