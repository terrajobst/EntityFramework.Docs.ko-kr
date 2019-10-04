---
title: InMemory 데이터베이스 공급자 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: b668e286993b9687be21aa815df4e8b8dd308c60
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813528"
---
# <a name="ef-core-in-memory-database-provider"></a>EF 코어 In-Memory 데이터베이스 공급자

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 메모리 내 데이터베이스에서 사용할 수 있습니다. 메모리 내 모드인 SQLite 공급자가 관계형 데이터베이스에 보다 적절한 테스트로 대체될 수 있지만 이 기능이 테스트에 유용할 수 있습니다. 공급자는 [Entity Framework Core 프로젝트](https://github.com/aspnet/EntityFrameworkCore)의 일부로 유지 관리됩니다.

## <a name="install"></a>설치

[Microsoft.EntityFrameworkCore.InMemory NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)를 설치합니다.

# <a name="net-core-clitabdotnet-core-cli"></a>[.NET Core CLI](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

# <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a>시작

다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.

* [InMemory 테스트](../../miscellaneous/testing/in-memory.md)
* [UnicornStore 샘플 애플리케이션 테스트](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>지원되는 데이터베이스 엔진

In Process 메모리 데이터베이스(테스트 전용으로 설계)
