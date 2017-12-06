---
title: "InMemory-EF 코어를 사용 하 여 테스트"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: c5c48c575e9fd693d1f28d1a6d10eb83ebbc9d70
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
---
# <a name="testing-with-inmemory"></a>InMemory를 사용 하 여 테스트

InMemory 공급자 접근 실제 데이터베이스 작업의 오버 헤드 없이 실제 데이터베이스에 연결 하는 수준의 암호화를 사용 하는 구성 요소를 테스트 하려는 경우 유용 합니다.

> [!TIP]  
> 이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) GitHub에서 합니다.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory 관계형 데이터베이스가 아닙니다.

EF 핵심 데이터베이스 공급자는 관계형 데이터베이스를 사용할 필요가 없습니다. InMemory는 테스트를 위해 일반 용도의 데이터베이스 되도록 설계 및 관계형 데이터베이스와 유사 하 게 설계 되지 않았습니다.

이 다음과의 몇 가지 예:
* InMemory를 사용 하면 관계형 데이터베이스의 참조 무결성 제약 조건을 위반 하는 데이터를 저장할 수 있습니다.

* 모델의 속성에 대 한 DefaultValueSql(string)를 사용 하는 경우 관계형 데이터베이스 API 이므로 InMemory에 대해 실행할 때 영향을 미치지 않습니다.

> [!TIP]  
> 목적으로 테스트에 대 한 이러한 차이 중요 하지 않습니다. 그러나 true 관계형 데이터베이스 처럼 작동 하는 것에 대해 테스트 하려는 경우 다음 사용을 고려해 [SQLite 메모리 내 모드](sqlite.md)합니다.

## <a name="example-testing-scenario"></a>테스트 시나리오의 예

서비스를 응용 프로그램 코드 블로그와 관련 된 많은 작업을 수행 하도록 허용 하는 것이 좋습니다. 내부적으로 사용 하는 `DbContext` SQL Server 데이터베이스에 연결 하 합니다. 코드를 수정 하지 않고이 서비스에 대 한 효율적인 테스트를 작성 하거나 많은 테스트를 만드는 작업을 수행할 수 있도록 InMemory 데이터베이스에 연결 하기 위한이 컨텍스트와 바꿔 유용할 컨텍스트의 double입니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>컨텍스트 사용 준비

### <a name="avoid-configuring-two-database-providers"></a>두 데이터베이스 공급자를 구성 하지 않으려면

테스트에서 InMemory 공급자를 사용 하는 컨텍스트 외부에서 구성 하려는 합니다. 재정의 하 여 데이터베이스 공급자를 구성 하는 경우 `OnConfiguring` 사용자 컨텍스트에서 다음 필요한 이미 구성 되지 않은 한 경우에 데이터베이스 공급자를 구성 하는 수 있도록 일부 조건부 코드를 추가 합니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> ASP.NET Core를 사용 하는 경우 다음 않아도이 코드 데이터베이스 공급자 (Startup.cs)의 컨텍스트 외부에서 이미 구성 되어 있으므로 합니다.

### <a name="add-a-constructor-for-testing"></a>테스트에 대 한 생성자를 추가 합니다.

가장 간단한 방법은 다른 데이터베이스에 대해 테스트할 수 있도록 허용 하는 생성자를 노출 하 여 컨텍스트를 수정 하는 것을 `DbContextOptions<TContext>`합니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`에 연결 하는 데이터베이스와 같은 해당 설정의 모든 컨텍스트를 지정 합니다. 사용자 컨텍스트에서 OnConfiguring 메서드를 실행 하 여 작성 되는 동일한 개체입니다.

## <a name="writing-tests"></a>테스트 작성

이 공급자를 사용 하 여 테스트 하는 키 InMemory 공급자를 사용 하기 위해 메모리 내 데이터베이스의 범위 제어 컨텍스트를 알 수 있다는 점입니다. 보통 각 테스트 메서드에 대 한 데이터베이스 정리 합니다.

InMemory 데이터베이스를 사용 하는 테스트 클래스의 예를 들면 다음과 같습니다. 각 테스트 메서드는 각 방법에는 고유한 InMemory 데이터베이스를 의미 하 고 고유한 데이터베이스 이름을 지정 합니다.

>[!TIP]
> 사용 하 여 `.UseInMemoryDatabase()` 확장 메서드를 참조 Nuget 패키지 `Microsoft.EntityFrameworkCore.InMemory`합니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
