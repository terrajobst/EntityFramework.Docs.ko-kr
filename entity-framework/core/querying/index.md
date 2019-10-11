---
title: 데이터 쿼리 - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 009235c3673a414e06d1a64f9877b60e7cde97b0
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181926"
---
# <a name="querying-data"></a>데이터 쿼리

Entity Framework Core는 LINQ(Language-Integrated Query)를 사용하여 데이터베이스에서 데이터를 쿼리합니다. LINQ를 사용하면 C#(또는 원하는 .NET 언어)을 사용하여 강력한 형식의 쿼리를 작성할 수 있습니다. 파생된 컨텍스트 및 엔터티 클래스를 사용하여 데이터베이스 개체를 참조합니다. EF Core는 LINQ 쿼리 표현을 데이터베이스 공급자에게 전달합니다. 그러면 데이터베이스 공급자는 LINQ 쿼리 표현을 데이터베이스별 쿼리 언어(예: 관계형 데이터베이스의 경우 SQL)로 변환합니다.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.

다음 코드 조각은 Entity Framework Core로 일반 작업을 수행하는 몇 가지 예제를 보여줍니다.

## <a name="loading-all-data"></a>모든 데이터 로드

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>단일 엔터티 로드

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Filtering

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>추가 참고 자료

- [LINQ 쿼리 식](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)에 대해 자세히 알아봅니다.
- EF Core에서 쿼리를 처리하는 방식에 대한 자세한 내용은 [쿼리 작동 방식](xref:core/querying/how-query-works)을 참조하세요.
