---
layout: post
title:  "[TIL] xv6 Study - Day 8"
date:   2021-07-21 09:00:00
author: KimSeoYe
categories: TIL
tags:   TIL xv6 kernel
cover:  "/assets/headers/hgu1.jpeg"
---

# Lab 2. Scheduling

- process의 priority를 수정하거나 가져올 수 있는 system call을 추가했다.
  - 모든 팀원들의 test case에서 동작할 수 있도록 하기 위해 api를 통일하였다.
  ```
  int getpriority(int pid, int * prio) ;
  int setpriority(int pid, int prio) ;
  ```
- Round-Robin Scheduler를 Priority Scheduler로 바꿔 보았다.
  - context switch가 일어날 때 RUNNABLE process 중 priority 값이 가장 큰 process를 선택한다.
  - 동일한 priority를 가진 process들은 Round-Robin으로 스케줄링한다. 
  - 이를 위해 직전에 scheduling한 process를 기억하는 global variable을 만들어 사용한다. (cursor)
- 스케줄러가 의도대로 잘 동작하는지를 테스트하기 위한 다양한 시나리오를 고민해 보았다.