---
layout: post
title: "[OSS] JQF+AFL Overall Architecture"
subtitle: "https://github.com/rohanpadhye/JQF"
categories: Study
tags: [study,oss,research]
---

## JQF+AFL 코드 리뷰

AFL <-named pipe, shm-> proxy <-named pipe-> JQF (w/ target)

JQF는 c/c++ 프로그램 대상인 AFL이 java 프로그램을 fuzzing할 수 있도록 구현해 둔 어댑터 같은 것이다.

## Key Scripts & main

**1. [/bin/jqf-afl-fuzz](https://github.com/rohanpadhye/JQF/blob/master/bin/jqf-afl-fuzz)**

* argument를 받고 여러 세팅을 해준 후, pilot run을 실행하여 타겟 클래스와 메소드가 JQF fuzz target으로 잘 작성되었는지 확인한 수 afl-fuzz를 실행하는 스크립트
* jqf-afl-fuzz의 각종 옵션들을 확인할 수 있다
* [#179](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/bin/jqf-afl-fuzz#L179) : afl-fuzz 실행 부분
    ```bash
    ...
    27  AFL_FUZZ="$AFL_DIR/afl-fuzz"
    ...
    130  class="$1"
    131  method="$2"
    132  target="$BIN_DIR/jqf-afl-target"
    ...
    179  exec "$AFL_FUZZ" $afl_options "$target" $target_options "$class" "$method" @@
    ```
    * target, 즉 jqf-afl-target으로 afl-fuzz를 실행하는 것을 알 수 있다
    * Q. target class와 method가 argument로 들어가는데, 어떻게 처리하고 있는지 확인해 보아야 할 것 같다

**2. [/bin/jqf-afl-target](https://github.com/rohanpadhye/JQF/blob/master/bin/jqf-afl-target)**

* afl-fuzz의 타겟 프로그램으로 사용되는 스크립트
* [#17](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/bin/jqf-afl-target#L17) : main class를 정의한다
* [#52-58](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/bin/jqf-afl-target#L52-L58) : JQF main과 proxy 간 통신을 위한 named pipe를 설정해준다
* [#60-66](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/bin/jqf-afl-target#L60-L66) : /script/jqf-driver.sh를 백그라운드에서 실행하고, 열어둔 named pipe를 사용해  proxy(/bin/afl-proxy)를 실행한다

**3. [/script/jqf-driver.sh](https://github.com/rohanpadhye/JQF/blob/master/scripts/jqf-driver.sh)**

* JQF class와 jar 파일, 즉 타겟을 찾아서 java를 실행해주는 스크립트
* 디테일한 부분은 아직 잘 모르겠다

**4. [/fuzz/.../AFLDriver.java](https://github.com/rohanpadhye/JQF/blob/master/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLDriver.java)**

* JQF의 main class로, 타겟 클래스와 메소드를 바꿔 낄 수 있는 fuzzing driver의 메인 함수라고 생각하면 편할 것 같다.
* [AFLGuidance](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLGuidance.java#L68) 인스턴스를 생성해 `GuidedFuzzing.run()`에 넘겨준다
* [`GuidedFuzzing.run()`](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/guidance/Guidance.java#L221)은 주어진 타겟 클래스와 메소드, guidance 정보 등을 사용해 Junit test를 실행한다.

## Instrumentation
JQF를 사용할 때 instrumentation은 런타임에 다이나믹하게 일어나며, event handler 처럼 동작한다. 이 부분은 디테일하게 리뷰하지 않았다.

**[/instrument/.../InstrumentingClassLoader.java](https://github.com/rohanpadhye/JQF/blob/master/instrument/src/main/java/edu/berkeley/cs/jqf/instrument/InstrumentingClassLoader.java#L46)**

* java 프로그램을 대상으로 하고 있으므로, 타겟인 bytecode를 수정한다
* `transformer.transform()` ([/instrument/.../SnoopInstructionTransformer.java](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/instrument/src/main/java/janala/instrument/SnoopInstructionTransformer.java#L84))을 사용해 bytecode를 수정한다
  * [#103-104](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/instrument/src/main/java/janala/instrument/SnoopInstructionTransformer.java#L103-L104) : 해당 클래스를 수정한 기록(cache)이 있는지 확인한다
  * [#122-125](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/instrument/src/main/java/janala/instrument/SnoopInstructionTransformer.java#L122-L125) : 캐시가 없으면 클래스를 변환하는데, 이 때 클래스를 읽고 쓰는 사이에 visitor를 끼워 넣는 방식을 사용한다
    * java 클래스를 분해, 수정, 재구성하기 위한 asm 라이브러리를 사용해 구현된 [`SnoopInstructionClassAdapter`](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/instrument/src/main/java/janala/instrument/SnoopInstructionClassAdapter.java#L9) 인스턴스를 visitor로 사용한다

## Event Handling (Coverage)

**1. [handleEvent()](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLGuidance.java#L355)**

* [여러 종류의 이벤트](https://github.com/rohanpadhye/JQF/tree/32e056412fa4953c33f9baef6c79564ccc07a3a5/instrument/src/main/java/edu/berkeley/cs/jqf/instrument/tracing/events)들 중 BranchEvent와 CallEvent에 대해서만 커버리지를 측정한다.
* 커버리지 측정은 비트맵(array)인 traceBits에 hit count를 저장하는 방식으로 이루어진다 ([code](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLGuidance.java#L385)). 측정된 커버리지 정보는 proxy를 통해 AFL쪽에 전달된다.
* [hash 함수](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/util/Hashing.java)를 사용하여 edgeId, 즉 traceBits의 인덱스를 구한다. knuth 알고리즘을 사용하며, BranchEvent와 CallEvent의 hash 값이 겹치지 않게 하기 위해 2개의 hash 함수를 구현하여 사용하고 있다.
* BranchEvent에 대해서는 timeout을 체크하는 로직이 추가되어 있다 ([code](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLGuidance.java#L365)). branchCount 10000번마다 timeout을 체크한다 ([code](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLGuidance.java#L400)).

**2. [handleResult()](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLGuidance.java#L244)**

* 저장된 traceBits와 status(SUCCESS, FAILURE, INVALID, TIMEOUT)를 적절히 가공 후 feedback 버퍼에 카피해, proxy를 통해 AFL쪽에 전송한다.
* [#267](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLGuidance.java#L267) : traceBits[0]이 0인 경우 AFL이 instrumentation이 되지 않았다고 판단하고 오류를 낸다고 한다. 이를 방지하기 위해 첫 번째 bit을 확인해 set 해주는 내용이다.
* [#271-312](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLGuidance.java#L271-L312) : result를 바탕으로 status를 판단한다. result는 handleResult()의 파라미터로 넘어오며, handleResult()는 [evaluate()](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/junit/quickcheck/FuzzStatement.java#L96)에 의해 불린다. evaluate()는 [guidance.run()](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/junit/quickcheck/FuzzStatement.java#L146)의 결과, 즉 발생하는 exception의 종류에 따라 status를 판단한다.
* [#315-320](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLGuidance.java#L314-L320) : status와 traceBits를 feedback 버퍼에 카피한다.
* [#324-329](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/java/edu/berkeley/cs/jqf/fuzz/afl/AFLGuidance.java#L323-L329) : feedback 버퍼의 내용을 proxy를 통해 AFL쪽으로 전송한다.


## Proxy

**[/fuzz/src/main/c/afl-proxy.c](https://github.com/rohanpadhye/JQF/blob/master/fuzz/src/main/c/afl-proxy.c)**

* /bin/jqf-afl-target가 실행하는 /bin/afl-proxy의 소스코드이다
* [log_to_file()](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/c/afl-proxy.c#L71) : `jqf-afl-fuzz`를 실행할 때 로그 파일 옵션을 줄 수 있는데, 옵션이 있으면 특정 파일에 로그를 남겨주는 것으로 보인다.
* JQF main과 proxy 사이의 통신은 named pipe를 사용한다.
* AFL과 proxy 사이의 통신도 named pipe(FORKSRV_FD, FORKSRV_FD+1)를 사용하는데, 추가적으로 커버리지 정보, 즉 trace_bits를 업데이트할 때는 AFL이 사용하는 shared memory를 업데이트해주는 방식을 사용한다.
  * [FORKSRV_FD](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/c/afl-proxy.h#L24) : 198로 정의되어 있으며, 198은 AFL로부터 읽을 때, 199는 AFL에 쓸 때 사용하는 pipe의 file descriptor인 것으로 보인다.
  * [SHM_ENV_VAR](https://github.com/rohanpadhye/JQF/blob/9436c4fdafee3f97d73f29ef7ecc3cd283924f7e/fuzz/src/main/c/afl-proxy.h#L23) : 환경변수 사용하여 AFL이 사용하는 shared memory의 id를 얻어온다.
* 무의미한 4 bytes를 주고받음으로써 connection setup을 진행한다.
* 전반적인 프로토콜은 다음과 같다.
  1. proxy가 AFL에 4바이트("HELO")를 전송하고, AFL로부터 4바이트("HELO")를 읽는다. (FORKSRV_FD, FORKSRV_FD+1 사용)
  2. proxy가 AFL에 4바이트의 child pid를 전송한다. 
     * JQF는 fork를 하지 않고, AFL에서 프로세스 kill을 할 필요가 없기 때문에 (JQF에서 타임아웃 체크를 함) 사실 특별한 의미는 없다.
  3. proxy가 JQF에 4바이트("HELO")를 전송하고, JQF로부터 4바이트의 status 정보를 읽는다.
  4. proxy가 JQF로부터 n바이트의 traceBits 정보를 읽고, shared memory를 통해 이를 AFL쪽에 전달한다.
  5. proxy가 FORKSRV_FD + 1을 통해 AFL에 4바이트의 status 정보를 전달한다.