---
title: EF Core 3.0의 새로운 기능 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: b6774f615b04bf9579aac5dea217e7321631da0c
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829189"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a>EF Core 3.0에 포함된 새로운 기능(현재 미리 보기 상태)

> [!IMPORTANT]
> 이후 릴리스의 기능 집합 및 일정은 항상 변경될 수 있으며 이 페이지를 최신 상태로 유지하려고 노력함에도 불구하고 최신 계획을 항상 반영할 수 없을 수도 있습니다.

다음 목록에는 EF Core 3.0용으로 계획된 주요한 새 기능이 포함되어 있습니다.
이러한 기능의 대부분은 현재 미리 보기에 포함되어 있지 않지만, RTM을 진행함에 따라 제공될 예정입니다.

그 이유는 릴리스 초기에 계획된 [호환성이 손상되는 변경](xref:core/what-is-new/ef-core-3.0/breaking-changes)을 구현하는 데 집중하고 있기 때문입니다.
이러한 많은 호환성이 손상되는 변경은 EF Core의 개선 사항입니다.
많은 기타 항목에서 추가 개선 사항을 차단 해제해야 합니다. 

진행 중인 버그 수정 및 개선 사항의 전체 목록은 [문제 추적기의 이 쿼리](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc)를 참조하세요.

## <a name="linq-improvements"></a>LINQ 기능 향상 

[추적 문제 #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

이 기능에 대한 작업이 시작되었지만 현재 미리 보기에는 포함되어 있지 않습니다.

LINQ를 사용하면 선택한 언어로 데이터베이스 쿼리를 작성할 수 있어서 다양한 종류의 정보를 활용하여 IntelliSense 및 컴파일 시간 형식 검사를 수행할 수 있습니다.
그러나 LINQ를 사용하면 복잡한 쿼리를 무제한으로 작성할 수도 있습니다. 이는 LINQ 공급자에게 항상 큰 문제였습니다.
EF Core의 처음 몇 버전에서는 쿼리 중 SQL로 변환할 수 있는 부분을 생각한 다음, 나머지 쿼리가 클라이언트의 메모리에서 실행되도록 허용하여 이 문제를 부분적으로 해결했습니다.
이러한 클라이언트 쪽 실행은 일부 상황에서는 바람직하지만, 다른 많은 경우에는 애플리케이션이 프로덕션에 배포될 때까지 식별되지 않을 수 있는 비효율적인 쿼리가 발생할 수 있습니다.
EF Core 3.0에서는 LINQ 구현 방식과 테스트 방법을 크게 변화시킬 계획입니다.
목표는 패치 릴리스에서 쿼리 중단을 방지하는 등 더욱 강력하게 만들고, 더 많은 식을 SQL로 정확하게 변환하고, 더 많은 경우에 효율적인 쿼리를 생성하며, 비효율적인 쿼리가 검색되지 않는 것을 방지하는 것입니다.

## <a name="cosmos-db-support"></a>Cosmos DB 지원 

[추적 문제 #8443](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

이 기능은 현재 미리 보기에 포함되어 있지만 아직 완전하지는 않습니다. 

Microsoft는 개발자가 EF 프로그래밍 모델에 친숙해져서 애플리케이션 데이터베이스로 Azure Cosmos DB를 쉽게 대상으로 지정할 수 있도록 하기 위해 EF Core의 Cosmos DB 공급자에 공을 들이고 있습니다.
목표는 글로벌 배포, "always on" 가용성, 탄력적인 확장성, 짧은 대기 시간, .NET 개발자에 훨씬 더 많은 액세스 가능성과 같은 몇몇 Cosmos DB의 장점을 활용하는 것입니다.
공급자는 Cosmos DB의 SQL API에 대해 자동 변경 내용 추적, LINQ, 값 변환 같은 대부분의 EF Core 기능을 사용할 수 있습니다.
Microsoft는 EF Core 2.2 전에 이러한 노력을 시작했으며, [공급자의 몇몇 미리 보기 버전을 제공했습니다](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).
새 계획은 EF Core 3.0과 함께 공급자를 계속 개발하는 것입니다. 

## <a name="c-80-support"></a>C# 8.0 지원

[추적 문제 #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[추적 문제 #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)

이 기능에 대한 작업이 시작되었지만 현재 미리 보기에는 포함되어 있지 않습니다.

고객이 EF Core를 사용하면서 비동기 스트림(`await foreach` 포함) 및 nullable 참조 형식과 같은 [C# 8.0의 새 기능](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/)의 일부를 활용하기를 바랍니다.

## <a name="reverse-engineering-of-database-views"></a>데이터베이스 뷰의 리버스 엔지니어링

[추적 문제 #1679](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

이 기능은 현재 미리 보기에 포함되어 있지 않습니다.

EF Core 2.1에서 도입되고 EF Core 3.0에서 키가 없는 엔터티 형식으로 간주되는 [쿼리 유형](xref:core/modeling/query-types)은 데이터베이스에서 읽을 수 있지만 업데이트할 수 없는 데이터를 나타냅니다.
이 특성은 대부분의 시나리오에서 데이터베이스 뷰에 매우 적합하므로 리버스 엔지리어닝 데이터베이스 뷰에서 키 없이 엔터티 형식을 자동화할 계획입니다.

## <a name="property-bag-entities"></a>속성 모음 엔터티 

[추적 문제 #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) 및 [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)

이 기능에 대한 작업이 시작되었지만 현재 미리 보기에는 포함되어 있지 않습니다. 

이 기능은 일반 속성 대신 인덱싱된 속성에 데이터를 저장하는 엔터티를 사용하고 동일한 .NET 클래스의 인스턴스(잠재적으로 `Dictionary<string, object>`만큼 단순한 인스턴스)를 사용하여 동일한 EF Core 모델에서 여러 엔터티 형식을 나타낼 수 있도록 하는 기능입니다.
이 기능은 EF Core에 대해 가장 요청이 많았던 기능 향상 중 하나인 조인 엔터티([issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368))가 없는 다 대 다 관계를 지원하는 발판입니다.

## <a name="ef-63-on-net-core"></a>.NET Core의 EF 6.3 

[추적 문제 EF6#271](https://github.com/aspnet/EntityFramework6/issues/271)

이 기능에 대한 작업이 시작되었지만 현재 미리 보기에는 포함되어 있지 않습니다. 

많은 기존 애플리케이션이 이전 버전의 EF를 사용하고 단지 .NET Core를 활용하기 위해 이전 버전의 EF를 EF Core로 포팅하는 일은 상당한 노력이 필요할 수 있다는 점을 이해합니다.
그러한 이유로, Microsoft는 다음 버전의 EF 6가 .NET Core 3.0에서 실행되도록 조정할 예정입니다.
이는 최소한의 변경으로 기존 애플리케이션의 포팅을 간단히 수행하기 위해서입니다.
몇 가지 제한 사항이 있습니다. 예:
- 새로운 공급자는 .NET Core에 포함된 SQL Server 지원 외에 다른 데이터베이스로 작업해야 합니다.
- SQL Server를 사용한 공간 지원은 사용할 수 없습니다.

또한 현재 시점에서 EF 6을 위해 계획된 새로운 기능은 없습니다.
