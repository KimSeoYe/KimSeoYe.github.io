---
layout: post
title:  "[TIL] xv6 Study - Day 3"
date:   2021-07-14 09:00:00
author: KimSeoYe
categories: TIL
tags:   TIL xv6 kernel
cover:  "/assets/headers/mountain.jpg"
---
# Lab0. Tutorial

### 3. The Kernel

- entry가 CR0_PG를 %CR0에 넣으면 paging hardware가 enable된다.
- 커널을 high memory에 mapping하기 위해 stack pointer를 initialize한다.<br>
  `movl $(stack + KSTACKSIZE), %esp`
  - 여기서 stack은 kernel.sym에 0x8010a5c0으로 선언되어 있으며, KSTACKSIZE는 4096으로 선언되어 있다. 합한 값이 high addr.이므로, entry의 실행이 끝나고 low mapping이 사라져도 stack이 valid할 수 있게 된다.
- 과정을 마치면 main으로 jump한다.

**The stack**
- esp : stack pointer, 현재 사용되는 stack의 가장 낮은 지점을 point한다.
- ebp : base pointer, SW convention과 관련이 있다.
  - C function의 entry에서 함수의 prologue code가 직전 함수의 ebp를 stack에 저장한다.
  - 현재의 esp를 ebp에 copy한다. (새로운 base pointer)
  - 저장된 ebp 값들을 활용하면 nested call을 backtrace할 수 있다.