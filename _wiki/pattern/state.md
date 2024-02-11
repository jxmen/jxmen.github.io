---
layout  : wiki
title   : 상태 패턴 (State)
summary : 객체 내부에서 자신의 상태를 변경한다
date    : 2024-02-11 15:35:25 +0900
updated : 2024-02-11 17:23:58 +0900
tag     : pattern 
toc     : true
public  : true
parent  : [[/pattern]] 
latex   : false
resource: 4EDE163C-DE85-4BD4-8329-9F5D00700807
---
* TOC
{:toc}

### 개요

상태 패턴은 객체 내부의 상태에 따라 행동을 제한하거나, 다른 상태 변경을 제어할 때 유용하게 사용할 수 있다.

### 구조 및 참여자

상태 패턴은 전략 패턴과 구조가 비슷하다. 

- State
	- 상태를 정의하는 인터페이스이다.
	- 상태가 할 수 있는 행동에 대해 이 인터페이스에서 정의한다. 
- ConcreteState
	- 상태를 구현하는 구현체로, 구현체마다 자신이 할 수 있는 행동을 구현한다.
	- Context에게 상태 변경을 위임한다.
- Context
	- State를 사용하는 객체

### 예제

#### 헤드 퍼스트 디자인 패턴

헤드 퍼스트 디자인 패턴에서는 뽑기 기계를 통해 예시를 들고 있다.

- 상태는 `매진`, `동전 없음`, `동전 있음`, `뽑는중` 등이 있다.
- 행동은 `코인 추가`, `코인 반환`, `손잡이 돌리기`, `알맹이 내보내기` 등이 있다.

현재 상태에 따라 내가 하려는 행동에 대해 변경할 수 있는/없는 상태가 있고, 이를 if-else 문으로 구현할 수 있다.

kotlin으로 간단하게 작성한 예시로는 다음과 같다.

```kotlin
fun insertQuarter() {
	if (state == "동전 없음") {
		println("코인을 추가합니다")
		state = "동전 있음"
	} else if (state == "매진") {
		println("매진 상태에서는 코인을 넣을 수 없습니다.")	
	} else if (state == "뽑는중") {
		println("뽑는중에서는 코인을 넣을 수 없습니다.")	
	}
}

fun ejectQuarter() {
	// (생략) 코인을 반환하는 행동에 따른 상태 처리 로직 ...
}

fun turnCrank() {
	// (일부 생략) 손잡이를 돌리는 행동에 따른 상태 처리 로직 ...
	
	if (state == "동전 있음") {
		state = "뽑는중"
		dispense()
	}
}

fun dispense() {
	// (생략) 알맹이 내보낸 후에 따른 상태 처리 로직 ...
}

```

각 행동에 따라 상태 처리에 따른 로직이 모두 달라지게 된다.

그렇다면 위와 같은 코드에서 새로운 상태가 추가된다면? 이벤트로 1/10 확률로 알맹이 2개를 내보내야 한다면 어떻게 처리할 것인가?

물론 if-else문에서 계속 추가하는 방식으로 구현할 수 있다. 하지만 이는 많은 코드 변경이 이루어지고, 복잡한 상태에서 실수할 여지가 커지게 된다.

그렇다면 이런 **바뀌는 부분을 캡슐화**하도록 설계를 하면 어떨까?

헤드 퍼스트 디자인에서는 다음과 같이 코드를 변경한다.

```java
interface State {
	void insertQuarter();
	void ejectQuarter();
	void turnCrank();
	void dispense();
}

class NoQuarterState implements State {
	GumballMachine gumballMachine;

	public NoQuarterState(GumballMachine gumballMachine) {
		this.gumballMachine = gumballMachine;
	}

	@Override
	public void insertQuarter() {
		System.out.println("동전을 투입합니다.");
		this.gumballMachine.setState(gumballMachine.getHasQuaterState());
	}

	// (중략) 나머지 메서드들 ...
}

class HasQuarterState implments State {

	GumballMachine gumballMachine;
	
	public HasQuarterState(GumballMachine gumballMachine) {
		this.gumballMachine = gumballMachine;
	}

	@Override
	public void insertQuarter() {
		System.out.println("이미 동전이 있습니다");
	}

	// (중략) 나머지 메서드들 ...
}

// 나머지 클래스들 ...
```

상태라는 인터페이스를 정의하고, 해당 인터페이스에 해야할 행동들을 정의한다.

그리고 각 상태별로 행동에 대해 제어한다.

새로운 상태가 추가된다면 상태 인터페이스를 구현하는 구현체를, 행동이 추가된다면 상태 인터페이스에 메서드를 추가한다. 이로 인해 필요한 부분에 대해서만 구현을 할 수 있도록 변경해주며, SOLID 원칙 중 OCP 원칙을 지킬 수 있게 된다.

#### 개인 프로젝트 User 상태 패턴 적용

개인 프로젝트 코드에 상태 패턴을 적용해보면 어떨까 궁금해서 한번 해보았다.

아래는 유저 상태를 상태 패턴으로 작성해본 예제이다.

- 상태가 register -> activate -> deleted로 변경 가능할 때 다음과 같이 작성할 수 있다.
 
```kotlin
interface UserState {
    fun signUp()
    fun activate()
    fun delete()
}

class RegisterUserState(
    private val user: User
) : UserState {

    override fun signUp() {
        println("이미 가입된 유저입니다.")
    }

    override fun activate() {
        println("유저가 활성화 되었습니다.")
        user.state = user.activateUserState
    }

    override fun delete() {
        println("활성화 상태에서만 유저를 삭제할 수 있습니다.")
    }
}

class ActivateUserState(
    private val user: User
) : UserState{
    override fun signUp() {
        println("이미 가입한 유저입니다.")
    }

    override fun activate() {
        println("이미 활성화된 유저입니다.")
    }

    override fun delete() {
        println("유저 삭제 처리를 진행합니다.")
        user.state = user.deleteUserState
    }
}

class DeleteUserState(
    private val user: User
): UserState {
    override fun signUp() {
        println("삭제된 유저입니다.")
    }

    override fun activate() {
        println("삭제된 유저입니다.")
    }

    override fun delete() {
        println("이미 삭제된 유저입니다.")
    }
}


class User() {
    var activateUserState: ActivateUserState
    var registerUserState: RegisterUserState
    var deleteUserState: DeleteUserState

    var state: UserState;

    init {
        this.activateUserState = ActivateUserState(this)
        this.registerUserState = RegisterUserState(this)
        this.deleteUserState = DeleteUserState(this)

        this.state = this.registerUserState
    }

    fun activate() {
        this.state.activate()
    }

    fun delete() {
        this.state.delete()
    }
}

fun main() {
    val user = User()
    user.activate()
    user.activate()

    user.delete()
    user.delete()
}
```

실행 결과

```bash
유저가 활성화 되었습니다.
이미 활성화된 유저입니다.
유저 삭제 처리를 진행합니다.
이미 삭제된 유저입니다.
```

이렇게 보니 지금은 상태 패턴이 과한 것 같다. 상태가 register -> activate -> deleted 단방향으로만 진행되기 때문에 그런 것 같다. deleted 상태에서 다시 activate로 갈 수 있는 등 여러 복잡한 상태에서는 유용하게 사용할 수 있을 것 같다.

### 참고 자료

- 헤드 퍼스트 디자인 패턴 - 개정판

