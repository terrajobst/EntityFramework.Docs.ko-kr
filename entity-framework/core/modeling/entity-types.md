---
title: 엔터티 형식-EF Core
description: Entity Framework Core를 사용 하 여 엔터티 형식을 구성 하 고 매핑하는 방법
author: roji
ms.date: 12/03/2019
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/entity-types
ms.openlocfilehash: b3d9ad753637d021d9aa52965da38091ae690f77
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502416"
---
# <a name="entity-types"></a><span data-ttu-id="b73ce-103">엔터티 형식</span><span class="sxs-lookup"><span data-stu-id="b73ce-103">Entity Types</span></span>

<span data-ttu-id="b73ce-104">형식에 대 한 DbSet을 컨텍스트에 포함 하는 것은 EF Core의 모델에 포함 됨을 의미 합니다. 일반적으로 이러한 형식을 *엔터티로*참조 합니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-104">Including a DbSet of a type on your context means that it is included in EF Core's model; we usually refer to such a type as an *entity*.</span></span> <span data-ttu-id="b73ce-105">EF Core는 데이터베이스에서 엔터티 인스턴스를 읽고 쓸 수 있으며 관계형 데이터베이스를 사용 하는 경우 마이그레이션을 통해 엔터티에 대 한 테이블을 만들 수 EF Core.</span><span class="sxs-lookup"><span data-stu-id="b73ce-105">EF Core can read and write entity instances from/to the database, and if you're using a relational database, EF Core can create tables for your entities via migrations.</span></span>

## <a name="including-types-in-the-model"></a><span data-ttu-id="b73ce-106">모델에 형식 포함</span><span class="sxs-lookup"><span data-stu-id="b73ce-106">Including types in the model</span></span>

<span data-ttu-id="b73ce-107">규칙에 따라 컨텍스트의 DbSet 속성에 노출 되는 형식은 모델에 엔터티로 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-107">By convention, types that are exposed in DbSet properties on your context are included in the model as entities.</span></span> <span data-ttu-id="b73ce-108">다른 검색 된 엔터티 형식의 탐색 속성을 재귀적으로 탐색 하 여 발견 되는 모든 형식과 마찬가지로 `OnModelCreating` 메서드에 지정 된 엔터티 형식도 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-108">Entity types that are specified in the `OnModelCreating` method are also included, as are any types that are found by recursively exploring the navigation properties of other discovered entity types.</span></span>

<span data-ttu-id="b73ce-109">아래 코드 샘플에는 모든 형식이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-109">In the code sample below, all types are included:</span></span>

* <span data-ttu-id="b73ce-110">`Blog`는 컨텍스트의 DbSet 속성에 노출 되기 때문에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-110">`Blog` is included because it's exposed in a DbSet property on the context.</span></span>
* <span data-ttu-id="b73ce-111">`Blog.Posts` 탐색 속성을 통해 검색 되기 때문에 `Post` 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-111">`Post` is included because it's discovered via the `Blog.Posts` navigation property.</span></span>
* <span data-ttu-id="b73ce-112">`AuditEntry` `OnModelCreating`에 지정 되어 있기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-112">`AuditEntry` because it is specified in `OnModelCreating`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a><span data-ttu-id="b73ce-113">모델에서 형식 제외</span><span class="sxs-lookup"><span data-stu-id="b73ce-113">Excluding types from the model</span></span>

<span data-ttu-id="b73ce-114">모델에 형식을 포함 하지 않으려면 제외할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-114">If you don't want a type to be included in the model, you can exclude it:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="b73ce-115">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="b73ce-115">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="b73ce-116">흐름 API</span><span class="sxs-lookup"><span data-stu-id="b73ce-116">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

## <a name="table-name"></a><span data-ttu-id="b73ce-117">표 이름</span><span class="sxs-lookup"><span data-stu-id="b73ce-117">Table name</span></span>

<span data-ttu-id="b73ce-118">규칙에 따라 엔터티를 노출 하는 DbSet 속성과 이름이 같은 데이터베이스 테이블에 매핑하기 위해 각 엔터티 형식이 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-118">By convention, each entity type will be set up to map to a database table with the same name as the DbSet property that exposes the entity.</span></span> <span data-ttu-id="b73ce-119">지정 된 엔터티에 대해 DbSet가 없으면 클래스 이름이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-119">If no DbSet exists for the given entity, the class name is used.</span></span>

<span data-ttu-id="b73ce-120">테이블 이름을 수동으로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-120">You can manually configure the table name:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="b73ce-121">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="b73ce-121">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="b73ce-122">흐름 API</span><span class="sxs-lookup"><span data-stu-id="b73ce-122">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a><span data-ttu-id="b73ce-123">테이블 스키마</span><span class="sxs-lookup"><span data-stu-id="b73ce-123">Table schema</span></span>

<span data-ttu-id="b73ce-124">관계형 데이터베이스를 사용 하는 경우 테이블은 데이터베이스의 기본 스키마에서 생성 된 규칙을 기준으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-124">When using a relational database, tables are by convention created in your database's default schema.</span></span> <span data-ttu-id="b73ce-125">예를 들어 Microsoft SQL Server은 `dbo` 스키마 (SQLite는 스키마를 지원 하지 않음)를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-125">For example, Microsoft SQL Server will use the `dbo` schema (SQLite does not support schemas).</span></span>

<span data-ttu-id="b73ce-126">다음과 같이 특정 스키마에서 테이블을 만들도록 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-126">You can configure tables to be created in a specific schema as follows:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="b73ce-127">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="b73ce-127">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="b73ce-128">흐름 API</span><span class="sxs-lookup"><span data-stu-id="b73ce-128">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

<span data-ttu-id="b73ce-129">각 테이블에 대 한 스키마를 지정 하는 대신 흐름 API를 사용 하 여 모델 수준에서 기본 스키마를 정의할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-129">Rather than specifying the schema for each table, you can also define the default schema at the model level with the fluent API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

<span data-ttu-id="b73ce-130">기본 스키마를 설정 하면 시퀀스와 같은 다른 데이터베이스 개체에도 영향을 줍니다.</span><span class="sxs-lookup"><span data-stu-id="b73ce-130">Note that setting the default schema will also affect other database objects, such as sequences.</span></span>
