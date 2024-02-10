---
layout  : wiki
title   : 옵저버 패턴 (Observer)
summary : 구독하고 있는 객체들에게 메시지를 보낸다
date    : 2024-02-10 20:57:31 +0900
updated : 2024-02-11 02:02:10 +0900
tag     : pattern 
toc     : true
public  : true
parent  : [[/pattern]] 
latex   : false
resource: A342C3BA-F895-41C8-8B17-AAC356C900BE
---
* TOC
{:toc}

### 개요

옵저버는 일대다 의존관계를 통해, 구독하고 있는 객체들에게 상태 변화를 통지하고 이를 인지하여 상태를 변화하거나 후속 작업을 진행할 수 있게 해준다.

### 활용성

다음과 같은 상황에서 쓰면 유용하다.

- 하나의 주제에 대한 변경이 일어났을때, 관련된 객체들이 모두 의존성을 가질 때
- 여러 상태의 변경이 필요하고, 개발자가 얼마나 많은 객체가 변경되는지 몰라도 될 때

옵저버 패턴은 다음과 같은 장점이 있다.

- 옵저버가 인터페이스로 구현됨에 따라, **느슨한 결합**을 유지한다.

### 구조 및 참여자 

구독할 주제인 Subject, 구독할 대상인 Observer로 나뉜다.

일반적으로 Subject와 Observer 모두 인터페이스이며, 이를 구현하는 Concrete Class로 구성된다.

![observer pattern structure]( /resource//3937a8f8-7b27-4252-bea3-b18f6861158d.png )

- Subject
	- 구독할 주체
	- Observer(감시자)를 추가/제거 하거나, 상태 변경 시 감시자에게 통지를 하는 역할을 한다.
- Observer
	- 구독할 주체가 변경됨에 따라 통지를 받는 주체. 주로 Subject가 Observer들의 목록들을 가지고 있다.

### 예제

아래 코드는 헤드 퍼스트 디자인 패턴의 코드를 바탕으로 내가 작성한 예시 코드이다.

블로그 글을 작성하면, 구독자들에게 메시지가 전달된다.

```java
interface Observer {
	void update(String lastPostName);
}

interface Subject {

	void notifyUpdateToObservers();

	void addObserver(Observer observer);
}

class MyBlog implements Subject {

	private final List<Observer> observers = new ArrayList<>();

	private String lastPostName;

	@Override
	public void notifyUpdateToObservers() {
		this.observers.forEach(o -> o.update(this.lastPostName));
	}

	@Override
	public void addObserver(Observer observer) {
		this.observers.add(observer);
	}


	public void writeNewPost(String name) {
		System.out.println("새 글 발행");
		this.lastPostName = name;

		this.notifyUpdateToObservers();
	}
}

class PostSubscriber implements Observer {

	private final Subject subject;
	private final String name;

	public PostSubscriber(Subject subject, String name) {
		this.subject = subject;
		this.subject.addObserver(this);

		this.name = name;
	}

	@Override
	public void update(String lastPostName) {
		System.out.println(this.name + "- 새 글이 발행되었습니다 : " + lastPostName);
	}
}

public class Main {
	public static void main(String[] args) {
		MyBlog myBlog = new MyBlog();
		new PostSubscriber(myBlog, "name1");
		new PostSubscriber(myBlog, "name2");

		myBlog.writeNewPost("옵저버 패턴이란");
	}
}
```

실행 결과는 다음과 같다.
```bash
> Task :Main.main()
새 글 발행
name1- 새 글이 발행되었습니다 : 옵저버 패턴이란
name2- 새 글이 발행되었습니다 : 옵저버 패턴이란
```

앞의 예제는 push 방식의 예제이며, pull 방식으로도 변경이 가능하다.

pull 방식이란 옵저버가 필요할 때마다 주제로부터 데이터를 당겨오도록 하는 방식이다.

pull 방식으로 코드를 변경하면 다음과 같다.

```java

import java.util.ArrayList;
import java.util.List;

interface Observer {
	void update();
}

interface Subject {

	void notifyUpdateToObservers();

	void addObserver(Observer observer);

	String getPostName();
}

class MyBlog implements Subject {

	private final List<Observer> observers = new ArrayList<>();
	private String lastPostName;

	@Override
	public void notifyUpdateToObservers() {
		this.observers.forEach(Observer::update);
	}

	@Override
	public void addObserver(Observer observer) {
		this.observers.add(observer);
	}

	@Override
	public String getPostName() {
		return this.lastPostName;
	}

	public void writeNewPost(String name) {
		System.out.println("새 글 발행");
		this.lastPostName = name;

		this.notifyUpdateToObservers();
	}
}

class PostSubscriber implements Observer {

	private final String name;
	private final Subject subject;

	public PostSubscriber(Subject subject, String name) {
		this.subject = subject;
		this.subject.addObserver(this);

		this.name = name;
	}

	@Override
	public void update() {
		System.out.println(this.name + " - 새 글이 발행되었습니다 : " + subject.getPostName());
	}
}

public class Main {
	public static void main(String[] args) {
		MyBlog myBlog = new MyBlog();
		new PostSubscriber(myBlog, "name1");
		new PostSubscriber(myBlog, "name2");

		myBlog.writeNewPost("옵저버 패턴이란");
	}
}
```

- update 메서드에서, subject에서 필요한 정보들을 getter를 통해 가져온다.

실행 결과는 다음과 같다.

```bash
> Task :Main.main()
새 글 발행
name1 - 새 글이 발행되었습니다 : 옵저버 패턴이란
name2 - 새 글이 발행되었습니다 : 옵저버 패턴이란
```

### 참고 자료

- GoF의 디자인 패턴
- 헤드 퍼스트 디자인 패턴
 
