---
title: Azure Cosmos DB 공급자-제한 EF Core
description: Entity Framework Core Azure Cosmos DB 공급자의 제한 사항
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 2631526b152d6ddcacf25173c8d51e4e3cb24500
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414562"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>EF Core Azure Cosmos DB 공급자 제한 사항

Cosmos 공급자에는 몇 가지 제한 사항이 있습니다. 이러한 제한 사항 중 상당수는 기본 Cosmos 데이터베이스 엔진에 대 한 제한의 결과 이며 EF에 국한 되지 않습니다. 그러나 대부분은 [아직 구현 되지 않았습니다](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>임시 제한 사항

- 컨테이너에 매핑되는 상속이 없는 엔터티 형식이 하나만 있는 경우에도 판별자 속성이 계속 사용 됩니다.
- 파티션 키가 있는 엔터티 형식은 일부 시나리오에서 제대로 작동 하지 않습니다.
- `Include` 호출은 지원 되지 않습니다.
- `Join` 호출은 지원 되지 않습니다.

## <a name="azure-cosmos-db-sdk-limitations"></a>Azure Cosmos DB SDK 제한 사항

- 비동기 메서드만 제공 됩니다.

> [!WARNING]
> 에서 사용 하 EF Core 하위 수준 메서드의 동기화 버전은 없으므로 반환 된 `Task`에 대 한 `.Wait()`를 호출 하 여 해당 기능을 현재 구현 합니다. 즉, `SaveChanges`와 같은 메서드를 사용 하거나 비동기 항목 대신 `ToList`를 사용 하면 응용 프로그램에서 교착 상태가 발생할 수 있습니다.

## <a name="azure-cosmos-db-limitations"></a>Azure Cosmos DB 제한 사항

[지원 되는 Azure Cosmos DB 기능](/azure/cosmos-db/modeling-data)에 대 한 전체 개요를 볼 수 있으며, 관계형 데이터베이스에 비해 가장 주목할 만한 차이점은 다음과 같습니다.

- 클라이언트에서 시작한 트랜잭션은 지원 되지 않습니다.
- 일부 파티션 간 쿼리는 관련 연산자에 따라 지원 되지 않거나 훨씬 느립니다.
