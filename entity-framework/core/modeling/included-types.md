---
title: 형식을 제외한 & 포함-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655733"
---
# <a name="including--excluding-types"></a>형식 포함 및 제외

모델에 형식을 포함 하면 EF는 해당 형식에 대 한 메타 데이터를 포함 하 고 데이터베이스에서 인스턴스를 읽고 쓰는 것을 시도 합니다.

## <a name="conventions"></a>규칙

규칙에 따라 컨텍스트의 `DbSet` 속성에 노출 된 형식이 모델에 포함 됩니다. 또한 `OnModelCreating` 메서드에 설명 된 형식도 포함 되어 있습니다. 마지막으로 검색 된 형식의 탐색 속성을 재귀적으로 탐색 하 여 발견 되는 모든 형식은 모델에도 포함 됩니다.

**예를 들어 다음 코드 목록에서는 세 가지 형식이 모두 검색 됩니다.**

* `Blog`는 컨텍스트의 `DbSet` 속성에 노출 되기 때문입니다.

* `Post` `Blog.Posts` 탐색 속성을 통해 검색 되기 때문입니다.

* `AuditEntry` `OnModelCreating`에 설명 되어 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 모델에서 형식을 제외할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 모델에서 형식을 제외할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
