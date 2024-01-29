---
layout  : wiki
title   : Kotlin Koans 필요한 내용 요약
summary : Intellij에서 제공하는 Kotlin 튜토리얼 플러그인 
date    : 2024-01-29 11:02:01 +0900
updated : 2024-01-29 11:06:03 +0900
tag     : kotlin
toc     : true
public  : true
parent  : [[Kotlin]]
latex   : false
resource: EB558039-AC80-4BDF-9913-DDDE2CAC3D3D
---
* TOC
{:toc}

- 정규식을 선언할때도 큰따옴표 3개를 사용한다 `"""\d{2}"""`
- let 구문은 null이 아닐때만 괄호 안의 구문이 실행된다

```jsx
message?.let { mailer.sendMessage(email, message)} // message를 it으로도 가능
```
 
