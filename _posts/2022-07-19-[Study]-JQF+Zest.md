---
layout: post
title: "[Study] JQF+Zest"
subtitle: "Semantic Fuzzing for Java"
categories: Study
tags: [study,oss,research]
---

## [JQF](https://github.com/rohanpadhye/JQF)

### Overview
* A feedback-directed fuzz testing platform for Java : JVM bytecode를 위한 AFL/LibFuzzer 같은 느낌
* Fuzz driver를 parametric JUnit test methods로 작성할 수 있음.
* Zest와 같은 coverage-guided fuzzing algorithm을 사용하여 [junit-quickcheck](https://github.com/pholser/junit-quickcheck) 스타일의 parameterized unit test를 실행할 수 있음
* `mvn jqf:fuzz` : JQF가 Zest를 실행 (default)
* 다음과 같은 pluggable fuzzing front-ends를 지원하는 모듈형 프레임워크
  * AFL: binary fuzzing ([tutorial](https://github.com/rohanpadhye/jqf/wiki/Fuzzing-with-AFL))
  * [Zest](https://rohan.padhye.org/files/zest-issta19.pdf): semantic fuzzing
  * PerfFuzz: complexity fuzzing
  * PLCheck: reinforcement learning
  * BigFuzz: Apache Spark fuzzing

### Example
* ([Apache Commons Collection](https://commons.apache.org/proper/commons-collections/)) [PatriciaTrie](https://commons.apache.org/proper/commons-collections/apidocs/org/apache/commons/collections4/trie/PatriciaTrie.html) 클래스의 property를 테스트하기 위한 JUnit-Quickcheck test
  * PracticalTrie가 input JDK Map으로 초기화 되어 있는지
  * Input map이 이미 하나의 key를 포함하고 있으면, 해당 key가 새로 생성된 PracticalTrie에 존재해야 함
```java
@RunWith(JQF.class)
public class PatriciaTrieTest {

    @Fuzz  /* The args to this method will be generated automatically by JQF */
    public void testMap2Trie(Map<String, Integer> map, String key) {
        // Key should exist in map
        assumeTrue(map.containsKey(key));   // the test is invalid if this predicate is not true

        // Create new trie with input `map`
        Trie trie = new PatriciaTrie(map);

        // The key should exist in the trie as well
        assertTrue(trie.containsKey(key));  // fails when map = {"x": 1, "x\0": 2} and key = "x"
    }
}
```  
* `mvn jqf:fuzz` : 자동 생성된 map과 key 값을 사용해 testMap2Trie()를 반복 호출
  
### Tutorial: Fuzzing with AFL ([link](https://github.com/rohanpadhye/jqf/wiki/Fuzzing-with-AFL))


### Document: JQF Maven Plugin ([link](https://github.com/rohanpadhye/JQF/wiki/JQF-Maven-Plugin))

### Document: Writing a JQF test ([link](https://github.com/rohanpadhye/JQF/wiki/Writing-a-JQF-test))
