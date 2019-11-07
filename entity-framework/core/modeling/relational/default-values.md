---
title: 기본값-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655911"
---
# <a name="default-values"></a>기본값

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

열의 기본값은 새 행이 삽입 되었지만 열에 값이 지정 되지 않은 경우 삽입 되는 값입니다.

## <a name="conventions"></a>규칙

규칙에 따라 기본값은 구성 되지 않습니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 기본값을 설정할 수 없습니다.

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 속성에 대 한 기본값을 지정할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

또한 기본값을 계산 하는 데 사용 되는 SQL 조각을 지정할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
