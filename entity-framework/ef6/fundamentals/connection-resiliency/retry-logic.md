---
title: 연결 복원 력 및 다시 시도 논리-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
caps.latest.revision: 3
ms.openlocfilehash: 4c6e296ecb8b43468408d359f44fd1d1a6edf864
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122573"
---
# <a name="connection-resiliency-and-retry-logic"></a>연결 복원 력 및 다시 시도 논리
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

데이터베이스 서버에 연결 하는 응용 프로그램 백 엔드에서 실패 및 네트워크 불안정으로 인해 연결 중단에 취약 항상 되었을 수 있습니다. 그러나 전용된 데이터베이스 서버에 대해 작업 기반 LAN 환경에서 이러한 오류는 드물게 충분히 이러한 오류를 처리 하는 추가 논리가 필요 하지 않음을 경우가 많습니다. 클라우드의 증가 함에 따라 덜 안정적인 네트워크 연결 중단 발생에 대 한 보다 일반적인 되었습니다를 통해 Windows Azure SQL Database와 같은 데이터베이스 서버 및 연결 기반 합니다. 이 일시적인 시간 제한 및 기타 일시적인 오류를 일으키는 네트워크 불안정 또는 연결 제한 등 서비스의 공정성 보장을 사용 하 여 데이터베이스를 클라우드 방어 기술 때문일 수 있습니다.  

연결 복원 력 자동으로 이러한 연결 중단으로 인해 실패 하는 명령을 다시 시도 하는 EF에 대 한 기능을 말합니다.  

## <a name="execution-strategies"></a>실행 전략  

연결 다시 시도 IDbExecutionStrategy 인터페이스의 구현에 의해 처리 됩니다. IDbExecutionStrategy 구현 작업을 수락 하 고 예외가 발생 하는 경우 다시 시도 하 여 적절 한 경우를 결정 하 고 있으면 다시 시도 대 한 지불 해야 합니다. EF와 함께 제공 되는 실행 전략 4 가지가 있습니다.  

1. **DefaultExecutionStrategy**:이 실행 전략은 모든 작업을 재시도 하지 않습니다, sql server 이외의 데이터베이스에 대 한 기본값입니다.  
2. **DefaultSqlExecutionStrategy**: 기본적으로 사용 되는 내부 실행 전략입니다. 그러나이 전략에 다시 시도 하지 않습니다, 그리고 일시적인 연결 복원 력을 할 수 있는 사용자에 게 알려 될 수 있는 모든 예외 바꿈됩니다.  
3. **DbExecutionStrategy**:이 클래스는 다른 실행 전략을 포함 하 여 사용자 고유의 사용자 지정에 대 한 기본 클래스로 적합 합니다. 초기 재시도 발생 0 지연 및 지연 시간을 사용 하 여 증가 기하급수적으로 최대 재시도 횟수가 적중 될 때까지 지 수 재시도 정책을 구현 합니다. 이 클래스는 추상 ShouldRetryOn 메서드는 예외를 재시도할 제어 하려면 파생 된 실행 전략에서 구현 될 수 있습니다.  
4. **SqlAzureExecutionStrategy**:이 실행 전략이 DbExecutionStrategy에서 상속 하 고 Azure SQL Database를 사용 하 여 작업 하는 경우 가능한 일시적인 것으로 알려진 된 예외가 다시 시도 합니다.

> [!NOTE]
> 실행 전략 2 및 4 EntityFramework.SqlServer 어셈블리에 있는 EF와 함께 제공 되는 Sql Server 공급자에 포함 되 고 SQL Server와 함께 작동 하도록 설계 되었습니다.  

## <a name="enabling-an-execution-strategy"></a>실행 전략을 사용 하도록 설정  

SetExecutionStrategy 메서드를 사용 하 여 EF 실행 전략을 사용 하 게 하는 가장 쉬운 방법은는 [DbConfiguration](~/ef6/fundamentals/configuring/code-based.md) 클래스:  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

이 코드는 SqlAzureExecutionStrategy SQL Server에 연결할 때 사용 하 여 EF 하도록 지시 합니다.  

## <a name="configuring-the-execution-strategy"></a>실행 전략 구성  

SqlAzureExecutionStrategy의 생성자는 MaxRetryCount 및 MaxDelay 두 매개 변수를 사용할 수 있습니다. MaxRetry 횟수가 전략을 다시 시도 하는 최대 횟수입니다. MaxDelay는 실행 전략을 사용 하는 재시도 사이의 최대 지연 시간을 나타내는 TimeSpan입니다.  

최대 재시도 횟수 1 및 최대 지연 시간을 30 초로 설정 하려면 다음 execue 어떻게 해야 합니까?  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy(
            "System.Data.SqlClient",
            () => new SqlAzureExecutionStrategy(1, TimeSpan.FromSeconds(30)));
    }
}
```  

SqlAzureExecutionStrategy 일시적인 오류가 발생 하지만 더 이상 다시 시도 하거나 최대 한도 각 재시도 사이의 지연 됩니다 첫 번째 시간 초과 또는 총 시간 최대 지연 시간에 도달 하는 즉시 다시 시도 합니다.  

실행 전략 제한 된 수의 예외는 일반적으로 tansient만 다시 시도 해결 하려면 오류가 일시적이 지 또는 시간이 너무 오래 걸리는 있는 경우 RetryLimitExceeded 예외 catch 할 뿐만 아니라 다른 오류를 처리 해야 자체입니다.  

## <a name="limitations"></a>제한 사항  

다시 시도 실행 전략을 사용 하는 경우 몇 가지 알려진 제한 사항을 가지 있습니다.  

### <a name="streaming-queries-are-not-supported"></a>스트리밍 쿼리가 지원 되지 않습니다.  

기본적으로 EF6 및 이후 버전 스트리밍 하는 것이 아니라 쿼리 결과 버퍼링 됩니다. 결과 수를 스트리밍 하려는 경우 LINQ to Entities 쿼리의 스트리밍 변경 하려면 AsStreaming 메서드를 사용할 수 있습니다.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

다시 시도 실행 전략을 등록 하는 경우 스트리밍 지원 되지 않습니다. 이 제한은 연결 되 고 반환 된 결과 통해 도중 삭제할 수 없습니다 때문에 존재 합니다. 이 문제가 발생 하면 EF 전체 쿼리를 다시 실행 해야 하는 경우 되었지만 신뢰할 수 있는 이미 어떤 결과가 반환 된 알 수 없습니다 (데이터 변경한 초기 쿼리가 전송 된 결과 다른 순서로 다시 가져올 수 있습니다, 결과 고유 식별자가 없을 수 있으므로 등.).  

### <a name="user-initiated-transactions-not-supported"></a>사용자가 지원 되지 않는 트랜잭션 시작  

다시 시도에서 발생 하는 실행 전략을 구성한 경우 트랜잭션 사용과 관련 한 몇 가지 제한이 있습니다.  

#### <a name="whats-supported-efs-default-transaction-behavior"></a>지원 되는 기능: EF의 기본 트랜잭션 동작  

기본적으로 EF는 트랜잭션 내에서 모든 데이터베이스 업데이트를 수행 합니다. 이렇게 하려면 아무 작업도 수행할 필요가 없습니다, 그리고 EF 항상이 자동으로 수행 합니다.  

예를 들어, 다음 코드에서는 자동으로 트랜잭션 내에서 SaveChanges 수행 합니다. SaveChanges가 트랜잭션을 롤백할 수는 다음 새 사이트의 중 하나를 삽입 하 고 데이터베이스에 적용할 변경 후에 실패 합니다. 컨텍스트도 SaveChanges 변경 내용을 적용 하는 다시 시도를 다시 호출할 수 있도록 상태에서 그대로 사용 합니다.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

#### <a name="whats-not-supported-user-initiated-transactions"></a>지원 되지않는 사항: 사용자 트랜잭션 시작  

다시 시도 실행 전략을 사용 하지 않는 경우에 단일 트랜잭션에서 여러 작업을 래핑할 수 있습니다. 예를 들어, 다음 코드는 단일 트랜잭션에서 두 SaveChanges 호출을 래핑합니다. 두 작업의 일부가 실패 하면 변경 하는 경우 적용 됩니다.  

``` csharp
using (var db = new BloggingContext())
{
    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Site { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }
}
```  

EF 모든 이전 작업을 다시 시도 하는 방법을 인식 하지 않기 때문에 다시 시도 실행 전략을 사용 하는 경우에 지원 되지 않습니다. 예를 들어, 두 번째 SaveChanges 실패 한 경우 다음 EF 더 이상 첫 번째 SaveChanges 호출을 다시 시도 하는 데 필요한 정보를 없습니다.  

#### <a name="possible-workarounds"></a>가능한 해결 방법  

##### <a name="suspend-execution-strategy"></a>실행 전략 일시 중단  

해결 방법 중 하나 트랜잭션 시작 사용자를 사용 해야 하는 코드 부분에 대 한 다시 시도 실행 전략 일시 중단 됩니다. 이 작업을 수행 하는 가장 쉬운 방법은 코드 SuspendExecutionStrategy 플래그 구성 클래스 및 변경 플래그를 설정 하는 경우 기본 (retying 아닌) 실행 전략을 반환할 실행 전략 람다를 추가 하는 것입니다.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;
using System.Runtime.Remoting.Messaging;

namespace Demo
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            this.SetExecutionStrategy("System.Data.SqlClient", () => SuspendExecutionStrategy
              ? (IDbExecutionStrategy)new DefaultExecutionStrategy()
              : new SqlAzureExecutionStrategy());
        }

        public static bool SuspendExecutionStrategy
        {
            get
            {
                return (bool?)CallContext.LogicalGetData("SuspendExecutionStrategy")  false;
            }
            set
            {
                CallContext.LogicalSetData("SuspendExecutionStrategy", value);
            }
        }
    }
}
```  

플래그 값을 저장 하도록 CallContext 사용 하는 것을 참고 합니다. 이 스레드 로컬 저장소에 유사한 기능을 제공 하지만 안전 비동기 쿼리를 포함 하 여 비동기 코드를 사용 하 여 Entity Framework와 함께 저장 됩니다.  

이제 사용자가 시작한 트랜잭션을 사용 하는 코드 섹션에 대 한 실행 전략 일시 중단할 수 있습니다 것입니다.  

``` csharp
using (var db = new BloggingContext())
{
    MyConfiguration.SuspendExecutionStrategy = true;

    using (var trn = db.Database.BeginTransaction())
    {
        db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
        db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
        db.SaveChanges();

        db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
        db.SaveChanges();

        trn.Commit();
    }

    MyConfiguration.SuspendExecutionStrategy = false;
}
```  

##### <a name="manually-call-execution-strategy"></a>실행 전략을 직접 호출  

다른 옵션은 수동으로 실행 전략을 사용 하 고 작업 중 하나가 실패 하면 모든 재시도 수 있도록 실행 논리의 전체 집합을 제공 하는 것입니다. -기술을 사용 하 여 실행 전략 일시 중단 작업을 계속 합니다 위의-재시도 가능한 코드 블록 내에서 사용 되는 컨텍스트 다시 시도 하지 않도록 되도록 합니다.  

다시 시도 하려면 코드 블록 내에서 컨텍스트를 생성 해야 하는 참고 합니다. 이렇게 하면 각 재시도 대 한 클린 상태로 시작 했습니다.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

MyConfiguration.SuspendExecutionStrategy = true;

executionStrategy.Execute(
    () =>
    {
        using (var db = new BloggingContext())
        {
            using (var trn = db.Database.BeginTransaction())
            {
                db.Blogs.Add(new Blog { Url = "http://msdn.com/data/ef" });
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                db.SaveChanges();

                db.Blogs.Add(new Blog { Url = "http://twitter.com/efmagicunicorns" });
                db.SaveChanges();

                trn.Commit();
            }
        }
    });

MyConfiguration.SuspendExecutionStrategy = false;
```  
