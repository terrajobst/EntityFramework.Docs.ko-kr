---
title: SQLite 데이터베이스 공급자-제한 사항-EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 69c40fcd8b7ddb925728b1bad9992ad2a81e7540
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994666"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF Core 데이터베이스 공급자의 제한 사항

SQLite 공급자에 많은 마이그레이션 제한 사항이 있습니다. 대부분의 이러한 제한 기본 SQLite 데이터베이스 엔진에서 제한 사항의 결과 되며 EF와 관련이 없습니다.

## <a name="modeling-limitations"></a>모델링 제한 사항

대부분의 관계형 데이터베이스 엔진에 공통 되는 개념을 모델링 하는 것에 대 한 Api를 정의 하는 일반적인 관계형 라이브러리 (Entity Framework 관계형 데이터베이스 공급자에 의해 공유). 이러한 개념 중 몇 SQLite 공급자가 지원 되지 않습니다.

* 스키마
* 시퀀스

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
| RenameColumn         | ✗          |                  |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (아무)  | 2.0              |
| DropSchema           | ✔ (아무)  | 2.0              |
| Insert               | ✔          | 2.0              |
| 업데이트               | ✔          | 2.0              |
| 삭제               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>마이그레이션 제한 사항 해결

일부 해결 방법을 사용 하면 수동으로 테이블을 수행 하 여 마이그레이션에 코드를 작성 하 여 이러한 한계를 다시 작성 합니다. 테이블 다시 빌드는 기존 테이블 이름 바꾸기, 새 테이블 만들기, 새 테이블에 데이터를 복사 및 이전 테이블을 삭제 해야 합니다. 사용 해야 합니다는 `Sql(string)` 일부이 단계를 수행 하는 방법입니다.

참조 [다른 종류의 테이블 스키마 변경 사항을 편집](http://sqlite.org/lang_altertable.html#otheralter) 자세한 SQLite 설명서.

나중에 EF 내부적으로 테이블 다시 빌드 방법을 사용 하 여 이러한 작업 일부를 지원할 수 있습니다. 할 수 있습니다 [GitHub 프로젝트에서이 기능은 추적](https://github.com/aspnet/EntityFrameworkCore/issues/329)합니다.
