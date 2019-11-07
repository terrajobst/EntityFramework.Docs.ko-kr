---
title: 키 (기본)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655942"
---
# <a name="keys-primary"></a>키(기본)

키는 각 엔터티 인스턴스에 대 한 기본 고유 식별자 역할을 합니다. 관계형 데이터베이스를 사용 하는 경우 *기본 키*의 개념에 매핑됩니다. 기본 키가 아닌 고유 식별자를 구성할 수도 있습니다 (자세한 내용은 [대체 키](alternate-keys.md) 참조).

다음 방법 중 하나를 사용 하 여 기본 키를 설정/만들 수 있습니다.

## <a name="conventions"></a>규칙

규칙에 따라 `Id` 또는 `<type name>Id` 라는 속성이 엔터티의 키로 구성 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 단일 속성을 엔터티의 키로 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 단일 속성을 엔터티의 키로 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

흐름 API를 사용 하 여 여러 속성을 엔터티의 키 (복합 키 라고 함)로 구성할 수도 있습니다. 복합 키는 흐름 API를 사용 하 여 구성할 수 있습니다. 규칙은 복합 키를 설정 하지 않으며 데이터 주석을 사용 하 여 구성할 수 없습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
