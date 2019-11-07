---
title: 사용자 지정 마이그레이션 기록 테이블-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/history-table
ms.openlocfilehash: 0007da7ce3d78fd8f17007ac79a395bb2e6efd86
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655714"
---
# <a name="custom-migrations-history-table"></a>사용자 지정 마이그레이션 기록 테이블

기본적으로 EF Core는 데이터베이스에 적용 된 마이그레이션을 `__EFMigrationsHistory`라는 테이블에 기록 하 여 추적 합니다. 다양 한 이유로이 테이블을 사용자의 요구에 맞게 사용자 지정 하는 것이 좋습니다.

> [!IMPORTANT]
> 마이그레이션을 적용 *한 후* 마이그레이션 기록 테이블을 사용자 지정 하는 경우 데이터베이스의 기존 테이블을 업데이트할 책임이 있습니다.

## <a name="schema-and-table-name"></a>스키마 및 테이블 이름

`OnConfiguring()` (또는 ASP.NET Core에서 `ConfigureServices()`)의 `MigrationsHistoryTable()` 메서드를 사용 하 여 스키마 및 테이블 이름을 변경할 수 있습니다. SQL Server EF Core 공급자를 사용 하는 예제는 다음과 같습니다.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MigrationTableNameContext.cs#TableNameContext)]

## <a name="other-changes"></a>기타 변경 내용

테이블의 추가 측면을 구성 하려면 공급자별 `IHistoryRepository` 서비스를 재정의 하 고 대체 합니다. SQL Server에서 MigrationId 열 이름을 *Id* 로 변경 하는 예는 다음과 같습니다.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepositoryContext)]

> [!WARNING]
> `SqlServerHistoryRepository`은 내부 네임 스페이스 내에 있으며 이후 릴리스에서 변경 될 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Schemas/Migrations/MyHistoryRepository.cs#HistoryRepository)]
