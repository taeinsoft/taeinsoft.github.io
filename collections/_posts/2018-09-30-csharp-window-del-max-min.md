---
title: "[C#] 윈도우폼의 최소, 최대, 닫기버튼 없애기"
categories:
  - CSharp
tags:
  - Windows
  - CSharp
  - .Net 
date: 2018-09-30
last_modified_at: 2021-11-03
toc: false
---

Windows form 혹은 WPF에서 윈도우 우측 상단의 닫기 버튼을 없애는 속성은 존재하지 않습니다. 이것을 없애려면 결국 Win32API 방식으로 해결해야 합니다.

Window Class의 파생클래스에 다음 소스코드를 추가합니다.

``` csharp
private const int GWL_STYLE = -16;
private const int WS_SYSMENU = 0x80000;
[DllImport("user32.dll", SetLastError = true)]
private static extern int GetWindowLong(IntPtr hWnd, int nIndex);
[DllImport("user32.dll")]
private static extern int SetWindowLong(IntPtr hWnd, int nIndex, int dwNewLong);
```

그 다음 Window 파생클래스의 Loaded Event 함수 안에 다음 소스코드를 추가하면 됩니다.

``` csharp
var hwnd = new WindowInteropHelper(this).Handle;
SetWindowLong(hwnd, GWL_STYLE, GetWindowLong(hwnd, GWL_STYLE) & ~WS_SYSMENU);
```
