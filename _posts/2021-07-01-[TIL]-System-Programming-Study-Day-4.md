---
layout: post
title:  "[TIL] System Programming Study - Day 4"
date:   2021-07-01 12:30:00
author: KimSeoYe
categories: TIL
tags:	TIL sysprog
cover:  "/assets/headers/hgu1.jpeg"
---
# fshare 1.0 : Simple File Sharing System

### Description
> **Background Study**<br>
> Q1. Change server.c and client.c to use multithreading instead of multiprocessing<br>
> Q2. Change server.c such that it regards the message received from a client as the name of a text file and transfers the text data of the file in the local directory back to the client.<br><br>
> **fshare - Command Line Interface**<br>
> - fshared (server) : `./fshared -p <port_num> -d <dir_name> &`
> - fshare (client) : `./fshare <ip_addr> <command> (<file_name>)`<br>
>     - commands : `list`, `get`, `put` <br>


### TIL
1. 스터디 초반에 공부했던 개념들(바이너리로 읽고 쓰기, directory나 file을 다루는 방법 등)과 TCP/IP를 함께 사용하며 간단한 file server를 만들기 시작했다. 새로운 학습과 복습이 병행되는 경험이었다.
2. 지금 우리가 개발하고 있는 것은 blocking 버전의 TCP 프로그램으로, 서버와 클라이언트가 각각 한번씩 데이터를 주고 받는다. 데이터를 모아뒀다가 한번에 보내기 때문에 속도가 빠르며, 파일 전송에는 blocking을 사용한다. 
3. 채팅처럼 한번 소켓을 연결한 후 여러 번 데이터를 전송하고 싶을 땐 다른 TCP 모드를 사용해야 한다. (non-blocking, no delay)
4. Multithreads 버전으로 서버와 클라이언트를 구현할 때, `accept()`를 통해 만들어진 새로운 소켓의 file descriptor의 address를 새로운 thread에 직접 넘기는 것은 안전하지 않다 - 다른 thread에 의해 override될 위험이 있다.
5. C언어의 `void *` 는 컴파일러가 type check를 하지 않는다.

### 느낀점
1. Command Line Interface를 개발하며 예외 처리를 할 때, `<ip address>:<port number>` parameter가 올바른 형식으로 입력되었는지를 어떻게 확인할 수 있을지가 고민이었다. 여러 아이디어를 떠올리던 중 정규표현식을 사용할 수 있지 않을까 라는 생각을 했는데, 아직 정규 표현식을 제대로 공부해본 적이 없어 아쉬웠다.
2. **문제를 정확히 파악하는 것은 정말 중요하다.** 문제를 꼼꼼히 읽으며 내가 구현해야 할 프로그램의 흐름을 이해하고 시작했을 때 훨씬 효율적이고 정확한 프로그래밍을 할 수 있었다.

