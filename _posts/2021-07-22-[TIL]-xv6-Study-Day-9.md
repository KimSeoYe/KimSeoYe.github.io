---
layout: post
title: "[TIL] xv6 Study - Day 9"
subtitle: "Lab 3. Memory Management"
categories: TIL
tags: [TIL,xv6,kernel]
---

- xv6의 user memory layout을 바꿔보았다.
  - Original Layout : code & data와 heap 사이에 1 page size의 stack이 있다 (grow backword)
  - Modified Layout : stack이 kernel 바로 밑에 있다 (grow backword)
- Memory layout을 바꾸기 위해, 프로그램을 메모리에 load하고 page table을 setup하는 과정을 수정했다.
  - process의 virtual address space에서 stack의 끝과 heap의 끝을 추적하는 field를 추가했다.
  - stack이 할당되는 위치를 KERNBASE 바로 밑으로 수정하였다.
  - 새로운 process를 fork할 때 virtual address space를 copy하는 함수를 수정했다. (stack 영역을 따로 copy하도록)
- xv6의 virtual address와 page table 구조를 이해하였다.
  - xv6가 새로운 page table을 할당하고 address를 mapping하는 과정을 이해하였다.
  - 32bit virtual address : 10-bit page directory index / 10-bit page table index / 12-bit offset (flags)