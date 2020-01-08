---
title: 엔터티 형식-EF Core
description: Entity Framework Core를 사용 하 여 엔터티 형식을 구성 하 고 매핑하는 방법
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502416"
---
# <a name="entity-types"></a>엔터티 형식

형식에 대 한 DbSet을 컨텍스트에 포함 하는 것은 EF Core의 모델에 포함 됨을 의미 합니다. 일반적으로 이러한 형식을 *엔터티로*참조 합니다. EF Core는 데이터베이스에서 엔터티 인스턴스를 읽고 쓸 수 있으며 관계형 데이터베이스를 사용 하는 경우 마이그레이션을 통해 엔터티에 대 한 테이블을 만들 수 EF Core.

## <a name="including-types-in-the-model"></a>모델에 형식 포함

규칙에 따라 컨텍스트의 DbSet 속성에 노출 되는 형식은 모델에 엔터티로 포함 됩니다. 다른 검색 된 엔터티 형식의 탐색 속성을 재귀적으로 탐색 하 여 발견 되는 모든 형식과 마찬가지로 `OnModelCreating` 메서드에 지정 된 엔터티 형식도 포함 됩니다.

아래 코드 샘플에는 모든 형식이 포함 되어 있습니다.

* `Blog`는 컨텍스트의 DbSet 속성에 노출 되기 때문에 포함 됩니다.
* `Blog.Posts` 탐색 속성을 통해 검색 되기 때문에 `Post` 포함 됩니다.
* `AuditEntry` `OnModelCreating`에 지정 되어 있기 때문입니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a>모델에서 형식 제외

모델에 형식을 포함 하지 않으려면 제외할 수 있습니다.

### <a name="data-annotationstabdata-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a>표 이름

규칙에 따라 엔터티를 노출 하는 DbSet 속성과 이름이 같은 데이터베이스 테이블에 매핑하기 위해 각 엔터티 형식이 설정 됩니다. 지정 된 엔터티에 대해 DbSet가 없으면 클래스 이름이 사용 됩니다.

테이블 이름을 수동으로 구성할 수 있습니다.

### <a name="data-annotationstabdata-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a>테이블 스키마

관계형 데이터베이스를 사용 하는 경우 테이블은 데이터베이스의 기본 스키마에서 생성 된 규칙을 기준으로 합니다. 예를 들어 Microsoft SQL Server은 `dbo` 스키마 (SQLite는 스키마를 지원 하지 않음)를 사용 합니다.

다음과 같이 특정 스키마에서 테이블을 만들도록 구성할 수 있습니다.

### <a name="data-annotationstabdata-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

각 테이블에 대 한 스키마를 지정 하는 대신 흐름 API를 사용 하 여 모델 수준에서 기본 스키마를 정의할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

기본 스키마를 설정 하면 시퀀스와 같은 다른 데이터베이스 개체에도 영향을 줍니다.
