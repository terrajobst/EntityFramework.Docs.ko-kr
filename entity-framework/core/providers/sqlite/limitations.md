---
title: SQLite 데이터베이스 공급자-제한 사항-EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: eaa7d5b1496172e4f3821433a1cd098ee7e8b737
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394796"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF Core 데이터베이스 공급자의 제한 사항

SQLite 공급자에 많은 마이그레이션 제한 사항이 있습니다. 대부분의 이러한 제한 기본 SQLite 데이터베이스 엔진에서 제한 사항의 결과 되며 EF와 관련이 없습니다.

## <a name="modeling-limitations"></a>모델링 제한 사항

대부분의 관계형 데이터베이스 엔진에 공통 되는 개념을 모델링 하는 것에 대 한 Api를 정의 하는 일반적인 관계형 라이브러리 (Entity Framework 관계형 데이터베이스 공급자에 의해 공유). 이러한 개념 중 몇 SQLite 공급자가 지원 되지 않습니다.

* 스키마
* 시퀀스
* 계산 열

## <a name="query-limitations"></a>쿼리 제한 사항

SQLite는 데이터 형식은 기본적으로 지원 하지 않습니다. EF Core 읽고 이러한 형식 및 같음 쿼리 값을 쓸 수 있습니다 (`where e.Property == value`)도 지원 됩니다. 그러나 다른 작업을 비교와 클라이언트에서 평가 순서 지정 해야 합니다.

* DateTimeOffset
* Decimal
* TimeSpan
* UInt64

대신 `DateTimeOffset`, 날짜/시간 값을 사용 하는 것이 좋습니다. 여러 표준 시간대를 처리 하는 경우에 값을 저장 하 고 적절 한 표준 시간대로 다시 변환 전에 UTC로 변환 하는 것이 좋습니다.

`Decimal` 형식은 높은 수준의 전체 자릿수를 제공 합니다. 그러나 전체 자릿수는 수준이 필요 하지 않으면, 좋습니다 double을 대신 사용 합니다. 사용할 수는 [값 변환기](../../modeling/value-conversions.md) 소수 클래스에서 사용 하 여 계속 합니다.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>마이그레이션 제한 사항

SQLite 데이터베이스 엔진이 다양 한 대부분의 기타 관계형 데이터베이스에서 지원 되는 스키마 작업을 지원 하지 않습니다. SQLite 데이터베이스에 지원 되지 않는 작업 중 하나를 적용 하려고 하면 다음을 `NotSupportedException` throw 됩니다.

| 작업            | 지원 되나요? | 버전 필요 |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✔          | 2.2.2            |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (아무)  | 2.0              |
| DropSchema           | ✔ (아무)  | 2.0              |
| Insert               | ✔          | 2.0              |
| 업데이트               | ✔          | 2.0              |
| 삭제               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>마이그레이션 제한 사항 해결

일부 해결 방법을 사용 하면 수동으로 테이블을 수행 하 여 마이그레이션에 코드를 작성 하 여 이러한 한계를 다시 작성 합니다. 테이블 다시 빌드에는 기존 테이블 이름 바꾸기, 새 테이블 만들기, 새 테이블에 데이터 복사 및 이전 테이블 삭제가 포함됩니다. 사용 해야 합니다는 `Sql(string)` 일부이 단계를 수행 하는 방법입니다.

참조 [다른 종류의 테이블 스키마 변경 사항을 편집](http://sqlite.org/lang_altertable.html#otheralter) 자세한 SQLite 설명서.

나중에 EF 내부적으로 테이블 다시 빌드 방법을 사용 하 여 이러한 작업 일부를 지원할 수 있습니다. 할 수 있습니다 [GitHub 프로젝트에서이 기능은 추적](https://github.com/aspnet/EntityFrameworkCore/issues/329)합니다.
