---
layout: post
title:  "[TIL] System Programming Study - Day 7"
date:   2021-07-06 12:30:00
author: KimSeoYe
categories: TIL
tags:   TIL sysprog
cover:  "/assets/headers/mountain.jpg"
---
# fshare 2.0

## Description


- 서버와 클라이언트의 디렉토리는 모두 비어있다고 가정한다.
- 서버와 클라이언트 모두 파일 이름과 버전에 대한 메타데이터를 담은 linked list(+ hidden file)을 가지고 있다.


- 클라이언트쪽 디렉토리에 새로운 파일이 생길 경우 자동으로 서버에 `put` 한다. (using `inotify()`)
- 서버는 파일을 받아 버전 정보를 업데이트한 뒤, 해당 클라이언트에게 버전 넘버를 알려준다.
- 클라이언트는 서버로부터 받은 버전 넘버를 list에 저장한다.


- 클라이언트는 지속적으로 서버에 `list`를 요청한다.
- 서버는 파일의 이름과 버전 정보를 함께 넘겨준다.
- 클라이언트가 가진 파일의 버전과 서버의 버전이 다를 경우 서버로부터 해당 file과 버전 정보를 `get` 한다. (파일이 없는 경우도 해당된다.)


<br>

## TIL

**inotify**
   1. `inotify()`를 사용하여 파일이나 디렉토리(List of files)를 모니터링할 수 있다. watch list에 포함된 파일/디렉토리에 일어난 event는 `inotify_event` 구조체에 저장되는데, 그 구조는 다음과 같다.
      ```
       struct inotify_event {
           int      wd;       // Watch descriptor 
           uint32_t mask;     // Mask describing event 
           uint32_t cookie;   // Unique cookie associating related events (for rename(2)) 
           uint32_t len;      // Size of name field (4byte)
           char     name[];   // Optional null-terminated name 
       }
      ```
   2. `inotify`에는 모니터링할 수 있는 다양한 event가 존재한다. 대표적인 예로 IN_CREATE, IN_DELETE, IN_MODIFY, IN_MOVED_FROM, IN_MOVED_TO 등이 있다.<br>각 event들의 이름을 통해 알 수 있듯이, inotify는 해당 디렉토리 안에서 create된 파일과 밖에서 create되어 디렉토리 안으로 옮겨진 파일을 다르게 인식한다. (delete와 moved from의 경우도 마찬가지이다.)
   3. `inotify`가 vim 에디터로 파일을 수정할 때 만들어지는 hidden file들도 모니터링하기 때문에 나중에 따로 다뤄 주어야 한다.
   4. `inotify`의 event를 읽을 때에도 `read()`를 사용한다. buffer에는 inotify_event 구조체와 파일의 이름이 순차적, 연속적으로 저장된다.


**socket programming**

1. socket 프로그래밍에서 `recv()`는 상대편 프로그램이 `closesocket()`함수를 호출해 접속을 종료하면 0을 리턴한다 (정상종료 - normal close). 에러가 발생했을 경우에는 -1을 리턴한다.
2. Blocking I/O는 I/O 작업이 진행되는 동안 다른 작업을 중단한 채 대기하는 것을 말하며, Non-blocking I/O는 I/O를 요청한 다음 진행상황과 상관 없이 바로 결과를 반환하는 것을 말한다. (다른 작업을 중단하지 않음) `fcntl()`을 사용하여 socket을 Non-blocking socket으로 설정할 수 있다.

<br>

## 느낀점
- 기능이 반복해서 쓰이는 부분을 함수로 만들어 두니 정말 편했지만, 그 함수를 잘못 구현해 놓으니 디버깅에 한참이 걸린다. 처음 코드를 짤 때 가능한 경우의 수를 최대한 생각하며 제대로 하자 🥲
