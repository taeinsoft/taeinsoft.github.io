---
title: "[C#] SQL Server Compact 3.5 를 활용한 로컬 데이터베이스 연결하기"
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

작은 응용프로그램이나 모바일 프로그램을 개발할때 DB가 필요할때가 있습니다. 그럴땐 보통 엑세스의 MDB나 SqlLite를 많이 쓰시는데요. Microsoft사의 SQL Server Compact 3.5 라는것도 있습니다. 확장자는 SDF로 되어있는 로컬데이터베이스입니다.

MSDN의 관련 자료는

http://msdn.microsoft.com/ko-kr/library/aa983321.aspx

위 페이지로 들어가시면 자세한 설명이 나와있습니다.

읽어보시면 드래그앤 드롭으로 윈폼에서 쉽게 DB를 연결할 수 있습니다.

하지만 순수하게 SQL 쿼리를 사용해서 데이터를 가져오는 방법은 나와있지 않아서 혹시 필요하신분들이 계시면 도움이 되었으면 좋겠습니다. 우선 위 페이지를 보면서 DB를 프로젝트에 생성하고, 테스트용 테이블을 하나 만듭니다.

데이터베이스의 파일 이름은 "DB.sdf"입니다. 저는 Test테이블에 Vehicle이라는 nvarchar형의 컬럼을 하나 추가했습니다.

``` csharp
using System;
using System.Data.SqlServerCe;

namespace SDFSampleCS
{
    class Program
    {
        static void Main(string[] args)
        {
            // 데이터베이스 연결
            string connectionString = @"Data Source=|DataDirectory|\DB.sdf";
            SqlCeConnection con = new SqlCeConnection(connectionString);
            con.Open();

            // 데이터베이스 커맨드 생성
            SqlCeCommand cmd = new SqlCeCommand();
             
            // 커맨드에 커넥션을 연결
            cmd.Connection = con;
             
            // 트랜잭션 생성
            SqlCeTransaction tran = con.BeginTransaction();
            cmd.Transaction = tran;
 
 
            // 쿼리 생성 : Insert 쿼리
            cmd.CommandText = "INSERT INTO Test VALUES('Car')";
 
            // 쿼리 실행
            cmd.ExecuteNonQuery();
 
            // 반복으로 몇개 더 넣어보겠습니다.
            cmd.CommandText = "INSERT INTO Test VALUES('Bus')";
            cmd.ExecuteNonQuery();
 
            cmd.CommandText = "INSERT INTO Test VALUES('Airplane')";
            cmd.ExecuteNonQuery();
 
            // 커밋
            tran.Commit();
             
            // SELECT 쿼리로 변경
            cmd.CommandText = "SELECT * FROM Test";
 
            // DataReader에 쿼리 결과값 저장
            SqlCeDataReader reader = cmd.ExecuteReader();
             
            // 결과값 출력
            while (reader.Read())
            {
                Console.WriteLine(reader["Vehicle"]);
            }
             
            con.Close();
        }
    }
}
```
