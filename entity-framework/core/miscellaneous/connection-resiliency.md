---
title: 연결 복원 력-EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 6d8cf117dfd94524a53e10bb4a23c2a44c4c8e7b
ms.sourcegitcommit: 33b2e84dae96040f60a613186a24ff3c7b00b6db
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56459174"
---
# <a name="connection-resiliency"></a>연결 복원력

연결 복원 력에 실패 한 데이터베이스 명령을 자동으로 다시 시도합니다. "실행" 하는 전략을 오류를 검색 하 고 명령을 다시 시도 하는 데 필요한 논리를 캡슐화를 제공 하 여 데이터베이스를 사용 하 여 기능을 사용할 수 있습니다. EF Core 공급자는 해당 특정 데이터베이스 오류 조건 및 최적의 재시도 정책에 맞게 실행 전략을 제공할 수 있습니다.

예를 들어 SQL Server 공급자는 SQL Server (SQL Azure 포함)에 특별히 조정 된 실행 전략을 포함 합니다. 다시 시도할 수 있는 예외 형식을 인식 하 고 최대 재시도, 재시도 등 간격에 대 한 가능한 기본값을 갖습니다.

실행 전략에 대해 옵션을 구성할 때 지정 됩니다. 이 일반적으로 `OnConfiguring` 메서드의 파생된 컨텍스트:

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

또는 `Startup.cs` ASP.NET Core 응용 프로그램에 대 한 합니다.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<PicnicContext>(
        options => options.UseSqlServer(
            "<connection string>",
            providerOptions => providerOptions.EnableRetryOnFailure()));
}
```

## <a name="custom-execution-strategy"></a>사용자 지정 실행 전략

자신만의 기본값을 변경 하려는 경우 사용자 지정 실행 전략을 등록 하려면 장치가 있습니다.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseMyProvider(
            "<connection string>",
            options => options.ExecutionStrategy(...));
}
```

## <a name="execution-strategies-and-transactions"></a>실행 전략 및 트랜잭션

오류에서 자동으로 다시 시도 하는 실행 전략을 실패 하는 재시도 블록에서 각 작업을 재생할 수 있어야 합니다. EF Core를 통해 수행 하는 각 작업 다시 시도가 설정 되 면 다시 시도 가능 작업 자체입니다. 즉, 각 쿼리와 각 호출이를 `SaveChanges()` 일시적인 오류가 발생 하는 경우 하나의 단위로 재시도 합니다.

그러나 코드를 사용 하는 트랜잭션이 시작 하는 경우 `BeginTransaction()` 하나의 단위로 처리 해야 하는 작업의 고유한 그룹을 정의 하는 및 재생할 수는 오류가 발생 하는 데 필요한은 트랜잭션에서 모든 합니다. 실행 전략을 사용 하는 경우이 작업을 수행 하려고 하면 다음과 같은 예외를 수신 합니다.

> InvalidOperationException: 구성된 실행 전략 ‘SqlServerRetryingExecutionStrategy’는 사용자가 시작한 트랜잭션을 지원하지 않습니다. 'DbContext.Database.CreateExecutionStrategy()'에서 반환된 실행 전략을 사용하여 트랜잭션을 다시 시도가 가능한 단위로 트랜잭션의 모든 작업을 실행합니다.

솔루션이 수동으로 실행 해야 하는 모든 항목을 나타내는 대리자를 사용 하 여 실행 전략을 호출 하는 것입니다. 일시적인 오류가 발생하면, 실행 전략에서 대리자를 다시 호출합니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

이 방법은 앰비언트 트랜잭션을 사용 하 여 사용할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>멱 등 성 문제와 트랜잭션 커밋 실패

일반적으로 연결 오류가 발생 하는 경우 현재 트랜잭션이 롤백됩니다. 그러나 트랜잭션 동안 연결이 삭제 되 면 되 고 커밋된 결과 트랜잭션 상태를 알 수 없는 합니다. 이 참조 하세요 [블로그 게시물](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) 대 한 자세한 내용은 합니다.

기본적으로 실행 전략은 작업을 다시 시도 트랜잭션이 롤백된 하지만 경우 없는 경우이 예외가 새로운 데이터베이스 상태를 호환 되지 않거나 발생할 수 있습니다 하는 경우 처럼 **데이터가 손상** 경우는 작업, 예를 들어 자동으로 생성 된 키 값을 사용 하 여 새 행을 삽입 하는 경우 특정 상태에 의존 하지 않습니다.

이 사용 하 여 처리 하는 방법은 여러 가지가 있습니다.

### <a name="option-1---do-almost-nothing"></a>옵션 1-Do nothing (거의)

트랜잭션 커밋하는 동안 연결 오류의 가능성 낮은 이므로 응용 프로그램이이 조건에서 실제로 발생 하는 경우 실패를 허용할 수도 있습니다.

그러나 중복 행을 추가 하는 대신 예외가 발생을 보장 하기 위해 저장소 생성 키를 사용 하지 않도록 해야 합니다. 클라이언트에서 생성 된 GUID 값 또는 클라이언트 쪽 값 생성기를 사용 하는 것이 좋습니다.

### <a name="option-2---rebuild-application-state"></a>옵션 2-응용 프로그램 상태를 다시 작성

1. 현재 삭제 `DbContext`합니다.
2. 새 `DbContext` 및 데이터베이스에서 응용 프로그램의 상태를 복원 합니다.
3. 마지막 작업 수 완료 되지 않은 성공적으로 사용자에 게 알립니다.

### <a name="option-3---add-state-verification"></a>옵션 3-추가 상태 확인

대부분의 데이터베이스 상태를 변경 하는 작업에 대 한 성공 여부를 확인 하는 코드를 추가 하는 것이 같습니다. EF-간편 하 게 하는 확장 메서드 제공 `IExecutionStrategy.ExecuteInTransaction`합니다.

이 메서드는 트랜잭션을 커밋합니다 및 받기도 함수 시작을 `verifySucceeded` 트랜잭션 커밋 중에 일시적인 오류가 발생할 때 호출 되는 매개 변수입니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> 여기 `SaveChanges` 사용 하 여 호출 `acceptAllChangesOnSuccess` 로 설정 `false` 의 상태를 변경 하지 않으려면 합니다 `Blog` 엔터티를 `Unchanged` 경우 `SaveChanges` 성공 합니다. 이렇게 하면 커밋이 실패 하 고 트랜잭션이 롤백됩니다 동일한 작업을 다시 시도 합니다.

### <a name="option-4---manually-track-the-transaction"></a>옵션 4-수동으로 트랜잭션 추적

저장소 생성 키를 사용 하거나 수행 된 작업에 의존 하지 않는 커밋 실패를 처리 하는 일반적인 방식으로 해야 하는 경우 각 트랜잭션 커밋이 실패 하면 체크 인 된 ID는 할당할 수 없습니다.

1. 트랜잭션의 상태를 추적 하는 데 사용 되는 데이터베이스에 테이블을 추가 합니다.
2. 각 트랜잭션의 시작 부분에 있는 테이블에 행을 삽입 합니다.
3. 커밋 중 연결이 실패 하는 경우 데이터베이스에서 해당 행의 존재 여부 확인 합니다.
4. 커밋이 성공 하면 테이블의 증가 방지 하려면 해당 행을 삭제 합니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> 확인에 사용 되는 컨텍스트 연결 되어 트랜잭션 커밋하는 동안 실패 한 경우 다시 확인 하는 동안 실패할 수를 정의 하는 실행 전략에 있는지 확인 합니다.
