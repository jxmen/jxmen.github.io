---
layout  : wiki
title   : tmux 설정 및 단축키
summary : 자꾸 까먹어서 기록용으로 남기는 문서
date    : 2024-02-11 02:28:26 +0900
updated : 2024-02-11 02:52:50 +0900
tag     : command tmux 
toc     : true
public  : true
parent  : [[/commands]] 
latex   : false
resource: 7CB9400B-6718-49F6-AF9D-932A3182BBF5
---
* TOC
{:toc}

## tmux란

```bash
> tldr tmux

tmux

Terminal multiplexer.
It allows multiple sessions with windows, panes, and more.
See also: `zellij`, `screen`.
More information: <https://github.com/tmux/tmux>.
```

Terminal multiplexer의 약자이다. 화면 분할이나 세션 기능들을 제공한다.

## 자주 쓰는 명령어

tmux의 대부분의 명령어는 `ctrl-b`와 조합하여 이루어진다.

- `<ctrl-b> :` : 명령어 입력

### 화면 분할

- `<ctrl-b> %` : 수직 분할
- `<ctrl-b> - "` : 수평 분할
- `<ctrl-b> 방향키` : 화면 이동

###  화면 크기 조절 & 바인딩

화면 아래쪽으로 이동 시 `resize-pane -D` 명령어를 입력하면 아래쪽으로 이동하게 된다.

- 상하좌우 각각 (UDLR)

하지만 이렇게 명령어를 매번 치는 것은 상당히 귀찮은 일이다.

tmux.conf 파일에 다음과 같은 설정을 추가하자.

```bash
bind -r h resize-pane -L 5
bind -r j resize-pane -D 5
bind -r k resize-pane -U 5
bind -r l resize-pane -R 5
```
- bind 중 -r을 사용하여, 창 크기를 지속적으로 조정할 수 있다.

이제 tmux로 진입 후, tmux.conf 파일을 불러와서 실행하도록 명령어를 입력해주자.

```bash
tmux source-file ~/.tmux.conf
```

## 참고 자료 

- <https://ko.linux-console.net/?p=15151>

