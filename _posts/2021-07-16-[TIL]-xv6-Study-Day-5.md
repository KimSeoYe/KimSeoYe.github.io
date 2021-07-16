---
layout: post
title:  "[TIL] xv6 Study - Day 5"
date:   2021-07-16 09:00:00
author: KimSeoYe
categories: TIL
tags:   TIL xv6 kernel
cover:  "/assets/headers/mountain.jpg"
---
# Lab1. System Calls

### Code : Assembly trap handlers (textbook p.42)

- int : processor가 trap을 발생시키도록 하는 instruction (x86에는 256개의 interrupt가 있다.)
- xv6는 trap이 일어났을 때 x86 하드웨어가 stack switch를 하도록 한다.
  - 하드웨어가 stack segment selector와 new value for esp(esp가 이제 어디를 point해야 하는지)를 load한다.
  - switchuvm : user process의 kernel stack의 top address를 task segment descriptor에 저장한다.
- Todo. 'stack segment selector'와 'task segment descriptor'에 대한 정보 더 찾아보기
  
<br>

**more description about trap & stack switch**
1. trap이 일어났을 때, 첫 번째로
  - process가 user mode에서 실행되고 있었을 경우 : task segment descriptor로부터 %esp(stack pointer reg.)와 %ss(stack segment reg.) 를 load >> 새로운 stack에 넣는다.
  - process가 kernel mode에서 실행되고 있었을 경우 : 아무 일도 하지 않는다.
2. processor가 eflag, cs, eip register를 push한다.
  - 몇몇 trap에 대해서는 error word를 push하기도 한다.
3. eip와 cs register를 해당하는 IDT entry로부터 load한다.
  - 각 entry가 error code, interrupt number를 push하고, alltraps로 jump한다.
4. alltraps가 ds, es, fs, gs, 범용 레지스터들을 push한다.

이 과정을 통해 kernel stack이 trap이 일어났을 때의 processor register 정보들을 담은 stack frame을 포함하게 된다.

<br>

### Code : System calls (textbook p.45)

`argint`, `argptr`, `argstr`, `argfd` : n번째 system call argument에 접근할 수 있도록 돕는 함수들
- trap frame에 저장된 user-space %esp 는 system call의 return address를 담고 있다.
- system call의 argument들은 그 바로 위에 있다. (%esp + 4)
- 따라서 n번째 argument는 %esp + 4 + 4*n 에 위치해 있다.

