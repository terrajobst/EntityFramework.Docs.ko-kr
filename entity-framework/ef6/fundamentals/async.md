---
title: 비동기 쿼리 및 저장-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: bf2039110962e8dd114242dcd0b9454963750774
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306592"
---
# <a name="async-query-and-save"></a>비동기 쿼리 및 저장
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

EF6는 비동기 쿼리에 대 한 지원을 도입 하 고 .NET 4.5에 도입 된 [async 및 wait 키워드](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) 를 사용 하 여 저장 합니다. 모든 응용 프로그램은 비동기의 이점을 누릴 수 있지만 장기 실행, 네트워크 또는 i/o 바인딩된 작업을 처리할 때 클라이언트 응답성 및 서버 확장성을 향상 시키는 데 사용할 수 있습니다.

## <a name="when-to-really-use-async"></a>비동기를 사용 하는 경우

이 연습의 목적은 비동기 및 동기 프로그램 실행 간의 차이를 쉽게 확인할 수 있도록 하는 방식으로 비동기 개념을 소개 하는 것입니다. 이 연습은 비동기 프로그래밍에서 혜택을 제공 하는 주요 시나리오를 설명 하기 위한 것이 아닙니다.

비동기 프로그래밍은 기본적으로 관리 되는 스레드의 계산 시간이 필요 하지 않은 작업을 기다리는 동안 다른 작업을 수행 하기 위해 현재 관리 되는 스레드 (.NET 코드를 실행 하는 스레드)를 해제 하는 데 중점을 두었습니다. 예를 들어 데이터베이스 엔진이 쿼리를 처리 하는 동안 .NET 코드에서 수행할 작업은 없습니다.

클라이언트 응용 프로그램 (WinForms, WPF 등)에서 현재 스레드를 사용 하 여 비동기 작업을 수행 하는 동안 UI의 응답성을 유지할 수 있습니다. 서버 응용 프로그램 (ASP.NET 등)에서 스레드를 사용 하 여 다른 들어오는 요청을 처리할 수 있습니다. 이렇게 하면 메모리 사용량을 줄이거나 서버의 처리량을 늘릴 수 있습니다.

비동기를 사용 하는 대부분의 응용 프로그램에서는 눈에 띄는 이점이 없으며,이는 좋지 않을 수도 있습니다. 테스트, 프로 파일링 및 일반적인 의미를 사용 하 여 커밋하기 전에 특정 시나리오에서 비동기의 영향을 측정 합니다.

다음은 async에 대해 자세히 알아볼 수 있는 몇 가지 추가 리소스입니다.

-   [Brandon Bray 's .NET 4.5의 async/wait 개요](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   MSDN Library의 [비동기 프로그래밍](https://msdn.microsoft.com/library/hh191443.aspx) 페이지
-   [Async를 사용 하 여 ASP.NET 웹 응용 프로그램을 빌드하는 방법](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (서버 처리량이 증가 하는 데모 포함)

## <a name="create-the-model"></a>모델 만들기

[Code First 워크플로](~/ef6/modeling/code-first/workflows/new-database.md) 를 사용 하 여 모델을 만들고 데이터베이스를 생성 하지만, 비동기 기능은 ef Designer를 사용 하 여 만든 것을 비롯 한 모든 EF 모델에서 작동 합니다.

-   콘솔 응용 프로그램을 만들고이를 **Asyncdemo** 로 호출 합니다.
-   EntityFramework NuGet 패키지 추가
    -   솔루션 탐색기에서 **Asyncdemo** 프로젝트를 마우스 오른쪽 단추로 클릭 합니다.
    -   **NuGet 패키지 관리 ...** 를 선택 합니다.
    -   NuGet 패키지 관리 대화 상자에서 **온라인** 탭을 선택 하 고 **entityframework** 패키지를 선택 합니다.
    -   **설치** 클릭
-   다음 구현으로 **Model.cs** 클래스를 추가 합니다.

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

이제 EF 모델이 있으므로 일부 데이터 액세스를 수행 하는 데 사용 하는 몇 가지 코드를 작성해 보겠습니다.

-   **Program.cs** 의 내용을 다음 코드로 바꿉니다.

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
                    Console.WriteLine("Calling SaveChanges.");
                    db.SaveChanges();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

이 코드는 데이터베이스에 새 **블로그** 를 저장 한 다음 데이터베이스에서 모든 **블로그** 를 검색 하 여 **콘솔**에 출력 하는 **PerformDatabaseOperations** 메서드를 호출 합니다. 그러면 프로그램에서 요일의 견적을 **콘솔**에 기록 합니다.

코드는 동기식 이므로 프로그램을 실행할 때 다음과 같은 실행 흐름을 관찰할 수 있습니다.

1.  **SaveChanges** 는 데이터베이스에 새 **블로그** 를 푸시하는 것으로 시작 합니다.
2.  **SaveChanges** 완료
3.  모든 **블로그의** 쿼리가 데이터베이스로 전송 됨
4.  쿼리가 반환 되 고 결과가 **콘솔** 에 기록 됩니다.
5.  요일의 견적이 **콘솔** 에 기록 됩니다.

![동기화 출력](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>비동기 설정

이제 프로그램을 시작 하 고 실행 했으므로 새 async 및 wait 키워드를 사용할 수 있습니다. Program.cs을 다음과 같이 변경 했습니다.

1.  줄 2: **System.object** 네임 스페이스에 대 한 USING 문은 EF async 확장 메서드에 대 한 액세스를 제공 합니다.
2.  줄 4: **System.object** 네임 스페이스에 대 한 using 문을 사용 하면 **작업** 유형을 사용할 수 있습니다.
3.  12 줄 & 18: **PerformSomeDatabaseOperations** (줄 12)의 진행률을 모니터링 하 고 프로그램 실행을 차단 하는 작업을 캡처한 후 프로그램의 모든 작업이 완료 되 면 (줄 18)이 작업이 완료 됩니다.
4.  25 줄: **PerformSomeDatabaseOperations** 가 **async** 로 표시 되도록 업데이트 하 고 **작업**을 반환 합니다.
5.  줄 35: 이제 비동기 버전의 SaveChanges를 호출 하 고 완료를 기다리는 중입니다.
6.  줄 42: 이제 ToList의 비동기 버전을 호출 하 고 결과를 기다리고 있습니다.

System.object 네임 스페이스에서 사용 가능한 확장 메서드의 포괄적인 목록은 QueryableExtensions 클래스를 참조 하세요. *또한 using 문에 "System.object 사용"을 추가 해야 합니다.*

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

이제 코드는 비동기 이므로 프로그램을 실행할 때 다른 실행 흐름을 관찰할 수 있습니다.

1.  **SaveChanges** 는 명령이 데이터베이스로 전송 된  *후 데이터베이스에 새 블로그를 푸시하는 시작을 시작 합니다. 현재 관리 되는 스레드에 더 이상 계산 시간이 필요 하지 않습니다. **PerformDatabaseOperations** 메서드는 실행이 완료 되지 않은 경우에도를 반환 하 고 Main 메서드의 프로그램 흐름은 계속 됩니다.*
2.  **요일에 대 한 견적은**
    주 메서드에서 수행할 작업이 더 이상 없으므로 콘솔*에 기록 됩니다. 관리 되는 스레드는 데이터베이스 작업이 완료 될 때까지 대기 호출에서 차단 됩니다. 완료 되 면 **PerformDatabaseOperations** 의 나머지가 실행 됩니다.*
3.  **SaveChanges** 완료
4.  데이터베이스에서 쿼리  를 처리 하는 동안에 *는 모든 블로그의 쿼리가 데이터베이스에 다시 전송 되며 관리 되는 스레드는 다른 작업을 수행할 수 있습니다. 다른 모든 실행이 완료 된 후에도 스레드는 대기 호출에서 중단 됩니다.*
5.  쿼리가 반환 되 고 결과가 **콘솔** 에 기록 됩니다.

![비동기 출력](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>요점은

이제 EF의 비동기 메서드를 사용 하는 것이 얼마나 쉬운지 살펴보았습니다. 비동기의 이점은 간단한 콘솔 앱에서 매우 명확 하지 않을 수 있지만, 장기 실행 또는 네트워크 바인딩된 작업이 응용 프로그램을 차단 하거나 많은 수의 스레드를 발생 시킬 수 있는 경우에도 이와 동일한 전략을 적용할 수 있습니다. 메모리 사용 공간을 늘립니다.
