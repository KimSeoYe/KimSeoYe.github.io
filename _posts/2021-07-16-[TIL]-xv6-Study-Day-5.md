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

### Code : System calls (textbook p.45)

- `argint`, `argptr`, `argstr`, `argfd` : n번째 system call argument에 접근할 수 있도록 돕는 함수들
  - trap frame에 저장된 user-space %esp 는 system call의 return address를 담고 있다.
  - system call의 argument들은 그 바로 위에 있다. (%esp + 4)
  - 따라서 n번째 argument는 %esp + 4 + 4*n 에 위치해 있다.

