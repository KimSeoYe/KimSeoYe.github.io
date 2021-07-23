---
layout: post
title:  "[TIL] xv6 Study - Day 2"
date:   2021-07-13 09:00:00
author: KimSeoYe
categories: TIL
tags:   TIL xv6 kernel
cover:  "/assets/headers/hgu1.jpeg"
---
# Lab 0. Tutorial

### 2. The Boot Loader

- ELF : Executable Linkable Format
- ELF binary
  - ELF header
  - program header table : load될 program section의 list를 담고 있다.
  - program sections : .text .rodata .data .bss ...

- VMA (Virtual Memory Address) : Link address. 해당 section이 load 되어야 하는 주소
- LMA (Load Memory Address) : Load address. 해당 section이 실행될 주소

- Boot sector의 load address와 link address는 같다 (0x7c00)
- Kernel의 load address와 link address는 다르다
  - load address : 0x00100000 - boot loader가 실제로 커널을 올리는 physical memory address (ROM BIOS의 바로 위)
  - link address : 0xf0100000 or 0x80100000 - 커널 코드가 실행될 곳

- `e_entry` in ELF header : 프로그램의 entry point의 link address