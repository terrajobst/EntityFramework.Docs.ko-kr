---
title: "SQLite 데이터베이스 공급자-도-전보다 EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>SQLite EF 코어 데이터베이스 공급자의 제한 사항

SQLite 공급자 마이그레이션 제한 사항을 번호가 있습니다. 대부분의 이러한 제한은 기본 SQLite 데이터베이스 엔진의 제한 한 결과 및 EF에 한정 되지 않는 합니다.

## <a name="modeling-limitations"></a>제한 사항 모델링

(Entity Framework 관계형 데이터베이스 공급자에 의해 공유) 일반적인 관계형 라이브러리는 대부분의 관계형 데이터베이스 엔진에 공통 된 개념 모델링에 대 한 Api를 정의 합니다. 이러한 개념 중 몇 SQLite 공급자가 지원 되지 않습니다.

* 스키마
* 시퀀스

## <a name="migrations-limitations"></a>마이그레이션 제한 사항

SQLite 데이터베이스 엔진에서 대부분의 다른 관계형 데이터베이스에서 지원 되는 스키마 작업의 수를 지원 하지 않습니다. SQLite 데이터베이스에 지원 되지 않는 작업 중 하나를 적용 하려는 경우는 `NotSupportedException` throw 됩니다.

| 작업            | 지원 되나요? |
| -------------------- | ---------- |
| AddColumn            | ✔          |
| AddForeignKey        | ✗          |
| AddPrimaryKey        | ✗          |
| AddUniqueConstraint  | ✗          |
| AlterColumn          | ✗          |
| CreateIndex          | ✔          |
| CreateTable          | ✔          |
| DropColumn           | ✗          |
| DropForeignKey       | ✗          |
| DropIndex            | ✔          |
| DropPrimaryKey       | ✗          |
| DropTable            | ✔          |
| DropUniqueConstraint | ✗          |
| RenameColumn         | ✗          |
| RenameIndex          | ✗          |
| RenameTable          | ✔          |

## <a name="migrations-limitations-workaround"></a>마이그레이션 제한 사항 해결 방법

일부를 방지할 수 있습니다 이러한 제한 사항을 수동으로 테이블을 수행 하 여 마이그레이션에 코드를 작성 하 여 다시 작성 합니다. 테이블 rebuild 기존 테이블 이름 바꾸기, 새 테이블 만들기, 새 테이블에 데이터를 복사 및 이전 테이블을 삭제 해야 합니다. 사용 해야 합니다는 `Sql(string)` 메서드를 다음이 단계 중 일부를 수행 합니다.

참조 [다른 종류의 테이블 스키마 변경 사항을 편집](http://sqlite.org/lang_altertable.html#otheralter) 자세한 내용은 SQLite 설명서에 있습니다.

나중에 EF 내부적 테이블 다시 작성 방법을 사용 하 여 이러한 작업 중 일부를 지원할 수 있습니다. 있습니다 수 [우리의 GitHub 프로젝트에서이 기능을 추적](https://github.com/aspnet/EntityFramework/issues/329)합니다.
