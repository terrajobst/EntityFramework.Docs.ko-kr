---
title: 쿼리 유형-EF 코어
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 4e02f106e086d243b23a60c02838f32555be210e
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/19/2018
---
# <a name="query-types"></a>쿼리 유형
> [!NOTE]
> 이 기능은 EF 코어 2.1의 새로운 기능

EF 핵심 모델 엔터티 형식 외에도 포함할 수 있습니다 _쿼리 유형에_, 엔터티 형식에 매핑되어 있지 않은 데이터에 대 한 데이터베이스 쿼리를 수행 하는 데 사용입니다.

쿼리 유형을 유사한 점이 엔터티 형식:

- 또한 추가할 수 있습니다 모델 중 하나에서 `OnModelCreating`, 또는 파생에 "설정" 속성을 통해 _DbContext_합니다.
- 이러한 여러 상속 매핑, 탐색 속성 (제한 사항에 아래 참조)와 같은 및 관계형 저장소를 대상 데이터베이스 개체 및 데이터 주석 또는 fluent API 메서드를 통해 열을 구성 하는 기능에서 동일한 매핑 기능을 지원 합니다.

그러나 엔터티에서 다른 지에 입력 하는 것:

- 키를 정의할 수는 필요 하지 않습니다.
- 변경 내용을 추적 하지 됩니다는 _DbContext_ 및 따라서는 되지 삽입, 업데이트 또는 삭제할 데이터베이스에 있습니다.
- 규칙으로 검색 되지 않습니다.
- 특히 탐색 매핑 기능이-의 일부 지원, 관계의 주 끝으로 작동 하지 않을 수 있습니다.
- 으로 해결 된 _ModelBuilder_ 를 사용 하는 `Query` 메서드 보다는 `Entity` 메서드.
- 에 매핑되는 _DbContext_ 형식의 속성을 통해 `DbQuery<T>` 보다는 `DbSet<T>`
- 사용 하 여 데이터베이스 개체에 매핑되는 `ToView` 메서드를 대신 `ToTable`합니다.
- 에 매핑할 수 있습니다는 _쿼리 정의_ -쿼리 형식에 대 한 데이터 소스 역할을 하는 모델에 선언 된 보조 쿼리는 쿼리를 정의 합니다.

쿼리 형식에 대 한 주요 사용 시나리오는 다음과 같습니다.

- 에 대 한 반환 형식으로 처리 하는 임시 `FromSql()` 쿼리 합니다.
- 데이터베이스 뷰 매핑입니다.
- 기본 키가 정의 되지 않은 테이블을 매핑하고 있습니다.
- 쿼리는 모델에 정의 된 매핑.

> [!TIP]
> 사용 하 여 이루어집니다 쿼리 유형을 데이터베이스 개체에 매핑하는 `ToView` fluent API입니다. 이 메서드에 지정 된 데이터베이스 개체는 EF 코어의 관점에서는 _보기_, 읽기 전용 쿼리 원본으로 처리 되는 의미 및 없습니다 update의 대상이 될, 삽입 또는 삭제 작업입니다. 그러나 데이터베이스 개체는 실제로 데이터베이스 뷰-읽기 전용으로 처리 될 데이터베이스 테이블 또는 것이 아닙니다. 반대로, 엔터티 형식에 대 한 EF 핵심 가정 데이터베이스 개체가 지정 하는 `ToTable` 메서드도 처리 될 수는 _테이블_, 즉 것 쿼리 원본으로 사용할 수 있지만 업데이트의 대상도 삭제 및 삽입 작업입니다. 데이터베이스 뷰 이름을 지정할 수는 실제로 `ToTable` 뷰는 데이터베이스에서 업데이트할 수 있는 되도록 구성 된 상태로 제대로 작동 해야 모든 항목 및 합니다.

## <a name="example"></a>예제

다음 예제에서는 쿼리 유형을 사용 하 여 데이터베이스 뷰를 쿼리 하는 방법을 보여 줍니다.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes)을 볼 수 있습니다.

첫째, 간단한 블로그 및 Post 모델 정의:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

다음으로 연결 된 각 블로그 게시물의 수를 쿼리할 수 있도록 간단한 데이터베이스 뷰를 정의 합니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

다음으로, 데이터베이스 보기에서 결과 보관할 클래스를 정의 합니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

다음으로에서는 구성에서 쿼리 유형을 _OnModelCreating_ 를 사용 하 여는 `modelBuilder.Query<T>` API입니다.
쿼리 형식에 대 한 매핑을 구성 하려면 표준 fluent 구성 Api를 사용 합니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

마지막으로, 표준 방식으로 데이터베이스 뷰를 쿼리할 수 했습니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Note도 컨텍스트 수준 쿼리 속성 (DbQuery)이이 형식에 대 한 쿼리의 루트 역할을 정의 합니다.
