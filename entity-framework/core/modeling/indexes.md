---
title: 인덱스-EF Core
author: roji
ms.date: 12/16/2019
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: 810fccc0c6b035f515107601b245811f7b4118a6
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502138"
---
# <a name="indexes"></a>인덱스

인덱스는 많은 데이터 저장소에서 일반적인 개념입니다. 데이터 저장소에서의 구현은 다를 수 있지만 열 (또는 열 집합)을 기반으로 조회를 보다 효율적으로 만드는 데 사용 됩니다.

데이터 주석을 사용 하 여 인덱스를 만들 수 없습니다. 흐름 API를 사용 하 여 다음과 같이 단일 열에 대 한 인덱스를 지정할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=4)]

또한 두 개 이상의 열에 대해 인덱스를 지정할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=4)]

> [!NOTE]
> 규칙에 따라 외래 키로 사용 되는 각 속성 (또는 속성 집합)에 인덱스가 생성 됩니다.
>
> EF Core는 속성의 고유 집합 마다 하나의 인덱스만 지원 합니다. 흐름 API를 사용 하 여 인덱스를 이미 정의 하는 속성 집합에서 규칙 또는 이전 구성으로 인덱스를 구성 하는 경우 해당 인덱스의 정의가 변경 됩니다. 규칙으로 만들어진 인덱스를 추가로 구성 하려는 경우에 유용 합니다.

## <a name="index-uniqueness"></a>인덱스 고유성

기본적으로 인덱스는 고유 하지 않습니다. 여러 행이 인덱스의 열 집합에 대해 동일한 값을 가질 수 있습니다. 다음과 같이 인덱스를 고유 하 게 만들 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=IndexUnique&highlight=5)]

인덱스의 열 집합에 대해 동일한 값을 사용 하 여 둘 이상의 엔터티를 삽입 하려고 하면 예외가 throw 됩니다.

## <a name="index-name"></a>인덱스 이름

규칙에 따라 관계형 데이터베이스에서 만든 인덱스는 `IX_<type name>_<property name>`이름이 지정 됩니다. 복합 인덱스의 경우 `<property name>`은 속성 이름의 밑줄로 구분 된 목록입니다.

흐름 API를 사용 하 여 데이터베이스에서 만든 인덱스의 이름을 설정할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexName.cs?name=IndexName&highlight=5)]

## <a name="index-filter"></a>인덱스 필터

일부 관계형 데이터베이스에서는 필터링 된 인덱스 또는 부분 인덱스를 지정할 수 있습니다. 이렇게 하면 열 값의 하위 집합만 인덱싱할 수 있으므로 인덱스의 크기를 줄이고 성능과 디스크 공간 사용을 모두 향상 시킬 수 있습니다. 필터링 된 인덱스 SQL Server에 대 한 자세한 내용은 [설명서를 참조](https://docs.microsoft.com/sql/relational-databases/indexes/create-filtered-indexes)하십시오.

흐름 API를 사용 하 여 SQL 식으로 제공 되는 인덱스에 대 한 필터를 지정할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexFilter.cs?name=IndexFilter&highlight=5)]

SQL Server 공급자를 사용 하는 경우 EF는 고유 인덱스의 일부인 모든 nullable 열에 대해 `'IS NOT NULL'` 필터를 추가 합니다. 이 규칙을 재정의 하려면 `null` 값을 제공 하면 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexNoFilter.cs?name=IndexNoFilter&highlight=6)]

## <a name="included-columns"></a>포괄 열

일부 관계형 데이터베이스를 사용 하면 인덱스에 포함 되지만 "키"의 일부가 아닌 열 집합을 구성할 수 있습니다. 이렇게 하면 테이블 자체에 액세스할 필요가 없으므로 쿼리의 모든 열이 키 또는 키가 아닌 열로 인덱스에 포함 될 때 쿼리 성능이 크게 향상 될 수 있습니다. 포괄 열 SQL Server에 대 한 자세한 내용은 [설명서를 참조](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns)하십시오.

다음 예에서는 `Url` 열이 인덱스 키의 일부 이므로 해당 열에 대 한 모든 쿼리 필터링에서 인덱스를 사용할 수 있습니다. 또한 `Title` 및 `PublishedOn` 열에만 액세스 하는 쿼리는 테이블에 액세스할 필요가 없으며 더 효율적으로 실행 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexInclude.cs?name=IndexInclude&highlight=5-9)]
