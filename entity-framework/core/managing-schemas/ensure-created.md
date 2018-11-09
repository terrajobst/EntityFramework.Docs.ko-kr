---
title: Api-EF Core 만들기 및 삭제
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285641"
---
# <a name="create-and-drop-apis"></a>Api 만들기 및 삭제

EnsureCreated 메서드와 EnsureDeleted 간단한 대안을 제공 [마이그레이션을](migrations/index.md) 데이터베이스 스키마를 관리 합니다. 이 경우 시나리오에서 유용한 데이터는 일시적 이며 스키마가 변경 하는 경우에 삭제할 수 있습니다. 예를 들어 하는 동안 프로토타입 생성, 테스트 또는 로컬 캐시에 대 한 합니다.

일부 공급자 (비관계형 특히) 마이그레이션을 지원 하지 않습니다. 이러한 경우 EnsureCreated은 데이터베이스 스키마를 초기화 하는 가장 쉬운 방법은 경우가 많습니다.

> [!WARNING]
> EnsureCreated 및 마이그레이션 함께 잘 작동 하지 않습니다. 마이그레이션을 사용 하는 경우 스키마를 초기화할 EnsureCreated를 사용 하지 마세요.

원활한 환경을 아닙니다 EnsureCreated에서 마이그레이션을로 전환 합니다. 이렇게 하려면 simpelest 방법은 데이터베이스를 삭제 하 고 마이그레이션을 사용 하 여 다시 만들어야 합니다. 나중에 마이그레이션을 사용 하 여 예상 되는 경우 EnsureCreated를 사용 하는 대신 마이그레이션을 사용 하 여 시작 하는 것이 좋습니다.

## <a name="ensuredeleted"></a>EnsureDeleted

EnsureDeleted 메서드는 존재 하는 경우 데이터베이스를 삭제 합니다. 적합 한 권한이 없으면 예외가 throw 됩니다.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated 존재 하지 않습니다 고 데이터베이스 스키마를 초기화 하는 경우 데이터베이스를 만듭니다. 모든 테이블이 존재 하는 경우 (다른 DbContext 클래스에 대 한 테이블 포함), 스키마 않습니다 초기화.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> 이러한 메서드의 비동기 버전도 사용할 수 있습니다.

## <a name="sql-script"></a>SQL 스크립트

EnsureCreated에서 사용 하는 SQL을 가져오려면 GenerateCreateScript 메서드를 사용할 수 있습니다.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>여러 DbContext 클래스

EnsureCreated는 테이블이 데이터베이스에 있는 경우에 작동 합니다. 필요한 경우 스키마를 초기화 해야 하는 경우 사용자 고유의 확인 쓰고 기본 IRelationalDatabaseCreator 서비스를 사용 하 여 스키마를 초기화할 수 있습니다.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
