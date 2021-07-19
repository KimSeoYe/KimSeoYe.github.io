---
layout: post
title:  "[TIL] xv6 Study - Day 6"
date:   2021-07-19 09:00:00
author: KimSeoYe
categories: TIL
tags:   TIL xv6 kernel
cover:  "/assets/headers/mountain.jpg"
---

# Lab 1. System Calls

### Example w/ `wait()` system call

1. User program : call `wait()`
2. trapasm.S : build trap frame (stack switch) & call `trap()`
3. trap.c : trap frame에 저장된 trap number가 T_SYSCALL이면 call `syscall()`
4. syscall.c : syscall table에서 tf->eax에 저장된 SYS_wait을 index로 사용하여 system call handler function인 `sys_wait()`을 부른다
5. sys_wait : `argptr()`을 사용해 user stack에 저장된 argument 가져오기 & call `wait()`

<br>

# Lab 2. Scheduling

**Goals:** Understand how the scheduler works. Understand how to implement a scheduling policy and characterize its impact on performance. Understand priority inversion and a possible solution for it.


### Chapter 5. Scheduling (p.59~)

- xv6의 경우 scheduler도 자신의 stack을 가지고 있다.
- (p1: user mode > kernel mode) > p1 kernel > scheduler kernel > p2 kernel > (p2: kernel mode > user mode)
- Context switch를 할 때도 stack switch가 일어난다.
  - context register set들을 stack에 push한 뒤 stack pointer를 바꿔줌으로써 context switch를 implement한다.
  - stack pointer가 바뀌었으므로 그 다음 pop하는 값들은 새로운 context의 값들이다.
- Timer interrupt가 일어날 때마다 Context switch를 시작하는 함수를 호출한다. 

