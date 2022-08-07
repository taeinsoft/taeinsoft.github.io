---
title: "Windows IME 관련 함수"
categories:
  - Win32API
tags:
  - Windows
  - C++
  - Win32API
date: 2018-09-29
last_modified_at: 2021-11-02
toc: false
---

먼저 IME를 사용하려면 프로젝트에 Imm32.lib를 추가하고, imm.h를 인클루드 해야한다.

# IME 메시지

## WM_IME_STARTCOMPOSITION
IME가 조립 문자열을 만들기 직전에 보냄. WPARAM, LPARAM의 값은 없음. 이 메시지를 DefWindowProc으로 보내지 않으면 조립윈도우가 나타나지 않는다.

## WM_IME_ENDCOMPOSITION
조립이 끝났다는 통지 메시지. 인수와 리턴값 없음. 커스텀 IME 윈도우를 작성하지 않는다면 무시해도 무방함.

## WM_IME_COMPOSITION
조립 상태가 변경될때마다 보내진다.
  * WPARAM - 조립중의 문자의 코드가 전달, 이 코드는 2byte의 DBCS문자로 조립중인 중간 문자코드이다.
  * LPARAM - 조립상태가 어떻게 변경되었는지, 이 문자를 어떻게 처리해야 하는지를 나타내는 플래그의 집합. 
    * 한글의 경우 다음 두 플래그가 특히 중요함. 이 플래그들이 있는지 살펴보면 문자가 완성된 것인지, 조립중인지 알 수 있음.
      * GCS_COMPSTR : 아직 문자를 조립중이라는 뜻, 즉 아직 한 음절이 완성되지는 않았음.
      * GCS_RESULTSTR : 한 음절을 완전히 조립했다는 뜻.

## WM_IME_CHAR
문자 하나가 완성되었을때 보내짐.
  * WPARAM - 완성된 문자의 코드가 전달. 1byte만 전달되는 WM_CHAR과는 달리 DBCS일 수 있음. (단, 유니코드 윈도우에서는 WM_CHAR, WM_IME_CHAR 모두 2byte이다.)
  * **이 메시지를 무시하면 한글 한 문자에 대해 WM_CHAR 메시지를 두번 받게 된다.**

## WM_IME_SETCONTEXT
응용프로그램이 활성/비활성화될 때 보내진다. WPARAM이 TRUE이면 활성, FALSE이면 비활성화 되었다는 뜻이다.

## WM_IME_NOTIFY
IME 윈도우가 변경되었다는 통지 메시지이다. WPARAM으로 어떤 변경인지 통보됨.한글 입력, 영문 입력모드를 변경할 때도 이 메시지가 전달된다.

<br>

# Input Context 생성, 해제
Input Context(입력 컨텍스트) : IME가 내부적으로 사용하는 구조체이며 조립 문자열, 변환모드, IME 윈도우의 위치 등 IME의 현재 상태에 대한 정보들이 저장됨.


## Input Context의 핸들을 얻는 함수
``` cpp
HIMC ImmGetContext(void);
BOOL ImmReleaseContext(HWND hWnd, HIMC hIMC);
```

## Input Context를 조작하는 함수
``` cpp
BOOL ImmGetConversionStatus(HIMC hIMC, LPDWORD lpfdwConversion, LPDWORD lpfdwSentence);
BOOL ImmSetConversionStatus(HIMC hIMC, DWORD fdwConversion, DWORD fdwSentence);
```
입력모드를 한글로 변환하고 싶으면
``` cpp
ImmSetConversionStatus(hImc, IME_CMODE_NATIVE, IME_SMODE_NONE);
```
입력모드를 영어로 변환하고 싶으면
``` cpp
ImmSetConversionStatus(hImc, 0, IME_SMODE_NONE);
```
를 호출하면 된다.

 하지만 시스템이 만드는 Default Input Context는 스레드 내의 모든 윈도우가 공유하기 때문에 한글이나 영문으로 변경시 모든 입력모드가 통일되게 변경된다. 만약 각각의 컨트롤에서 다른 입력모드를 주고 싶다면 윈도우별로 InputContext를 생성, 해제하여 연결시키면 된다. 

## Input Context를 생성, 해제하는 함수
``` cpp
// InputContext를 위한 메모리를 할당하고, 그 핸들을 리턴
HIMC ImmCreateContext(void);

// 특정 윈도우와 연결
HIMC ImmAssociateContext(HWND hWnd, HIMC hIMC);

// InputContext를 메모리에서 해제
BOOL ImmDestroyContext(HIMC hIMC);
```
InputContext를 생성, 해제할 경우에는 Pen, Brush를 생성, 해제할때와 같은 방법으로 한다.