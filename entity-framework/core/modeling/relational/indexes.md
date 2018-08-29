---
title: 인덱스-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993219"
---
# <a name="indexes"></a>인덱스

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

관계형 데이터베이스에서 인덱스의 Entity Framework core에서 인덱스와 동일한 개념에 매핑됩니다.

## <a name="conventions"></a>규칙

인덱스 이름은 규칙에 따라 `IX_<type name>_<property name>`합니다. 복합 인덱스에 대 한 `<property name>` 속성 이름에 밑줄 구분 된 목록이 있습니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 인덱스를 구성할 수 있습니다.

## <a name="fluent-api"></a>Fluent API

인덱스의 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

또한 필터를 지정할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

추가 SQL Server 공급자 EF를 사용 하 여 고유 인덱스의 일부인 모든 null 허용 열에 대 한 필터는 ' IS NOT NULL'. 제공할 수 있습니다이 규칙을 재정의 하는 `null` 값입니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
