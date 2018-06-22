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
# <a name="shadow-properties"></a><span data-ttu-id="906aa-102">그림자 속성</span><span class="sxs-lookup"><span data-stu-id="906aa-102">Shadow Properties</span></span>

<span data-ttu-id="906aa-103">그림자 속성은.NET 엔터티 클래스에 정의 되어 있지 않은 하지만 EF 핵심 모델에서 해당 엔터티 형식에 대해 정의 된 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="906aa-104">값과 이러한 특성의 상태 변경 추적기에 순수 하 게 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="906aa-105">섀도 속성은 매핑된 엔터티 형식에 노출 되지 않아야 하는 데이터베이스에는 데이터가 있는 경우에 유용 합니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="906aa-106">여기서 두 엔터티 간의 관계는 데이터베이스의 외래 키 값으로 표시 되지만 관계는 엔터티 형식 간에 탐색 속성을 사용 하는 엔터티 형식에 관리 되는 외래 키 속성에 대 한 가장 자주 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="906aa-107">그림자의 속성 값을 통해 변경 된 `ChangeTracker` API입니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="906aa-108">그림자 속성을 통해 LINQ 쿼리에서 참조할 수 있습니다는 `EF.Property` 정적 메서드입니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="906aa-109">규칙</span><span class="sxs-lookup"><span data-stu-id="906aa-109">Conventions</span></span>

<span data-ttu-id="906aa-110">그림자 속성 관계를 검색 하지만 종속 엔터티 클래스에 없는 외래 키 속성이 없을 때 일반적으로 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="906aa-111">이 경우 섀도 외래 키 속성을 소개 합니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="906aa-112">그림자 외래 키 속성 이름이 지정 됩니다 `<navigation property name><principal key property name>` (종속 엔터티가 주 엔터티를 가리키는에 탐색 명명 사용 됨).</span><span class="sxs-lookup"><span data-stu-id="906aa-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="906aa-113">주 키 속성 이름에는 탐색 속성의 경우 이름이 됩니다 `<principal key property name>`합니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="906aa-114">종속 엔터티가에 탐색 속성이 없을 경우 그 자리에 보안 주체 유형 이름이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="906aa-115">예를 들어 다음 코드 샘플 됩니다는 `BlogId` 에 도입 되 고 섀도 속성은 `Post` 엔터티.</span><span class="sxs-lookup"><span data-stu-id="906aa-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

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

## <a name="data-annotations"></a><span data-ttu-id="906aa-116">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="906aa-116">Data Annotations</span></span>

<span data-ttu-id="906aa-117">데이터 주석을 사용한 그림자 속성을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="906aa-118">Fluent API</span><span class="sxs-lookup"><span data-stu-id="906aa-118">Fluent API</span></span>

<span data-ttu-id="906aa-119">섀도 속성을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="906aa-120">문자열 오버 로드를 호출한 후 `Property` 다른 속성에 대 한 구성 호출 하 여 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="906aa-121">이름에 제공 되는 경우는 `Property` (그림자 속성 또는 엔터티 클래스에 정의 된 하나), 기존 속성의 이름을 일치 하는 메서드가 다음 코드는 새 섀도 속성을 소개 하는 대신 해당 기존 속성 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="906aa-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

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
