---
layout: post
title: "[Study] JQF"
subtitle: "https://github.com/rohanpadhye/JQF"
categories: Study
tags: [study,oss,research]
---

[https://github.com/rohanpadhye/JQF](https://github.com/rohanpadhye/JQF)

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

#### Setup & Install JQF
```bash
# Clone and build AFL
git clone https://github.com/google/afl && (cd afl && make)

# Set AFL directory location
export AFL_DIR=$(pwd)/afl

# Clone and build JQF
git clone https://github.com/rohanpadhye/jqf && jqf/setup.sh

### See usage of JQF-AFL
jqf/bin/jqf-afl-fuzz
```

#### Write a test driver
* JQF의 test driver는 JUnit 스타일 클래스로, 다음과 같은 annotation을 사용해야 한다.
  * `@RunWith(JQF.class)` : test class 위에 위치
  * `@Fuzz` : test method 위에 위치
* Test method는 `public void` 타입이어야 한다.
* JQF+AFL을 사용할 경우, test method은 딱 하나의 `InputStream` 타입 formal parameter를 가져야 한다.
* 작성한 test driver를 `javac`를 사용해 컴파일한다. 
  * 이 때 JQF와 모든 dependency들을 classpath에 포함시켜야 하는데, JQF는 이것을 `classpath.sh` 배쉬 스크립트를 통해 사용할 수 있도록 만들어 두었다.
* 예제
  * PngTest.java
    ```java
    import javax.imageio.ImageIO;
    import javax.imageio.ImageReader;
    import javax.imageio.stream.ImageInputStream;
    import java.io.FileOutputStream;
    import java.io.IOException;
    import java.io.InputStream;

    import edu.berkeley.cs.jqf.fuzz.Fuzz;
    import edu.berkeley.cs.jqf.fuzz.JQF;
    import org.junit.Assume;
    import org.junit.runner.RunWith;

    @RunWith(JQF.class)
    public class PngTest {

        @Fuzz /* JQF will generate inputs to this method */
        public void testRead(InputStream input)  {
            // Create parser
            ImageReader reader = ImageIO.getImageReadersByFormatName("png").next();

            // Decode image from input stream
            try {
                reader.setInput(ImageIO.createImageInputStream(input));

                // Bound dimensions to avoid OOM
                Assume.assumeTrue(reader.getHeight(0) <= 256);
                Assume.assumeTrue(reader.getWidth(0) <= 256);

                // Decode first image in the input stream
                reader.read(0); 
            } catch (IOException e) {
                // This exception signals invalid input and not a test failure
                Assume.assumeNoException(e);
            }
        }
    }
    ```
  * compile
    ```bash
    javac -cp .:$(jqf/scripts/classpath.sh) PngTest.java 
    ```

#### Fuzzing with jqf-afl-fuzz
```bash
jqf/bin/jqf-afl-fuzz -i $AFL_DIR/testcases/images/png/ PngTest testRead
```

### Document: JQF Maven Plugin ([link](https://github.com/rohanpadhye/JQF/wiki/JQF-Maven-Plugin))

### Document: Writing a JQF test ([link](https://github.com/rohanpadhye/JQF/wiki/Writing-a-JQF-test))
