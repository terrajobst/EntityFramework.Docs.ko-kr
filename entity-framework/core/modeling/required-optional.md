---
title: 필수/선택적 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929812"
---
# <a name="required-and-optional-properties"></a>필수 및 선택적 속성

속성에 포함 하려면 유효한 경우 선택적으로 간주 됩니다 `null`합니다. 경우 `null` 필수 속성으로 간주 됩니다 속성에 할당할 올바른 값이 아닙니다.

## <a name="conventions"></a>규칙

규칙에 따라 CLR 형식이 null을 포함할 수 있습니다 속성 구성지 것입니다 옵션으로 (`string`, `int?`, `byte[]`등.). CLR 형식이 null을 포함할 수 없습니다. 속성을 구성할 수는 필요에 따라 (`int`, `decimal`, `bool`등.).

> [!NOTE]  
> CLR 형식이 null을 포함할 수 없습니다. 속성은 선택적으로 구성할 수 없습니다. 항상 Entity Framework에 필요한 속성을 간주 됩니다.

## <a name="data-annotations"></a>데이터 주석

필수 속성 임을 데이터 주석을 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Fluent API

필수 속성 임을 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

