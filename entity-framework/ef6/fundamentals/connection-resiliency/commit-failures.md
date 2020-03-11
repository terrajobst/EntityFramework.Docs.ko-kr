---
title: 트랜잭션 커밋 실패 처리-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: 27e75e6a1919ee2300fe76cfcdf67cceaad887b3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414676"
---
# <a name="handling-transaction-commit-failures"></a>트랜잭션 커밋 실패 처리
> [!NOTE]
> **Ef 6.1** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 6.1에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

6\.1의 일부로 EF: 일시적인 연결 오류가 트랜잭션 커밋의 승인에 영향을 주는 경우 자동으로 검색 하 고 복구할 수 있는 EF: 새 연결 복원 력 기능을 도입 하 고 있습니다. 이 시나리오에 대 한 자세한 내용은 블로그 게시물 [SQL Database 연결 및 멱 등 성 문제](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)에 설명 되어 있습니다.  요약 하자면, 시나리오는 트랜잭션 커밋 중에 예외가 발생 하는 경우 두 가지 가능한 원인이 있습니다.  

1. 서버에서 트랜잭션 커밋이 실패 했습니다.
2. 서버에서 트랜잭션 커밋이 성공 했지만 연결 문제로 인해 성공 알림이 클라이언트에 도달 하지 못했습니다.  

첫 번째 상황이 응용 프로그램에서 발생 하거나 사용자가 작업을 다시 시도할 수 있지만 두 번째 상황에서 재시도를 방지 하 고 응용 프로그램이 자동으로 복구 될 수 있습니다. 문제는 커밋 중에 예외가 보고 된 실제 이유를 검색 하는 기능이 없으므로 응용 프로그램에서 올바른 작업 과정을 선택할 수 없습니다. EF 6.1의 새로운 기능을 통해 EF는 트랜잭션이 성공한 경우 데이터베이스를 다시 확인 하 고 적절 한 작업을 투명 하 게 수행할 수 있습니다.  

## <a name="using-the-feature"></a>기능 사용  

이 기능을 사용 하도록 설정 하려면 **Dbconfiguration**의 생성자에 [settransactionhandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) 에 대 한 호출을 포함 해야 합니다. **Dbconfiguration**에 익숙하지 않은 경우 [코드 기반 구성](~/ef6/fundamentals/configuring/code-based.md)을 참조 하세요. 이 기능은 EF6에서 도입 된 자동 재시도와 함께 사용할 수 있습니다 .이는 일시적인 오류로 인해 트랜잭션이 실제로 서버에서 커밋하지 못한 상황에 도움이 됩니다.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

public class MyConfiguration : DbConfiguration  
{
  public MyConfiguration()  
  {  
    SetTransactionHandler(SqlProviderServices.ProviderInvariantName, () => new CommitFailureHandler());  
    SetExecutionStrategy(SqlProviderServices.ProviderInvariantName, () => new SqlAzureExecutionStrategy());  
  }  
}
```  

## <a name="how-transactions-are-tracked"></a>트랜잭션 추적 방법  

이 기능을 사용 하도록 설정 하면 EF가 **__Transactions**라는 데이터베이스에 새 테이블을 자동으로 추가 합니다. EF에서 트랜잭션을 만들 때마다이 테이블에 새 행이 삽입 되 고 커밋 중에 트랜잭션 오류가 발생 하는 경우 해당 행이 있는지 확인 됩니다.  

EF가 더 이상 필요 하지 않을 때 테이블에서 행을 정리 하는 데 가장 적합 하지만, 응용 프로그램이 중간에 종료 될 때 테이블이 커질 수 있으며, 따라서 경우에 따라 테이블을 수동으로 제거 해야 할 수도 있습니다.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>이전 버전과의 커밋 실패를 처리 하는 방법

EF 6.1 이전에는 EF 제품의 커밋 실패를 처리 하는 메커니즘이 없었습니다. 이전 버전의 EF6에 적용 될 수 있는 이러한 상황을 처리 하는 몇 가지 방법이 있습니다.  

* 옵션 1-아무 작업도 수행 하지 않음  

  트랜잭션 커밋 중에 연결 오류가 발생 하는 경우이 상태가 실제로 발생 하는 경우 응용 프로그램이 실패할 수 있습니다.  

* 옵션 2-데이터베이스를 사용 하 여 상태 다시 설정  

  1. 현재 DbContext를 취소 합니다.  
  2. 새 DbContext을 만들고 데이터베이스에서 응용 프로그램의 상태를 복원 합니다.  
  3. 마지막 작업이 성공적으로 완료 되지 않았을 수 있음을 사용자에 게 알립니다.  

* 옵션 3-수동으로 트랜잭션 추적  

  1. 트랜잭션 상태를 추적 하는 데 사용 되는 데이터베이스에 추적 되지 않은 테이블을 추가 합니다.  
  2. 각 트랜잭션의 시작 부분에서 테이블에 행을 삽입 합니다.  
  3. 커밋하는 동안 연결이 실패 하는 경우 데이터베이스에 해당 하는 행이 있는지 확인 합니다.  
     - 행이 있는 경우 트랜잭션이 성공적으로 커밋 됨에 따라 정상적으로 계속 진행 합니다.  
     - 행이 없으면 실행 전략을 사용 하 여 현재 작업을 다시 시도 합니다.  
  4. 커밋이 성공한 경우 테이블의 증가를 방지 하려면 해당 행을 삭제 합니다.  

[이 블로그 게시물](https://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) 에는 SQL Azure에서이를 수행 하기 위한 샘플 코드가 포함 되어 있습니다.  
