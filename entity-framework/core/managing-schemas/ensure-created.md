---
title: Api 만들기 및 삭제-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
ms.openlocfilehash: 88c1403d2fae740ad78bb7c41d404b0dd91e86ae
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813437"
---
# <a name="create-and-drop-apis"></a>API 만들기 및 삭제

EnsureCreated 및 EnsureDeleted 메서드는 데이터베이스 스키마를 관리 하기 위한 [마이그레이션](migrations/index.md) 에 대 한 간단한 대안을 제공 합니다. 이러한 메서드는 데이터가 일시적 이며 스키마가 변경 될 때 삭제할 수 있는 시나리오에서 유용 합니다. 예를 들어 프로토타입, 테스트 또는 로컬 캐시에 대 한입니다.

일부 공급자 (특히 비관계형가 아님)는 마이그레이션을 지원 하지 않습니다. 이러한 공급자의 경우 EnsureCreated은 종종 데이터베이스 스키마를 초기화 하는 가장 쉬운 방법입니다.

> [!WARNING]
> EnsureCreated와 마이그레이션은 함께 제대로 작동 하지 않습니다. 마이그레이션을 사용 하는 경우 스키마를 초기화 하는 데 EnsureCreated를 사용 하지 마세요.

EnsureCreated에서 마이그레이션으로의 변환은 원활한 환경이 아닙니다. 이 작업을 수행 하는 가장 간단한 방법은 데이터베이스를 삭제 하 고 마이그레이션을 사용 하 여 다시 만드는 것입니다. 나중에 마이그레이션을 사용할 것으로 예상 되는 경우 EnsureCreated를 사용 하는 대신 마이그레이션을 시작 하는 것이 가장 좋습니다.

## <a name="ensuredeleted"></a>EnsureDeleted

EnsureDeleted 메서드는 데이터베이스 (있는 경우)를 삭제 합니다. 적절 한 권한이 없는 경우 예외가 throw 됩니다.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated가 존재 하지 않는 경우 데이터베이스를 만들고 데이터베이스 스키마를 초기화 합니다. 테이블이 있으면 (다른 DbContext 클래스에 대 한 테이블 포함) 스키마가 초기화 되지 않습니다.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> 이러한 메서드의 비동기 버전도 사용할 수 있습니다.

## <a name="sql-script"></a>SQL 스크립트

EnsureCreated에서 사용 하는 SQL을 가져오기 위해 GenerateCreateScript 메서드를 사용할 수 있습니다.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>여러 DbContext 클래스

EnsureCreated는 데이터베이스에 테이블이 없는 경우에만 작동 합니다. 필요한 경우 직접 검사를 작성 하 여 스키마를 초기화 해야 하는지 여부를 확인 하 고 기본 IRelationalDatabaseCreator 서비스를 사용 하 여 스키마를 초기화할 수 있습니다.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
