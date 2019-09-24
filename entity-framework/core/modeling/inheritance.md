---
title: 상속-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: 1f20c455176b5922364584f8c7688c15a4c3f0f9
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197293"
---
# <a name="inheritance"></a>상속

EF 모델의 상속은 데이터베이스에서 엔터티 클래스의 상속이 표시 되는 방식을 제어 하는 데 사용 됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 데이터베이스에서 상속이 표시 되는 방식을 결정 하는 것은 데이터베이스 공급자에 게 있습니다. 관계형 데이터베이스 공급자를 사용 하 여이를 처리 하는 방법은 [상속 (관계형 데이터베이스)](relational/inheritance.md) 을 참조 하세요.

EF는 둘 이상의 상속 된 형식이 모델에 명시적으로 포함 된 경우에만 상속을 설정 합니다. EF는 모델에 포함 되지 않은 기본 또는 파생 형식을 검색 하지 않습니다. 상속 계층 구조의 각 형식에 대해 *dbset<TEntity>*  를 노출 하 여 모델에 형식을 포함할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

계층 구조에 있는 하나 이상의 엔터티에 대해 *dbset<TEntity>*  을 노출 하지 않으려는 경우 흐름 API를 사용 하 여 모델에 포함 되도록 할 수 있습니다.
규칙을 사용 하지 않는 경우를 사용 하 여 `HasBaseType`명시적으로 기본 형식을 지정할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> 를 사용 `.HasBaseType((Type)null)` 하 여 계층에서 엔터티 형식을 제거할 수 있습니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석은 상속을 구성 하는 데 사용할 수 없습니다.

## <a name="fluent-api"></a>Fluent API

상속에 대 한 흐름 API는 사용 중인 데이터베이스 공급자에 따라 달라 집니다. 관계형 데이터베이스 공급자에 대해 수행할 수 있는 구성에 대 한 [상속 (관계형 데이터베이스)](relational/inheritance.md) 을 참조 하세요.
