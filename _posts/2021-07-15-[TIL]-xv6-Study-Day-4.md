---
layout: post
title:  "[TIL] xv6 Study - Day 4"
date:   2021-07-15 09:00:00
author: KimSeoYe
categories: TIL
tags:   TIL xv6 kernel
cover:  "/assets/headers/mountain.jpg"
---
# Lab1. System Calls

### Deeper Understanding - execption and system call

- xv6 커널에 event가 일어나면 HW가 kernel mode로 전환하여 trap을 처리하도록 한다.
- trap frame : user stack을 가리키는 pointer 등 중요한 argument들을 trap handler에게 넘겨주기 위해 사용되는 data structure이다.

**syscall**
- `syscall()` : 모든 system call들에 대한 top level handler.
  - trap frame에 저장된 eax register(from user code)로부터 system call number를 얻어, system call handler table의 index로 사용한다.
  - system call의 return value도 trap frame의 eax register에 저장한다.
  - 이것이 kernel과 user program이 argument를 주고 value를 받는 방식이다.
- 각 system call에 대한 hadler들은 그 기능에 따라 각각 다른 곳에 구현되어 있다.
  - `sys_xyz()` 로 naming 되어있다. (xyz: system call의 이름)
  - 이 handler가 해당 system call의 실제 implementation이 있는 함수(ex. `kill()`)를 호출한다.
  - `sysproc.h`에는 이런 handler들이 trap frame에 저장된 정보를 사용해 user stack으로부터의 파라미터들을 가져올 수 있도록 돕는 함수들이 정의되어 있다. (`argint()`, `argstr()`, ...)

<br>

### 느낀점
- 어제 내내 무엇을 먼저 공부해야 할지 감을 잡지 못하고 헤맸었는데, 문제의 의도와 내가 뭘 모르는지를 제대로 파악하지 못했기 때문인 것 같았다.
- 그래서 오늘은 Lab1에서 내가 해야 하는 것이 무엇인지, 이 문제를 해결하기 위해 어떤 것들을 알아야 하는지를 먼저 정리하며 큰 그림을 그리려고 노력해 보았다. 시간이 오래 걸리는 것 같아 조급해지기도 했지만 문제와 요구사항, 힌트 등을 끝까지 하나하나 짚어보고 나니 내가 지금 모르고 있는 것이 무엇인지, 무엇을 더 공부해야 하는지, 이 문제에 어떻게 접근해야 할지 방향을 잡을 수 있었다.