---
title: 속성-EF Core 포함 및 제외
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929826"
---
# <a name="including--excluding-properties"></a>속성 포함 및 제외

모델의 속성을 포함 하 여 EF에 해당 속성에 대 한 메타 데이터를 의미 하 고 읽기 및 쓰기를 데이터베이스 값을 하려고 합니다.

## <a name="conventions"></a>규칙

규칙에 따라 공용 속성 getter 및 setter를 사용 하 여 모델에 포함 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석 모델에서 속성을 제외를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Fluent API

모델에서 속성을 제외 하려면 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
