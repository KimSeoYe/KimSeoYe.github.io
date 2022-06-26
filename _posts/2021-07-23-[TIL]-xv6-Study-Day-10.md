---
layout: post
title: "[TIL] xv6 Study - Day 10"
subtitle: "Lab 3. Memory Management"
categories: TIL
tags: [TIL,xv6,kernel]
---

# Lab 3. Memory Management

- Stack overflow로 인한 page fault가 일어나면 stack을 한 페이지씩 grow할 수 있도록 수정해 보았다.
  - Trap을 handling하는 코드에 page fault의 case를 추가했다.
  - Stack overflow로 인한 page fault가 맞는지를 확인하여 (using `%cr2`) 새로운 stack을 allocate한다.

# Additional. LKM Rootkit

- 특정 process의 kill을 막는 Loadable Kernel Module을 구현해 보았다.
  - 모듈의 init, exit function을 통해 커널에 모듈을 install, remove할 수 있게 했다.
  - 커널에 모듈을 install, remove하고 설치된 모듈의 list를 확인할 수 있게 되었다. (insmod, rmmod, lsmod)
  - Proc file system을 사용해 Kernel module과 user level program 사이의 communication을 구현했다.
  - 커널의 system call table에 접근하여 내가 만든 함수를 특정 system call 대신 실행할 수 있게 했다.