---
layout: post
title:  "[TIL] xv6 Study - Day 7"
date:   2021-07-20 09:00:00
author: KimSeoYe
categories: TIL
tags:   TIL xv6 kernel
cover:  "/assets/headers/mountain.jpg"
---

# Lab 2. Scheduling

**Goals:** Understand how the scheduler works. Understand how to implement a scheduling policy and characterize its impact on performance. Understand priority inversion and a possible solution for it.

### System Call

`setpriority()` system call을 추가하기 위해서, 
- **proc.c**에 `setpriority(int pid, int val)`를 implement한다.
- **defs.h**에 setpriority의 정의를 추가한다.
- **sysproc.c**에 handler function `sys_setpriority()`를 추가한다. 이때, `argint()`를 사용해 argument들을 가져와 `setpriority()`를 실행한다.
- **syscall.h**에 새로운 system call number(SYS_setpriority)를 정의한다.
- **syscall.c**에 handler function을 정의하고, syscall table에 해당 function을 추가한다.
- **usys.S**에 `SYSCALL(setpriority)`를 추가한다.
- user program이 해당 system call을 사용할 수 있도록 **user.h**에 setpriority의 정의를 추가한다.
