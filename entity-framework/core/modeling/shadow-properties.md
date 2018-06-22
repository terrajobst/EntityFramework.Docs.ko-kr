---
title: 그림자 속성-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053553"
---
# <a name="shadow-properties"></a>그림자 속성

그림자 속성은.NET 엔터티 클래스에 정의 되어 있지 않은 하지만 EF 핵심 모델에서 해당 엔터티 형식에 대해 정의 된 속성입니다. 값과 이러한 특성의 상태 변경 추적기에 순수 하 게 유지 됩니다.

섀도 속성은 매핑된 엔터티 형식에 노출 되지 않아야 하는 데이터베이스에는 데이터가 있는 경우에 유용 합니다. 여기서 두 엔터티 간의 관계는 데이터베이스의 외래 키 값으로 표시 되지만 관계는 엔터티 형식 간에 탐색 속성을 사용 하는 엔터티 형식에 관리 되는 외래 키 속성에 대 한 가장 자주 사용 됩니다.

그림자의 속성 값을 통해 변경 된 `ChangeTracker` API입니다.

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

그림자 속성을 통해 LINQ 쿼리에서 참조할 수 있습니다는 `EF.Property` 정적 메서드입니다.

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a>규칙

그림자 속성 관계를 검색 하지만 종속 엔터티 클래스에 없는 외래 키 속성이 없을 때 일반적으로 만들 수 있습니다. 이 경우 섀도 외래 키 속성을 소개 합니다. 그림자 외래 키 속성 이름이 지정 됩니다 `<navigation property name><principal key property name>` (종속 엔터티가 주 엔터티를 가리키는에 탐색 명명 사용 됨). 주 키 속성 이름에는 탐색 속성의 경우 이름이 됩니다 `<principal key property name>`합니다. 종속 엔터티가에 탐색 속성이 없을 경우 그 자리에 보안 주체 유형 이름이 사용 됩니다.

예를 들어 다음 코드 샘플 됩니다는 `BlogId` 에 도입 되 고 섀도 속성은 `Post` 엔터티.

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용한 그림자 속성을 만들 수 있습니다.

## <a name="fluent-api"></a>Fluent API

섀도 속성을 구성 하려면 Fluent API를 사용할 수 있습니다. 문자열 오버 로드를 호출한 후 `Property` 다른 속성에 대 한 구성 호출 하 여 연결할 수 있습니다.

이름에 제공 되는 경우는 `Property` (그림자 속성 또는 엔터티 클래스에 정의 된 하나), 기존 속성의 이름을 일치 하는 메서드가 다음 코드는 새 섀도 속성을 소개 하는 대신 해당 기존 속성 구성 됩니다.

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
