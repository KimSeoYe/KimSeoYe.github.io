---
layout: post
title: "[TIL] System Programming Study - Day 2"
subtitle: "Start making star: simple tar"
categories: TIL
tags: [TIL,sysprog]
---

### Description
        
>This task asks you to construct `star`, a simplified version of a file-archiving program `tar`. <br>
>star provides two functionalities. 
>1. it generates a file that aggregates all files in a specified directory (i.e., archive).
>2. for a given archive file generated by itself, star restores all files with their original file names (i.e., extract).
        
_다른 팀원들이 개발한 star로 내가 archive한 파일을 extract할 수 있어야 한다!_

### 진행 과정
1. github 만들기
2. Command-line interface 개발 (Error 처리)
3. 테스트 케이스 준비 + 전체 미팅 (테스트 케이스 공유)
4. 1개 파일에 대해 동작하는 프로그램 만들기 + 팀별 리뷰 미팅

### TIL
1. Command-line interface를 개발하는 데 이렇게 오랜 시간을 써 본 것은 처음인 것 같다. 가능한 경우의 수를 생각하고 case를 최대한 효율적으로 분류한 뒤 코드 짜는 것을 시작해 보았다. 생각했던 것보다 Command-line에서 가능한 에러를 처리하는 것이 훨씬 중요하다는 것을 깨달았다.
2. CLI에서 에러를 처리하는 과정에서 `access()` 함수를 사용하여 대상 파일이나 폴더의 존재 유무를 검사해 보았다. 다만 아직 이 `access()`가 어떤 방식으로 동작하는지(파일의 어느 부분까지 접근하는지)는 자세히 공부하지 못했다.
3. C언어의 구조체가 메모리에 어떤 식으로 저장되는지를 지금까지 완전히 잘못 이해하고 있었다는 것을 알게 되었다. `struct`는 메모리에 array처럼 저장된다!
4. File은 forward로 읽거나 쓴다. `fwrite`를 할 때 원했던 크기보다 적게 쓰는 일이 발생할 수 있는데, 그럴때 어떻게 남은 부분을 이어 쓸 수 있을지가 궁금했다. 그래서 제한된 크기의 버퍼로 파일을 읽는 도중에 file pointer를 `fwrite`를 하기 전으로 reposition할 수 있는지가 궁금했는데, 그럴 수 없다. file pointer가 정확히 어떤 것인지에 대한 이해가 아직 부족한 것 같다. (비슷한 맥락에서, `fwrite`를 한 뒤 `fclose`를 하기 위해 `rewind`를 할 필요가 **전혀** 없다.)
5. C언어는 assembly에서 최소한의 level만 올린 언어다. 따라서 코드가 어떤 instruction 들로 interpret 되는지를 이해할 수 있어야 C언어를 제대로 이해한 것이라고 할 수 있다.
6. 프로그램 안에서 에러 처리는 orthogonal 하다. 따라서 프로그램의 전체 흐름을 해치지 않게(?) 처리할 수 있으면 좋다. Lable을 사용하여 Java에서처럼 exceptoin을 throw-catch 하는 것과 비슷한 구조로 처리할 수 있다.
7. If문 안에서 `fread`나 `fwrite`의 에러를 처리할 때, 조건보다는 핵심이 되는 `fread/fwrite`의 실행을 앞에 두는 것이 깔끔하다.

### 느낀점
오늘은 왠지 느낀점이 쓰고 싶었다:)
1. 오늘은 (1)프로그램의 구조와 (2)생각을 마치고 코딩하기 두 가지에 최대한 집중해 보려고 노력했다.
2. 프로그램의 논리적 구조를 신경쓰려다 보니 프로그래밍을 할 때 고민해야 할 것들이 아주 많아졌다. 지금까지 내가 얼마나 많은 것들을 대충 넘기며 코딩해왔는지를 느낄 수 있었다...
3. 생각을 마치고 코딩을 시작하는게 생각보다 쉽지 않아서, 주석으로 프로그램이나 함수의 논리를 끝까지 써보기 전에는 코딩을 시작하지 않기로 스스로 약속해 보았다. 시간이 조금 걸렸지만 논리가 훨씬 더 잘 정리되는 것 같았다. 그때그때 생긴 질문이나 고민들도 기록해보았다. (* 코드 안에 코드를 설명하는 주석을 남기는 것은 좋지 않다고 한다. 코드만으로도 논리가 깔끔하게 읽혀야 한다.)
```
/*
        Comman line interface
        ./star archive <archive-file-name> <target directoxry path> -> zip
        ./star list <archive-file-name> -> show the paths of all files aggregated in the <archive-file-name>
        ./star extract <archive-file-name> -> restore all files and directories archived in <archive-file-name>

        * invalid option 
            * do not pass the option
            * argv[1] not in "archive" or "extract" or "list" ? 
        * case 1. "archive"
            * # of parameter : 4
            * target file name is invalid : already exist
            * target directory name is invalid (not exist)
        * case 2. "extract"
            * # of parameter : 3
            * target file name is invalid : not exist
        * case 3. "list"
            * # of parameter : 3
            * target file name is invalid : not exist

        Q. timing for checking existance of target file name or directory ?
            CLI에서 먼저 파일이나 디렉토리가 있는지 없는지를 체크해서, 없으면 아예 프로그램을 종료시키는 것이
            나중에 file이나 directory를 열 때 검사하도록 미뤄두는 것보다 나을까?
            file이나 directory를 열 때는 존재 유무 말고도 다른 문제가 발생할 수도 있으니까
            CLI에서 먼저 해두는 게 나을 것 같다고 판단하긴 했지만,,,
        Q. 아예 각 파라미터를 받는 함수를 따로..? Ex) get_option, get_file, get_dir ...
            매 함수마다 option별 case를 나눠줘야 하는데, 이게 맞는 건지 사실 잘 모르겠다.
        Q. 파라미터를 받는 과정 전체를 함수로 만들어보려고 했는데, star_dir이 필요한 경우와 필요 없는 경우가 있어서 조금 고민..
            어차피 변수는 선언할거니까 일단 함수로 빼 보자.
*/
```