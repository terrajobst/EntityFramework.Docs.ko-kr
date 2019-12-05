---
title: 인덱스 (관계형 데이터베이스)-EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824567"
---
# <a name="indexes-relational-database"></a>인덱스 (관계형 데이터베이스)

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

관계형 데이터베이스의 인덱스는 Entity Framework의 핵심 인덱스와 동일한 개념에 매핑됩니다.

## <a name="conventions"></a>표기 규칙

규칙에 따라 인덱스는 `IX_<type name>_<property name>`이름이 지정 됩니다. 복합 인덱스의 경우에는 `<property name>` 속성 이름의 밑줄로 구분 된 목록이 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 인덱스를 구성할 수 없습니다.

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 인덱스 이름을 구성할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

필터를 지정할 수도 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

SQL Server 공급자를 사용 하는 경우 EF는 고유 인덱스의 일부인 모든 nullable 열에 대해 `'IS NOT NULL'` 필터를 추가 합니다. 이 규칙을 재정의 하려면 `null` 값을 제공 하면 됩니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a>SQL Server 인덱스에 열 포함

쿼리의 모든 열이 키 또는 키가 아닌 열로 인덱스에 포함 되는 경우에는 [포괄 열이 있는 인덱스](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) 를 구성 하 여 쿼리 성능을 크게 향상 시킬 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
