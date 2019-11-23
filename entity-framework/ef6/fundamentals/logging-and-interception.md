---
title: 데이터베이스 작업 로깅 및 가로채기-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 35b0284a5ad8b2b732f074589bd458d243312575
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181667"
---
# <a name="logging-and-intercepting-database-operations"></a>데이터베이스 작업 로깅 및 가로채기
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

Entity Framework 6부터 데이터베이스에 명령을 보낼 Entity Framework 언제 든 지이 명령은 응용 프로그램 코드에 의해 가로챌 수 있습니다. 이는 SQL 로깅을 위해 가장 일반적으로 사용 되지만 명령을 수정 하거나 중단 하는 데 사용할 수도 있습니다.  

구체적으로 말하면 EF에는 다음이 포함 됩니다.  

- DataContext 로그인과 유사한 컨텍스트의 로그 속성 LINQ to SQL  
- 로그로 전송 되는 출력의 내용 및 형식을 사용자 지정 하는 메커니즘  
- 제어/유연성을 제공 하는 가로채기의 하위 수준 구성 요소  

## <a name="context-log-property"></a>컨텍스트 로그 속성  

DbContext 속성은 문자열을 사용 하는 모든 메서드에 대 한 대리자로 설정할 수 있습니다. 가장 일반적으로 사용 되는 모든 TextWriter는 해당 TextWriter의 "Write" 메서드로 설정 합니다. 현재 컨텍스트에서 생성 된 모든 SQL은 해당 기록기에 로깅됩니다. 예를 들어 다음 코드는 콘솔에 SQL을 기록 합니다.  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

컨텍스트를 확인 합니다. .Log가 Console. Write로 설정 됩니다. 이는 콘솔에 SQL을 기록 하는 데 필요 합니다.  

약간의 출력을 볼 수 있도록 몇 가지 간단한 쿼리/삽입/업데이트 코드를 추가 해 보겠습니다.  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    var blog = context.Blogs.First(b => b.Title == "One Unicorn");

    blog.Posts.First().Title = "Green Eggs and Ham";

    blog.Posts.Add(new Post { Title = "I do not like them!" });

    context.SaveChangesAsync().Wait();
}
```  

그러면 다음과 같은 출력이 생성 됩니다.  

``` SQL
SELECT TOP (1)
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title]
    FROM [dbo].[Blogs] AS [Extent1]
    WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 4 ms with result: SqlDataReader

SELECT
    [Extent1].[Id] AS [Id],
    [Extent1].[Title] AS [Title],
    [Extent1].[BlogId] AS [BlogId]
    FROM [dbo].[Posts] AS [Extent1]
    WHERE [Extent1].[BlogId] = @EntityKeyValue1
-- EntityKeyValue1: '1' (Type = Int32)
-- Executing at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader

UPDATE [dbo].[Posts]
SET [Title] = @0
WHERE ([Id] = @1)
-- @0: 'Green Eggs and Ham' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 12 ms with result: 1

INSERT [dbo].[Posts]([Title], [BlogId])
VALUES (@0, @1)
SELECT [Id]
FROM [dbo].[Posts]
WHERE @@ROWCOUNT > 0 AND [Id] = scope_identity()
-- @0: 'I do not like them!' (Type = String, Size = -1)
-- @1: '1' (Type = Int32)
-- Executing asynchronously at 10/8/2013 10:55:41 AM -07:00
-- Completed in 2 ms with result: SqlDataReader
```  

이는 데이터베이스 초기화가 이미 발생 한 경우의 출력입니다. 데이터베이스 초기화가 아직 발생 하지 않은 경우에는 새 데이터베이스를 확인 하거나 만들 때 모든 작업에서 마이그레이션을 수행 하는 것을 보여 주는 훨씬 더 많은 출력이 있습니다.  

## <a name="what-gets-logged"></a>기록 되는 항목  

Log 속성이 설정 되 면 다음 내용이 모두 로깅됩니다.  

- 모든 유형의 명령에 대 한 SQL 예를 들면 다음과 같습니다.  
    - 일반 LINQ 쿼리, eSQL 쿼리 및 SqlQuery와 같은 메서드의 원시 쿼리를 포함 하는 쿼리  
    - SaveChanges의 일부로 생성 된 삽입, 업데이트 및 삭제  
    - 지연 로드에 의해 생성 된 것과 같은 관계 로딩 쿼리  
- 매개 변수  
- 명령이 비동기적으로 실행 되 고 있는지 여부  
- 명령의 실행이 시작 된 시간을 나타내는 타임 스탬프입니다.  
- 명령이 성공적으로 완료 되었는지, 예외를 throw 하 여 실패 했는지, 비동기의 경우가 취소 되었는지 여부  
- 결과 값의 일부 표시  
- 명령을 실행 하는 데 걸린 대략적인 시간입니다. 이는 명령을 전송 하 여 결과 개체를 다시 가져오는 시간입니다. 결과를 읽을 시간은 포함 되지 않습니다.  

위의 예제 출력을 살펴보면 로깅되는 네 개의 명령 각각은 다음과 같습니다.  

- 컨텍스트 호출로 인해 발생 하는 쿼리입니다. 블로그. 첫 번째  
    - "First"는 ToString을 호출할 수 있는 IQueryable을 제공 하지 않으므로 SQL을 가져오는 ToString 메서드가이 쿼리에 대해 작동 하지 않습니다.  
- 블로그의 지연 로드로 인해 발생 하는 쿼리입니다. 게시물과  
    - 지연 로드가 발생 하는 키 값에 대 한 매개 변수 세부 정보를 확인 합니다.  
    - 기본값이 아닌 값으로 설정 된 매개 변수의 속성만 로깅됩니다. 예를 들어 Size 속성은 0이 아닌 경우에만 표시 됩니다.  
- SaveChangesAsync에서 생성 되는 두 명령 하나는 게시 제목을 변경 하는 업데이트이 고, 다른 하나는 삽입 하 여 새 게시물을 추가 합니다.  
    - FK 및 Title 속성에 대 한 매개 변수 세부 정보를 확인 합니다.  
    - 이러한 명령이 비동기적으로 실행 되 고 있음을 확인 합니다.  

## <a name="logging-to-different-places"></a>다른 위치에 로깅  

위에서 설명한 것 처럼 콘솔에 대 한 로깅은 매우 간단 합니다. 다른 종류의 [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)를 사용 하 여 메모리, 파일 등에 쉽게 기록할 수도 있습니다.  

LINQ to SQL에 대해 잘 알고 있는 경우에는 LINQ to SQL에서 Log 속성이 실제 TextWriter 개체 (예: Console)로 설정 되어 있는 동안 Log 속성이 문자열을 허용 하는 메서드 (예:)로 설정 된 것을 알 수 있습니다. , Console. Write 또는 Console을 입력 합니다. 그 이유는 문자열에 대 한 싱크로 사용할 수 있는 대리자를 허용 하 여 TextWriter에서 EF를 분리 하는 것입니다. 예를 들어, 일부 로깅 프레임 워크가 이미 있고 다음과 같은 로깅 메서드를 정의 한다고 가정해 보겠습니다.  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

이는 다음과 같이 EF Log 속성에 후크 될 수 있습니다.  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

## <a name="result-logging"></a>결과 로깅  

기본로 거는 명령이 데이터베이스에 전송 되기 전에 타임 스탬프가 있는 명령 텍스트 (SQL), 매개 변수 및 "실행 중" 줄을 기록 합니다. 경과 된 시간을 포함 하는 "완료 됨" 줄은 명령 실행 후에 기록 됩니다.  

비동기 명령의 경우 비동기 작업이 실제로 완료, 실패 또는 취소 될 때까지 "완료 됨" 줄이 기록 되지 않습니다.  

"Completed" 줄에는 명령 유형 및 실행 성공 여부에 따라 다른 정보가 포함 됩니다.  

### <a name="successful-execution"></a>성공한 실행  

성공적으로 완료 된 명령의 경우 "완료 된 결과:"로 완료 된 출력에는 결과를 표시 합니다. 데이터 판독기를 반환 하는 명령의 경우 결과 표시는 반환 되는 [Dbdatareader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) 의 유형입니다. 위에 표시 된 update 명령과 같은 정수 값을 반환 하는 명령의 경우에는 해당 정수를 표시 합니다.  

### <a name="failed-execution"></a>실패 한 실행  

예외를 throw 하 여 실패 한 명령의 경우 출력에 예외의 메시지가 포함 됩니다. 예를 들어 SqlQuery를 사용 하 여 존재 하는 테이블을 쿼리하면 로그 출력이 다음과 같이 나타납니다.  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

### <a name="canceled-execution"></a>취소 된 실행  

태스크가 취소 된 비동기 명령의 경우 예외가 발생 하 여 결과가 실패할 수 있습니다 .이는 취소 하려고 할 때 기본 ADO.NET 공급자가 자주 수행 하는 작업입니다. 이 문제가 발생 하지 않고 작업이 완전히 취소 되 면 출력은 다음과 같이 표시 됩니다.  

```console
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>로그 콘텐츠 및 서식 변경  

아래에서 데이터베이스 .Log 속성은 DatabaseLogFormatter 개체를 사용 합니다. 이 개체는 문자열과 DbContext을 허용 하는 대리자에 IDbCommandInterceptor 구현 (아래 참조)을 효과적으로 바인딩합니다. 즉, DatabaseLogFormatter의 메서드는 EF에서 명령을 실행 하기 전과 후에 호출 됩니다. 이러한 DatabaseLogFormatter 메서드는 로그 출력을 수집 하 고 서식을 지정 하 여 대리자에 게 보냅니다.  

### <a name="customizing-databaselogformatter"></a>DatabaseLogFormatter 사용자 지정  

DatabaseLogFormatter에서 파생 되는 새 클래스를 만들고 메서드를 적절 하 게 재정의 하 여 로깅되는 내용 및 형식을 지정 하는 방법을 변경할 수 있습니다. 재정의할 가장 일반적인 방법은 다음과 같습니다.  

- LogCommand –이를 재정의 하 여 명령이 실행 되기 전에 기록 되는 방법을 변경 합니다. 기본적으로 LogCommand는 각 매개 변수에 대해 Logcommand를 호출 합니다. 대신 재정의 또는 핸들 매개 변수에서 동일한 작업을 수행 하도록 선택할 수 있습니다.  
- LogResult –이를 재정의 하 여 명령 실행 결과를 기록 하는 방법을 변경 합니다.  
- LogParameter –이를 재정의 하 여 매개 변수 로깅의 형식 및 내용을 변경 합니다.  

예를 들어 각 명령이 데이터베이스로 전송 되기 전에 한 줄만 기록 하려는 경우를 가정해 보겠습니다. 이 작업은 다음 두 가지 재정의를 통해 수행할 수 있습니다.  

- LogCommand를 재정의 하 여 SQL의 단일 줄 서식 지정 및 작성  
- LogResult를 재정의 하 여 아무 작업도 수행 하지 않습니다.  

코드는 다음과 같습니다.

``` csharp
public class OneLineFormatter : DatabaseLogFormatter
{
    public OneLineFormatter(DbContext context, Action<string> writeAction)
        : base(context, writeAction)
    {
    }

    public override void LogCommand<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        Write(string.Format(
            "Context '{0}' is executing command '{1}'{2}",
            Context.GetType().Name,
            command.CommandText.Replace(Environment.NewLine, ""),
            Environment.NewLine));
    }

    public override void LogResult<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
    }
}
```  

출력을 기록 하려면 쓰기 메서드를 호출 하기만 하면 됩니다 .이 메서드는 구성 된 쓰기 대리자로 출력을 보냅니다.  

(이 코드는 예제 처럼 줄 바꿈 제거를 단순화 합니다. 복잡 한 SQL을 볼 때 제대로 작동 하지 않을 수 있습니다.)  

### <a name="setting-the-databaselogformatter"></a>DatabaseLogFormatter 설정  

새 DatabaseLogFormatter 클래스를 만든 후에는 EF를 사용 하 여 등록 해야 합니다. 코드 기반 구성을 사용 하 여이 작업을 수행 합니다. 즉, DbContext 클래스와 동일한 어셈블리의 DbConfiguration에서 파생 되는 새 클래스를 만든 후이 새 클래스의 생성자에서 SetDatabaseLogFormatter를 호출 하는 것을 의미 합니다. 예를 들면 다음과 같습니다.  

``` csharp
public class MyDbConfiguration : DbConfiguration
{
    public MyDbConfiguration()
    {
        SetDatabaseLogFormatter(
            (context, writeAction) => new OneLineFormatter(context, writeAction));
    }
}
```  

### <a name="using-the-new-databaselogformatter"></a>새 DatabaseLogFormatter 사용  

이 새로운 DatabaseLogFormatter는 이제 데이터베이스 .Log가 설정 될 때마다 사용 됩니다. 따라서 1 부에서 코드를 실행 하면 다음과 같은 결과가 출력 됩니다.  

```console
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>가로채기 구성 요소  

지금까지 DbContext를 사용 하 여 EF에 의해 생성 된 SQL을 기록 하는 방법을 살펴보았습니다. 그러나이 코드는 보다 일반적인 가로채기를 위해 일부 하위 수준 구성 요소에 비해 비교적 씬 외관입니다.  

### <a name="interception-interfaces"></a>가로채기 인터페이스  

가로채기 코드는 가로채기 인터페이스의 개념을 중심으로 빌드됩니다. 이러한 인터페이스는 IDbInterceptor에서 상속 하며 EF가 일부 작업을 수행할 때 호출 되는 메서드를 정의 합니다. 이는 가로채는 개체 형식 마다 하나의 인터페이스를 포함 하는 것입니다. 예를 들어 IDbCommandInterceptor 인터페이스는 EF가 ExecuteNonQuery, ExecuteScalar, ExecuteReader 및 관련 된 메서드에 대 한 호출을 수행 하기 전에 호출 되는 메서드를 정의 합니다. 마찬가지로이 인터페이스는 이러한 각 작업이 완료 될 때 호출 되는 메서드를 정의 합니다. 위에서 살펴본 DatabaseLogFormatter 클래스는 명령을 기록 하기 위해이 인터페이스를 구현 합니다.  

### <a name="the-interception-context"></a>가로채기 컨텍스트입니다.  

인터셉터 인터페이스에 정의 된 메서드를 살펴보면 모든 호출에 DbInterceptionContext 형식의 개체 또는 DbCommandInterceptionContext\<\>와 같이이에서 파생 된 일부 형식의 개체가 지정 되는 것을 알 수 있습니다. 이 개체는 EF에서 수행 하는 동작에 대 한 컨텍스트 정보를 포함 합니다. 예를 들어 DbContext를 대신 하 여 작업을 수행 하는 경우 DbContext는 DbInterceptionContext에 포함 됩니다. 마찬가지로, 비동기적으로 실행 되는 명령의 경우 IsAsync 플래그는 DbCommandInterceptionContext에 설정 됩니다.  

### <a name="result-handling"></a>결과 처리  

DbCommandInterceptionContext\<\> 클래스에는 Result, OriginalResult, Exception 및 Originalresult 이라는 속성이 포함 되어 있습니다. 이러한 속성은 작업이 실행 되기 전에 호출 되는 가로채기 메서드를 호출 하는 경우 null/0으로 설정 됩니다. 즉, ... 메서드를 실행 합니다. 작업이 실행 되 고 성공 하면 Result와 OriginalResult가 작업의 결과로 설정 됩니다. 이러한 값은 작업이 실행 된 후 호출 되는 가로채기 메서드에서 관찰 될 수 있습니다. 즉, ... 메서드를 실행 했습니다. 마찬가지로 작업에서을 throw 하는 경우 Exception 및 OriginalException 속성이 설정 됩니다.  

#### <a name="suppressing-execution"></a>실행 억제  

인터셉터가 명령 실행 전에 결과 속성을 설정 하는 경우 (... 메서드를 실행 하는 경우 EF는 실제로 명령을 실행 하려고 시도 하지 않고 단순히 결과 집합을 사용 합니다. 즉, 인터셉터는 명령이 실행 되지 않도록 할 수 있지만 명령이 실행 된 것 처럼 EF를 계속 합니다.  

이를 사용 하는 방법의 예는 일반적으로 래핑 공급자를 사용 하 여 수행 된 명령 일괄 처리입니다. 인터셉터는 나중에 일괄 처리로 명령을 저장 하지만 명령이 정상적으로 실행 되었음을 EF로 "간주" 합니다. 일괄 처리를 구현 하는 데이 두 가지 이상의 작업이 필요 하지만이는 가로채기 결과를 변경 하는 방법의 예입니다.  

다음 중 하나에서 예외 속성을 설정 하 여 실행을 억제할 수도 있습니다. 메서드를 실행 합니다. 이렇게 하면 지정 된 예외를 throw 하 여 작업 실행이 실패 한 것 처럼 EF가 계속 됩니다. 이로 인해 응용 프로그램의 작동이 중단 될 수 있지만 일시적인 예외 또는 EF에서 처리 하는 다른 예외가 발생할 수도 있습니다. 예를 들어 명령 실행이 실패 하면 테스트 환경에서 응용 프로그램의 동작을 테스트 하는 데 사용할 수 있습니다.  

#### <a name="changing-the-result-after-execution"></a>실행 후 결과 변경  

명령을 실행 한 후 인터셉터가 결과 속성을 설정 하는 경우 (... 실행 된 메서드) EF는 실제로 작업에서 반환 된 결과 대신 변경 된 결과를 사용 합니다. 마찬가지로, 인터셉터에서 명령이 실행 된 후에 예외 속성을 설정 하는 경우 EF는 작업이 예외를 throw 한 것 처럼 set 예외를 throw 합니다.  

인터셉터는 예외를 throw 하지 않아야 함을 나타내기 위해 Exception 속성을 null로 설정할 수도 있습니다. 이는 작업 실행이 실패 한 경우에 유용할 수 있지만 인터셉터가 작업에 성공한 것 처럼 계속 진행 합니다. 이 경우에도 EF가 계속 해 서 작동 하는 결과 값을 갖도록 결과를 설정 해야 합니다.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult 및 Originalresult  

EF가 작업을 실행 한 후 실행이 실패 하면 Result 및 OriginalResult 속성을 설정 하 고, 예외와 함께 실행이 실패 한 경우에는 Exception 및 Originalresult 속성을 설정 합니다.  

OriginalResult 및 Originalresult 속성은 읽기 전용 이며 실제로 작업을 실행 한 후에만 EF에 의해 설정 됩니다. 이러한 속성은 인터셉터로 설정할 수 없습니다. 즉, 모든 인터셉터는 작업이 실행 될 때 발생 한 실제 예외 또는 결과가 아닌 다른 인터셉터에 의해 설정 된 예외 나 결과를 구분할 수 있습니다.  

### <a name="registering-interceptors"></a>인터셉터 등록  

하나 이상의 가로채기 인터페이스를 구현 하는 클래스를 만든 후에는 DbInterception 클래스를 사용 하 여 EF에 등록할 수 있습니다. 예를 들면 다음과 같습니다.  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

DbConfiguration 코드 기반 구성 메커니즘을 사용 하 여 앱 도메인 수준에서 인터셉터를 등록할 수도 있습니다.  

### <a name="example-logging-to-nlog"></a>예: NLog에 로깅  

여기에 모두 IDbCommandInterceptor 및 [Nlog](https://nlog-project.org/) 를 사용 하는 예제를 살펴보겠습니다.  

- 비 비동기식으로 실행 되는 명령에 대 한 경고 기록  
- 실행 시 throw 되는 명령에 대 한 오류를 기록 합니다.  

다음은 위와 같이 등록 해야 하는 로깅을 수행 하는 클래스입니다.  

``` csharp
public class NLogCommandInterceptor : IDbCommandInterceptor
{
    private static readonly Logger Logger = LogManager.GetCurrentClassLogger();

    public void NonQueryExecuting(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void NonQueryExecuted(
        DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ReaderExecuting(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ReaderExecuted(
        DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    public void ScalarExecuting(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfNonAsync(command, interceptionContext);
    }

    public void ScalarExecuted(
        DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
    {
        LogIfError(command, interceptionContext);
    }

    private void LogIfNonAsync<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (!interceptionContext.IsAsync)
        {
            Logger.Warn("Non-async command used: {0}", command.CommandText);
        }
    }

    private void LogIfError<TResult>(
        DbCommand command, DbCommandInterceptionContext<TResult> interceptionContext)
    {
        if (interceptionContext.Exception != null)
        {
            Logger.Error("Command {0} failed with exception {1}",
                command.CommandText, interceptionContext.Exception);
        }
    }
}
```  

이 코드는 가로채기 컨텍스트를 사용 하 여 명령이 비동기적으로 실행 되지 않는 경우를 검색 하 고 명령을 실행 하는 동안 오류가 발생 한 경우를 검색 합니다.  
