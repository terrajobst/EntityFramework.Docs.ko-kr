---
title: 연결 복원 력 및 재시도 논리-EF6
author: AndriySvyryd
ms.date: 11/20/2019
ms.assetid: 47d68ac1-927e-4842-ab8c-ed8c8698dff2
ms.openlocfilehash: 50e65bed32d0cfcf42746da0d632f9e990424b97
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824843"
---
# <a name="connection-resiliency-and-retry-logic"></a>연결 복원 력 및 재시도 논리
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

데이터베이스 서버에 연결 하는 응용 프로그램은 백 엔드 오류 및 네트워크 불안정성으로 인 한 연결 중단에 항상 취약 합니다. 그러나 전용 데이터베이스 서버에 대 한 작업을 수행 하는 LAN 기반 환경에서는 이러한 오류는 자주 발생 하지 않는 오류를 처리 하는 추가 논리가 필요 하지 않습니다. Windows Azure SQL Database와 같은 클라우드 기반 데이터베이스 서버와 신뢰할 수 없는 네트워크를 통한 연결을 사용 하 여 연결 중단이 발생 하는 것이 더 일반적입니다. 이는 연결 제한과 같은 서비스의 공평을 보장 하기 위해 클라우드 데이터베이스에서 사용 하는 방어 기술 이거나 간헐적 시간 제한 및 기타 일시적인 오류를 야기 하는 네트워크 불안정 때문에 발생할 수 있습니다.  

연결 복원 력은 EF가 이러한 연결 중단으로 인해 실패 하는 모든 명령을 자동으로 다시 시도 하는 기능을 의미 합니다.  

## <a name="execution-strategies"></a>실행 전략  

연결 다시 시도는 IDbExecutionStrategy 인터페이스의 구현에 의해 처리 됩니다. IDbExecutionStrategy 구현은 작업을 수락 하 고, 예외가 발생 하면 다시 시도가 적절 한지 확인 하 고, 다시 시도 하는 경우 다시 시도 하는 일을 담당 합니다. EF와 함께 제공 되는 4 가지 실행 전략은 다음과 같습니다.  

1. **Defaultexecutionstrategy**:이 실행 전략은 작업을 다시 시도 하지 않으며 sql server 이외의 데이터베이스에 대 한 기본값입니다.  
2. **Defaultsqlexecutionstrategy**: 기본적으로 사용 되는 내부 실행 전략입니다. 그러나이 전략은 다시 시도 하지 않지만 사용자에 게 연결 복원 력을 사용 하도록 설정 하려는 경우 일시적으로 발생할 수 있는 모든 예외를 래핑합니다.  
3. **Dbexecutionstrategy**:이 클래스는 고유한 사용자 지정을 포함 하 여 다른 실행 전략의 기본 클래스로 적합 합니다. 이는 초기 재시도 시간이 0 지연 된 상태에서 발생 하 고 최대 다시 시도 횟수에 도달할 때까지 지연 시간이 늘어납니다. 지 수 다시 시도 정책을 구현 합니다. 이 클래스에는 다시 시도해 야 하는 예외를 제어 하기 위해 파생 된 실행 전략에서 구현 될 수 있는 추상 ShouldRetryOn 메서드가 있습니다.  
4. **SqlAzureExecutionStrategy**:이 실행 전략은 dbexecutionstrategy에서 상속 되며 Azure SQL Database 작업할 때 일시적일 수 있는 것으로 알려진 예외를 다시 시도 합니다.

> [!NOTE]
> 실행 전략 2와 4는 EF와 함께 제공 되는 Sql Server 공급자에 포함 되어 있습니다 .이 공급자는 EntityFramework. SqlServer 어셈블리 이며 SQL Server와 함께 작동 하도록 설계 되었습니다.  

## <a name="enabling-an-execution-strategy"></a>실행 전략 사용  

EF를 사용 하 여 실행 전략을 사용 하는 가장 쉬운 방법은 [Dbconfiguration](~/ef6/fundamentals/configuring/code-based.md) 클래스의 SetExecutionStrategy 메서드를 사용 하는 것입니다.  

``` csharp
public class MyConfiguration : DbConfiguration
{
    public MyConfiguration()
    {
        SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
    }
}
```  

이 코드는 SQL Server에 연결할 때 SqlAzureExecutionStrategy를 사용 하도록 EF에 지시 합니다.  

## <a name="configuring-the-execution-strategy"></a>실행 전략 구성  

SqlAzureExecutionStrategy의 생성자는 Maxretrycount는 여야 및 MaxDelay의 두 매개 변수를 사용할 수 있습니다. MaxRetry count는 전략이 다시 시도 하는 최대 횟수입니다. MaxDelay는 실행 전략에서 사용할 재시도 사이의 최대 지연 시간을 나타내는 TimeSpan입니다.  

최대 다시 시도 횟수를 1로 설정 하 고 최대 지연 시간을 30 초로 설정 하려면 다음을 실행 합니다.  

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

SqlAzureExecutionStrategy은 일시적 오류가 발생 하는 경우 즉시 다시 시도 하지만 최대 재시도 한도를 초과 하거나 총 시간이 최대 지연에 도달할 때까지 각 다시 시도 사이에서 더 오래 지연 됩니다.  

실행 전략은 일반적으로 일시적인 제한 된 수의 예외만 다시 시도 하지만 오류가 일시적이 지 않거나 해결 하는 데 너무 오래 걸리는 경우에는 RetryLimitExceeded 예외를 catch 하는 것 외에도 다른 오류를 처리 해야 합니다. 자체.  

다시 시도 하는 실행 전략을 사용 하는 경우 몇 가지 제한 사항이 있습니다.  

## <a name="streaming-queries-are-not-supported"></a>스트리밍 쿼리는 지원 되지 않습니다.  

기본적으로 EF6 이상 버전에서는 쿼리 결과를 스트리밍하는 대신 버퍼링 합니다. 결과가 스트리밍되는 경우 AsStreaming 메서드를 사용 하 여 LINQ to Entities 쿼리를 스트리밍으로 변경할 수 있습니다.  

``` csharp
using (var db = new BloggingContext())
{
    var query = (from b in db.Blogs
                orderby b.Url
                select b).AsStreaming();
    }
}
```  

다시 시도 하는 실행 전략을 등록 하면 스트리밍이 지원 되지 않습니다. 이러한 제한은 반환 되는 결과를 통해 연결을 삭제할 수 있기 때문에 존재 합니다. 이 경우 EF는 전체 쿼리를 다시 실행 해야 하지만 이미 반환 된 결과를 확실 하 게 확인할 수 있는 방법이 없습니다. 초기 쿼리를 보낸 후 데이터가 변경 될 수 있습니다. 결과가 다른 순서로 반환 될 수 있습니다. 결과는 고유 식별자를 가질 수 없습니다. , 등).  

## <a name="user-initiated-transactions-are-not-supported"></a>사용자가 시작한 트랜잭션은 지원 되지 않습니다.  

다시 시도를 발생 시키는 실행 전략을 구성 하면 트랜잭션 사용과 관련 하 여 몇 가지 제한 사항이 있습니다.  

기본적으로 EF는 트랜잭션 내에서 모든 데이터베이스 업데이트를 수행 합니다. 이 작업을 수행 하기 위해 어떤 작업도 수행할 필요가 없습니다. EF는 항상이 작업을 자동으로 수행 합니다.  

예를 들어 다음 코드에서 SaveChanges는 트랜잭션 내에서 자동으로 수행 됩니다. 새 사이트의 하나를 삽입 한 후에 SaveChanges가 실패 한 경우에는 트랜잭션이 롤백되고 변경 내용이 데이터베이스에 적용 되지 않습니다. 또한 컨텍스트는 SaveChanges를 다시 호출 하 여 변경 내용 적용을 다시 시도할 수 있는 상태로 남아 있습니다.  

``` csharp
using (var db = new BloggingContext())
{
    db.Blogs.Add(new Site { Url = "http://msdn.com/data/ef" });
    db.Blogs.Add(new Site { Url = "http://blogs.msdn.com/adonet" });
    db.SaveChanges();
}
```  

다시 시도 하는 실행 전략을 사용 하지 않는 경우 여러 작업을 단일 트랜잭션으로 래핑할 수 있습니다. 예를 들어 다음 코드는 단일 트랜잭션에서 두 개의 SaveChanges 호출을 래핑합니다. 두 작업 중 하나라도 실패 하면 변경 내용이 적용 되지 않습니다.  

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

EF는 이전 작업을 인식 하지 못하고 다시 시도 하는 방법을 인식 하지 못하기 때문에 다시 시도 하는 실행 전략을 사용 하는 경우에는 지원 되지 않습니다. 예를 들어 두 번째 SaveChanges가 실패 한 경우 EF는 첫 번째 SaveChanges 호출을 다시 시도 하는 데 필요한 정보를 더 이상 포함 하지 않습니다.  

### <a name="solution-manually-call-execution-strategy"></a>해결 방법: 실행 전략을 수동으로 호출 합니다.  

이 솔루션은 실행 전략을 수동으로 사용 하 고 작업 중 하나가 실패할 경우 모든 작업을 다시 시도할 수 있도록 실행할 전체 논리 집합을 제공 하는 것입니다. DbExecutionStrategy에서 파생 된 실행 전략을 실행 하는 경우 SaveChanges에서 사용 된 암시적 실행 전략을 일시 중단 합니다.  

다시 시도할 모든 컨텍스트를 코드 블록 내에서 생성 해야 합니다. 이렇게 하면 각 다시 시도에 대해 정리 상태로 시작 됩니다.  

``` csharp
var executionStrategy = new SqlAzureExecutionStrategy();

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
```  
