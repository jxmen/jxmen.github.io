---
layout  : wiki
title   : 동시성 이슈
summary : 동시성 이슈와 문제 해결 사례 정리
date    : 2024-02-19 23:49:16 +0900
updated : 2024-02-20 01:31:27 +0900
tag     : 
toc     : true
public  : true
parent  : [[/cs]] 
latex   : false
resource: 3BCA78B6-7019-4FD0-AFAF-3CA574322837
---
* TOC
{:toc}

### 동시성 이슈란

동시성 이슈는 주로 멀티 스레드 환경에서 여러 스레드가 동시에 변경 가능한 데이터를 접근하고 변경할 때 데이터 불일치가 발생하는 현상을 의미한다.

주로 스레드에서 동기화되지 않은 데이터를 읽은 후 해당 값을 업데이트하는 경우에 발생한다.

선착순 이벤트를 시작했다면 사용자의 요청이 순간적으로 물밀듯이 쏟아질 것이다. 혹은 흔히 말하는 '따닥'으로 API 호출이 중복으로 발생되는 경우를 방지해야 한다. 특히 돈과 관련된 정보일수록 이러한 동시성 이슈에 대응하는 것은 중요할 것이다.

### 트랜잭션 격리 레벨

데이터베이스 트랜잭션 격리 레벨을 어떻게 설정하느냐에 따라 여러 동시성 이슈가 발생할 수 있다.

### Database Lock

데이터베이스 락을 통해서도 동시성 제어가 가능하다.

다만 락의 경우 데이터 일관성을 보장해 줄 순 있지만 트래픽이 늘어나면 성능 저하도 우려될 수 있다.

MySQL에서는 `select ... for update` 라는 구문을 통해 해당 행을 비관적 락(Optimistic Lock)을 걸도록 사용할 수도 있다.

그 외에 낙관적 락에 대해서도 존재한다.

### 스레드 동기화

데이터에 접근하거나 변경시에 해당 영역이나 메서드를 하나의 스레드만 접근 가능하도록 제어하는 것도 가능하다. Java의 경우 `synchronized` 키워드를 통해 제어한다.

```java
// 하나의 스레드만 메서드에 접근 가능
public synchronized void updateCount(int count) {
	this.count = count;
}
```

### 동시성 이슈 테스트

#### Java - Multi Thread

Java에서는 병렬 프로그래밍 지원을 위해 `java.util.concurrent`라는 패키지를 제공한다.

Java에서 여러 스레드를 만들고 스레드들이 동시에 작업을 수행하는 식으로 동시성 이슈를 테스트할 수 있다.

다음은 `ThreadPoolExecutor`를 사용한 예제이다.

```java
import org.junit.jupiter.api.Test;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

import static org.assertj.core.api.AssertionsForClassTypes.assertThat;

public class ExecutorServiceTest {

	@Test
	void test() throws InterruptedException {
		int nThread = 30;
		ExecutorService executor = Executors.newFixedThreadPool(nThread);
		Counter counter = new Counter();

		for (int i = 0; i < nThread; i++) {
			for (int j = 0; j < 100; j++) {
				 executor.execute(() -> { counter.increase(); });
			}
		}

		Thread.sleep(1000); // 메인 스레드가 종료되지 않도록 대기한다.

		assertThat(counter.getCount()).isEqualTo(30 * 100);
	}

	static class Counter {
		private int count = 0;

		public synchronized void increase() {
			count++;
		}

		public int getCount() {
			return count;
		}
	}
}
```

- 만약 Spring을 사용중이라면 Service레이어의 코드를 여러번 호출하여 내가 원하는대로 동작하는지 검증할 수 있을 것이다.

#### Jmeter를 통해 동시에 API 성능 테스트

[Jmeter](https://github.com/apache/jmeter)라는 오픈소스를 통해 Http 요청과 ThreadGroup을 만들고 이를 동시에 요청할 수 있도록 할 수 있다.

![jmeter example]( /resource//9776cd2d-8c9b-4fd9-b485-ddfcfe7ec219.png )

- 선착순으로 1명만 성공해야 하는 케이스 Test

