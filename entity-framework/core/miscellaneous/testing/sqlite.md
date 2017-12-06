---
title: "SQLite-EF 코어를 사용 하 여 테스트"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="testing-with-sqlite"></a>SQLite를 사용 하 여 테스트

SQLite는 SQLite를 사용 하 여 실제 데이터베이스 작업의 오버 헤드 없이 관계형 데이터베이스에 대해 테스트를 작성할 수 있는 메모리 내 모드를 있습니다.

> [!TIP]  
> 이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) GitHub에서

## <a name="example-testing-scenario"></a>테스트 시나리오의 예

서비스를 응용 프로그램 코드 블로그와 관련 된 많은 작업을 수행 하도록 허용 하는 것이 좋습니다. 내부적으로 사용 하는 `DbContext` SQL Server 데이터베이스에 연결 하 합니다. 코드를 수정 하지 않고이 서비스에 대 한 효율적인 테스트를 작성 하거나 많은 테스트를 만드는 작업을 수행할 수 있도록 메모리 SQLite 데이터베이스에 연결 하기 위한이 컨텍스트와 바꿔 유용할 컨텍스트의 double입니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>컨텍스트 사용 준비

### <a name="avoid-configuring-two-database-providers"></a>두 데이터베이스 공급자를 구성 하지 않으려면

테스트에서 InMemory 공급자를 사용 하는 컨텍스트 외부에서 구성 하려는 합니다. 재정의 하 여 데이터베이스 공급자를 구성 하는 경우 `OnConfiguring` 사용자 컨텍스트에서 다음 필요한 이미 구성 되지 않은 한 경우에 데이터베이스 공급자를 구성 하는 수 있도록 일부 조건부 코드를 추가 합니다.

> [!TIP]  
> ASP.NET Core를 사용 하는 경우 다음 않아도이 코드 이후 데이터베이스 공급자 (Startup.cs)의 컨텍스트 외부에서 구성 됩니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>테스트에 대 한 생성자를 추가 합니다.

가장 간단한 방법은 다른 데이터베이스에 대해 테스트할 수 있도록 허용 하는 생성자를 노출 하 여 컨텍스트를 수정 하는 것을 `DbContextOptions<TContext>`합니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`에 연결 하는 데이터베이스와 같은 해당 설정의 모든 컨텍스트를 지정 합니다. 사용자 컨텍스트에서 OnConfiguring 메서드를 실행 하 여 작성 되는 동일한 개체입니다.

## <a name="writing-tests"></a>테스트 작성

이 공급자를 사용 하 여 테스트 하는 키 SQLite 사용 하기 위해 메모리 내 데이터베이스의 범위 제어 컨텍스트를 알 수 있다는 점입니다. 데이터베이스의 범위는 중괄호와 닫는 연결에 의해 제어 됩니다. 데이터베이스 연결이 열려 있는 기간으로 범위가 지정 됩니다. 보통 각 테스트 메서드에 대 한 데이터베이스 정리 합니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
