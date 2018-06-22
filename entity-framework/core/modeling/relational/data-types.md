---
title: 데이터 형식-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053503"
---
# <a name="data-types"></a>데이터 형식

> [!NOTE]  
> 이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다. 여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).

속성이 매핑되는 열의 데이터베이스 관련 형식에 데이터 형식을 참조 합니다.

## <a name="conventions"></a>규칙

규칙에 따라 데이터베이스 공급자 속성의 CLR 형식을 기반으로 데이터 형식을 선택 합니다. 또한 계정으로 사용 하며 같은 구성 된 다른 메타 데이터를 [최대 길이](../max-length.md)속성이 기본 키 등의 일부 인지 여부에 관계 없이 합니다.

SQL Server 사용 예를 들어 `datetime2(7)` 에 대 한 `DateTime` 속성 및 `nvarchar(max)` 에 대 한 `string` 속성 (또는 `nvarchar(450)` 에 대 한 `string` 키로 사용 되는 속성).

## <a name="data-annotations"></a>데이터 주석

열에 대 한 정확한 데이터 형식을 지정 하려면 데이터 주석을 사용할 수 있습니다.

예를 들어 다음 코드를 구성 `Url` 유니코드가 아닌 문자열의 최대 길이 `200` 및 `Rating` 같이 정밀도 10 진수 `5` 의 크기를 조정 하 고 `2`합니다.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a>Fluent API

또한 열에 대 한 동일한 데이터 형식을 지정 하는 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
