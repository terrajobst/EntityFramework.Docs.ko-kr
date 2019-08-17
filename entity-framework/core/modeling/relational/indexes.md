---
title: 인덱스-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: dfada7446f812f3c277572cc1338441272e8f448
ms.sourcegitcommit: 7b7f774a5966b20d2aed5435a672a1edbe73b6fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/17/2019
ms.locfileid: "69565361"
---
# <a name="indexes"></a>인덱스

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

관계형 데이터베이스의 인덱스는 Entity Framework의 핵심 인덱스와 동일한 개념에 매핑됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 인덱스의 이름은 `IX_<type name>_<property name>`입니다. 복합 인덱스 `<property name>` 의 경우 속성 이름의 밑줄로 구분 된 목록이 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 인덱스를 구성할 수 없습니다.

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 인덱스 이름을 구성할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

필터를 지정할 수도 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

SQL Server 공급자 EF를 사용 하는 경우 고유 인덱스의 일부인 모든 null 허용 열에 대해 ' IS NOT NULL ' 필터를 추가 합니다. 이 규칙을 재정의 하려면 값을 `null` 제공할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>SQL Server 인덱스에 열 포함

쿼리의 모든 열이 키 또는 키가 아닌 열로 인덱스에 포함 되는 경우에는 [포괄 열이 있는 인덱스](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) 를 구성 하 여 쿼리 성능을 크게 향상 시킬 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/ForSqlServerHasIndex.cs?name=Model)]
