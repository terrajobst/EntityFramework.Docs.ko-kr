---
title: 데이터 형식-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993523"
---
# <a name="data-types"></a>데이터 형식

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

데이터 형식 속성을 매핑되는 열의 데이터베이스 특정 형식을 가리킵니다.

## <a name="conventions"></a>규칙

규칙에 따라 데이터베이스 공급자에 속성의 CLR 형식에 따라 데이터 형식을 선택 합니다. 또한 계정에 걸리는 같은 구성 된 다른 메타 데이터를 [최대 길이](../max-length.md)속성이 기본 키 등의 일부 인지 여부에 관계 없이 합니다.

SQL Server에서 사용 하는 예를 들어 `datetime2(7)` 에 대 한 `DateTime` 속성 및 `nvarchar(max)` 에 대 한 `string` 속성 (또는 `nvarchar(450)` 에 대 한 `string` 키로 사용 되는 속성).

## <a name="data-annotations"></a>데이터 주석

열에 대 한 정확한 데이터 형식을 지정 하려면 데이터 주석을 사용할 수 있습니다.

예를 들어 다음 코드를 구성 `Url` 의 최대 길이 사용 하 여 비유니코드 문자열로 `200` 하 고 `Rating` 자릿수와 소수 `5` 의 크기를 조정 하 고 `2`입니다.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Fluent API

또한 열에 대 한 동일한 데이터 형식을 지정 하는 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
