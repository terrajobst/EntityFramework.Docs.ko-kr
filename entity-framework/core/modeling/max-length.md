---
title: 최대 길이-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929851"
---
# <a name="maximum-length"></a>최대 길이

지정된 된 속성에 사용할 적절 한 데이터 형식에 대 한 데이터 저장소에 힌트를 제공 최대 길이 구성 합니다. 최대 길이에 적용 됩니다 배열 데이터 형식이 같은 `string` 고 `byte[]`입니다.

> [!NOTE]  
> Entity Framework 데이터 공급자에 게 전달 하기 전에 최대 길이의 유효성 검사를 수행 하지 않습니다. 해당 하는 경우의 유효성을 검사 하려면 공급자 또는 데이터 저장소는 것입니다. 예를 들어, 기본 열의 데이터 형식에 예외가 발생 하면 최대 길이 초과 하는 SQL Server를 대상으로 하는 경우 과도 한 데이터를 저장할 수을 허용 하지 않습니다.

## <a name="conventions"></a>규칙

규칙에 따라 해당 에서만 데이터베이스 공급자 속성에 대 한 적절 한 데이터 형식을 선택 합니다. 길이가 속성을 데이터베이스 공급자는 일반적으로 데이터의 가장 긴 길이 허용 하는 데이터 형식을 선택 합니다. Microsoft SQL Server가 사용 하는 예를 들어 `nvarchar(max)` 에 대 한 `string` 속성 (또는 `nvarchar(450)` 열을 키로 사용 되는 경우).

## <a name="data-annotations"></a>데이터 주석

속성에 대 한 최대 길이 구성 하려면 데이터 주석을 사용할 수 있습니다. 이 예제에서는이 인해 SQL Server를 대상으로 하는 `nvarchar(500)` 사용 되는 데이터 형식입니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a>Fluent API

속성에 대 한 최대 길이 구성 하는 Fluent API를 사용할 수 있습니다. 이 예제에서는이 인해 SQL Server를 대상으로 하는 `nvarchar(500)` 사용 되는 데이터 형식입니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
