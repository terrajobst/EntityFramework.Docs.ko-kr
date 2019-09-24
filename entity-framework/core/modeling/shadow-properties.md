---
title: 그림자 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: 5fdc4c50c295f73d0fa5eef3518adf4d3eb95599
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197707"
---
# <a name="shadow-properties"></a><span data-ttu-id="f18de-102">섀도 속성</span><span class="sxs-lookup"><span data-stu-id="f18de-102">Shadow Properties</span></span>

<span data-ttu-id="f18de-103">섀도 속성은 .NET 엔터티 클래스에 정의 되어 있지 않지만 EF Core 모델에서 해당 엔터티 형식에 대해 정의 되는 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="f18de-104">이러한 속성의 값과 상태는 전적으로 변경 추적에서 유지 관리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="f18de-105">섀도 속성은 매핑된 엔터티 형식에 노출 되지 않아야 하는 데이터가 데이터베이스에 있는 경우에 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="f18de-106">이러한 속성은 외래 키 속성에 사용 되는 경우가 가장 많습니다. 여기서 두 엔터티 간의 관계는 데이터베이스의 외래 키 값으로 표시 되지만 엔터티 형식 간의 탐색 속성을 사용 하 여 엔터티 형식에서 관계가 관리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="f18de-107">API를 `ChangeTracker` 통해 섀도 속성 값을 가져오고 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="f18de-108">정적 메서드를 `EF.Property` 통해 LINQ 쿼리에서 섀도 속성을 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="f18de-109">규칙</span><span class="sxs-lookup"><span data-stu-id="f18de-109">Conventions</span></span>

<span data-ttu-id="f18de-110">그림자 속성은 관계가 검색 되었지만 종속 엔터티 클래스에 외래 키 속성이 없는 경우 규칙에 따라 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="f18de-111">이 경우에는 섀도 외래 키 속성이 도입 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="f18de-112">섀도 외래 키 속성의 이름은 `<navigation property name><principal key property name>` 로 지정 됩니다 (주 엔터티를 가리키는 종속 엔터티에 대 한 탐색은 명명에 사용 됨).</span><span class="sxs-lookup"><span data-stu-id="f18de-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="f18de-113">주 키 속성 이름에 탐색 속성의 이름이 포함 된 경우이 이름은 `<principal key property name>`입니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="f18de-114">종속 엔터티에 대 한 탐색 속성이 없는 경우 주 형식 이름이 대신 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="f18de-115">예를 들어 다음 코드 목록에서는 `BlogId` `Post` 엔터티에 대해 섀도 속성이 도입 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/ShadowForeignKey.cs)] -->
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

## <a name="data-annotations"></a><span data-ttu-id="f18de-116">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="f18de-116">Data Annotations</span></span>

<span data-ttu-id="f18de-117">데이터 주석으로는 섀도 속성을 만들 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f18de-118">Fluent API</span><span class="sxs-lookup"><span data-stu-id="f18de-118">Fluent API</span></span>

<span data-ttu-id="f18de-119">흐름 API를 사용 하 여 섀도 속성을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="f18de-120">의 `Property` 문자열 오버 로드를 호출한 후에는 다른 속성에 대 한 구성 호출을 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="f18de-121">`Property` 메서드에 제공 된 이름이 기존 속성의 이름 (shadow 속성 또는 엔터티 클래스에 정의 된 속성)과 일치 하는 경우 코드는 새 shadow 속성을 도입 하는 대신 기존 속성을 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="f18de-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/ShadowProperty.cs?highlight=7,8)] -->
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
