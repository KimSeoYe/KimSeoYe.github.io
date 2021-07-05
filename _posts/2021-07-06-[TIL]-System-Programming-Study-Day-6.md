---
layout: post
title:  "[TIL] System Programming Study - Day 6"
date:   2021-07-02 12:30:00
author: KimSeoYe
categories: TIL
tags:   TIL sysprog
cover:  "/assets/headers/mountain.jpg"
---
# Simple Chatting Program

<br>

### TIL

- `setsocketopt()`를 사용하여 non-blocking I/O를 구현할 수 있다.
    ```c
    int option = 1 ;
    setsockopt(new_socket, SOL_TCP, TCP_NODELAY, &option, sizeof(option)) ;
    ```
- option을 1로 주는 것은 Nagle 알고리즘(가능하면 조금씩 자주 보내지 말고 한번에 많이 보낸다)을 off 한다는 의미이다. (0일 경우 Nagle 알고리즘 on -> Default)
- TCP_NODELAY로 socket의 옵션을 설정하면 write stream을 shutdown하지 않아도 데이터가 전송된다. buffering을 하지 않고 바로 데이터를 주고 받으므로 보내진 데이터가 없어도 계속 recv를 하고 있어야 해 비효율적이지만 간단한 채팅 서버를 구현하는 데는 적합하다.
- `send()`를 할 때 connection에 문제가 생긴 경우 SIGPIPE signal을 보내 handler가 따로 없을 경우 프로세스가 종료되지만, MSG_NOSIGNAL 옵션을 사용하여 signal을 보내지 않도록 설정할 수 있다. 이런 경우 `send()`에 문제가 있을 경우 -1을 리턴한다.
- 서버나 클라이언트 모두 메세지를 한번에 완전히(끝까지) 받지 못할 가능성이 있으므로, 메세지를 보내기 전에 메세지의 길이를 먼저 보내 체크할 수 있도록 하면 안전하다.

<br>

### 느낀점
- 오늘 Deadlock을 직접 경험해 보았다(...) 교과서에서 읽었던 상황을 직접 마주해보니 신기하기도 했고, 대충 생각하며 코딩하면 실수하기 딱 좋겠구나 하는 생각이 들었다.
- 오늘따라 유난히 디버깅에 걸린 시간이 길었던 것 같다. 돌이켜보니 역시 귀찮음 때문에 논리를 완벽히 정리하고 시작하지 못했던 것이 이유였다. 