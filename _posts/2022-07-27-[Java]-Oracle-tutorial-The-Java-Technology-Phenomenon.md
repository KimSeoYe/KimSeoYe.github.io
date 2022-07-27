---
layout: post
title: "[Java] Oracle tutorial: The Java Technology Phenomenon"
subtitle: "https://docs.oracle.com/javase/tutorial/getStarted/intro/index.html"
categories: Study
tags: [study,java]
---

### The Java Programming Language

Java에서 모든 소스코드는 `.java` 확장자를 가진 plain text 파일로 작성된다. 이 소스코드들은 `javac` 컴파일러에 의해 `.class` 파일들로 컴파일된다. 클래스 파일은 프로세서 고유의 (native to your processor) 코드가 아닌 bytecodes로 이루어져 있다. 여기서 bytecode란 JVM(Java Virtual Machine)을 위한 기계어를 말한다. `java` 런쳐 툴은 JVM의 instance를 가지고 어플리케이션을 실행한다. 클래스 파일이 JVM 위에서 실행되기 때문에, Java 어플리케이션은은 platform-dependent하지 않다.

![image](https://user-images.githubusercontent.com/47961698/181164977-13c6589d-bf77-4f74-b41a-c01e2c9e693d.png)

![image](https://user-images.githubusercontent.com/47961698/181165071-c9b159da-0510-4eef-b18a-fa1e3e76ac98.png)


### The Java Platform

플랫폼이란 프로그램이 실행되는 하드웨어나 소프트웨어 환경을 의미하며, 대표적인 예로 Microsoft Windows, Linux, Solaris OS, Mac OS 등이 있다.

대부분의 플랫폼은 OS와 하드웨어의 조합으로 설명되지만, 자바 플랫폼은 hardware-based 플랫폼 위에서 실행되는 software-only 플랫폼이라는 점에서 다른 것들과 차이가 있다.

자바 플랫폼은 두가지 컴포넌트로 구성된다.
* JVM (Java Virtual machine)
* Java API (Application Programming Interface)

JVM은 자바 플랫폼의 기초이며, 다양한 hardware-based 플랫폼에 이식된다.

API는 ready-made 소프트웨어 컴포넌트들의 큰 집합으로, 연관된 클래스들과 interface들이 라이브러리로 묶여있다. 이 라이브러리를 package라고 한다.