---
title: "[C#] 폴더 경로 가져오기"
categories:
  - CSharp
tags:
  - Windows
  - CSharp
  - .Net 
date: 2018-09-30
last_modified_at: 2021-11-02
toc: false
---

C#에서 윈도우즈의 폴더나 로그온사용자 이름등을 가져다 쓰고 싶을때는 Environment 클래스를 살펴보면 됩니다.
관련 자료가 있는 MSDN  주소는 

Environment 클래스 : https://docs.microsoft.com/ko-kr/dotnet/api/system.environment?view=net-5.0

열거형 : https://docs.microsoft.com/ko-kr/dotnet/api/system.environment.specialfolder?view=net-5.0

아래의 예제에 부족함을 느낀다면 위 주소로 들어가셔서 살펴보시면 됩니다.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Directory
{
class Program
{
static void Main(string[] args)
{
// 현재 디렉토리
string currentDir = Environment.CurrentDirectory;
Console.WriteLine(currentDir);

            // 현재 로밍 사용자의 응용 프로그램 관련 데이터에 대한 공용 리포지토리로 사용되는 디렉토리
            string appData = Environment.GetFolderPath(Environment.SpecialFolder.ApplicationData);
            Console.WriteLine(appData);
 
            // 바탕화면 디렉토리
            string desktop = Environment.GetFolderPath(Environment.SpecialFolder.DesktopDirectory);
            Console.WriteLine(desktop);
 
            // 내문서 디렉토리
            string myDoc = Environment.GetFolderPath(Environment.SpecialFolder.MyDocuments);
            Console.WriteLine(myDoc);
        }
    }
}
```
