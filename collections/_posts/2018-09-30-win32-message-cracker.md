---
title: "[Win32API] 메시지 크래커 사용하기"
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

Win32 API를 이용하여 프로그램을 작성할때 윈도우메시지 하나하나를 처리해주려면 상당히 신경쓸 부분이 많아지게 됩니다. 우선 WPARAM과 LPARAM의 값이 무엇을 담고 있는지도 봐야하구, 메시지가 어떻게 넘어오고, 처리되는지 등등... 상당히 수동적이라 자세하게 건드릴 수 있지만 그만큼 짜증나기도 합니다. 이럴때 메시지 크래커를 사용하면 그런 수고로움을 조금은 덜 수 있습니다.


#사용법

windowsx.h 파일을 include하고, windowsx.h 파일을 열어서 문자열 찾기로 해당 메시지를 찾습니다. WM_CREATE 이런식으로 찾으면 매크로 함수 위에 주석으로 함수의 프로토타입이 제공되어져 있는걸 확인할 수 있습니다. 그걸 그냥 복사해서 붙여넣으시고, 함수 정의 해서 사용하시면 됩니다. 윈도우 프로시저 안에는 아래와 같은 HANDLE_MSG() 메크로를 이용하면 됩니다.

``` cpp
#include <windows.h>
#include <windowsx.h>

LRESULT CALLBACK WndProc(HWND hWnd, UINT iMessage, WPARAM wParam ,LPARAM lParam);
HINSTANCE g_hInst;
HWND hWndMain;
LPCTSTR lpszClass=TEXT("Class");

BOOL OnCreate(HWND hWnd, LPCREATESTRUCT lpCreateStruct);
void OnPaint(HWND hWnd);
void OnDestroy(HWND hWnd);

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
 
    while (GetMessage(&Message,NULL,0,0))
    {
        TranslateMessage(&Message);
        DispatchMessage(&Message);
    }
    return (int)Message.wParam;
}

LRESULT CALLBACK WndProc(HWND hWnd,UINT iMessage,WPARAM wParam,LPARAM lParam)
{
    switch (iMessage)
    {
        HANDLE_MSG(hWnd, WM_CREATE, OnCreate);
        HANDLE_MSG(hWnd, WM_PAINT, OnPaint);
        HANDLE_MSG(hWnd, WM_DESTROY, OnDestroy);
    }
    
    return(DefWindowProc(hWnd,iMessage,wParam,lParam));
}

BOOL OnCreate(HWND hWnd, LPCREATESTRUCT lpCreateStruct)
{
    hWndMain=hWnd;
    return TRUE;
}

void OnPaint(HWND hWnd)
{
    HDC hdc;
    PAINTSTRUCT ps;
    hdc=BeginPaint(hWnd, &ps);
    EndPaint(hWnd, &ps);
}

void OnDestroy(HWND hWnd)
{
    PostQuitMessage(0);
}
```
