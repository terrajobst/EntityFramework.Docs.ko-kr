---
title: 연결 복원 력-EF Core
author: rowanmiller
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: 07646e6ead845c38537945a03367ac7f50784236
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414136"
---
# <a name="connection-resiliency"></a>연결 복원력

연결 복원 력을 실패 한 데이터베이스 명령을 자동으로 다시 시도 합니다. 이 기능은 실패를 검색 하 고 명령을 다시 시도 하는 데 필요한 논리를 캡슐화 하는 "실행 전략"을 제공 하 여 모든 데이터베이스에서 사용할 수 있습니다. EF Core 공급자는 특정 데이터베이스 오류 조건 및 최적의 재시도 정책에 맞는 실행 전략을 제공할 수 있습니다.

예를 들어 SQL Server 공급자에는 SQL Server에 맞게 특별히 조정 된 실행 전략 (SQL Azure 포함)이 포함 되어 있습니다. 다시 시도할 수 있는 예외 유형과 최대 다시 시도 횟수, 다시 시도 간 지연 등의 적절 한 기본값을 가진 예외 유형을 인식 합니다.

컨텍스트에 대 한 옵션을 구성할 때 실행 전략이 지정 됩니다. 이는 일반적으로 파생 컨텍스트의 `OnConfiguring` 메서드에 있습니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

또는 `Startup.cs` ASP.NET Core 응용 프로그램:

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

기본값을 변경 하려는 경우 고유한 사용자 지정 실행 전략을 등록 하는 메커니즘이 있습니다.

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

오류 시 자동으로 다시 시도 하는 실행 전략은 실패 한 재시도 블록에서 각 작업을 재생할 수 있어야 합니다. 다시 시도를 사용 하도록 설정 하면 EF Core을 통해 수행 하는 각 작업이 자체 다시 시도 가능 작업이 됩니다. 즉, 일시적인 오류가 발생 하는 경우 각 쿼리 및 `SaveChanges()`에 대 한 각 호출은 하나의 단위로 다시 시도 됩니다.

그러나 코드가 `BeginTransaction()`를 사용 하 여 트랜잭션을 시작 하는 경우 하나의 단위로 처리 해야 하는 사용자 고유의 작업 그룹을 정의 하 고 트랜잭션 내의 모든 항목을 재생 해야 하는 경우 오류가 발생 합니다. 실행 전략을 사용할 때이 작업을 수행 하려고 하면 다음과 같은 예외가 발생 합니다.

> InvalidOperationException: 구성 된 실행 전략 ' SqlServerRetryingExecutionStrategy '은 사용자가 시작한 트랜잭션을 지원 하지 않습니다. 'DbContext.Database.CreateExecutionStrategy()'에서 반환된 실행 전략을 사용하여 트랜잭션을 다시 시도가 가능한 단위로 트랜잭션의 모든 작업을 실행합니다.

이 솔루션은 실행 해야 하는 모든 항목을 나타내는 대리자를 사용 하 여 실행 전략을 수동으로 호출 하는 것입니다. 일시적인 오류가 발생하면, 실행 전략에서 대리자를 다시 호출합니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

이 방법은 앰비언트 트랜잭션과 함께 사용할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#AmbientTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>트랜잭션 커밋 실패 및 멱 등 성 문제

일반적으로 연결 오류가 발생 하면 현재 트랜잭션이 롤백됩니다. 그러나 트랜잭션을 커밋하는 동안 연결이 끊어진 경우 트랜잭션 결과 상태를 알 수 없습니다. 

기본적으로 실행 전략은 트랜잭션이 롤백된 것 처럼 작업을 다시 시도 하지만 그렇지 않은 경우에는 새 데이터베이스 상태가 호환 되지 않는 경우 예외가 발생 하거나, 자동 생성 된 키 값이 있는 새 행을 삽입 하는 등의 작업에서 특정 상태를 사용 하지 않는 경우 **데이터가 손상** 될 수 있습니다.

여러 가지 방법으로이를 처리할 수 있습니다.

### <a name="option-1---do-almost-nothing"></a>옵션 1-전혀 하지 않음 (거의)

트랜잭션 커밋 중에 연결 오류가 발생 하는 경우이 상태가 실제로 발생 하는 경우 응용 프로그램이 실패할 수 있습니다.

그러나 중복 행을 추가 하는 대신 예외가 throw 되도록 하기 위해 저장소에서 생성 된 키를 사용 하지 않도록 해야 합니다. 클라이언트에서 생성 된 GUID 값 또는 클라이언트 쪽 값 생성기를 사용 하는 것이 좋습니다.

### <a name="option-2---rebuild-application-state"></a>옵션 2-응용 프로그램 상태 다시 빌드

1. 현재 `DbContext`을 삭제 합니다.
2. 새 `DbContext`을 만들고 데이터베이스에서 응용 프로그램의 상태를 복원 합니다.
3. 마지막 작업이 성공적으로 완료 되지 않았을 수 있음을 사용자에 게 알립니다.

### <a name="option-3---add-state-verification"></a>옵션 3-상태 확인 추가

데이터베이스 상태를 변경 하는 대부분의 작업에 대해 성공 여부를 확인 하는 코드를 추가할 수 있습니다. EF는이 작업을 보다 쉽게 `IExecutionStrategy.ExecuteInTransaction`하는 확장 메서드를 제공 합니다.

이 메서드는 트랜잭션을 시작 하 고 커밋하고 트랜잭션 커밋 중에 일시적인 오류가 발생할 때 호출 되는 `verifySucceeded` 매개 변수 에서도 함수를 수락 합니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> `Unchanged` 성공 하는 경우 `Blog` 엔터티의 상태를 `SaveChanges`으로 변경 하지 않도록 `acceptAllChangesOnSuccess`를 `false`로 설정 하 여이 `SaveChanges` 호출 됩니다. 이렇게 하면 커밋이 실패 하 고 트랜잭션이 롤백되는 경우 동일한 작업을 다시 시도할 수 있습니다.

### <a name="option-4---manually-track-the-transaction"></a>옵션 4-수동으로 트랜잭션 추적

저장소에서 생성 된 키를 사용 해야 하는 경우 또는 수행 된 작업에 의존 하지 않는 커밋 실패를 처리 하는 일반적인 방법이 필요한 경우 커밋에 실패할 때 확인 되는 ID를 각 트랜잭션에 할당할 수 있습니다.

1. 트랜잭션 상태를 추적 하는 데 사용 되는 데이터베이스에 테이블을 추가 합니다.
2. 각 트랜잭션의 시작 부분에서 테이블에 행을 삽입 합니다.
3. 커밋하는 동안 연결이 실패 하는 경우 데이터베이스에 해당 하는 행이 있는지 확인 합니다.
4. 커밋이 성공한 경우 테이블의 증가를 방지 하려면 해당 행을 삭제 합니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> 확인 하는 데 사용 되는 컨텍스트에 트랜잭션 커밋 중에 오류가 발생 한 경우이를 확인 하는 동안 연결이 다시 실패할 가능성이 있으므로 실행 전략을 정의 했는지 확인 합니다.
