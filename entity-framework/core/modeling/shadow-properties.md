---
title: 그림자 속성-EF Core
author: AndriySvyryd
ms.date: 01/03/2020
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 229cfd83f75b01dff9ac9ad30ee55c7cc727c19e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414712"
---
# <a name="shadow-properties"></a>섀도 속성

섀도 속성은 .NET 엔터티 클래스에 정의 되어 있지 않지만 EF Core 모델에서 해당 엔터티 형식에 대해 정의 되는 속성입니다. 이러한 속성의 값과 상태는 전적으로 변경 추적에서 유지 관리 됩니다. 섀도 속성은 매핑된 엔터티 형식에 노출 되지 않아야 하는 데이터가 데이터베이스에 있는 경우에 유용 합니다.

## <a name="foreign-key-shadow-properties"></a>외래 키 그림자 속성

섀도 속성은 외래 키 속성에 사용 되는 경우가 가장 많습니다. 여기서 두 엔터티 간의 관계는 데이터베이스의 외래 키 값으로 표시 되지만 엔터티 간의 탐색 속성을 사용 하 여 엔터티 형식에서 관계가 관리 됩니다. 종류. 규칙에 따라, 관계를 검색 했지만 종속 엔터티 클래스에 외래 키 속성이 없는 경우 EF는 그림자 속성을 소개 합니다.

속성의 이름은 `<navigation property name><principal key property name>` (주 엔터티를 가리키는 종속 엔터티에 대 한 탐색은 명명에 사용 됨). 주 키 속성 이름에 탐색 속성 이름이 포함 된 경우에는 이름이 `<principal key property name>`됩니다. 종속 엔터티에 대 한 탐색 속성이 없는 경우 주 형식 이름이 대신 사용 됩니다.

예를 들어 다음 코드 목록에서는 `Post` 엔터티에 `BlogId` shadow 속성이 추가 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/ShadowForeignKey.cs?name=Conventions&highlight=21-23)]

## <a name="configuring-shadow-properties"></a>그림자 속성 구성

흐름 API를 사용 하 여 섀도 속성을 구성할 수 있습니다. `Property`의 문자열 오버 로드를 호출한 후에는 다른 속성에 대 한 구성 호출을 연결할 수 있습니다. 다음 샘플에서는 `Blog`에 `LastUpdated`라는 CLR 속성이 없으므로 shadow 속성이 생성 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ShadowProperty.cs?name=ShadowProperty&highlight=8)]

`Property` 메서드에 제공 된 이름이 기존 속성의 이름 (shadow 속성 또는 엔터티 클래스에 정의 된 속성)과 일치 하는 경우 코드는 새 shadow 속성을 도입 하는 대신 기존 속성을 구성 합니다.

## <a name="accessing-shadow-properties"></a>그림자 속성 액세스

`ChangeTracker` API를 통해 섀도 속성 값을 가져오고 변경할 수 있습니다.

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

`EF.Property` 정적 메서드를 통해 LINQ 쿼리에서 섀도 속성을 참조할 수 있습니다.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```
