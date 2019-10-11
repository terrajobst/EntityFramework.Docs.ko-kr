---
title: SQLite 데이터베이스 공급자-제한 사항-EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 2f80dc195265787318ac4925dd937da45ffad011
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72179777"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF Core 데이터베이스 공급자 제한 사항

SQLite 공급자에는 여러 마이그레이션 제한 사항이 있습니다. 이러한 제한 사항 중 대부분은 기본 SQLite 데이터베이스 엔진의 제한 사항 때문 이며 EF에 국한 되지 않습니다.

## <a name="modeling-limitations"></a>모델링 제한 사항

공통 관계형 라이브러리 (Entity Framework 관계형 데이터베이스 공급자에서 공유)는 대부분의 관계형 데이터베이스 엔진에 공통적인 모델링 개념에 대 한 Api를 정의 합니다. 이러한 개념 중 몇 가지는 SQLite 공급자가 지원 하지 않습니다.

* 스키마
* 시퀀스
* 계산 열

## <a name="query-limitations"></a>쿼리 제한 사항

SQLite는 기본적으로 다음 데이터 형식을 지원 하지 않습니다. 이러한 형식의 값을 읽고 쓸 수 EF Core 같음 (`where e.Property == value`)에 대 한 쿼리도 지원 됩니다. 그러나 비교 및 정렬과 같은 다른 작업은 클라이언트에서 평가 해야 합니다.

* DateTimeOffset
* Decimal
* TimeSpan
* UInt64

@No__t-0 대신 DateTime 값을 사용 하는 것이 좋습니다. 여러 표준 시간대를 처리 하는 경우에는 저장 하기 전에 값을 UTC로 변환한 다음 다시 적절 한 표준 시간대로 변환 하는 것이 좋습니다.

@No__t-0 형식은 높은 수준의 전체 자릿수를 제공 합니다. 그러나 정밀도 수준이 필요 하지 않은 경우에는 double을 대신 사용 하는 것이 좋습니다. [값 변환기](../../modeling/value-conversions.md) 를 사용 하 여 클래스에서 decimal을 계속 사용할 수 있습니다.

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a>마이그레이션 제한 사항

SQLite 데이터베이스 엔진은 대부분의 다른 관계형 데이터베이스에서 지원 되는 많은 스키마 작업을 지원 하지 않습니다. 지원 되지 않는 작업 중 하나를 SQLite 데이터베이스에 적용 하려고 하면 `NotSupportedException`이 throw 됩니다.

| 연산            | 되지? | 버전 필요 |
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
| EnsureSchema         | ✔ (작동 안 함)  | 2.0              |
| DropSchema           | ✔ (작동 안 함)  | 2.0              |
| Insert               | ✔          | 2.0              |
| 업데이트               | ✔          | 2.0              |
| 삭제               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>마이그레이션 제한 해결 방법

마이그레이션에 수동으로 코드를 작성 하 여 테이블 다시 빌드를 수행 하면 이러한 제한 사항 중 일부를 해결할 수 있습니다. 테이블 다시 빌드에는 기존 테이블 이름 바꾸기, 새 테이블 만들기, 새 테이블에 데이터 복사 및 이전 테이블 삭제가 포함됩니다. @No__t-0 메서드를 사용 하 여 이러한 단계 중 일부를 수행 해야 합니다.

자세한 내용은 SQLite 설명서에서 [다른 종류의 테이블 스키마 변경 만들기](https://sqlite.org/lang_altertable.html#otheralter) 를 참조 하세요.

앞으로 EF는 내부적으로 테이블 다시 작성 방법을 사용 하 여 이러한 작업 중 일부를 지원할 수 있습니다. [GitHub 프로젝트에서이 기능을 추적할](https://github.com/aspnet/EntityFrameworkCore/issues/329)수 있습니다.
