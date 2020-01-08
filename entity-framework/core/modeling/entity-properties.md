---
title: 엔터티 속성-EF Core
description: Entity Framework Core를 사용 하 여 엔터티 속성을 구성 하 고 매핑하는 방법
author: roji
ms.date: 12/10/2019
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/entity-properties
ms.openlocfilehash: b67603fbffd1f1c8506bc21f8972c851eb8eef29
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502428"
---
# <a name="entity-properties"></a><span data-ttu-id="b6e95-103">엔터티 속성</span><span class="sxs-lookup"><span data-stu-id="b6e95-103">Entity Properties</span></span>

<span data-ttu-id="b6e95-104">모델의 각 엔터티 형식에는 데이터베이스에서 읽고 쓸 EF Core는 속성 집합이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-104">Each entity type in your model has a set of properties, which EF Core will read and write from the database.</span></span> <span data-ttu-id="b6e95-105">관계형 데이터베이스를 사용 하는 경우 엔터티 속성은 테이블 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-105">If you're using a relational database, entity properties map to table columns.</span></span>

## <a name="included-and-excluded-properties"></a><span data-ttu-id="b6e95-106">포함 된 속성 및 제외 된 속성</span><span class="sxs-lookup"><span data-stu-id="b6e95-106">Included and excluded properties</span></span>

<span data-ttu-id="b6e95-107">규칙에 따라 getter 및 setter가 있는 모든 public 속성이 모델에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-107">By convention, all public properties with a getter and a setter will be included in the model.</span></span>

<span data-ttu-id="b6e95-108">특정 속성은 다음과 같이 제외할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-108">Specific properties can be excluded as follows:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="b6e95-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="b6e95-109">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?name=IgnoreProperty&highlight=6)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="b6e95-110">흐름 API</span><span class="sxs-lookup"><span data-stu-id="b6e95-110">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?name=IgnoreProperty&highlight=3,4)]

***

## <a name="column-names"></a><span data-ttu-id="b6e95-111">열 이름</span><span class="sxs-lookup"><span data-stu-id="b6e95-111">Column names</span></span>

<span data-ttu-id="b6e95-112">규칙에 따라 관계형 데이터베이스를 사용 하는 경우 엔터티 속성은 속성과 이름이 같은 테이블 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-112">By convention, when using a relational database, entity properties are mapped to table columns having the same name as the property.</span></span>

<span data-ttu-id="b6e95-113">다른 이름을 사용 하 여 열을 구성 하는 것을 선호 하는 경우 다음과 같이 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-113">If you prefer to configure your columns with different names, you can do so as following:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="b6e95-114">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="b6e95-114">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnName.cs?Name=ColumnName&highlight=3)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="b6e95-115">흐름 API</span><span class="sxs-lookup"><span data-stu-id="b6e95-115">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnName.cs?Name=ColumnName&highlight=3-5)]

***

## <a name="column-data-types"></a><span data-ttu-id="b6e95-116">열 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="b6e95-116">Column data types</span></span>

<span data-ttu-id="b6e95-117">관계형 데이터베이스를 사용 하는 경우 데이터베이스 공급자는 속성의 .NET 형식을 기반으로 데이터 형식을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-117">When using a relational database, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="b6e95-118">구성 된 [최대 길이](#maximum-length)와 같은 다른 메타 데이터 (속성이 기본 키의 일부 인지 여부 등)도 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-118">It also takes into account other metadata, such as the configured [maximum length](#maximum-length), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="b6e95-119">예를 들어 SQL Server `DateTime` 속성을 `datetime2(7)` `string` 열에 매핑하고 속성을 `nvarchar(max)` 열 (또는 키로 사용 되는 속성의 경우 `nvarchar(450)`)에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-119">For example, SQL Server maps `DateTime` properties to `datetime2(7)` columns, and `string` properties to `nvarchar(max)` columns (or to `nvarchar(450)` for properties that are used as a key).</span></span>

<span data-ttu-id="b6e95-120">열에 정확한 데이터 형식을 지정 하도록 열을 구성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-120">You can also configure your columns to specify an exact data type for a column.</span></span> <span data-ttu-id="b6e95-121">예를 들어 다음 코드는 최대 길이가 `200` 인 비유니코드 문자열로 `Url`를 구성 하 고 `2``5` 및 소수 자릿수를 사용 하 여 `Rating` 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-121">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`:</span></span>

### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="b6e95-122">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="b6e95-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/ColumnDataType.cs?name=ColumnDataType&highlight=4,6)]

### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="b6e95-123">흐름 API</span><span class="sxs-lookup"><span data-stu-id="b6e95-123">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ColumnDataType.cs?name=ColumnDataType&highlight=5-6)]

***

### <a name="maximum-length"></a><span data-ttu-id="b6e95-124">최대 길이</span><span class="sxs-lookup"><span data-stu-id="b6e95-124">Maximum length</span></span>

<span data-ttu-id="b6e95-125">최대 길이를 구성 하면 지정 된 속성에 대해 선택할 적절 한 열 데이터 형식에 대 한 힌트를 데이터베이스 공급자에 게 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-125">Configuring a maximum length provides a hint to the database provider about the appropriate column data type to choose for a given property.</span></span> <span data-ttu-id="b6e95-126">최대 길이는 `string` 및 `byte[]`와 같은 배열 데이터 형식에만 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-126">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]
> <span data-ttu-id="b6e95-127">Entity Framework는 공급자에 게 데이터를 전달 하기 전에 최대 길이에 대 한 유효성 검사를 수행 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-127">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="b6e95-128">해당 하는 경우 공급자 또는 데이터 저장소에서 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-128">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="b6e95-129">예를 들어 SQL Server를 대상으로 지정 하는 경우 최대 길이를 초과 하면 기본 열의 데이터 형식에 따라 초과 되는 데이터가 저장 되는 것을 허용 하지 않기 때문에 예외가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-129">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

<span data-ttu-id="b6e95-130">다음 예제에서는 최대 길이 500를 구성 하면 SQL Server에서 `nvarchar(500)` 형식의 열이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-130">In the following example, configuring a maximum length of 500 will cause a column of type `nvarchar(500)` to be created on SQL Server:</span></span>

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="b6e95-131">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="b6e95-131">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?name=MaxLength&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="b6e95-132">흐름 API</span><span class="sxs-lookup"><span data-stu-id="b6e95-132">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?name=MaxLength&highlight=3-5)]

***

## <a name="required-and-optional-properties"></a><span data-ttu-id="b6e95-133">필수 및 선택적 속성</span><span class="sxs-lookup"><span data-stu-id="b6e95-133">Required and optional properties</span></span>

<span data-ttu-id="b6e95-134">속성은 `null`를 포함 하는 데 유효 하면 선택 사항으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-134">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="b6e95-135">`null` 속성에 할당 되는 유효한 값이 아닌 경우 필수 속성으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-135">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span> <span data-ttu-id="b6e95-136">관계형 데이터베이스 스키마에 매핑할 때 필수 속성은 null을 허용 하지 않는 열로 생성 되 고 선택적 속성은 null을 허용 하는 열로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-136">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

### <a name="conventions"></a><span data-ttu-id="b6e95-137">표기 규칙</span><span class="sxs-lookup"><span data-stu-id="b6e95-137">Conventions</span></span>

<span data-ttu-id="b6e95-138">규칙에 따라 .NET 형식이 null을 포함할 수 있는 속성은 선택 사항으로 구성 되 고, .NET 형식은 null을 포함할 수 없는 속성은 필수로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-138">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="b6e95-139">예를 들어 .NET 값 형식 (`int`, `decimal`, `bool`등)을 포함 하는 모든 속성은 필수로 구성 되며 nullable .NET 값 형식 (`int?`, `decimal?`, `bool?`등)을 포함 하는 모든 속성은 선택 사항으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-139">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="b6e95-140">C#8에는 null이 포함 될 수 있는지 여부를 나타내는 [nullable 참조 형식](/dotnet/csharp/tutorials/nullable-reference-types)이라는 새로운 기능이 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-140">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="b6e95-141">이 기능은 기본적으로 사용 하지 않도록 설정 되어 있으며, 사용 하도록 설정 된 경우 다음과 같은 방식으로 EF Core의 동작을 수정 합니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-141">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="b6e95-142">Nullable 참조 형식이 사용 하지 않도록 설정 된 경우 (기본값) .NET 참조 형식이 포함 된 모든 속성은 규칙에 따라 선택적으로 구성 됩니다 (예: `string`).</span><span class="sxs-lookup"><span data-stu-id="b6e95-142">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="b6e95-143">Nullable 참조 형식이 사용 되는 경우 속성은 .NET 형식의 C# null 허용 여부에 따라 구성 됩니다. `string?`은 선택적으로 구성 되지만 `string`는 필수로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-143">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="b6e95-144">다음 예제에서는 nullable 참조 기능을 사용 하지 않도록 설정 하 고 (기본값) 사용 하도록 설정 된 필수 및 선택적 속성이 포함 된 엔터티 형식을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-144">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

#### <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="b6e95-145">Null을 허용 하지 않는 참조 형식 (기본값)</span><span class="sxs-lookup"><span data-stu-id="b6e95-145">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

#### <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="b6e95-146">Nullable 참조 형식 사용</span><span class="sxs-lookup"><span data-stu-id="b6e95-146">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="b6e95-147">코드에 C# 표시 되는 null 허용 여부를 EF Core의 모델 및 데이터베이스로 전달 하 고 흐름 API 또는 데이터 주석을 사용 하 여 동일한 개념을 두 번 표현 하므로 nullable 참조 형식을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-147">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="b6e95-148">기존 프로젝트에서 nullable 참조 형식을 사용 하도록 설정 하는 경우 주의 해야 합니다. 이전에 선택적으로 구성 된 참조 형식 속성은 nullable로 명시적으로 주석 처리 되지 않는 한 이제 필수로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-148">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="b6e95-149">관계형 데이터베이스 스키마를 관리할 때이로 인해 데이터베이스 열의 null 허용 여부를 변경 하는 마이그레이션이 생성 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-149">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="b6e95-150">Nullable 참조 형식 및 EF Core와 함께 사용 하는 방법에 대 한 자세한 내용은 [이 기능에 대 한 전용 설명서 페이지를 참조](xref:core/miscellaneous/nullable-reference-types)하세요.</span><span class="sxs-lookup"><span data-stu-id="b6e95-150">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

### <a name="explicit-configuration"></a><span data-ttu-id="b6e95-151">명시적 구성</span><span class="sxs-lookup"><span data-stu-id="b6e95-151">Explicit configuration</span></span>

<span data-ttu-id="b6e95-152">규칙에 따라 선택적인 속성은 다음과 같이 필수로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6e95-152">A property that would be optional by convention can be configured to be required as follows:</span></span>

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="b6e95-153">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="b6e95-153">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?name=Required&highlight=4)]

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="b6e95-154">흐름 API</span><span class="sxs-lookup"><span data-stu-id="b6e95-154">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?name=Required&highlight=3-5)]

***
