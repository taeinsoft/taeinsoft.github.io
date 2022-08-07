---
title: "[C#] UTF-8과 EUC-KR 인코딩"
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

C#에서 UTF-8과 EUC-KR을 바이트코드로 변환하는 방법입니다. 항상 개발을 하거나, 혹은 프로그래밍 공부를 하다보면 한글을 어떻게 표현하고, 어떻게 조작해야하는지 고민할때가 많습니다. C#에서도 예외는 아니었는데, 웹브라우저 컨트롤을 쓰다가 한글이 포함된 주소가 먹히지 않는 사이트를 발견하고, 삽질을 시작했더랬습니다...^^ 조금은 어렵게, 혹은 쉽게 문제를 해결하였습니다.

인터넷 주소를 살펴보면 한글이 바이트코드로 전달되는데, UTF-8로 전달될때가 있고, EUC-KR로 전달될때가 있어서 전달되는 바이트코드가 UTF-8인지 EUC-KR인지 아래의 예제를 사용해보시면 확인해보실 수 있습니다.

``` csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;

namespace Encoding
{
    class Program
    {
        static void Main(string[] args)
        {
            string s = "김희선";
            Console.WriteLine("원본문자열 : {0}", s);

            // 코드페이지 번호는 http://msdn.microsoft.com/ko-kr/library/system.text.encoding.aspx 에서 확인하시면 됩니다.
            int euckrCodepage = 51949;
             
            // 인코딩을 편리하게 해주기 위해서 인코딩클래스 변수를 만듭니다.
            System.Text.Encoding utf8 = System.Text.Encoding.UTF8;
            System.Text.Encoding euckr = System.Text.Encoding.GetEncoding(euckrCodepage);
 
            // 위에서 만든 변수를 이용하여 Byte의 배열로 문자열을 인코딩하여 얻는 부분입니다.
            byte[] utf8Bytes = utf8.GetBytes(s);
            Console.Write("UTF-8 : ");
            foreach (byte b in utf8Bytes)
            {
                Console.Write("{0:X} ", b); // byte를 16진수로 표기합니다.
            }
            Console.Write("\n");
 
            byte[] euckrBytes = euckr.GetBytes(s);
            Console.Write("EUC-KR : ");
            foreach (byte b in euckrBytes)
            {
                Console.Write("{0:X} ", b); // byte를 16진수로 표기합니다.
            }
            Console.Write("\n");
 
            // 인코딩된것을 문자열로 변환하기
            string decodedStringByEUCKR = euckr.GetString(euckrBytes);
            string decodedStringByUTF8 = utf8.GetString(utf8Bytes);
            Console.WriteLine("EUC-KR로 디코딩된 문자열 : " + decodedStringByEUCKR);
            Console.WriteLine("UTF-8로 디코딩된 문자열 : " + decodedStringByUTF8);
        }
    }
}
```
