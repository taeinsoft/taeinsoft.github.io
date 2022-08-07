---
title: "[Win32API] 기본 윈도우 창 띄우기"
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

Visual studio에서도 윈도우만 뜨는 기본 코드는 자동으로 생성을 해주고, 찰스페졸드의 책에도 기본 소스코드는 나와있습니다. 하지만 처음 윈도우즈 프로그래밍을 공부하기에는 군더더기가 많은 소스라고 생각됩니다.

제가 본 코드중에는 아래 코드가 가장 간단하게 구성되어있습니다.

"Windows API 정복" 책에 기술된 소스코드입니다.

``` cpp
#include <windows.h>

LRESULT CALLBACK WndProc(HWND,UINT,WPARAM,LPARAM);
HINSTANCE g_hInst;
HWND hWndMain;
LPCTSTR lpszClass=TEXT("Class");

int APIENTRY WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, LPSTR lpszCmdParam, int nCmdShow)
{
    HWND hWnd;
    MSG Message;
    WNDCLASS WndClass;
    g_hInst=hInstance;

    WndClass.cbClsExtra=0;
    WndClass.cbWndExtra=0;
    WndClass.hbrBackground=(HBRUSH)(COLOR_WINDOW+1);
    WndClass.hCursor=LoadCursor(NULL,IDC_ARROW);
    WndClass.hIcon=LoadIcon(NULL,IDI_APPLICATION);
    WndClass.hInstance=hInstance;
    WndClass.lpfnWndProc=WndProc;
    WndClass.lpszClassName=lpszClass;
    WndClass.lpszMenuName=NULL;
    WndClass.style=CS_HREDRAW | CS_VREDRAW;
    RegisterClass(&WndClass);
 
    hWnd=CreateWindow(lpszClass,lpszClass,WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT,CW_USEDEFAULT,CW_USEDEFAULT,CW_USEDEFAULT,
        NULL,(HMENU)NULL,hInstance,NULL);
    ShowWindow(hWnd,nCmdShow);
 
    while (GetMessage(&Message,NULL,0,0)) {
        TranslateMessage(&Message);
        DispatchMessage(&Message);
    }
    return (int)Message.wParam;
}

LRESULT CALLBACK WndProc(HWND hWnd,UINT iMessage,WPARAM wParam,LPARAM lParam)
{
    HDC hdc;
    PAINTSTRUCT ps;

    switch (iMessage) {
    case WM_CREATE:
        hWndMain=hWnd;
        return 0;
    case WM_PAINT:
        hdc=BeginPaint(hWnd, &ps);
        EndPaint(hWnd, &ps);
        return 0;
    case WM_DESTROY:
        PostQuitMessage(0);
        return 0;
    }
    return(DefWindowProc(hWnd,iMessage,wParam,lParam));
}
```
