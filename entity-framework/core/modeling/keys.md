---
title: 키-EF Core
description: Entity Framework Core를 사용할 때 엔터티 형식에 대 한 키를 구성 하는 방법
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/keys
ms.openlocfilehash: abd65a5ea079a49fd7a3bbc84a9337f6ee19fab1
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413992"
---
# <a name="keys"></a>Keys

키는 각 엔터티 인스턴스에 대 한 고유 식별자 역할을 합니다. EF의 엔터티에는 대부분 관계형 데이터베이스의 *기본 키* 개념에 매핑되는 단일 키가 있습니다 (키가 없는 엔터티의 경우 [키가 없는 엔터티](xref:core/modeling/keyless-entity-types)참조). 엔터티는 기본 키 이외의 추가 키를 가질 수 있습니다 (자세한 내용은 [대체 키](#alternate-keys) 참조).

규칙에 따라 `Id` 또는 `<type name>Id` 라는 속성은 엔터티의 기본 키로 구성 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3,11)]

> [!NOTE]
> [소유 하는 엔터티 형식은](xref:core/modeling/owned-entities) 다른 규칙을 사용 하 여 키를 정의 합니다.

다음과 같이 단일 속성을 엔터티의 기본 키로 구성할 수 있습니다.

## <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?name=KeySingle&highlight=3)]

## <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?name=KeySingle&highlight=4)]

***

여러 속성을 엔터티의 키로 구성할 수도 있습니다 .이를 복합 키 라고 합니다. 복합 키는 흐름 API를 사용 하 여 구성할 수 있습니다. 규칙은 복합 키를 설정 하지 않으며 데이터 주석을 사용 하 여 구성할 수 없습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?name=KeyComposite&highlight=4)]

## <a name="primary-key-name"></a>기본 키 이름

규칙에 따라 관계형 데이터베이스에서 기본 키는 `PK_<type name>`이름으로 만들어집니다. 다음과 같이 primary key 제약 조건의 이름을 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyName.cs?name=KeyName&highlight=5)]

## <a name="key-types-and-values"></a>키 유형 및 값

EF Core는 `string`, `Guid`, `byte[]` 및 기타를 포함 하 여 기본 키로 기본 형식의 속성을 사용 하는 것을 지원 하지만 모든 데이터베이스가 키로 모든 형식을 지 원하는 것은 아닙니다. 경우에 따라 키 값을 지원 되는 형식으로 자동으로 변환할 수 있습니다. 그렇지 않으면 변환을 [수동으로 지정](xref:core/modeling/value-conversions)해야 합니다.

컨텍스트에 새 엔터티를 추가 하는 경우 키 속성은 항상 기본값이 아닌 값을 가져야 하지만 일부 유형은 [데이터베이스에 의해 생성](xref:core/modeling/generated-properties)됩니다. 이 경우 EF는 추적을 위해 엔터티가 추가 될 때 임시 값을 생성 하려고 시도 합니다. [SaveChanges](/dotnet/api/Microsoft.EntityFrameworkCore.DbContext.SaveChanges) 가 호출 되 면 임시 값이 데이터베이스에 의해 생성 된 값으로 바뀝니다.

> [!Important]
> 키 속성의 값이 데이터베이스에 의해 생성 되 고 엔터티가 추가 될 때 기본값이 아닌 값이 지정 된 경우 EF는 엔터티가 데이터베이스에 이미 있는 것으로 가정 하 고 새 엔터티를 삽입 하는 대신 업데이트 하려고 합니다. 이 값을 해제 하지 않으려면 [생성 된 속성에 대해 명시적 값을 지정 하는 방법](../saving/explicit-values-generated-properties.md)을 참조 하세요.

## <a name="alternate-keys"></a>대체 키

대체 키는 기본 키 외에도 각 엔터티 인스턴스에 대 한 대체 고유 식별자 역할을 합니다. 이를 관계의 대상으로 사용할 수 있습니다. 관계형 데이터베이스를 사용 하는 경우이는 대체 키 열에 대 한 고유 인덱스/제약 조건 개념 및 열을 참조 하는 하나 이상의 foreign key 제약 조건에 매핑됩니다.

> [!TIP]
> 열에 고유성을 적용 하려면 대체 키 대신 고유 인덱스를 정의 합니다 ( [인덱스](indexes.md)참조). EF에서 대체 키는 읽기 전용 이며 외래 키의 대상으로 사용할 수 있기 때문에 고유 인덱스에 대 한 추가 의미 체계를 제공 합니다.

일반적으로 필요에 따라 대체 키가 소개 되며 수동으로 구성할 필요가 없습니다. 규칙에 따라 기본 키가 아닌 속성을 관계의 대상으로 식별 하면 대체 키가 도입 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

또한 단일 속성을 대체 키로 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=4)]

여러 속성을 복합 대체 키로 알려진 대체 키로 구성할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=4)]

마지막으로 규칙에 따라 대체 키에 대해 도입 된 인덱스 및 제약 조건을 `AK_<type name>_<property name>` 이름이 지정 됩니다 (복합 대체 키 `<property name>`는 속성 이름의 밑줄로 구분 된 목록). 대체 키의 인덱스 및 unique 제약 조건의 이름을 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyName.cs?name=AlternateKeyName&highlight=5)]
