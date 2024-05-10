---
layout : wiki
title : neovim + lazyvim + leetcode 플러그인으로 터미널에서 leetcode 문제를 풀어보자
summary : 
date : 2024-05-10 11:41:39 +0900
updated : 2024-05-10 11:41:39 +0900
tag : productivity nvim lazyvim leetcode
toc : true
public : true
parent : [[/productivity]]
latex : false
resource: B4BAAFD9-778F-4406-9DCA-3E6708A7E41C
---
* TOC
{:toc}

2024/05/10 글을 처음 작성하는 시점에서 나는 leetcode 문제를 터미널에서도 풀고있다. 지인에게 설정 방법을 공유하고 기록용으로 해당 글을 작성한다.

### neovim 설치하기

[neovim](https://github.com/neovim/neovim)은 vim을 fork하여 만든 오픈소스로, 기타 여러 플러그인을 깔지 않아도 vim에서 제공하는 기본 기능보다 더 준수한 기능들을 제공한다. 

특히 기존 vim을 설정하기 위한 언어인 `vimscript`를 내가 자세히 써보지 않았지만, vim 개발자들 사이에서는 그리 좋지 않은 언어라고 들었다.

이에 비해 neovim은 설정을 `lua` 스크립트 언어로 하기에 이에 대한 스트레스가 덜하다.

### lazyvim 설치하기

[lazyvim](https://github.com/LazyVim/LazyVim)은 neovim의 패키지 매니저, 설정 파일 관리 등의 다양한 기능을 지원하여 neovim을 더 쉽게 쓸 수 있게 해준다.

lazyvim을 설치하면 기존 `~/.config/nvim`에 있던 디렉토리는 `~/.config/nvim.bak`으로 복사된다.

### lazyvim에서 leetcode 플러그인 설치 - leetcode.nvim

(lazyvim외에 다른 플러그인 매니저 등을 통해 leetcode 플러그인을 테스트해 보지는 않았다.)

우선 [leetcode.nvim](https://github.com/kawre/leetcode.nvim) 문서에서도 lazyvim을 기반으로 설치하는 방법을 가이드하고 있어, lazyvim으로 설치하였다.

lazyvim의 경우 `~/.config/nvim/lua/plugins/` 폴더 내에 모든 플러그인 설정들을 읽어들인다고 한다. 나의 경우 플러그인마다 하나의 파일을 만들어서 관리하고 있다. [설정 파일](https://github.com/jxmen/dotfiles/blob/main/.config/nvim/lua/plugins/leetcode.lua) [^1]

lua 스크립트를 저장하고 nvim 명령어만 입력하면 lazyvim이 알아서 플러그인을 설치해줄 것이다.

### leetcode창 열기

기본 설정된 명령어로 `nvim leetcode.nvim` 명령어를 사용하면 이제 릿코드 UI가 나올 것이다! 여기까지 왔다면 이제 문제를 열심히 풀면 된다!

### (부록) nerd 폰트 설치하기

터미널에서 일부 지원하지 않는 아이콘이 있다면 깨짐 현상이 발생할 수 있다.

찾아보니 [Nerd 폰트](https://github.com/ryanoasis/nerd-fonts)가 개발자에게 많은 아이콘을 지원한다고 하여, nerd 폰트를 iterm2에 적용했다.

나의 경우 D2Coding 폰트를 평소 좋아하여 해당 글꼴을 다운받아서 적용했다.

![image]( /resource//82cc34ea-6e3d-4c14-98e9-f69565eed4b3.png )

### 쓰면서 느낀 아쉬운 점

이 부분은 이슈를 남기거나 직접 기능을 개발해도 좋을 것 같다.

- SQL 문제는 지원하지 않는 것 같다. 
  - 문제 목록에서 SQL로 분류된 문제가 검색되지 않는다. 
  - SQL이 오히려 터미널에서만 풀기에 적합하다고 생각했는데, 왜 지원하지 않는지는 찾아보아야 할 것 같다.
- Tree Visualizer 같은 부가적인 기능을 지원하지 않는다.
  - Tree 문제의 경우 릿코드 사이트에서는 배열만 입력해도 트리가 어떻게 구성되어 있는지 알려주는데, 이 부분이 없어서 조금 아쉽다.
- [leethub](https://chromewebstore.google.com/detail/leethub-v2/mhanfgfagplhgemhjfeolkkdidbakocm)처럼 문제를 풀었을 때 자동으로 github commit이 되는 기능을 지원하지 않는다.
  - 내가 요새 잔디심기에 관심이 있어, 이 기능이 없어서 좀 아쉽다.

### 각주
[^1]: 이 부분은 나중에 너무 파편화 되어있다고 판단되면 init.lua 등 하나의 파일로 옮기는 작업을 할 수 있습니다. 링크에 있는 파일 경로가 다를 시 제보 부탁드립니다.

