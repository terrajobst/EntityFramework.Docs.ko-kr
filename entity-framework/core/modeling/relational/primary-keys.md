---
title: 기본 키-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656043"
---
# <a name="primary-keys"></a>기본 키

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

기본 키 제약 조건은 각 엔터티 형식의 키에 대해 도입 됩니다.

## <a name="conventions"></a>규칙

규칙에 따라 데이터베이스의 기본 키 이름이 `PK_<type name>`로 지정 됩니다.

## <a name="data-annotations"></a>데이터 주석

기본 키의 관계형 데이터베이스 관련 요소는 데이터 주석을 사용 하 여 구성할 수 없습니다.

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 데이터베이스의 primary key 제약 조건 이름을 구성할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
