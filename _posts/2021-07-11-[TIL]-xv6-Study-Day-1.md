---
layout: post
title:  "[TIL] xv6 Study - Day 1"
date:   2021-07-11 09:00:00
author: KimSeoYe
categories: TIL
tags:   TIL xv6 kernel
cover:  "/assets/headers/mountain.jpg"
---
# Lab0. Tutorial

### 1. Physical Address Space

- GNU 소프트웨어 시스템을 위한 기본 디버거인 gdb(GNU Debugger)를 사용해 보았다.
  - b : breakpoint
  - x/Ni ADDR : 메모리에서의 instruction 실행을 확인할 수 있다. (N: 확인할 instruction의 개수)
  - c : continue
  - si : step instruction
- 초기의 PC는 1MB의 physical memory만 사용할 수 있었다. 
  - 640KB Low Memory : RAM
  - 384KB : reserved by HW (non-volatile)
    - BIOS : Basic Input Ouptut System
  - 사용할 수 있는 메모리의 크기가 확장되면서, 이전의 SW들과의 호환성 유지를 위해 RAM 영역이 640KB Low memory와 Extended memory 영역으로 구분되었다.


**ROM BIOS**
1. Interrupt descriptor table을 셋업한다.
2. PCI bus와 여러 device들을 initialize한다.
3. bootable device를 search한다.
4. Boot sector를 메모리에 load하여 boot loader를 읽는다.
5. 컨트롤을 boot loader에게 넘겨준다.

<br>

### 2. The Boot Loader

- Boot sector : bootable disck의 첫번째 sector로, boot loader의 코드가 들어있다.
- Boot loader
    1. 프로세서를 real mode에서 32-bit protected mode로 전환한다.
    2. hard disk로부터 커널을 읽어온다.


