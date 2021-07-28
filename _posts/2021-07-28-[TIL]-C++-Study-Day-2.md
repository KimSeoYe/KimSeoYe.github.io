---
layout: post
title:  "[TIL] C++ Study - Day 2"
date:   2021-07-28 09:00:00
author: KimSeoYe
categories: TIL
tags:   TIL c++
cover:  "/assets/headers/hgu1.jpeg"
---

# Chapter 3. Reference Types

- C++ 의 references (using &)에 대해 공부했다.
- `const` 키워드에 대해 공부했다.
  - 함수의 argument를 const로 선언하면 함수의 scope 내에서 해당 argument의 수정을 막는다.
  - method를 const로 선언하는 것은 해당 method 내에서 current object의 state를 수정하지 않는다는 의미이다.
  - object의 member variable을 const로 선언하면, 해당 멤버 변수는 initialization 이후에는 수정할 수 없다.
- 객체의 멤버들을 초기화하기 위해 constructor에서 Member initializer lists를 사용할 수 있다.

<br>

# Chater 4. The Object Life Cycle

- 여러 object들의 storage duration에 대해 공부했다 : 여러 storage duration의 차이를 알고 사용하는 방법을 배웠다.
  - Automatic Storage Duration : Automatic object는 code block의 시작에서 allocate되고 끝에서 deallocate된다. (Ex. function parameters, local variables)
  - Static Storage Duration : Static object는 `static`이나 `extern` 키워드를 사용해 선언된 객체들으로, 프로그램이 시작할 때 allocate되고 프로그램이 끝날 때 deallocate된다.
    - Local Static Variable : 함수 안에서 static으로 선언된 변수, 함수의 첫 실행 때 allocate되고 프로그램이 끝날 때 deallocate된다.
    - Static Members : Class의 instance에 영향받지 않고 공유되는 member
  - Thread-Local Storage Duration : `thread-local` 키워드를 사용하며, 각 thread마다 thread-local 변수의 copy를 가지게 된다.
  - Dynamic Storage Duration : 요청에 의해 allocate / deallocate 된다.

- throw - catch 를 통해 다양한 execption을 handling하는 방법을 배웠다.