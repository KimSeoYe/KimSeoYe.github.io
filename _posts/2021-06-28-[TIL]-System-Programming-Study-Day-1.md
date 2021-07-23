---
layout: post
title:  "[TIL] System Programming Study - Day 1"
date:   2021-06-28 12:30:00
author: KimSeoYe
categories: TIL
tags:   TIL sysprog
cover:  "/assets/headers/hgu1.jpeg"
---
# Preparing for star : Q1 ~ Q4

1. 파일을 Binary로 읽고 쓰는 것과 Text로 읽고 쓰는 것의 차이를 이해하기 위해 **fread()** 와 **fwrite()** 를 사용해 보았다. <br>파일을 바이너리로 읽는다는 것은 간단하게 말하면 파일을 byte 단위로 읽는다는 것으로, '바이너리 파일을 읽는다'는 것과는 다르다.
2. **dirent.h** 의 **opendir()** 로 디렉토리를 열고, **readdir()** 로 읽어서 dirent 구조체를 사용해 directory entry에 접근할 수 있다.
3. **stat system call** 을 사용해 파일의 크기, 타입과 같은 정보를 얻을 수 있다. 
4. 바로 코드를 짜는 것 보다 **생각을 마치고** 짜는 것이 훨씬 더 효율적이고 안전하다는 것을 다시 한 번 깨닫게 되었다. <br>생각하기를 스킵하는 것은 힘이 빠지거나 마음이 급할 때 자주 반복되는 습관인데, 그럴 때마다 헤매는 시간이나 디버깅하는 시간이 배로 늘어났던 것 같다.
5. 코드를 통해 내가 짠 프로그램의 논리를 쉽게 읽어낼 수 있도록 늘 세심하게 신경써야겠다. <br>교수님의 코드 리뷰를 들으며, 내가 짠 코드를 읽으며 괴로워했던 경험이 오늘 말고도 꽤 있었다는 것이 기억났다.
