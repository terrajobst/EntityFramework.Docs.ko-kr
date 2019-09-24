---
title: 속성을 제외 하 고 & 포함-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197418"
---
# <a name="including--excluding-properties"></a>속성 포함 및 제외

모델에 속성을 포함 하면 EF는 해당 속성에 대 한 메타 데이터를 포함 하 고 데이터베이스의 값을 읽고 쓰는 것을 시도 합니다.

## <a name="conventions"></a>규칙

규칙에 따라 getter 및 setter가 있는 공용 속성은 모델에 포함 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 모델에서 속성을 제외할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 모델에서 속성을 제외할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
