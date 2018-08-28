---
title: EF Core-SQLite를 사용 하 여 테스트
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996870"
---
# <a name="testing-with-sqlite"></a>SQLite를 사용 하 여 테스트

SQLite에는 메모리 내 모드에서는 SQLite를 사용 하 여 실제 데이터베이스 작업의 오버 헤드 없이 관계형 데이터베이스에 대해 테스트를 쓸 수 있도록 합니다.

> [!TIP]  
> 이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) GitHub에서

## <a name="example-testing-scenario"></a>예제 테스트 시나리오

다음 서비스를 블로그와 관련 된 몇 가지 작업을 수행 하는 응용 프로그램 코드를 허용 하는 것이 좋습니다. 사용 하 여 내부적으로 `DbContext` SQL Server 데이터베이스에 연결 하는 합니다. 많은 테스트를 만드는 작업을 수행 하거나 코드를 수정 하지 않고이 서비스에 대 한 효율적인 테스트를 작성할 수 있도록 메모리에 SQLite 데이터베이스에 연결 하려면이 컨텍스트를 교환 하려면 유용 컨텍스트의 double입니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>컨텍스트 준비

### <a name="avoid-configuring-two-database-providers"></a>두 데이터베이스 공급자를 구성 하지 않으려면

테스트에서 외부적으로 InMemory 공급자를 사용 하도록 컨텍스트를 구성 하려고 합니다. 데이터베이스 공급자를 재정의 하 여 구성 하는 경우 `OnConfiguring` 프로그램 컨텍스트에서 해야 이미 구성 되지 않은 한 경우에 데이터베이스 공급자를 구성 하는 일부 조건부 코드를 추가 합니다.

> [!TIP]  
> ASP.NET Core를 사용 하는 경우 다음 필요가 없습니다이 코드 (Startup.cs)의 컨텍스트 외부에서 데이터베이스 공급자 구성 되었기 때문입니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>테스트에 대 한 생성자를 추가 합니다.

가장 간단한 방법은 다른 데이터베이스에 대해 테스트할 수 있도록 허용 하는 생성자를 노출 하 여 컨텍스트를 수정 하는 것을 `DbContextOptions<TContext>`입니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` 모든 데이터베이스에 연결 하는 등 설정을 컨텍스트를 나타냅니다. 이 컨텍스트에서 OnConfiguring 메서드를 실행 하 여 기본 제공 되는 동일한 개체입니다.

## <a name="writing-tests"></a>테스트 작성

이 공급자를 사용 하 여 테스트 키가 SQLite를 사용 하 고 메모리 내 데이터베이스의 범위 제어 컨텍스트를 알 수 있습니다. 데이터베이스의 범위를 열고 연결을 닫는 중 제어 됩니다. 데이터베이스는 연결이 열려 있는 기간 범위로 제한 됩니다. 일반적으로 각 테스트 메서드에 대 한 데이터베이스를 정리 합니다.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
