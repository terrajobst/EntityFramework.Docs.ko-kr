---
title: 로깅 및 데이터베이스 작업-EF6 가로채기
author: divega
ms.date: 2016-10-23
ms.assetid: b5ee7eb1-88cc-456e-b53c-c67e24c3f8ca
ms.openlocfilehash: 2e16502abf54be3f3b2f63fe69d2605ef13dea27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994637"
---
# <a name="logging-and-intercepting-database-operations"></a>로깅 및 데이터베이스 작업 가로채기
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

Entity Framework 명령을 데이터베이스에이 명령을 보냅니다 언제 든 지 Entity Framework 6을 사용 하 여 시작 하는 응용 프로그램 코드에서 가로챌 수 있습니다. 이 SQL 로깅을 위한 가장 많이 사용 됩니다 있지만 수정 또는 중단 명령을 사용할 수도 있습니다.  

특히, EF에 포함 됩니다.  

- LINQ to SQL에서에서 DataContext.Log 비슷합니다 컨텍스트에 대 한 로그 속성  
- 콘텐츠 및 로그로 전송 하는 출력의 서식 지정을 사용자 지정 하는 메커니즘  
- 컨트롤/유연성을 제공 하는 인터 셉 션 용 낮은 수준의 구성 요소  

## <a name="context-log-property"></a>상황에 맞는 로그 속성  

문자열을 사용 하는 모든 메서드에 대 한 대리자를 DbContext.Database.Log 속성을 설정할 수 있습니다. 가장 일반적으로 "Write" 메서드는 textwriter로 설정 하 여 모든 TextWriter를 사용 하 여 사용 됩니다. 현재 컨텍스트에서 생성 된 모든 SQL 기록기에 기록 됩니다. 예를 들어, 다음 코드를 콘솔에 SQL 로깅됩니다.  

``` csharp
using (var context = new BlogContext())
{
    context.Database.Log = Console.Write;

    // Your code here...
}
```  

해당 컨텍스트를 확인 합니다. Database.Log은 Console.Write로 설정 됩니다. 이것이 SQL 콘솔에 로그인 하는 데 필요한 모든입니다.  

### <a name="example-output"></a>예제 출력  

일부 출력을 볼 수 있도록 몇 가지 간단한 쿼리/삽입/업데이트 코드를 추가 해 보겠습니다.  

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

다음과 같은 출력이 생성 됩니다.  

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

(이 모든 데이터베이스 초기화 이미 발생 한 것으로 가정 하는 출력 note 합니다. 데이터베이스 초기화 있을 것을 이미 오류가 발생 하지 하는 경우 모든 작업 마이그레이션을 보여 주는 더 많은 출력은 내부적으로 확인 하거나 새 데이터베이스를 만듭니다.)  

### <a name="what-gets-logged"></a>로그를 가져옵니다.  

다음의 모든 로그 속성이 설정 된 경우 기록 됩니다.  

- SQL 명령의 모든 다양 한 종류입니다. 예를 들어:  
    - 기본 LINQ 쿼리, eSQL 쿼리 및 SqlQuery 같은 방법 중에서 원시 쿼리를 포함 하 여 쿼리  
    - 삽입, 업데이트 및 삭제 SaveChanges의 일부로 생성  
    - 예: 지연 로드 하 여 생성 된 쿼리를 로드 하는 관계  
- 매개 변수  
- 명령을 비동기적으로 실행 되는 여부  
- 명령 실행을 시작할 때를 나타내는 타임 스탬프  
- 명령이 성공적으로 완료 된 예외를 throw 하 여 또는 비동기에 대 한 실패 여부 취소 되었습니다.  
- 결과 값의 일부 표시  
- 명령을 실행 하는 데 걸리는 시간의 대략적인 크기입니다. 이 결과 개체를 다시 가져오는 중에 명령을 보내기에서 시간 note 합니다. 결과 읽을 시간이 포함 되지 않습니다.  

위 예제 출력을 보면 각 기록 4 개 명령은 다음과 같습니다.  

- 컨텍스트에 대 한 호출에서 발생 하는 쿼리. Blogs.First  
    - 이후이 쿼리에 대 한 SQL 가져오기의 ToString 메서드는 작동 하지는 "First"는 ToString 호출 IQueryable 제공 하지 않습니다.  
- 블로그의 지연 로드에서 발생 하는 쿼리. 게시물  
    - 발생 하는 지연 로드에 대 한 키 값에 대 한 매개 변수 세부 정보는 통지  
    - 기본이 아닌 값으로 설정 된 매개 변수 속성만 기록 됩니다. 예를 들어, 크기 속성은 0이 아닌 경우 것을 표시만 됩니다.  
- SaveChangesAsync;에서 발생 하는 두 개의 명령 새 게시를 추가 하려면 삽입에 대 한 다른 게시물 제목을 변경에 대 한 업데이트에 대 한  
    - FK 및 Title 속성에 대 한 매개 변수 세부 정보를 확인 합니다.  
    - 이러한 명령을 비동기적으로 실행 되는 알 수 있습니다.  

### <a name="logging-to-different-places"></a>다른 위치에 로깅  

로깅이 위에 표시 된 대로 콘솔은 아주 간단 합니다. 메모리, 파일 등 다양 한 종류를 사용 하 여 로그인에 쉽습니다입니다 [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx)합니다.  

익숙한 경우 linq to SQL에서 LINQ to SQL Log 속성은 실제 TextWriter 개체로 설정 (예: Console.Out) 하는 동안 로그 설정 (예: 문자열을 허용 하는 메서드에 EF의 않았음을 알 수 있습니다 Console.Write 또는 Console.Out.Write). 이러한 이유로 문자열에 대 한 싱크로 작동할 수 있는 모든 대리자를 그대로 사용 하 여 EF에서 TextWriter 분리 하기 위해서입니다. 예를 들어, 일부 로깅 프레임 워크를 이미 있는 고 로깅 메서드를 정의 한다고 가정 다음과 같이 합니다.  

``` csharp
public class MyLogger
{
    public void Log(string component, string message)
    {
        Console.WriteLine("Component: {0} Message: {1} ", component, message);
    }
}
```  

이 같이 EF Log 속성을 연결할 수 없습니다.  

``` csharp
var logger = new MyLogger();
context.Database.Log = s => logger.Log("EFApp", s);
```  

### <a name="result-logging"></a>결과 로깅  

기본으로 거 명령이 데이터베이스에 전송 되기 전에 타임 스탬프를 사용 하 여 SQL 명령 텍스트, 매개 변수 및 "실행" 줄 기록 합니다. 경과 된 시간을 포함 하 "완료 됨된" 줄이 기록 된 다음 명령 실행 합니다.  

비동기 작업이 실제로 완료, 실패 되거나 취소 될 때까지 비동기에 대 한 "완료 됨된" 줄을 명령에 기록 되지 않습니다.  

"완료 됨된" 줄 유형 명령 및 실행 성공 여부에 따라 다른 정보를 포함 합니다.  

#### <a name="successful-execution"></a>성공적으로 실행  

명령의 출력을 성공적으로 완료 하는 디렉터리는 "결과 사용 하 여 ms x에서 완료:" 뒤에 결과 표시 합니다. 결과 데이터 판독기를 반환 하는 명령 표시의 형식인 [DbDataReader](https://msdn.microsoft.com/library/system.data.common.dbdatareader.aspx) 반환 합니다. 업데이트와 같은 정수 값을 반환 하는 명령에 대 한 표시 된 것 처럼 위에 표시 된 명령에는 해당 정수입니다.  

#### <a name="failed-execution"></a>실패 한 실행  

예외가 발생 하면 실패 하는 명령에 대 한 출력 메시지는 예외를 포함 합니다. 예를 들어, SqlQuery 존재 하는 테이블에 대해 쿼리를 사용 하 여 결과 로그에서 출력 같이:  

``` SQL
SELECT * from ThisTableIsMissing
-- Executing at 5/13/2013 10:19:05 AM
-- Failed in 1 ms with error: Invalid object name 'ThisTableIsMissing'.
```  

#### <a name="canceled-execution"></a>취소 된 실행  

작업이 취소 되는 비동기 명령에 대 한 기본 ADO.NET 공급자 자주 수행 취소 하려고 시도 하는 경우 이므로 결과 예외를 사용 하 여 오류를 수 있습니다. 그렇지 않은 경우 출력은 다음과 같아야 하는 다음 작업이 정상적으로 취소 되:  

```  
update Blogs set Title = 'No' where Id = -1
-- Executing asynchronously at 5/13/2013 10:21:10 AM
-- Canceled in 1 ms
```  

## <a name="changing-log-content-and-formatting"></a>변경 로그 콘텐츠 및 서식 지정  

### <a name="databaselogformatter"></a>DatabaseLogFormatter  

내부적으로 속성을 사용 하면 Database.Log DatabaseLogFormatter 개체를 사용 합니다. 이 개체는 효과적으로 문자열 및 DbContext를 허용 하는 대리자를 IDbCommandInterceptor 구현을 (아래 참조)를 바인딩합니다. 즉, DatabaseLogFormatter 메서드는 호출 명령이 실행 전후 EF에서. 이러한 DatabaseLogFormatter 메서드 수집 로그 출력 형식 및 대리자에 게 보냅니다.  

### <a name="customizing-databaselogformatter"></a>사용자 지정 DatabaseLogFormatter  

로깅되는 및의 서식 지정 방법을 변경 DatabaseLogFormatter에서 파생 되 고 적절 하 게 메서드를 재정의 하는 새 클래스를 만들어 구현할 수 있습니다. 재정의 하는 가장 일반적인 방법은 다음과 같습니다.  

- LogCommand-이 명령을 실행 하기 전에 기록 되는 방식 변경 설정을 재정의 합니다. LogParameter; 각 매개 변수에 대해 LogCommand 기본적으로 호출 재정의에서 동일한 작업을 수행 하거나 매개 변수를 다르게 처리 하도록 선택할 수 있습니다.  
- LogResult – 명령 실행에서 결과 기록 되는 방식을 변경 하려면이 옵션을 재정의 합니다.  
- LogParameter – 서식 및 매개 변수 로깅의 내용을 변경 하려면이 옵션을 재정의 합니다.  

예를 들어, 각 명령을 데이터베이스에 전송 되기 전에 한 줄을 기록할 한다고 가정해 보겠습니다. 이 두 가지 재정의 사용 하 여 수행할 수 있습니다.  

- 서식을 지정 하 고 SQL 단일 줄 LogCommand 재정의  
- 아무 작업도 하지 않으려면 LogResult를 재정의 합니다.  

코드를 다음과 같이 보입니다.

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

기록할 단순히 호출 구성된 쓰기 대리자에 출력을 보내는 Write 메서드에 출력 합니다.  

(이 코드 예제 처럼 간단한 줄 바꿈 제거 않은 것을 참고 합니다. 가능성이 작동 하지 않습니다 복잡 한 SQL 보기에 적합 합니다.)  

### <a name="setting-the-databaselogformatter"></a>DatabaseLogFormatter 설정  

EF를 사용 하 여 등록할 새 DatabaseLogFormatter 클래스를 만든 후에 해당 해야 합니다. 이렇게 하는 코드 기반 구성을 사용 하 여 합니다. 간단히 말해이 DbContext 클래스와 동일한 어셈블리에서 DbConfiguration에서 파생 되는 새 클래스를 만들고 호출한 다음이 새 클래스의 생성자에서 SetDatabaseLogFormatter 의미 합니다. 예를 들어:  

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

### <a name="using-the-new-databaselogformatter"></a>새 DatabaseLogFormatter를 사용 하 여  

이 새 DatabaseLogFormatter Database.Log 설정 되어 언제 든 지 이제 사용 됩니다. 따라서 파트 1에서에서 코드를 실행 합니다. 이제 하면 다음과 같은 출력 합니다.  

```  
Context 'BlogContext' is executing command 'SELECT TOP (1) [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title]FROM [dbo].[Blogs] AS [Extent1]WHERE (N'One Unicorn' = [Extent1].[Title]) AND ([Extent1].[Title] IS NOT NULL)'
Context 'BlogContext' is executing command 'SELECT [Extent1].[Id] AS [Id], [Extent1].[Title] AS [Title], [Extent1].[BlogId] AS [BlogId]FROM [dbo].[Posts] AS [Extent1]WHERE [Extent1].[BlogId] = @EntityKeyValue1'
Context 'BlogContext' is executing command 'update [dbo].[Posts]set [Title] = @0where ([Id] = @1)'
Context 'BlogContext' is executing command 'insert [dbo].[Posts]([Title], [BlogId])values (@0, @1)select [Id]from [dbo].[Posts]where @@rowcount > 0 and [Id] = scope_identity()'
```  

## <a name="interception-building-blocks"></a>인터 셉 션 구성 요소  

지금까지 EF에서 생성 된 SQL을 기록할 DbContext.Database.Log를 사용 하는 방법을 살펴봤습니다. 하지만이 코드는 비교적 씬 외관 실제로 보다 일반적인 인터 셉 션에 대 한 일부 낮은 수준의 구성 요소입니다.  

### <a name="interception-interfaces"></a>인터 셉 션 인터페이스  

인터 셉 션 코드는 인터 셉 션 인터페이스의 개념을 기반으로 합니다. 이러한 인터페이스 IDbInterceptor에서 상속 하 고 EF 작업을 수행할 때 호출 되는 메서드를 정의 합니다. 의도 형식의 개체를 가로 챘 당 하나의 인터페이스입니다. 예를 들어 IDbCommandInterceptor 인터페이스는 EF가 ExecuteNonQuery, ExecuteScalar, ExecuteReader, 및 관련된 메서드를 호출 하기 전에 호출 되는 메서드를 정의 합니다. 마찬가지로, 인터페이스는 이러한 각 작업에 완료 될 때 호출 되는 메서드를 정의 합니다. 위에서 살펴본 DatabaseLogFormatter 클래스 명령을 기록이 인터페이스를 구현 합니다.  

### <a name="the-interception-context"></a>인터 셉 션 컨텍스트  

인터셉터 인터페이스를 정의 하는 방법을 검색 하는 것은 호출할 때마다 DbInterceptionContext 형식의 개체 또는 형식 지정에서 파생 되 DbCommandInterceptionContext 같은 명백한\<\>합니다. 이 개체는 EF 수행 되는 작업에 대 한 컨텍스트 정보를 포함 합니다. 예를 들어, DbContext를 대신 하 여 작업 수행 되는 경우 DbContext는 DbInterceptionContext에 포함 됩니다. 마찬가지로, 비동기적으로 실행 되는 명령에 대 한 IsAsync 플래그 DbCommandInterceptionContext에 설정 됩니다.  

### <a name="result-handling"></a>결과 처리  

DbCommandInterceptionContext\< \> 클래스 결과, OriginalResult, 예외 및 OriginalException를 호출 하는 속성을 포함 합니다. 이러한 속성은 작업이 실행 되기 전에 호출 되는 인터 셉 션 메서드 호출에 대 한 null/0으로 설정-즉,에 대 한 합니다... 메서드를 실행 합니다. 작업이 실행 되 고 성공 하면 다음 결과 및 OriginalResult으로 설정 됩니다는 작업의 결과. 이러한 값의 작업을 실행 한 후 호출 되는 인터 셉 션 메서드에서 관찰할 수 있습니다-즉, 합니다... 메서드를 실행된 합니다. 마찬가지로, 작업을 throw 하는 경우 다음 예외 및 OriginalException 속성을 설정 됩니다.  

#### <a name="suppressing-execution"></a>실행을 표시 하지 않기  

경우 인터셉터는 명령이 실행 되기 전에 결과 속성을 설정 하는 (중 하나에... 메서드를 실행 하는) 다음 EF는 실제로 명령을 실행 하려고 하지 않습니다 하지만 결과 집합 대신 사용 하면 됩니다. 즉,는 인터셉터 명령 실행을 표시 하지 않을 수 있지만 명령이 실행 된 것 처럼 EF가 됩니다.  

이 사용할 수 있는 방법의 예로 일괄 처리 명령이 래핑 공급자를 사용 하 여 일반적으로 수행 되었습니다. 인터셉터 일괄 처리로 나중에 실행할 명령을 저장 하지만 "보안상" EF 명령을 정상적으로 실행 된 합니다. Note는 일괄 처리를 구현 하려면 이보다 더 필요 하지만 변경 인터 셉 션 결과 사용할 수 있는 방법의 예제입니다.  

실행 중에 Exception 속성을 설정 하 여도 무시 될 수는 중... 메서드를 실행 합니다. 이렇게 하면 지정 된 예외를 throw 하 여 작업의 실행이 실패 했습니다 처럼 계속 하는 EF 합니다. 이 수를 당연히 응용 프로그램 충돌을 않지만 일시적인 예외 또는 일부 기타 예외가 EF에서 처리 되는 수도 있습니다. 예를 들어이 사용할 수 있습니다 테스트 환경에서 명령 실행이 실패 하는 경우 응용 프로그램의 동작을 테스트 하려면.  

#### <a name="changing-the-result-after-execution"></a>실행 후 결과 변경합니다.  

경우 인터셉터는 명령 실행 후 결과 속성을 설정 하는 (중 하나에... 메서드 실행) EF 변경된 결과 사용 하 여 실제로 작업에서 반환 된 결과 대신 됩니다. 마찬가지로, 명령이 실행 후 예외 속성을 설정 하는 인터셉터를 하는 경우 다음 EF 예외를 throw 합니다를 집합으로 작업 예외를 throw 했습니다.  

인터셉터 없는 예외를 throw 해야를 나타내기 위해 null로 Exception 속성을 설정할 수도 있습니다. 이 작업을 실행 하지 못했지만 인터셉터가 계속 작업이 성공한 것 처럼 EF 경우 유용할 수 있습니다. 일반적으로 또한 여기에 EF에서 일부 결과 값으로 계속 작업할 수 있도록 결과 설정 합니다.  

#### <a name="originalresult-and-originalexception"></a>OriginalResult 및 OriginalException  

EF는 작업 실행 후 예외를 사용 하 여 실행에 실패 한 실행을 실패 하지 않은 경우의 결과 및 OriginalResult 속성 또는 예외 및 OriginalException 속성을 설정 됩니다.  

OriginalResult 및 OriginalException 속성을 읽기 전용 이며만 실제로 작업을 실행 한 후 EF에서 설정 됩니다. 인터셉터에서 이러한 속성을 설정할 수 없습니다. 이 모든 인터셉터는 예외 또는 실제 예외 대신 일부 다른 인터셉터가 설정 된 결과 나 작업이 실행 될 때 발생 한 결과 구분할 수는 것을 의미 합니다.  

### <a name="registering-interceptors"></a>인터셉터를 등록합니다.  

인터 셉 션 인터페이스 중 하나 이상을 구현 하는 클래스 만들어지면 DbInterception 클래스를 사용 하 여 EF를 사용 하 여 등록할 수 있습니다. 예를 들어:  

``` csharp
DbInterception.Add(new NLogCommandInterceptor());
```  

인터셉터는 DbConfiguration 코드 기반 구성 메커니즘을 사용 하 여 응용 프로그램 도메인 수준에서 등록할 수 있습니다.  

### <a name="example-logging-to-nlog"></a>예: NLog에 로깅  

보겠습니다이 모든 예제를 사용 하 여 해당 IDbCommandInterceptor 및 [NLog](http://nlog-project.org/) 하려면:  

- 비 비동기적으로 실행 되는 모든 명령에 대 한 경고를 기록 합니다.  
- 실행 될 때 throw 되는 모든 명령에 대 한 오류를 기록  

위에 표시 된 대로 등록 해야 하는 로깅을 수행 하는 클래스는 다음과 같습니다.  

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

명령을 비 비동기적으로 실행 되는 경우를 검색 하 고 명령을 실행 하는 오류가 있을 때 발견 하이 코드는 인터 셉 션 컨텍스트를 사용 하는 방법을 확인할 수 있습니다.  
