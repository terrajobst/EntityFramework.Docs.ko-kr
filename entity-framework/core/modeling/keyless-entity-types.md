---
title: 키가 없는 엔터티 형식-EF Core
description: Entity Framework Core를 사용 하 여 키가 없는 엔터티 형식을 구성 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 520c9ed93240c05deee36fa527a3757490fd7082
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414646"
---
# <a name="keyless-entity-types"></a>키 없는 엔터티 형식

> [!NOTE]
> 이 기능은 쿼리 유형 이름 아래 EF Core 2.1에 추가 되었습니다. EF Core 3.0에서 개념은 키가 없는 엔터티 형식으로 이름이 변경 되었습니다.

일반 엔터티 형식 외에도 EF Core 모델에는 키 값이 포함 되지 않은 데이터에 대 한 데이터베이스 쿼리를 수행 하는 데 사용할 수 있는 _키가 없는 엔터티 형식이_포함 될 수 있습니다.

## <a name="keyless-entity-types-characteristics"></a>키가 없는 엔터티 형식 특징

키가 없는 엔터티 형식은 상속 매핑 및 탐색 속성과 같은 일반적인 엔터티 형식과 동일한 매핑 기능을 대부분 지원 합니다. 관계형 저장소에서 대상 데이터베이스 개체와 데이터 주석 또는 fluent API 메서드를 통해 열을 구성할 수 있습니다.

그러나 다음과 같은 점에서 일반 엔터티 형식과는 다릅니다.

- 키를 정의할 수 없습니다.
- 는 _DbContext_ 의 변경 내용에 대해서는 추적 되지 않으므로 데이터베이스에 삽입, 업데이트 또는 삭제 되지 않습니다.
- 규칙에 따라 검색 되지 됩니다.
- 는 다음과 같이 탐색 매핑 기능의 하위 집합만 지원 합니다.
  - 관계의 주 끝으로 작동 하지 않을 수 있습니다.
  - 소유 된 엔터티를 탐색 하지 못할 수 있습니다.
  - 일반 엔터티를 가리키는 참조 탐색 속성만 포함할 수 있습니다.
  - 엔터티는 키가 없는 엔터티 형식에 대 한 탐색 속성을 포함할 수 없습니다.
- `.HasNoKey()` 메서드 호출로 구성 해야 합니다.
- _정의 쿼리에_매핑될 수 있습니다. 정의 쿼리는 키가 없는 엔터티 형식에 대 한 데이터 소스 역할을 하는 모델에 선언 된 쿼리입니다.

## <a name="usage-scenarios"></a>사용 시나리오

키가 없는 엔터티 형식에 대 한 몇 가지 주요 사용 시나리오는 다음과 같습니다.

- [원시 SQL 쿼리의](xref:core/querying/raw-sql)반환 형식으로 제공 됩니다.
- 기본 키가 포함 되지 않은 데이터베이스 뷰에 매핑됩니다.
- 기본 키가 정의 되지 않은 테이블에 매핑합니다.
- 모델에 정의 된 쿼리에 매핑.

## <a name="mapping-to-database-objects"></a>데이터베이스 개체에 대 한 매핑

`ToTable` 또는 `ToView` 흐름 API를 사용 하 여 키가 없는 엔터티 형식을 데이터베이스 개체에 매핑할 수 있습니다. EF Core 관점에서이 메서드에 지정 된 데이터베이스 개체는 _뷰가_읽기 전용 쿼리 원본으로 처리 되 고 update, insert 또는 delete 작업의 대상일 수 없음을 의미 합니다. 그러나 데이터베이스 개체가 실제로 데이터베이스 뷰가 되어야 한다는 의미는 아닙니다. 또는 읽기 전용으로 처리 되는 데이터베이스 테이블 일 수도 있습니다. 반대로, 일반 엔터티 형식의 경우 EF Core `ToTable` 메서드에 지정 된 데이터베이스 개체를 _테이블로_처리할 수 있다고 가정 합니다. 즉, 쿼리 원본으로 사용할 수 있지만 업데이트, 삭제 및 삽입 작업의 대상으로 사용할 수 있습니다. 실제로는 `ToTable`에서 데이터베이스 뷰의 이름을 지정할 수 있으며, 데이터베이스에서 뷰를 업데이트할 수 있도록 구성 하는 한 모든 것이 제대로 작동 해야 합니다.

> [!NOTE]
> `ToView`은 개체가 데이터베이스에 이미 존재 하 고 마이그레이션에 의해 생성 되지 않는다고 가정 합니다.

## <a name="example"></a>예제

다음 예제에서는 키가 없는 엔터티 형식을 사용 하 여 데이터베이스 뷰를 쿼리 하는 방법을 보여 줍니다.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes)을 볼 수 있습니다.

먼저 간단한 블로그 및 게시물 모델을 정의합니다.

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

다음으로 연결 된 각 블로그 게시물의 수를 쿼리할 수 있는 간단한 데이터베이스 뷰를 정의 합니다.

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

다음으로, 데이터베이스 보기에서 결과 포함 된 클래스를 정의 합니다.

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

다음으로 `HasNoKey` API를 사용 하 여 _Onmodelcreating_ 에서 키가 없는 엔터티 형식을 구성 합니다.
흐름 구성 API를 사용 하 여 키가 없는 엔터티 형식에 대 한 매핑을 구성 합니다.

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

다음으로 `DbSet<T>`를 포함 하도록 `DbContext`를 구성 합니다.

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

마지막으로 표준 방식으로 데이터베이스 뷰를 쿼리할 수 있습니다.

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> 참고로,이 형식에 대 한 쿼리의 루트 역할을 하는 데 사용할 컨텍스트 수준 쿼리 속성 (DbSet)도 정의 했습니다.
