---
layout: post
title: "[Study] JQF+Zest"
subtitle: "Semantic Fuzzing for Java"
categories: Study
tags: [study,oss,research]
---

### [JQF](https://github.com/rohanpadhye/JQF)
* A feedback-directed fuzz testing platform for Java : JVM bytecode를 위한 AFL/LibFuzzer 같은 느낌
* Fuzz driver를 parametric JUnit test methods로 작성할 수 있음.
* Zest와 같은 coverage-guided fuzzing algorithm을 사용하여 [junit-quickcheck](https://github.com/pholser/junit-quickcheck) 스타일의 parameterized unit test를 실행할 수 있음
* `mvn jqf:fuzz` : JQF가 Zest를 실행 (default)
* 다음과 같은 pluggable fuzzing front-ends를 지원하는 모듈형 프레임워크
  * AFL => binary fuzzing
  * [Zest](https://rohan.padhye.org/files/zest-issta19.pdf) : semantically valid input을 생성하는 방향으로 coverage-guided fuzzing을 적용하는 알고리즘 => semantic fuzzing
  * PerfFuzz
  * PLCheck
  * BigFuzz

