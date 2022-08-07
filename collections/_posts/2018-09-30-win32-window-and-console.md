---
title: "윈도우창과 콘솔창 2개 띄우기"
categories:
- Win32API
tags:
- Windows
- C++
- Win32API
date: 2018-09-30
last_modified_at: 2021-11-02
toc: false
---

보통 디버깅용으로 테스트중인 프로그램 이외에 콘솔창을 하나 더 띄우고 싶을때가 있습니다. 이럴때 아래 코드를 한줄 쳐주면 콘솔창이 같이 뜨게 됩니다.
``` cpp
#pragma comment(linker, "/entry:WinMainCRTStartup /subsystem:console")
```
