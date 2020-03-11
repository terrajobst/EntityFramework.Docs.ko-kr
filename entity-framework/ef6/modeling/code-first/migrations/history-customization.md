---
title: 마이그레이션 기록 테이블 사용자 지정-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: eb19f367611a86f685557a6741a5f2f0bad6b718
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415684"
---
# <a name="customizing-the-migrations-history-table"></a>마이그레이션 기록 테이블 사용자 지정
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

> [!NOTE]
> 이 문서에서는 기본 시나리오에서 Code First 마이그레이션를 사용 하는 방법을 알고 있다고 가정 합니다. 그렇지 않으면 계속 하기 전에 [Code First 마이그레이션](~/ef6/modeling/code-first/migrations/index.md) 읽어야 합니다.

## <a name="what-is-migrations-history-table"></a>마이그레이션 기록 테이블 이란?

마이그레이션 기록 테이블은 Code First 마이그레이션에서 데이터베이스에 적용 되는 마이그레이션에 대 한 세부 정보를 저장 하는 데 사용 되는 테이블입니다. 기본적으로 데이터베이스의 테이블 이름은 \_\_MigrationHistory 되며 데이터베이스에 첫 번째 마이그레이션을 적용할 때 생성 됩니다. Entity Framework 5에서는 응용 프로그램이 Microsoft Sql Server 데이터베이스를 사용 하는 경우이 테이블이 시스템 테이블 이었습니다. 이는 Entity Framework 6에서 변경 되었지만 마이그레이션 기록 테이블은 더 이상 시스템 테이블로 표시 되지 않습니다.

## <a name="why-customize-migrations-history-table"></a>마이그레이션 기록 테이블을 사용자 지정 하는 이유

마이그레이션 기록 테이블은 Code First 마이그레이션만 사용 하 고 수동으로 변경 하면 마이그레이션이 중단 될 수 있습니다. 그러나 경우에 따라 기본 구성이 적절 하지 않으며 테이블을 사용자 지정 해야 합니다. 예를 들면 다음과 같습니다.

-   3 개의<sup>rd</sup> 파티 마이그레이션 공급자를 사용 하도록 설정 하려면 열의 이름 및/또는 패싯을 변경 해야 합니다.
-   테이블의 이름을 변경 하려고 합니다.
-   \_\_MigrationHistory 테이블에는 기본이 아닌 스키마를 사용 해야 합니다.
-   지정 된 버전의 컨텍스트에 대 한 추가 데이터를 저장 해야 하므로 테이블에 추가 열을 추가 해야 합니다.

## <a name="words-of-precaution"></a>예방 조치 단어

마이그레이션 기록 테이블을 변경 하는 것은 강력 하지만이를 과도 하 게 수행 하지 않도록 주의 해야 합니다. EF runtime은 현재 사용자 지정 된 마이그레이션 기록 테이블이 런타임과 호환 되는지 여부를 확인 하지 않습니다. 그렇지 않으면 응용 프로그램이 런타임에 중단 되거나 예측할 수 없는 방식으로 동작할 수 있습니다. 데이터베이스당 여러 컨텍스트를 사용 하는 경우에는 여러 컨텍스트에서 동일한 마이그레이션 기록 테이블을 사용 하 여 마이그레이션에 대 한 정보를 저장할 수 있는 경우에도 훨씬 더 중요 합니다.

## <a name="how-to-customize-migrations-history-table"></a>마이그레이션 기록 테이블을 사용자 지정 하는 방법

시작 하기 전에 첫 번째 마이그레이션을 적용 하기 전에만 마이그레이션 기록 테이블을 사용자 지정할 수 있음을 알아야 합니다. 이제 코드를 실행 합니다.

첫째, HistoryContext 클래스에서 파생 된 클래스를 만들어야 하는 경우를 들 수 있습니다. HistoryContext 클래스는 DbContext 클래스에서 파생 되므로 마이그레이션 기록 테이블을 구성 하는 것은 흐름 API를 사용 하 여 EF 모델을 구성 하는 것과 매우 비슷합니다. OnModelCreating 메서드를 재정의 하 고 흐름 API를 사용 하 여 테이블을 구성 하기만 하면 됩니다.

>[!NOTE]
> 일반적으로 EF 모델을 구성 하는 경우 base를 호출할 필요가 없습니다. DbContext. Onmodelcreating ()의 본문이 비어 있으므로 재정의 된 OnModelCreating 메서드에서 OnModelCreating ()가 발생 합니다. 이는 마이그레이션 기록 테이블을 구성 하는 경우에는 그렇지 않습니다. 이 경우 OnModelCreating () 재정의에서 가장 먼저 해야 할 일은 base를 호출 하는 것입니다. OnModelCreating ()입니다. 이렇게 하면 기본 방법으로 마이그레이션 기록 테이블이 구성 되며이는 재정의 방법에서 조정 됩니다.

마이그레이션 기록 테이블의 이름을 바꾸고 "admin" 이라는 사용자 지정 스키마에 배치 하려는 경우를 가정해 보겠습니다. 또한 DBA는 MigrationId 열의 이름을 Migration\_ID로 변경 하려고 합니다.  HistoryContext에서 파생 된 다음 클래스를 만들어이를 달성할 수 있습니다.

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

사용자 지정 HistoryContext 준비 되 면 [코드 기반 구성을](https://msdn.com/data/jj680699)통해 등록 하 여 EF에서이를 인식 하 게 해야 합니다.

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

매우 간단 합니다. 이제 패키지 관리자 콘솔로 이동 하 여-Migration, 추가-마이그레이션 및 마지막으로 업데이트를 사용할 수 있습니다. 이로 인해 HistoryContext 파생 클래스에서 지정한 세부 정보에 따라 구성 된 마이그레이션 기록 테이블이 데이터베이스에 추가 됩니다.

![데이터베이스](~/ef6/media/database.png)
