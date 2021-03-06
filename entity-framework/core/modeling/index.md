---
title: 모델 만들기 및 구성 - EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 0f44d9684ca5c8435d83085f9038860309bd82a2
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412778"
---
# <a name="creating-and-configuring-a-model"></a>모델 만들기 및 구성

Entity Framework는 엔터티 클래스의 형태에 따라 규칙 집합을 사용하여 모델을 작성합니다. 규칙에서 발견한 항목을 보충 및/또는 재정의하기 위해 추가적인 구성을 지정할 수 있습니다.

이 문서에서는 모든 데이터 저장소를 대상으로 하는 모델에 적용할 수 있는 구성과, 모든 관계형 데이터베이스를 대상으로 할 때 적용할 수 있는 구성을 다룹니다. 공급자는 특정 데이터 저장소에만 해당하는 구성을 활성화할 수도 있습니다. 공급자 특정 구성에 대한 설명서는  [데이터베이스 공급자](../providers/index.md)  섹션을 참조하세요.

> [!TIP]  
> GitHub에서 이 문서의  [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) 을 볼 수 있습니다.

## <a name="use-fluent-api-to-configure-a-model"></a>Fluent API를 사용하여 모델 구성

파생된 컨텍스트에서  `OnModelCreating`  메서드를 재정의하고  `ModelBuilder API`를 사용하여 모델을 구성할 수 있습니다. 이것은 가장 강력한 구성 방법으로, 엔터티 클래스를 수정하지 않고도 구성을 지정할 수 있습니다. Fluent API 구성은 우선 순위가 가장 높으며 규칙과 데이터 주석을 재정의합니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a>데이터 주석을 사용하여 모델 구성

특성(데이터 주석이라고 함)을 클래스와 속성에 정의할 수도 있습니다. 데이터 주석은 규칙을 재정의하지만 흐름 API 구성이 데이터 주석을 재정의합니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
