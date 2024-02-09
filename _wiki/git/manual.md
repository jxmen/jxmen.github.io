---
layout  : wiki
title   : Git 명령어 & 매뉴얼
summary : 명령어 기록용
date    : 2024-02-10 00:56:07 +0900
updated : 2024-02-10 06:27:24 +0900
tag     : git 
toc     : true
public  : true
parent  : [[/git]]
latex   : false
resource: 00BACC0F-BF24-4773-BB9C-C23984AE8D70
---
* TOC
{:toc}

### rm

```bash
git rm -r --cached .
```

파일을 삭제할 때 사용한다.

- `-r` : 디렉토리
- `--cached` : 로컬에서만 삭제

### config

git에서 커스텀으로 사용할 옵션들을 설정한다.

- `core.ignorecase` :
	```bash
	git config core.ignorecase false
	```

	- 대소문자 구분을 무시할지 여부를 설정한다.
	- false로 설정할 경우, 파일의 대소문자가 변경되었다면 commit의 대상이 된다.

### branch

작업중인 브랜치에 대한 명령어를 수행한다.

- `--show-current`

	```bash
	git branch --show-current
	```
	- 현재 브랜치 이름을 출력한다.

- `--edit-description`
	- 로컬에 깃 브랜치에 대한 설명을 적을 수 있다.
	- 보통 Jira를 사용하면 브랜치 이름을 지라 티켓 번호로 사용하는데, 이 명령어를 사용하면 간단하게 작업할 내용을 메모할 수 있어 유용하다. [^1]
		
	![git branch --edit-description]( /resource//1a6f0329-d521-48fe-bd32-f4e2467f04d7.png )

### 각주

[^1]: <https://x.com/hwidongsuh/status/1755835588723585128?s=20>
