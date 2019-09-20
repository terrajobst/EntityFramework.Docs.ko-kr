---
title: 필수/선택적 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149149"
---
# <a name="required-and-optional-properties"></a>필수 및 선택적 속성

속성에 포함 `null`하기에 유효한 경우 속성은 선택 사항으로 간주 됩니다. 이 `null` 속성에 유효한 값이 아닌 경우에는 필수 속성으로 간주 됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 .net 형식이 null을 포함할 수 있는 속성은 선택적 (`string`, `int?`, `byte[]`등)으로 구성 됩니다. CLR 형식이 null을 포함할 수 없는 속성은 필수 (`int`, `decimal`, `bool`등)로 구성 됩니다.

> [!NOTE]  
> .NET 형식을 null을 포함할 수 없는 속성은 선택 사항으로 구성할 수 없습니다. 속성은 항상 Entity Framework에 필요한 것으로 간주 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 속성이 필요 함을 나타낼 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Fluent API

흐름 API를 사용 하 여 속성이 필요 함을 나타낼 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

