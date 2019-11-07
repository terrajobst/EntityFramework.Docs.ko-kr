---
title: 인덱스-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655693"
---
# <a name="indexes"></a>인덱스

인덱스는 많은 데이터 저장소에서 일반적인 개념입니다. 데이터 저장소에서의 구현은 다를 수 있지만 열 (또는 열 집합)을 기반으로 조회를 보다 효율적으로 만드는 데 사용 됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 외래 키로 사용 되는 각 속성 (또는 속성 집합)에 인덱스가 생성 됩니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 인덱스를 만들 수 없습니다.

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 단일 속성에 대 한 인덱스를 지정할 수 있습니다. 기본적으로 인덱스는 고유 하지 않습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

인덱스가 고유 하도록 지정할 수도 있습니다. 즉, 두 엔터티가 지정 된 속성에 대해 동일한 값을 가질 수 없습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

둘 이상의 열에 대해 인덱스를 지정할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> 속성의 고유 집합 마다 하나의 인덱스만 있습니다. 흐름 API를 사용 하 여 인덱스를 이미 정의 하는 속성 집합에서 규칙 또는 이전 구성으로 인덱스를 구성 하는 경우 해당 인덱스의 정의가 변경 됩니다. 규칙으로 만들어진 인덱스를 추가로 구성 하려는 경우에 유용 합니다.
