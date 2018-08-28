---
title: 쿼리 유형-EF Core
author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
uid: core/modeling/query-types
ms.openlocfilehash: bacb121ca00a9b0aa00bfe201de4f95113472d70
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996701"
---
# <a name="query-types"></a>쿼리 형식
> [!NOTE]
> 이 기능은 EF Core 2.1의 새로운 기능

엔터티 형식 외에도 EF Core 모델을 포함할 수 있습니다 _쿼리 유형_는 엔터티 형식에 매핑되어 있지 않은 데이터에 대 한 데이터베이스 쿼리를 처리할 수 있습니다.

쿼리 형식에 많은 유사점이 엔터티 유형

- 또한 추가할 수 있습니다 모델 중 하나에 `OnModelCreating`, 또는 파생에 "set" 속성을 통해 _DbContext_합니다.
- 다양 한 동일한 매핑 기능을 상속 매핑, 탐색 속성 (아래의 제한 사항 참조)와 같은 한, 관계형 저장소를 대상 데이터베이스 개체 및 데이터 주석 또는 fluent API 메서드를 통해 열을 구성 하는 기능을 지원 합니다.

그러나 엔터티에서 다른 형식으로:

- 키를 정의할 필요가 없습니다.
- 변경 내용을 추적 되지 않습니다는 _DbContext_ 및 따라서 되지 삽입, 업데이트 되거나 삭제 된 데이터베이스에 있습니다.
- 규칙에 따라 검색 되지 됩니다.
- 특히 탐색 매핑-기능의 하위 집합이 지원:
  - 관계의 주 끝으로 작동 하지 않을 수 있습니다.
  - 엔터티를 가리키는 참조 탐색 속성에만 사용할 수 있습니다.
  - 엔터티 쿼리 형식에 탐색 속성을 포함할 수 없습니다.
- 해결 합니다 _ModelBuilder_ 사용 하 여 합니다 `Query` 메서드 대신 `Entity` 메서드.
- 에 매핑되는 _DbContext_ 형식의 속성을 통해 `DbQuery<T>` 대신 `DbSet<T>`
- 사용 하 여 데이터베이스 개체에 매핑되는 `ToView` 메서드를 대신 `ToTable`합니다.
- 매핑될 수를 _쿼리 정의_ -쿼리 형식에 대 한 데이터 원본 사용 되는 모델에서 선언 하는 보조 쿼리는 쿼리를 정의 합니다.

쿼리 형식에 대 한 주요 사용 시나리오는 다음과 같습니다.

- 에 대 한 반환 형식으로 제공 하는 임시 `FromSql()` 쿼리 합니다.
- 데이터베이스 뷰 매핑.
- 기본 키가 정의 되지 않은 테이블에 매핑합니다.
- 모델에 정의 된 쿼리에 매핑.

> [!TIP]
> 사용 하 여 이루어집니다 쿼리 형식 데이터베이스 개체에 매핑하는 `ToView` fluent API. 이 메서드에 지정 된 데이터베이스 개체는 EF Core의 관점에서을 _보기_, 읽기 전용 쿼리 원본으로 처리 됩니다 의미 및 없습니다 업데이트의 대상이 될, 삽입 또는 삭제 작업입니다. 그러나 데이터베이스 개체가 실제로 데이터베이스 뷰를 사용할 필요가-읽기 전용으로 처리 될 데이터베이스 테이블 또는 수는이 아닙니다. 반대로 엔터티 형식에 대 한 EF Core는 데이터베이스 개체에서 지정한으로 가정 합니다 `ToTable` 방법으로 처리 될 수는 _테이블_, 즉 쿼리 원본으로 사용할 수 있지만 업데이트도 대상인 삭제 및 삽입 작업입니다. 데이터베이스 뷰 이름을 지정할 수는 사실 `ToTable` 뷰는 구성 데이터베이스에서 업데이트할 수 있는으로 정상적으로 작동 해야 모든 합니다.

## <a name="example"></a>예제

다음 예제에서는 쿼리 유형을 사용 하 여 데이터베이스 뷰를 쿼리 하는 방법을 보여 줍니다.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFrameworkCore/tree/master/samples/QueryTypes)을 볼 수 있습니다.

먼저 간단한 블로그 및 게시물 모델을 정의합니다.

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

다음으로 연결 된 각 블로그 게시물의 수를 쿼리할 수 있는 간단한 데이터베이스 뷰를 정의 합니다.

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

다음으로, 데이터베이스 보기에서 결과 포함 된 클래스를 정의 합니다.

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

다음으로 쿼리 형식을 구성 _OnModelCreating_ 사용 하는 `modelBuilder.Query<T>` API.
쿼리 형식에 대 한 매핑을 구성 하려면 표준 fluent 구성 Api를 사용 합니다.

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

마지막으로 표준 방식으로 데이터베이스 뷰를 쿼리할 수 있습니다.

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> 또한 상황에 맞는 쿼리 속성 (DbQuery)이이 형식에 대 한 쿼리의 루트 역할을 정의한 note 합니다.
