---
layout: post
title:  "[TIL] System Programming Study - Day 3"
date:   2021-06-30 12:30:00
author: KimSeoYe
categories: TIL
tags:   TIL sysprog
cover:  "/assets/headers/mountain.jpg"
---
# Understanding the basics of TCP/IP & socket api 

### Description
> Preparing to implement fshare 1.0: Simple File Sharing System
> - Study the basics of the TCP/IP networking with a simple server-client example
>   - [server.c](https://github.com/KimSeoYe/OperatingSystem/blob/sysprog/IPC/server.c) 
>   - [client.c](https://github.com/KimSeoYe/OperatingSystem/blob/sysprog/IPC/client.c)
> - Have a Q&A session with the teammates after personally understanding the code.

### TIL
1. TCP와 UDP의 차이는 데이터를 전송한 뒤 그것을 온전히 받았는지를 guarantee 하는지의 여부에 있다. TCP의 경우 데이터를 끝까지 다 recieve 했는지를 체크하여 다 받을 때 까지 반복적으로 보내는 반면, UDP의 경우 데이터가 완전히 전송되지 않을 수도 있다. 
   - 따라서 TCP에서는 **데이터가 끝까지 전송되지 않을 수도 있다**는 사실을 항상 염두에 두고, **완전히 전송될 때까지 send 해야 한다.**
   - 네트워크 통신에서는 데이터가 제대로 전송되는 것이 신기한 일이라고 한다. 그만큼 도중에 데이터가 유실될 수 있는 경우의 수가 매우 많다고...
2. `bind()`는 해당 프로세스가 만든 pipe를 port와 연결해주는 함수이다. 즉 'port를 점유한다'는 의미로 이해할 수 있다.
3. client에서는 `bind()`를 할 필요가 없다. 즉, port를 점유할 필요 없이, server가 점유한 port에 `connect()` 하면 된다.
4. 네트워크를 통해 데이터를 전송할 때 `shutdown()`의 역할이 매우 중요하다.  데이터를 `send()`한 후 출력 스트림을 `shutdown()` 하지 않으면 데이터를 buffer에만 가지고 있을 수도 있다. 따라서 `shutdown()`을 함으로써 `send()`가 끝났다는 것을 알려주어야 한다.
5. Multiple communication이 가능하게 하기 위해, 그리고 새롭게 들어오는 client에 대한 빠른 response를 보장하기 위해 server는 `listen()`에 성공한 후 그 port를 직접 이용하지 않는다. `accept()`를 사용해 새로운 socket을 만들어 그 socket을 child process 혹은 thread가 이용하게 한다. 즉, 새로운 connection을 만들어 준다. (내부적으로는 port가 하나 새로 생긴다고 한다.) 이렇게 함으로써 server는 빠르게 다른 client를 기다리기 위해(`listen`) 돌아갈 수 있다.

        
