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