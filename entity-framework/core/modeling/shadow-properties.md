---
title: 섀도 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
uid: core/modeling/shadow-properties
ms.openlocfilehash: b7b7b10642564dfa3dbc05755188b5b5c63e0d03
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993804"
---
# <a name="shadow-properties"></a><span data-ttu-id="09568-102">섀도 속성</span><span class="sxs-lookup"><span data-stu-id="09568-102">Shadow Properties</span></span>

<span data-ttu-id="09568-103">섀도 속성은.NET 엔터티 클래스에 정의 되지 않은 하지만 EF Core 모델에서 해당 엔터티 형식에 대해 정의 된 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="09568-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="09568-104">값 및 이러한 속성의 상태를 변경 추적기에서 순수 하 게 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="09568-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="09568-105">섀도 속성 매핑된 엔터티 형식에서 노출 되지 않아야 하는 데이터베이스에 데이터가 있을 때 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="09568-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="09568-106">여기서 두 엔터티 간의 관계 데이터베이스의 외래 키 값이 표현 하지만 관계 엔터티 형식 간의 탐색 속성을 사용 하 여 엔터티 형식에 대해 관리 되는 외래 키 속성에 대해 가장 자주 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="09568-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="09568-107">섀도 속성 값을 구한 후 통해 변경할 수는 `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="09568-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="09568-108">섀도 속성을 통해 LINQ 쿼리에서 참조할 수 있습니다는 `EF.Property` 정적 메서드입니다.</span><span class="sxs-lookup"><span data-stu-id="09568-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="09568-109">규칙</span><span class="sxs-lookup"><span data-stu-id="09568-109">Conventions</span></span>

<span data-ttu-id="09568-110">섀도 속성 관계 검색 되지만 외래 키 속성이 종속 엔터티 클래스에 위치한 경우 규칙에 따라 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="09568-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="09568-111">이 경우 그림자 외래 키 속성을 소개 합니다.</span><span class="sxs-lookup"><span data-stu-id="09568-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="09568-112">섀도 외래 키 속성의 이름은 `<navigation property name><principal key property name>` (탐색 창에 종속 엔터티가 주 엔터티를 가리키는 명명 사용 됨).</span><span class="sxs-lookup"><span data-stu-id="09568-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="09568-113">주 키 속성 이름에는 탐색 속성의 이름을 포함 되어 있으면 이름이 됩니다 `<principal key property name>`합니다.</span><span class="sxs-lookup"><span data-stu-id="09568-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="09568-114">종속 엔터티의 탐색 속성이 없는 경우에 보안 주체 유형 이름은 해당 위치에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="09568-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="09568-115">예를 들어, 다음 코드 목록은 하면를 `BlogId` 소개 되 섀도 속성은 `Post` 엔터티.</span><span class="sxs-lookup"><span data-stu-id="09568-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="09568-116">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="09568-116">Data Annotations</span></span>

<span data-ttu-id="09568-117">데이터 주석을 사용한 섀도 속성을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="09568-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="09568-118">Fluent API</span><span class="sxs-lookup"><span data-stu-id="09568-118">Fluent API</span></span>

<span data-ttu-id="09568-119">섀도 속성을 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="09568-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="09568-120">문자열 오버 로드를 호출한 후 `Property` 다른 속성의 경우 구성 호출을 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="09568-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="09568-121">이름을 제공 하는 경우는 `Property` 섀도 속성, 엔터티 클래스에 정의 된 기존 속성의 이름을 일치 하는 메서드가 다음 코드는 새 섀도 속성을 소개 하는 것이 아니라 기존 속성에 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="09568-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
