---
layout  : wiki
title   : tcpdump 
summary : tcp 패킷 캡처 명령어 
date    : 2024-02-29 17:48:01 +0900
updated : 2024-08-11 18:22:10 +0900
tag     : tcp 
toc     : true
public  : true
parent  : [[/commands]] 
latex   : false
resource: 52A1BEFB-32CE-41AA-A5AE-22940E6E50F0
---
* TOC
{:toc}

### tcpdump

> Dump traffic on a network. [^1]

tcpdump는 네트워크상에서 패킷을 캡처할 수 있도록 해주는 cli 명령어이다.

### 옵션

- `-i`: 인터페이스
	- localhost의 경우 `lo0`을 지정한다.
- `-w`: 쓸 파일명 지정
- `-vv`: 더 자세하게 출력. HTTP response 등도 같이 확인이 가능하다.

### 같이 쓰면 좋은 명령어

- ifconfig
  - 네트워크 인터페이스 목록을 볼 때 사용한다.

### 예제

`localhost:8080`의 패킷 `packat.pcap` 파일에 남기기

```bash
tcpdump -i lo0 -w packat.pcap port 8080
```

tcpdump로 생성한 파일 읽기

```bash
tcpdump -qns 0 -X -r packat.pcap
```

더 상세하게 보기 `-vv`
```bash
tcpdump -i ens5 -vv port 80
```

### dump한 파일 프로그램으로 간편하게 보기

파일을 cli 상에서 읽기에는 불편하다. [wireshark](https://wireshark.org)이라는 GUI 프로그램으로 패킷을 더 쉽게 보고 분석할 수 있다.

tcpdump 명령어로 분석한 `.pcap` 확장자의 파일을 해당 프로그램에 업로드하여 패킷을 보는 것이 가능하다.

### 주석

[^1]: `tldr tcpdump`

