---
title: 계산 열-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655922"
---
# <a name="computed-columns"></a>계산된 열

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

계산 열은 해당 값이 데이터베이스에서 계산 되는 열입니다. 계산 열은 테이블의 다른 열을 사용 하 여 해당 값을 계산할 수 있습니다.

## <a name="conventions"></a>규칙

규칙에 따라 계산 열은 모델에 생성 되지 않습니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석으로 계산 열을 구성할 수 없습니다.

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 속성이 계산 열에 매핑되도록 지정할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
