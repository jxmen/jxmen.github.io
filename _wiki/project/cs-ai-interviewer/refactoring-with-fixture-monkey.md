---
layout  : wiki
title   : Fixture Monkey로 테스트 픽스처를 쉽게 생성하고 리팩토링 해보자 
summary : 
date    : 2024-08-29 22:27:24 +0900
updated : 2024-09-01 21:46:54 +0900
tag     : test fixture-monkey
toc     : true
public  : true
parent  : [[/project/cs-ai-interviewer]]
latex   : false
resource: 1D083588-F665-472C-B351-C85891766FA3
---
* TOC
{:toc}

### 개요

Naver의 오픈소스인 [Fixture Monkey](https://github.com/naver/fixture-monkey)를 프로젝트에 적용하여 테스트 픽스처를 쉽게 생성하고, 기존 코드를 리팩토링 하는 과정을 소개하도록 하겠습니다.

### 기존 테스트 코드의 문제점

평소와 같이 테스트를 작성하던 날이였습니다. Spring AI를 사용중인 프로젝트에서 Prompt 메시지들을 생성하는 로직을 `PromptMessageFactory`로 추출하였고, 이에 대한 테스트 코드를 작성하려고 하였습니다.

Kotlin의 mocking 라이브러리인 `mockk`로 코드를 작성한 코드는 다음과 같은 형태였습니다.

```kotlin
it("이전 채팅이 있을 때 권한 부여, 질문에 대한 답변, 이전 답변, 사용자 답변 메시지 목록을 반환한다") {
	val command = mockk<CreateSubjectAnswerCommand>()
	val chat1 = mockk<Chat>()
	val chat2 = mockk<Chat>()
	val chat3 = mockk<Chat>()
	val chatContent1 = mockk<ChatContent>()
	val chatContent2 = mockk<ChatContent>()
	val chatContent3 = mockk<ChatContent>()

	every { command.answer } returns "이것은 답변입니다"
	every { command.chats } returns listOf(chat1, chat2, chat3)
	every { command.subject.question } returns "AI란 무엇인가요?"

	every { chat1.isAnswer() } returns false
	every { chat1.isQuestion() } returns true
	every { chat1.content } returns chatContent1
	every { chatContent1.message } returns "질문"
	every { chat2.isAnswer() } returns true
	every { chat2.isQuestion() } returns false
	every { chat2.content } returns chatContent2
	every { chatContent2.message } returns "답변"
	every { chat3.isAnswer() } returns false
	every { chat3.isQuestion() } returns true
	every { chat3.content } returns chatContent3
	every { chatContent3.message } returns "꼬리 질문"

	val result = PromptMessageFactory.create(command)

	result shouldHaveSize 5
	result[0] shouldBe UserMessage(PromptMessageFactory.grantInterviewerRoleMessage)
	result[1] shouldBe AssistantMessage(PromptMessageFactory.getAiAnswerContentFromQuestion(command.subject.question))
	result[2] shouldBe UserMessage(chatContent2.message)
	result[3] shouldBe AssistantMessage(chatContent3.message)
	result[4] shouldBe UserMessage(command.answer)
}

```

무언가 안좋은 신호가 있는 것 같지 않나요? 

가장 큰 문제점은 테스트를 작성하기 위해 굉장히 많은 양의 코드를 작성해야 한다는 것이라고 생각했습니다. 또한 내부 로직의 경우 다음과 같이 `isAnswer`, `isQuestion` 메서드 값에 따라 영향을 받습니다. 

```kotlin
// 내부 private 메서드 내용
private fun createMessagesFromBeforeChats(chats: List<Chat>): List<Message> {
	return chats.drop(1).map {
		when {
			it.isAnswer() -> UserMessage(it.content.message)
			it.isQuestion() -> AssistantMessage(it.content.message)
			else -> throw IllegalArgumentException("Unknown chat type")
		}
	}
}
```

저는 해당 부분에 대해서만 집중해서 테스트를 작성하고 싶으나, 내부 객체인 content, content.message 등 로직 검증에 중요하지 않은 것들까지 mocking해야 한다는 점 또한 문제라고 생각됩니다.

### Fixture Monkey를 통해 개선해보자.

Naver에서는 Fixture Monkey라는 테스트 픽스처를 쉽게 생성하기 위한 오픈소스를 제공합니다. 테스트 객체를 굉장히 쉽게 만들 수 있다는 점이죠! Chat이라는 클래스에 대한 테스트 객체를 만들고 싶다면 다음과 같이 작성할 수 있습니다:

```kotlin
val fixtureMonkey = FixtureMonkey.builder().plugin(KotlinPlugin()).build()
val chatFixture: Chat = fixtureMonkey.giveMeOne();
```

또한 필요한 프로퍼티만 지정하거나, 식을 넣을 수 있고 그 외에 값은 랜덤한 값을 채워준다는 특징이 있습니다.

다음은 fixture monkey로 작성한 코드입니다. chat은 ChatContent를 가지고 있고, 질문과 답변이냐에 따라 question, answer등의 타입으로 구분합니다. 그래서 다음과 같이 작성했습니다.

```kotlin
it("이전 채팅이 있을 때, 첫 채팅을 잘라낸 메시지 목록을 반환해야 합니다") {
	// given
	val questionContent =
		fixtureMonkey
			.giveMeBuilder<ChatContent>()
			.setExp(ChatContent::chatType, ChatType.QUESTION)
			.sample()
	val answerContent =
		fixtureMonkey
			.giveMeBuilder<ChatContent>()
			.setExp(ChatContent::chatType, ChatType.ANSWER)
			.sample()

	val firstQuestionChat = fixtureMonkey.giveMeBuilder<Chat>().setExp(Chat::content, questionContent).sample()
	val answerChat = fixtureMonkey.giveMeBuilder<Chat>().setExp(Chat::content, answerContent).sample()
	val questionChat = fixtureMonkey.giveMeBuilder<Chat>().setExp(Chat::content, questionContent).sample()
	val command =
		fixtureMonkey
			.giveMeBuilder<CreateSubjectAnswerCommand>()
			.setExp(
				CreateSubjectAnswerCommand::chats,
				listOf(firstQuestionChat, answerChat, questionChat),
			).sample()

	// when
	val result = PromptMessageFactory.create(command)

	// then
	result shouldHaveSize 5
	result[0] shouldBe UserMessage(PromptMessageFactory.grantInterviewerRoleMessage)
	result[1] shouldBe AssistantMessage(PromptMessageFactory.getAiAnswerContentFromQuestion(command.subject.question))
	result[2] shouldBe UserMessage(answerChat.content.message)
	result[3] shouldBe AssistantMessage(questionChat.content.message)
	result[4] shouldBe UserMessage(command.answer)
}

```

ChatContent들을 fixtureMonkey를 통해 타입이 지정된 값으로 생성하고, 채팅에 chatContent를 세팅하도록 하였습니다.

하지만 이 코드 역시 조금 아쉽습니다. Chat을 생성할 때부터 내부 Content에 대한 값을 지정할 수는 없을까요?

### 중첩된 property에 대해서도 정의하여 코드를 더 개선해보자

정말 다행히도 [중첩된 프로퍼티](https://naver.github.io/fixture-monkey/v1-0-0-kor/docs/plugins/kotlin-plugin/kotlin-exp/#%ec%a4%91%ec%b2%a9%eb%90%9c-%ed%94%84%eb%a1%9c%ed%8d%bc%ed%8b%b0%eb%a5%bc-%ec%b0%b8%ec%a1%b0)에 대해서 선언하는 것도 지원합니다.

그래서 다음과 같이 코드를 변경하였습니다.

```kotlin
it("이전 채팅이 있을 때, 첫 채팅을 잘라낸 메시지 목록을 반환해야 합니다") {
	// given
	val firstQuestionChat =
		fixtureMonkey
			.giveMeBuilder<Chat>()
			.setExp(Chat::content into ChatContent::chatType, ChatType.QUESTION)
			.sample()
	val answerChat =
		fixtureMonkey
			.giveMeBuilder<Chat>()
			.setExp(Chat::content into ChatContent::chatType, ChatType.ANSWER)
			.sample()
	val questionChat =
		fixtureMonkey
			.giveMeBuilder<Chat>()
			.setExp(Chat::content into ChatContent::chatType, ChatType.QUESTION)
			.sample()
	val command =
		fixtureMonkey
			.giveMeBuilder<CreateSubjectAnswerCommand>()
			.setExp(
				CreateSubjectAnswerCommand::chats,
				listOf(firstQuestionChat, answerChat, questionChat),
			).sample()

	// when
	val result = PromptMessageFactory.create(command)

	// then
	result shouldHaveSize 5
	result[0] shouldBe UserMessage(PromptMessageFactory.grantInterviewerRoleMessage)
	result[1] shouldBe AssistantMessage(PromptMessageFactory.getAiAnswerContentFromQuestion(command.subject.question))
	result[2] shouldBe UserMessage(answerChat.content.message)
	result[3] shouldBe AssistantMessage(questionChat.content.message)
	result[4] shouldBe UserMessage(command.answer)
}
```

question chat을 생성하는 부분이 중복되어, private method로 추출했습니다.

```kotlin
it("이전 채팅이 있을 때, 첫 채팅을 잘라낸 메시지 목록을 반환해야 합니다") {
	// given
	val firstQuestionChat = createQuestionChat(fixtureMonkey) // use private function
	val answerChat = createAnswerChat(fixtureMonkey) // use private function
	val questionChat = createQuestionChat(fixtureMonkey) // use private function
	val command =
		fixtureMonkey
			.giveMeBuilder<CreateSubjectAnswerCommand>()
			.setExp(CreateSubjectAnswerCommand::chats, listOf(firstQuestionChat, answerChat, questionChat))
			.sample()

	// when
	val result = PromptMessageFactory.create(command)

	// then
	result shouldHaveSize 5
	result[0] shouldBe UserMessage(PromptMessageFactory.grantInterviewerRoleMessage)
	result[1] shouldBe AssistantMessage(PromptMessageFactory.getAiAnswerContentFromQuestion(command.subject.question))
	result[2] shouldBe UserMessage(answerChat.content.message)
	result[3] shouldBe AssistantMessage(questionChat.content.message)
	result[4] shouldBe UserMessage(command.answer)
}

```

이렇게 변경이 완료되었습니다! 처음 코드와 비교했을때 굉장히 간결하게 변경되었습니다.

### 마무리

Fixture Monkey를 통해 더 빠르게 테스트를 작성하고, 로직 검증 시 중요한 부분에만 집중할 수 있게 되었습니다.

테스트 픽스처를 생성할 때 시간이 많이 소요되어 테스트 작성 시 걸림돌이 된다면, 사용해보시는 것을 추천합니다.

### Reference

- [Fixture Monkey GitHub](https://github.com/naver/fixture-monkey)
- [Fixture Monkey Docs](https://naver.github.io/fixture-monkey)
	
