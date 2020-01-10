---
title: InMemory를 사용 하 여 테스트-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: fcd2f99ad06fd30ef9e36fd1e5a6a09fe0a45d07
ms.sourcegitcommit: 4e86f01740e407ff25e704a11b1f7d7e66bfb2a6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75781120"
---
# <a name="testing-with-inmemory"></a>InMemory 테스트

InMemory 공급자는 실제 데이터베이스 작업의 오버 헤드 없이 실제 데이터베이스에 연결 하는 것과 유사한 항목을 사용 하 여 구성 요소를 테스트 하려는 경우에 유용 합니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing)을 볼 수 있습니다.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory가 관계형 데이터베이스가 아닙니다.

EF Core 데이터베이스 공급자는 관계형 데이터베이스가 될 필요는 없습니다. InMemory는 테스트를 위한 범용 데이터베이스로 설계 되었으며 관계형 데이터베이스를 모방 하도록 설계 되지 않았습니다.

이에 대 한 몇 가지 예는 다음과 같습니다.

* InMemory를 통해 관계형 데이터베이스에서 참조 무결성 제약 조건을 위반 하는 데이터를 저장할 수 있습니다.
* 모델의 속성에 DefaultValueSql (string)을 사용 하는 경우이는 관계형 데이터베이스 API 이며 InMemory에 대해 실행 하는 경우에는 영향을 주지 않습니다.
* [Timestamp/row 버전](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` 또는 `IsRowVersion`)을 통한 동시성은 지원 되지 않습니다. 이전 동시성 토큰을 사용 하 여 업데이트를 수행 하는 경우 [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) 이 throw 되지 않습니다.

> [!TIP]  
> 많은 테스트를 위해 이러한 차이가 중요 하지 않습니다. 그러나 진정한 관계형 데이터베이스 처럼 동작 하는 항목을 테스트 하려면 [SQLite 메모리 내 모드](sqlite.md)를 사용 하는 것이 좋습니다.

## <a name="example-testing-scenario"></a>예제 테스트 시나리오

응용 프로그램 코드에서 블로그와 관련 된 일부 작업을 수행할 수 있도록 하는 다음 서비스를 고려 하십시오. 내부적으로는 SQL Server 데이터베이스에 연결 하는 `DbContext`을 사용 합니다. 코드를 수정 하거나 컨텍스트의 테스트 double을 만들기 위해 많은 작업을 수행 하지 않고도이 서비스에 대 한 효율적인 테스트를 작성할 수 있도록이 컨텍스트를 바꿔 InMemory 데이터베이스에 연결 하는 것이 유용할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>컨텍스트 준비

### <a name="avoid-configuring-two-database-providers"></a>두 데이터베이스 공급자를 구성 하지 마십시오.

테스트에서 InMemory 공급자를 사용 하도록 컨텍스트를 외부적으로 구성 합니다. 컨텍스트에서 `OnConfiguring`를 재정의 하 여 데이터베이스 공급자를 구성 하는 경우 데이터베이스 공급자가 아직 구성 되지 않은 경우에만 구성 되도록 일부 조건부 코드를 추가 해야 합니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> ASP.NET Core를 사용 하는 경우 데이터베이스 공급자가 컨텍스트 외부 (Startup.cs)에서 이미 구성 되었으므로이 코드가 필요 하지 않습니다.

### <a name="add-a-constructor-for-testing"></a>테스트를 위한 생성자 추가

다른 데이터베이스에 대해 테스트를 사용 하도록 설정 하는 가장 간단한 방법은 `DbContextOptions<TContext>`을 허용 하는 생성자를 노출 하도록 컨텍스트를 수정 하는 것입니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`은 연결할 데이터베이스와 같은 모든 설정에 컨텍스트를 알려 줍니다. 이는 컨텍스트에서 OnConfiguring 메서드를 실행 하 여 작성 된 것과 동일한 개체입니다.

## <a name="writing-tests"></a>테스트 작성

이 공급자를 사용 하 여 테스트 하는 핵심은 InMemory 공급자를 사용 하도록 컨텍스트에 지시 하 고 메모리 내 데이터베이스의 범위를 제어 하는 기능입니다. 일반적으로 각 테스트 메서드에 대해 완전히 정리 된 데이터베이스를 원합니다.

다음은 InMemory 데이터베이스를 사용 하는 테스트 클래스의 예제입니다. 각 테스트 메서드는 고유한 데이터베이스 이름을 지정 합니다. 즉, 각 메서드에 고유한 InMemory 데이터베이스가 있습니다.

>[!TIP]
> `.UseInMemoryDatabase()` 확장 메서드를 사용 하려면 NuGet 패키지 [microsoft.entityframeworkcore.tools.dotnet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)를 참조 하세요.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
