---
title: 마이그레이션 기록 테이블-EF6를 사용자 지정
author: divega
ms.date: 2016-10-23
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: ad07b1fe97b3dafe789c0315bd555f979fc412e7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996763"
---
# <a name="customizing-the-migrations-history-table"></a>마이그레이션 기록 테이블을 사용자 지정
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

> [!NOTE]
> 이 문서에서는 기본 시나리오에서 Code First 마이그레이션을 사용 하는 방법을 알고 있다고 가정 합니다. 이렇게 하지 않으면 경우 읽기 해야 [Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/index.md) 계속 하기 전에 합니다.

## <a name="what-is-migrations-history-table"></a>마이그레이션 기록 테이블 이란?

마이그레이션 기록 테이블은 데이터베이스에 적용 되는 마이그레이션에 대 한 정보를 저장 하기 위해 Code First 마이그레이션을 사용 하는 테이블입니다. 기본적으로 데이터베이스에서 테이블의 이름은 \_ \_MigrationHistory 하며 첫 번째 마이그레이션 수행 된 데이터베이스를 적용 하는 경우 생성 됩니다. Entity Framework 5이이 테이블 응용 프로그램이 Microsoft Sql Server 데이터베이스를 사용 하는 경우 시스템 테이블을 했습니다. 그러나 Entity Framework 6에서 변경 된이 및 마이그레이션 기록 테이블의 시스템 테이블을 더 이상 표시 됩니다.

## <a name="why-customize-migrations-history-table"></a>마이그레이션 기록 테이블을 사용자 지정 이유?

마이그레이션 기록 테이블 에서만 Code First 마이그레이션을 사용 해야 하 고 수동으로 변경 하면 마이그레이션을 중단할 수 있습니다. 그러나 경우에 따라 기본 구성은 적합 하지 않습니다 및 사용자 지정, 예를 들어 테이블에 필요:

-   이름 및/또는 3을 사용 하려면 열의 측면을 변경 해야 할<sup>rd</sup> 파티 마이그레이션 공급자
-   테이블의 이름을 변경.
-   에 대 한 기본이 아닌 스키마를 사용 해야 합니다 \_ \_MigrationHistory 테이블
-   컨텍스트는 지정 된 버전에 대 한 추가 데이터를 저장 해야 하며 테이블에 추가 열을 추가 해야 하므로

## <a name="words-of-precaution"></a>예방 조치로 단어

마이그레이션 기록 테이블을 변경 하는 것은 강력 하지만 지나치게 많이 되지 않도록 주의 해야 합니다. EF 런타임에 현재 확인 하지 여부를 사용자 지정된 마이그레이션 기록 테이블은는 런타임과 호환 됩니다. 응용 프로그램이 없는 경우 런타임 시 중단 또는 예기치 않은 방식으로 동작 하도록 수 있습니다. 이 훨씬 더 중요 데이터베이스별 다중 컨텍스트를 사용 하는 경우는 경우 여러 컨텍스트에서 사용할 수 동일한 마이그레이션 기록 테이블의 마이그레이션에 대 한 정보를 저장 합니다.

## <a name="how-to-customize-migrations-history-table"></a>마이그레이션 기록 테이블을 사용자 지정 하는 방법

시작 하기 전에 알아야 첫 번째 마이그레이션을 적용 하기 전에만 마이그레이션 기록 테이블 사용자 지정할 수 있습니다. 이제 코드입니다.

첫째, System.Data.Entity.Migrations.History.HistoryContext 클래스에서 파생 된 클래스를 만드는 해야 합니다. 마이그레이션 기록 테이블을 구성 하는 것은 매우 유사 fluent API를 사용 하 여 EF 모델을 구성 하므로 HistoryContext 클래스는 DbContext 클래스에서 파생 됩니다. OnModelCreating 메서드를 재정의 하 여 fluent API를 사용 하 여 테이블을 구성 하기만 하면 됩니다.

>[!NOTE]
> 일반적으로 EF 모델을 구성한 경우에 기본 호출할 필요가 없습니다. DbContext.OnModelCreating() 이후 OnModelCreating 메서드 재정의에서 onmodelcreating ()의 본문이 비어 있습니다. 마이그레이션 기록 테이블을 구성 하는 경우에 해당 아닙니다. 이 경우 첫 번째 onmodelcreating () 재정의에서 할 일은 실제로 기본를 호출 하는 것입니다. Onmodelcreating ()입니다. 이렇게 하면 마이그레이션 기록 테이블에서 메서드를 재정의 한 다음 조정 하는 기본 방식으로 구성 됩니다.

마이그레이션 기록 테이블의 이름을 바꾸고 "관리자" 라는 사용자 지정 스키마를 저장 하려는 경우를 가정해 봅니다. 또한 DBA 알려주실 것 마이그레이션 MigrationId 열 이름을 바꾸려면\_id입니다.  HistoryContext에서 파생 된 다음 클래스를 만들어이 달성할 수 있습니다.

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

EF를 통해 등록 하 여 인식 확인 해야 하는 사용자 지정 프로그램 HistoryContext 준비 되 면 [코드 기반 구성](http://msdn.com/data/jj680699):

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

이제 거의 완성 되었습니다. Enable-migrations, 패키지 관리자 콘솔을 이동할 수 있습니다 이제 추가 마이그레이션 및 마지막으로 데이터베이스 업데이트 합니다. 마이그레이션 기록 테이블 HistoryContext 파생 클래스에서 지정 된 세부 정보에 따라 구성 데이터베이스에 추가 그러면 합니다.

![데이터베이스](~/ef6/media/database.png)
