---
title: 연결 복원 력-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 11/15/2016
ms.assetid: e079d4af-c455-4a14-8e15-a8471516d748
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-resiliency
ms.openlocfilehash: aec69577cd4b19fdebedb33ed6fd8f2665b0a032
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053533"
---
# <a name="connection-resiliency"></a>연결 복원력

연결 복원에 실패 한 데이터베이스 명령을 자동으로 다시 시도합니다. 제공 된 "실행" 하는 전략을 오류를 감지 하 고 명령을 다시 시도 하는 데 필요한 논리를 캡슐화 하 여 모든 데이터베이스와 기능을 사용할 수 있습니다. EF 핵심 공급자는 해당 특정 데이터베이스 오류 조건 및 최적의 다시 시도 정책에 맞게 실행 전략을 제공할 수 있습니다.

예를 들어, SQL Server 공급자는 SQL Server (SQL Azure 등) 하기 위해 특별히 설계 되는 실행 전략을 포함 합니다. 다시 시도할 수 있는 예외 유형을 확인 하 고 있어서 최대 재시도 횟수, 재시도 등의 간격에 대 한 실제 기본값입니다.

실행 전략 여 컨텍스트에 대 한 옵션을 구성할 때 지정 됩니다. 이 일반적으로는 `OnConfiguring` 또는 파생 컨텍스트의 메서드 `Startup.cs` ASP.NET Core 응용 프로그램에 대 한 합니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#OnConfiguring)]

## <a name="custom-execution-strategy"></a>사용자 지정 실행 전략

자신만의 기본값을 변경 하려는 경우 사용자 지정 실행 전략을 등록 하는 메커니즘이 있습니다.

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

오류에 자동으로 다시 시도 하는 실행 전략 각 작업에 실패 하는 재시도 블록에서 재생 수 있어야 합니다. EF 코어를 통해 수행 하는 각 작업 즉, 각 쿼리 및를 호출할 때마다 다시 시도할 수는 자체 작업이 됩니다. 다시 시도 설정 되 면 `SaveChanges()` 일시적인 오류가 발생 하는 경우 하나의 단위로 재시도 합니다.

그러나 코드를 사용 하 여 트랜잭션을 시작 하는 경우 `BeginTransaction()` 하나의 단위로 처리 해야 하는 작업의 사용자 정의 그룹을 정의 하는, 즉, 트랜잭션 내의 모든 항목이 다시 재생할 수는 오류가 발생 해야 합니다. 실행 전략을 사용 하는 경우이 작업을 수행 하려고 하면 다음과 같은 예외를 받습니다.

> InvalidOperationException: 구성 된 실행 전략 'SqlServerRetryingExecutionStrategy' 사용자가 시작한 트랜잭션을 지원 하지 않습니다. 'DbContext.Database.CreateExecutionStrategy()'에서 반환 된 실행 전략을 사용 하 여 트랜잭션을 다시 시도할 수 한 단위로 모든 작업을 실행 합니다.

해결 방법은 수동으로 실행 해야 하는 모든 항목을 나타내는 대리자를 사용 하 여 실행 전략을 호출 합니다. 일시적인 오류가 발생 하는 경우 실행 전략 대리자를 다시 호출 합니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#ManualTransaction)]

## <a name="transaction-commit-failure-and-the-idempotency-issue"></a>트랜잭션 커밋 실패 및 멱 문제

일반적으로 연결 오류가 있는 경우 현재 트랜잭션이 롤백됩니다. 그러나 트랜잭션이 된 상태는 연결이 끊어질 경우 되 고 커밋된 결과로 생성 된 트랜잭션 상태를 알 합니다. 이 참조 [블로그 게시물](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) 내용을 확인 합니다.

기본적으로는 실행 전략은 작업을 다시 시도 트랜잭션이 롤백 되었습니다 있지만 그렇지 않은 경우 경우 이렇게 하면 예외가 새 데이터베이스 상태 호환 되지 않거나 시킬 수 있는 경우 **데이터 손상** 경우는 작업 예를 들어 자동 생성 된 키 값을 가진 새 행을 삽입할 때 특정 상태에 의존 하지 않습니다.

이 처리 하는 방법은 여러 가지가 있습니다.

### <a name="option-1---do-almost-nothing"></a>옵션 1-Do 거의 대부분 아무것도 표시 되지 않음

이 상태는 실제로 발생 하는 경우 작동이 실패 하도록 응용 프로그램에 대 한 허용 되므로 트랜잭션 커밋하는 동안 연결 오류가 발생할 가능성이 낮습니다.

그러나 중복 행을 추가 하는 대신 예외가 발생을 보장 하기 위해 저장소 생성 키를 사용 하지 않도록 해야 합니다. 클라이언트에서 생성 된 GUID 값 또는 클라이언트 쪽 값 생성기를 사용 하는 것이 좋습니다.

### <a name="option-2---rebuild-application-state"></a>옵션 2-Rebuild 응용 프로그램 상태

1. 현재 삭제 `DbContext`합니다.
2. 새 `DbContext` 데이터베이스에서 응용 프로그램의 상태를 복원 합니다.
3. 마지막 작업 수 완료 되지 않은 성공적으로 사용자에 게 알립니다.

### <a name="option-3---add-state-verification"></a>옵션 3-상태 확인 추가

대부분의 데이터베이스 상태를 변경 하는 작업 성공 여부를 확인 하는 코드를 추가 하는 것이 같습니다. -더 쉽게 확인 하는 확장 메서드를 제공 하는 EF `IExecutionStrategy.ExecuteInTransaction`합니다.

시작 하 고 트랜잭션을 커밋합니다 및 에서도의 함수를 적용이 메서드는 `verifySucceeded` 트랜잭션 커밋 중에 일시적인 오류가 발생할 때 호출 되는 매개 변수입니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Verification)]

> [!NOTE]
> 여기 `SaveChanges` 와 함께 호출 되는지 `acceptAllChangesOnSuccess` 로 설정 `false` 의 상태를 변경 하지 않으려면는 `Blog` 엔터티를 `Unchanged` 경우 `SaveChanges` 성공 합니다. 따라서 커밋에 실패 하 고 트랜잭션이 롤백 동일한 작업을 다시 시도를 합니다.

### <a name="option-4---manually-track-the-transaction"></a>옵션 4-트랜잭션을 수동으로 추적

저장소 생성 키를 사용 하거나 수행 되는 작업에 의존 하지 않는 제네릭 방법이 커밋 실패 처리 필요한 해야 할 경우 각 트랜잭션 커밋이 실패 하면 체크 인 된 ID는 할당할 수 수 없습니다.

1. 트랜잭션의 상태를 추적 하는 데 사용 되는 데이터베이스에 테이블을 추가 합니다.
2. 각 트랜잭션의 시작 부분에 있는 테이블에 행을 삽입 합니다.
3. 커밋 연결에 실패 하면 데이터베이스에서 해당 행이 있는지 확인 합니다.
4. 커밋에 성공 하면 테이블의 증가 방지 하기 위해 해당 행을 삭제 합니다.

[!code-csharp[Main](../../../samples/core/Miscellaneous/ConnectionResiliency/Program.cs#Tracking)]

> [!NOTE]
> 확인에 사용 되는 컨텍스트 연결이 트랜잭션 커밋 중 실패 한 경우 다시 확인 하는 동안 실패할 수로 정의 하는 실행 전략에 있는지 확인 합니다.
