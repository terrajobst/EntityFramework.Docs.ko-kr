---
title: 트랜잭션 커밋 오류-EF6를 처리합니다.
author: divega
ms.date: 2016-10-23
ms.assetid: 5b1f7a7d-1b24-4645-95ec-5608a31ef577
ms.openlocfilehash: a22a651851bc46e2bf1fe652b3b9a921ec22b70b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996839"
---
# <a name="handling-transaction-commit-failures"></a>트랜잭션 커밋 오류 처리
> [!NOTE]
> **EF6.1 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 6.1에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

6.1 일부로 EF에 대 한 새 연결 복원 력 기능 도입 됩니다: 검색 하 고 일시적인 연결 오류가 발생할 트랜잭션 커밋 승인을 영향을 주는 경우 자동으로 복구 하는 기능입니다. 시나리오의 전체 세부 정보는 블로그 게시물에 잘 설명 [SQL Database 연결 및 멱 등 성 문제가](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx)합니다.  요약 하자면, 시나리오의 트랜잭션 커밋하는 동안 예외가 발생 하는 경우는 두 가지 가능한 원인:  

1. 서버에서 트랜잭션 커밋이 실패 했습니다.
2. 서버에서 트랜잭션 커밋에 성공 했지만 연결 문제를 방지 클라이언트에 도달 하에서 성공 알림  

사용자나 응용 프로그램이 첫 번째 상황은 표시 되는 경우 작업을 다시 시도할 수 있지만 두 번째 상황이 발생 하면 다시 시도 하지 않아야 합니다 및 응용 프로그램을 자동으로 복구할 수 없습니다. 과제 없으면 무엇 이었습니까 실제 응용 프로그램이 올바른 작업 과정을 선택할 수 없습니다. 예외가, 커밋하는 동안 보고 된 이유를 검색할 수 있습니다. 6.1 EF의에서 새로운 기능에는 트랜잭션이 성공 하 고 오른쪽 작업 과정을 투명 하 게 수행 하는 경우 데이터베이스를 사용 하 여 다시 확인 하려면 EF 수 있습니다.  

## <a name="using-the-feature"></a>기능 사용  

기능을 사용 하도록 설정 하려면에 대 한 호출을 포함 해야 [SetTransactionHandler](https://msdn.microsoft.com/library/system.data.entity.dbconfiguration.setdefaulttransactionhandler.aspx) 의 생성자에서 프로그램 **DbConfiguration**합니다. 익숙한 없는 경우 **DbConfiguration**를 참조 하십시오 [코드 기반 구성](~/ef6/fundamentals/configuring/code-based.md)합니다. 트랜잭션이 실제로 커밋하지 못했습니다 일시적인 오류로 인해 서버에서 상황에 도움이 되는 ef6를 도입 했습니다 자동 재시도와 함께에서이 기능을 사용할 수 있습니다.  

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

## <a name="how-transactions-are-tracked"></a>트랜잭션은 추적 하는 방법  

라는 데이터베이스에 EF가 새 테이블을 추가할 자동으로 기능을 사용 하는 경우 **__Transactions**합니다. EF에서 트랜잭션 만들어지고 커밋하는 동안 트랜잭션 오류가 발생할 경우 해당 행이 있는지 검사 될 때마다 새 행이이 테이블에 삽입 됩니다.  

EF는 더 이상 필요 하지 않은 경우 테이블의 행을 정리 하는 최고의 수행 하지만 일부 경우에서 수동으로 테이블을 제거 해야 할 수 있습니다에 대 한 중간 응용 프로그램에 종료 되 면 테이블 증가할 수 있습니다.  

## <a name="how-to-handle-commit-failures-with-previous-versions"></a>이전 버전을 사용 하 여 커밋 실패를 처리 하는 방법

EF 6.1 이전 EF 제품에 커밋 실패를 처리 하는 메커니즘 하지 못했습니다. EF6의 이전 버전에 적용할 수 있는 이러한 상황을 처리 하는 방법은 여러 가지가 있습니다.  

### <a name="option-1---do-nothing"></a>옵션 1-작업 안 함  

트랜잭션 커밋하는 동안 연결 오류의 가능성 낮은 이므로 응용 프로그램이이 조건에서 실제로 발생 하는 경우 실패를 허용할 수도 있습니다.  

## <a name="option-2---use-the-database-to-reset-state"></a>옵션 2-데이터베이스를 사용 하 여 상태를 다시 설정  

1. 현재 DbContext를 삭제 합니다.  
2. 새 DbContext를 만들고 데이터베이스에서 응용 프로그램의 상태를 복원 합니다.  
3. 마지막 작업 수 완료 되지 않은 성공적으로 사용자에 게 알릴  

## <a name="option-3---manually-track-the-transaction"></a>옵션 3-수동으로 트랜잭션 추적  

1. 트랜잭션의 상태를 추적 하는 데 데이터베이스에 추적 된 테이블을 추가 합니다.  
2. 각 트랜잭션의 시작 부분에 있는 테이블에 행을 삽입 합니다.  
3. 커밋 중 연결이 실패 하는 경우 데이터베이스에서 해당 행의 존재 여부 확인 합니다.  
    - 행이 있는 경우 정상적으로 계속, 트랜잭션이 성공적으로 커밋된 처럼  
    - 행이 없으면 실행 전략을 사용 하 여 현재 작업을 다시 시도 합니다.  
4. 커밋이 성공 하면 테이블의 증가 방지 하려면 해당 행을 삭제 합니다.  

[이 블로그 게시물](http://blogs.msdn.com/b/adonet/archive/2013/03/11/sql-database-connectivity-and-the-idempotency-issue.aspx) SQL Azure이 작업을 수행 하는 것에 대 한 샘플 코드가 포함 되어 있습니다.  
