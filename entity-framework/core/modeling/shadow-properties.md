---
title: 그림자 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: ab57358dd247e32c4ca0f57d07b4cb98f2b85d29
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655958"
---
# <a name="shadow-properties"></a>섀도 속성

섀도 속성은 .NET 엔터티 클래스에 정의 되어 있지 않지만 EF Core 모델에서 해당 엔터티 형식에 대해 정의 되는 속성입니다. 이러한 속성의 값과 상태는 전적으로 변경 추적에서 유지 관리 됩니다.

섀도 속성은 매핑된 엔터티 형식에 노출 되지 않아야 하는 데이터가 데이터베이스에 있는 경우에 유용 합니다. 이러한 속성은 외래 키 속성에 사용 되는 경우가 가장 많습니다. 여기서 두 엔터티 간의 관계는 데이터베이스의 외래 키 값으로 표시 되지만 엔터티 형식 간의 탐색 속성을 사용 하 여 엔터티 형식에서 관계가 관리 됩니다.

`ChangeTracker` API를 통해 섀도 속성 값을 가져오고 변경할 수 있습니다.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

`EF.Property` 정적 메서드를 통해 LINQ 쿼리에서 섀도 속성을 참조할 수 있습니다.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>규칙

그림자 속성은 관계가 검색 되었지만 종속 엔터티 클래스에 외래 키 속성이 없는 경우 규칙에 따라 만들 수 있습니다. 이 경우에는 섀도 외래 키 속성이 도입 됩니다. 섀도 외래 키 속성의 이름은 `<navigation property name><principal key property name>` (주 엔터티를 가리키는 종속 엔터티에 대 한 탐색은 명명에 사용 됨). 주 키 속성 이름에 탐색 속성 이름이 포함 된 경우에는 이름이 `<principal key property name>`됩니다. 종속 엔터티에 대 한 탐색 속성이 없는 경우 주 형식 이름이 대신 사용 됩니다.

예를 들어 다음 코드 목록에서는 `BlogId` shadow 속성이 `Post` 엔터티에 도입 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions)]

## <a name="data-annotations"></a>데이터 주석

데이터 주석으로는 섀도 속성을 만들 수 없습니다.

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 섀도 속성을 구성할 수 있습니다. `Property`의 문자열 오버 로드를 호출한 후에는 다른 속성에 대 한 구성 호출을 체인으로 연결할 수 있습니다.

`Property` 메서드에 제공 된 이름이 기존 속성의 이름 (shadow 속성 또는 엔터티 클래스에 정의 된 속성)과 일치 하는 경우 코드는 새 shadow 속성을 도입 하는 대신 기존 속성을 구성 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]
