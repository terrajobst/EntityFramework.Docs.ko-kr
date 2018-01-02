---
title: "InMemory 데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a>EF 코어 In-Memory 데이터베이스 공급자

이 데이터베이스 공급자를 설치하면 Entity Framework Core를 메모리 내 데이터베이스에서 사용할 수 있습니다. Entity Framework Core를 사용하는 코드를 테스트할 때 유용합니다. 이 공급자는 [EntityFramework GitHub 프로젝트](https://github.com/aspnet/EntityFramework)의 일부로 유지 관리됩니다.

## <a name="install"></a>설치

[Microsoft.EntityFrameworkCore.InMemory NuGet 패키지](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)를 설치합니다.

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>시작

다음 리소스는 이 공급자를 시작하는 데 도움이 될 수 있습니다.
* [InMemory 테스트](../../miscellaneous/testing/in-memory.md)

* [UnicornStore 샘플 응용 프로그램 테스트](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs) 

## <a name="supported-database-engines"></a>지원되는 데이터베이스 엔진

* 기본 제공 메모리 내 데이터베이스(테스트 전용으로 설계)

## <a name="supported-platforms"></a>지원되는 플랫폼

* .NET Framework(4.5.1부터)

* .NET Core

* Mono(4.2.0부터)

* 유니버설 Windows 플랫폼
