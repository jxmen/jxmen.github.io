---
layout  : wiki
title   : 전략 패턴 (Strategy)
summary : 알고리즘을 캡슐화하여 런타임에서 변경할 수 있도록 한다
date    : 2024-02-10 17:50:10 +0900
updated : 2024-02-11 15:37:12 +0900
tag     : pattern
toc     : true
public  : true
parent  : [[/pattern]]
latex   : false
resource: 19C9E87F-889B-4E55-8BD5-49C41C727C01
---
* TOC
{:toc}

전략 패턴은 동일 계열의 알고리즘을 캡슐화하고, 이 알고리즘을 변경하거나 손쉽게 확장할 수 있도록 해주는 패턴이다.

- 일반적인 구조는 인터페이스를 통해 알고리즘을 추상화 하고, 이 인터페이스의 구현체를 외부에서 주입하는 방식으로 사용한다. 
- 만약 내가 다른 전략을 사용하고 싶다면, 인터페이스를 구현하는 구현체에 해당 알고리즘을 구현하는 구현체로 변경한다.

### 활용성

다음과 같은 경우에는 전략 패턴을 사용하면 좋다.

- 동일 계열 알고리즘이 자주 변경되는 상황이 있을 경우
- 알고리즘이 추후 추가될 가능성이 있는 경우
- 여러 행동에 의해 복잡한 다중 조건문을 사용하는 경우 [^1]

### 구조 및 참여자 [^2]

![strategy pattern structure]( /resource//1992ff68-1792-4708-9c52-e70e679281de.png )

- Context
	- 알고리즘(전략)을 사용하는 클래스
	- Strategy 인터페이스에 정의한 내용을 통해 실제 알고리즘을 사용한다.
	- `setStrategy()` 같은 메서드를 제공하면, 외부에서 전략을 변경할 수 있도록 제공할 수 있다.
- Strategy
	- 알고리즘에 대한 인터페이스
- ConcreteStrategy
	- Starategy 인터페이스를 구현하는 구현체 클래스

### 예제 코드

#### 헤드 퍼스트 디자인 패턴

아래는 헤드 퍼스트 디자인 패턴에 나오는 전략 패턴을 TS로 조금 수정한 코드이다. 

```ts
const mallardDuck = new MallardDuck();
mallardDuck.performQuack(); // 꽥
mallardDuck.performFly(); // 날고 있어요

mallardDuck.setFlyBehavior(new FlyNoWay()); // 나는 전략 변경

mallardDuck.performFly(); // 날 수 없어요
```

### 전략 패턴과 테스트

전략 패턴을 통해 테스트 하기 어려운 부분을 테스트 하기 쉽도록 설계하는 것도 가능하다. 다음과 같은 java 예제 코드를 보자.

```java
public class Car {
	private static final Random RANDOM = new Random();

	private int score;

	public Car() {
		this.score = 0;
	}

	public void race() {
		this.score += RANDOM.nextInt(10);
	}

	public boolean wins(Car car2) {
		return this.getScore() > car2.getScore();
	}

	private int getScore() {
		return this.score;
	}
}
```

wins() 메서드를 테스트하는 테스트코드는 다음과 같다.

```java
class CarTest {

	@Test
	void wins_메서드_테스트() {
		Car car1 = new Car();
		car1.race();

		Car car2 = new Car();
		car2.race();

		assert car1.wins(car2);
	}
}
```

이 테스트는 통과할 때도 있고 통과하지 못할 때도 있다. race() 메서드가 랜덤한 score를 증가시키기 때문이다.

랜덤은 우리가 제어할 수 없는 영역이다. 테스트 코드에서는 우리가 제어할 수 있게 설계하는 것이 중요하다.

랜덤으로 숫자를 반환하는 것을 인터페이스로 분리한다면, 우리가 테스트 하는 시점에서는 원하는 값을 넣어 테스트가 가능하도록 할 수 있을 것이다.

다음은 변경한 코드이다.

```java
@FunctionalInterface
public interface RandomNumerGenerator {
	int generate();
}

public class TestAbleCar {
	private static final RandomNumerGenerator DEFAULT_RANDOM_NUMBER_GENERATOR;

	static{
		Random random = new Random();
		DEFAULT_RANDOM_NUMBER_GENERATOR = () -> random.nextInt(10);
	}

	private int score = 0;

	public void race() {
		this.race(DEFAULT_RANDOM_NUMBER_GENERATOR);
	}

	public void race(RandomNumerGenerator randomNumerGenerator) {
		this.score += randomNumerGenerator.generate();
	}

	public boolean wins(TestAbleCar other) {
		return this.getScore() > other.getScore();
	}

	private int getScore() {
		return this.score;
	}
}
```

```java
class TestAbleCarTest {

	@Test
	void wins_매서드_테스트() {
		TestAbleCar car1 = new TestAbleCar();
		car1.race(() -> 4); // 4값 주입

		TestAbleCar car2 = new TestAbleCar();
		car2.race(() -> 3); // 3값 주입

		assert car1.wins(car2);
	}
}
```

테스트에서 고정된 값을 주입해주기 때문에, 온전하게 wins() 메서드를 테스트할 수 있다.

실제 Car를 사용시에는 인자 없이 사용하여 기본 구현체로 DefaultRandomNumberGenerator와 같은 구현체를 사용하게 하도록 설계하면 좋을 것이다.

### 참고 자료

- GoF의 디자인 패턴
- 헤드 퍼스트 디자인 패턴 - 개정판

### 각주
[^1]: GoF의 디자인 패턴 p409
[^2]: GoF의 디자인 패턴 p409


