---
title: 비동기 쿼리 및 저장-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 758f8bc3d14fc1f60f14ff14f4251aeed057c518
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994465"
---
# <a name="async-query-and-save"></a>비동기 쿼리 및 저장
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

EF6 비동기 쿼리를 사용 하 여 저장 지원을 도입 합니다 [async 및 await 키워드](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) .NET 4.5에서 도입 된 합니다. 일부 응용 프로그램은 비동기에서 이점을 얻을 수, 하는 동안 장기 실행, 네트워크 또는 O 바인딩된 작업을 처리 하는 경우 클라이언트 응답성 및 서버 확장성을 개선 하기 위해 사용할 수 있습니다.

## <a name="when-to-really-use-async"></a>실제로 비동기를 사용 하는 경우

이 연습의 목적은 쉽게 비동기 및 동기 프로그램 실행 간의 차이 알 수 있도록 하는 방식으로 비동기 개념을 소개 하는 것입니다. 이 연습에서는 아닙니다 주요 시나리오 중 하나를 보여 주기 위해 혜택을 제공 하는 비동기 프로그래밍 합니다.

비동기 프로그래밍은 주로 계산 든 필요 하지 않은 작업까지 기다리는 동안 다른 작업을 수행 하려면 현재 관리 되는 스레드 (스레드가 실행 중인.NET 코드)를 확보에서 관리 되는 스레드입니다. 예를 들어 데이터베이스 엔진은 쿼리를 처리 하는 동안 없는.NET 코드에 의해 수행 됩니다.

클라이언트 응용 프로그램 (WinForms, WPF 등)에서 현재 스레드에서 비동기 작업이 수행 되는 동안 UI를 응답 가능한 상태로 유지 하려면 사용할 수 있습니다. -들어오는 다른 요청을 처리 하는 데 사용할 수는 스레드 서버 응용 프로그램 (ASP.NET 등)에서 메모리 사용량을 낮춥니다. 하거나 서버의 처리량을 늘릴 수 있습니다이.

비동기를 사용 하 여 대부분의 응용 프로그램에서에 눈에 띄는 경우 이점은 없고 있고도 저하 될 수 있습니다. 테스트, 프로 파일링 및 상식적인을 사용 하 여 커밋하기 전에 특정 시나리오에서 비동기의 영향을 측정 하기.

몇 가지 추가 비동기에 대 한 학습 리소스는 다음과 같습니다.

-   [Brandon Bray의 async/await.NET 4.5 개요](http://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [비동기 프로그래밍](https://msdn.microsoft.com/library/hh191443.aspx) MSDN 라이브러리에서 페이지
-   [비동기를 사용 하 여 ASP.NET 웹 응용 프로그램 빌드 방법](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (향상 된 서버 처리량 데모 포함)

## <a name="create-the-model"></a>모델 만들기

하지만 사용 합니다 [Code First 워크플로](~/ef6/modeling/code-first/workflows/new-database.md) 모델 EF 디자이너를 사용 하 여 만든 라이선스 포함 하는 모든 EF 모델을 사용 하 여 비동기 기능을 작동 데이터베이스를 생성 하려면.

-   콘솔 응용 프로그램을 만들고 **AsyncDemo**
-   EntityFramework NuGet 패키지 추가
    -   솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 합니다 **AsyncDemo** 프로젝트
    -   선택 **NuGet 패키지 관리...**
    -   NuGet 패키지 관리 대화 상자에서 선택 합니다 **Online** 탭을 선택 합니다 **EntityFramework** 패키지
    -   클릭 **설치**
-   추가 된 **Model.cs** 다음 구현 클래스

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
        }

        public class Blog
        {
            public int BlogId { get; set; }
            public string Name { get; set; }

            public virtual List<Post> Posts { get; set; }
        }

        public class Post
        {
            public int PostId { get; set; }
            public string Title { get; set; }
            public string Content { get; set; }

            public int BlogId { get; set; }
            public virtual Blog Blog { get; set; }
        }
    }
```

 

## <a name="create-a-synchronous-program"></a>동기 프로그램 만들기

이제 EF 모델을 만들었으므로 일부 데이터 액세스를 수행 하는 데 사용 하는 일부 코드를 작성해 보겠습니다.

-   내용을 바꿉니다 **Program.cs** 다음 코드를 사용 하 여

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine();
                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

이 코드는 호출을 **PerformDatabaseOperations** 새 저장 하는 메서드 **블로그** 및 데이터베이스에 다음 검색 모든 **블로그** 데이터베이스에서 하 고 출력 하는 **콘솔**합니다. 그러면 프로그램 일의 따옴표를 기록 합니다 **콘솔**.

동기 코드 이므로 프로그램을 실행 하면 다음 실행 흐름을 확인할 수 있습니다 것:

1.  **SaveChanges** 새 적용할 시작 **블로그** 데이터베이스로
2.  **SaveChanges** 완료
3.  모든 쿼리 **블로그** 데이터베이스로 전송 됩니다
4.  쿼리를 반환 하 고 결과에 기록 되며 **콘솔**
5.  날짜의 견적에 쓸 때 **콘솔**

![SyncOutput](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>비동기 만들기

이제 프로그램을 실행 했으므로 새 비동기를 사용 하 고 await 키워드 시작할 수 있습니다. Program.cs에 다음 변경 내용을 만들었습니다.

1.  2 번 줄: using 문을 합니다 **System.Data.Entity** 네임 스페이스 액세스를 제공 EF 비동기 확장 메서드.
2.  줄 4:를 사용 하 여 문을 합니다 **System.Threading.Tasks** 네임 스페이스를 사용할 수 있습니다 합니다 **작업** 형식입니다.
3.  줄 12 & 18:의 진행률을 모니터링 하는 작업으로 캡처하는 것 **PerformSomeDatabaseOperations** (줄 12) 및 다음이 프로그램 실행 차단 작업 완료 한 번에 모든 작업 프로그램 (18 번 줄) 수행에 대 한 합니다.
4.  25 번 줄: 업데이트 되었습니다 **PerformSomeDatabaseOperations** 로 표시 되어야 **비동기** 반환 하 고는 **작업**합니다.
5.  줄 35:는 이제 SaveChanges의 비동기 버전을 호출 하 고 해당 완료 대기 중입니다.
6.  줄 42:는 이제 ToList의 비동기 버전을 호출 하 고 결과에서 대기 중입니다.

System.Data.Entity 네임 스페이스에서 사용 가능한 확장 메서드의 전체 목록을, QueryableExtensions 클래스를 가리킵니다. *또한 해야 "System.Data.Entity"를 사용 하 여 추가 하 여 문입니다.*

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

이제 코드의 비동기, 프로그램을 실행 하면 다양 한 실행 흐름을 확인할 수 있습니다 것:

1.  **SaveChanges** 새 적용할 시작 **블로그** 데이터베이스로 *명령을 보낸 후 데이터베이스에 더 이상 현재 관리 되는 스레드에서 데 필요한 시간을 계산 합니다. 합니다 **PerformDatabaseOperations** 메서드 (경우에 실행 완료 되지 않은)를 반환 하 고 Main 메서드에서 프로그램 흐름을 계속 합니다.*
2.  **하루 중 견적 콘솔에 기록 됩니다**
    *대기에서 관리 되는 스레드는 차단 Main 메서드에서 수행할 작업이 더 이상 이므로 데이터베이스 작업이 완료 될 때까지 호출 합니다. 완료 되 면 나머지 우리의 **PerformDatabaseOperations** * 실행 됩니다.
3.  **SaveChanges** 완료
4.  모든 쿼리 **블로그** 데이터베이스로 전송 됩니다 *다시 관리 되는 스레드는 데이터베이스의 쿼리 처리 되는 동안 다른 작업을 수행할 수 있습니다. 다른 모든 실행 완료 후 스레드가 대기 호출 하지만 중단만 됩니다.*
5.  쿼리를 반환 하 고 결과에 기록 되며 **콘솔**

![AsyncOutput](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>코딩할

이제 살펴본 것이 얼마나 쉬운지 확인 하려면 EF의 비동기 메서드를 사용 합니다. 비동기의 장점은 매우 간단한 콘솔 앱을 사용 하 여 명확 하지 않을 수 있습니다., 있지만 이러한 동일한 전략 또는 있는 경우 장기 실행 또는 네트워크 바인딩된 작업 수 그렇지 않은 경우 응용 프로그램을 차단 하면 많은 수의 스레드를 적용할 수 있습니다. 메모리 사용 공간을 늘립니다.
