---
title: Foreign Key 제약 조건-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655993"
---
# <a name="foreign-key-constraints"></a>외래 키 제약 조건

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

외래 키 제약 조건은 모델의 각 관계에 대해 도입 됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 foreign key 제약 조건의 이름은 `FK_<dependent type name>_<principal type name>_<foreign key property name>`입니다. 복합 외래 키의 경우에는 쉼표로 구분 된 외래 키 속성 이름 목록이 `<foreign key property name>` 됩니다.

## <a name="data-annotations"></a>데이터 주석

외래 키 제약 조건 이름은 데이터 주석을 사용 하 여 구성할 수 없습니다.

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 관계에 대 한 foreign key 제약 조건 이름을 구성할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
