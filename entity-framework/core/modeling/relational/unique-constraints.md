---
title: 대체 키 (고유 제약 조건)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994193"
---
# <a name="alternate-keys-unique-constraints"></a>대체 키 (고유 제약 조건)

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

모델에서 각 대체 키에 대 한 unique 제약 조건을 도입 되었습니다.

## <a name="conventions"></a>규칙

규칙에 따라 인덱스 및 대체 키에 대 한 소개 하는 제약 조건 이름은 `AK_<type name>_<property name>`합니다. 대체는 복합 키에 대 한 `<property name>` 속성 이름에 밑줄 구분 된 목록이 있습니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 unique 제약 조건은 구성할 수 있습니다.

## <a name="fluent-api"></a>Fluent API

대체 키에 대 한 인덱스 및 제약 조건 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
