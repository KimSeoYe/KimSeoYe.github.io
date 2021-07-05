---
layout: post
title:  "[TIL] System Programming Study - Day 5"
date:   2021-07-02 12:30:00
author: KimSeoYe
categories: TIL
tags:   TIL sysprog
cover:  "/assets/headers/mountain.jpg"
---
# fshare 1.0 : Simple File Sharing System

### Description
> **fshare - Command Line Interface**<br>
> - fshared (server) : `./fshared -p <port_num> -d <dir_name> &`
> - fshare (client) : `./fshare <ip_addr> <command> (<file_name>)`<br>
>     - commands : `list`, `get`, `put` <br>

<br>

### TIL

-  `strtok()`는 string에서 두 번째 인자(delim)를 찾아 `\0`로 바꾸고 주소를 반환한다. 그러나 연속적으로 string을 tokenize할 때, `strtok()`는 다음 실행 때 첫 번째 인자로 넣어줄 `\0`의 주소를 static으로 선언하여 저장하기 때문에 thread-safe하지 않다. 따라서 multi-thread 프로그램을 작성할 때에는 `strtok_r()` 함수를 사용해야 한다.
-  다음은 `strtok()`의 내부 구조이다. 보이는 것처럼 `char * olds`가 static으로 선언되어 있으며, `__strtok_r()`의 세번째 인자로 넘겨주는 것을 알 수 있다.
    ```c
    char *
    strtok (char *s, const char *delim)
    {
        static char *olds;
        return __strtok_r (s, delim, &olds);
    }

    char *
    __strtok_r (char *s, const char *delim, char **save_ptr)
    {   
        char *end;
        
        if (s == NULL)
            s = *save_ptr;

        if (*s == '\0')
        {
            *save_ptr = s;
            return NULL;
        }
            
        /* Scan leading delimiters.  */
        s += strspn (s, delim);
        if (*s == '\0')
        {
            *save_ptr = s;
            return NULL;
        }

        /* Find the end of the token.  */
        end = s + strcspn (s, delim);
        if (*end == '\0')
        {
            *save_ptr = end;
            return s;
        }

        /* Terminate the token and make *SAVE_PTR point past it.  */
        *end = '\0';
        *save_ptr = end + 1;
        return s;
    }
    ```
-  `strtok()`에서 호출되는 `__strtok_r()`는 `strtok_r()`가 내부적으로 alias되어 있는 것이라고 한다.

<br>

-  코드에서 메인이 되는 로직을 위로 가게 짜면 깔끔하다.
-  `send()`, `recv()`를 이용하여 데이터를 정확하게 주고 받을 때, 바이트 수를 지정하여 안전하게 데이터를 보내거나 받는 함수를 만들어 사용하면 좋다.

<br>

### 느낀점
1. `strtok()`는 과제를 할 때 마다 자주 사용했던 함수인데, 어떻게 동작하는지를 자세히 찾아본 건 이번이 처음이었다. 단순하게만 생각했었는데 multi-threads에서 사용했을 때 문제가 생길 수 밖에 없겠다는 것을 이해하게 되었다. 만약 이 함수가 어떻게 작동하는지 모르고 multi-threads 프로그램에서 사용했다면 한참을 헤맸을 것이다.
2. 어떤 라이브러리나 api를 사용할 때 그 동작을 정확히 이해하는 것이 얼마나 중요한지를 깨닫게 되었다.