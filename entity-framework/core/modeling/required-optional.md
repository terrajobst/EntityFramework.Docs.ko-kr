---
title: 필수 및 선택적 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: fd9e96e6f79965e63b07c21217edd004fd5c4d54
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197847"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="814af-102">필수 및 선택적 속성</span><span class="sxs-lookup"><span data-stu-id="814af-102">Required and Optional Properties</span></span>

<span data-ttu-id="814af-103">속성에 포함 `null`하기에 유효한 경우 속성은 선택 사항으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="814af-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="814af-104">이 `null` 속성에 유효한 값이 아닌 경우에는 필수 속성으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="814af-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

<span data-ttu-id="814af-105">관계형 데이터베이스 스키마에 매핑할 때 필수 속성은 null을 허용 하지 않는 열로 생성 되 고 선택적 속성은 null을 허용 하는 열로 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="814af-105">When mapping to a relational database schema, required properties are created as non-nullable columns, and optional properties are created as nullable columns.</span></span>

## <a name="conventions"></a><span data-ttu-id="814af-106">규칙</span><span class="sxs-lookup"><span data-stu-id="814af-106">Conventions</span></span>

<span data-ttu-id="814af-107">규칙에 따라 .NET 형식이 null을 포함할 수 있는 속성은 선택 사항으로 구성 되 고, .NET 형식은 null을 포함할 수 없는 속성은 필수로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="814af-107">By convention, a property whose .NET type can contain null will be configured as optional, whereas properties whose .NET type cannot contain null will be configured as required.</span></span> <span data-ttu-id="814af-108">예를 들어 .net 값 형식 (`int`, `decimal`, `bool`, 등)이 포함 된 모든 속성은 필수로 구성 되며 nullable .net 값 형식 (`int?`, `decimal?`, `bool?`등)이 있는 모든 속성은입니다. 선택 사항으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="814af-108">For example, all properties with .NET value types (`int`, `decimal`, `bool`, etc.) are configured as required, and all properties with nullable .NET value types (`int?`, `decimal?`, `bool?`, etc.) are configured as optional.</span></span>

<span data-ttu-id="814af-109">C#8에는 null이 포함 될 수 있는지 여부를 나타내는 [nullable 참조 형식](/dotnet/csharp/tutorials/nullable-reference-types)이라는 새로운 기능이 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="814af-109">C# 8 introduced a new feature called [nullable reference types](/dotnet/csharp/tutorials/nullable-reference-types), which allows reference types to be annotated, indicating whether it is valid for them to contain null or not.</span></span> <span data-ttu-id="814af-110">이 기능은 기본적으로 사용 하지 않도록 설정 되어 있으며, 사용 하도록 설정 된 경우 다음과 같은 방식으로 EF Core의 동작을 수정 합니다.</span><span class="sxs-lookup"><span data-stu-id="814af-110">This feature is disabled by default, and if enabled, it modifies EF Core's behavior in the following way:</span></span>

* <span data-ttu-id="814af-111">Nullable 참조 형식이 사용 하지 않도록 설정 된 경우 (기본값) .NET 참조 형식이 포함 된 모든 속성은 규칙에 따라 선택적으로 구성 `string`됩니다 (예:).</span><span class="sxs-lookup"><span data-stu-id="814af-111">If nullable reference types are disabled (the default), all properties with .NET reference types are configured as optional by convention (e.g. `string`).</span></span>
* <span data-ttu-id="814af-112">Nullable 참조 형식이 사용 되는 경우 해당 .net 형식의 C# null 허용 여부에 따라 속성이 구성 됩니다 .는 `string?` `string` 필수로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="814af-112">If nullable reference types are enabled, properties will be configured based on the C# nullability of their .NET type: `string?` will be configured as optional, whereas `string` will be configured as required.</span></span>

<span data-ttu-id="814af-113">다음 예제에서는 nullable 참조 기능을 사용 하지 않도록 설정 하 고 (기본값) 사용 하도록 설정 된 필수 및 선택적 속성이 포함 된 엔터티 형식을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="814af-113">The following example shows an entity type with required and optional properties, with the nullable reference feature disabled (the default) and enabled:</span></span>

# <a name="without-nullable-reference-types-defaulttabwithout-nrt"></a>[<span data-ttu-id="814af-114">Null을 허용 하지 않는 참조 형식 (기본값)</span><span class="sxs-lookup"><span data-stu-id="814af-114">Without nullable reference types (default)</span></span>](#tab/without-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/CustomerWithoutNullableReferenceTypes.cs?name=Customer&highlight=4-8)]

# <a name="with-nullable-reference-typestabwith-nrt"></a>[<span data-ttu-id="814af-115">Nullable 참조 형식 사용</span><span class="sxs-lookup"><span data-stu-id="814af-115">With nullable reference types</span></span>](#tab/with-nrt)

[!code-csharp[Main](../../../samples/core/Miscellaneous/NullableReferenceTypes/Customer.cs?name=Customer&highlight=4-6)]

***

<span data-ttu-id="814af-116">코드에 C# 표시 되는 null 허용 여부를 EF Core의 모델 및 데이터베이스로 전달 하 고 흐름 API 또는 데이터 주석을 사용 하 여 동일한 개념을 두 번 표현 하므로 nullable 참조 형식을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="814af-116">Using nullable reference types is recommended since it flows the nullability expressed in C# code to EF Core's model and to the database, and obviates the use of the Fluent API or Data Annotations to express the same concept twice.</span></span>

> [!NOTE]
> <span data-ttu-id="814af-117">기존 프로젝트에서 nullable 참조 형식을 사용 하도록 설정 하는 경우 주의 해야 합니다. 이전에 선택적으로 구성 된 참조 형식 속성은 nullable로 명시적으로 주석 처리 되지 않는 한 이제 필수로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="814af-117">Exercise caution when enabling nullable reference types on an existing project: reference type properties which were previously configured as optional will now be configured as required, unless they are explicitly annotated to be nullable.</span></span> <span data-ttu-id="814af-118">관계형 데이터베이스 스키마를 관리할 때이로 인해 데이터베이스 열의 null 허용 여부를 변경 하는 마이그레이션이 생성 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="814af-118">When managing a relational database schema, this may cause migrations to be generated which alter the database column's nullability.</span></span>

<span data-ttu-id="814af-119">Nullable 참조 형식 및 EF Core와 함께 사용 하는 방법에 대 한 자세한 내용은 [이 기능에 대 한 전용 설명서 페이지를 참조](xref:core/miscellaneous/nullable-reference-types)하세요.</span><span class="sxs-lookup"><span data-stu-id="814af-119">For more information on nullable reference types and how to use them with EF Core, [see the dedicated documentation page for this feature](xref:core/miscellaneous/nullable-reference-types).</span></span>

## <a name="configuration"></a><span data-ttu-id="814af-120">Configuration</span><span class="sxs-lookup"><span data-stu-id="814af-120">Configuration</span></span>

<span data-ttu-id="814af-121">규칙에 따라 선택적인 속성은 다음과 같이 필수로 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="814af-121">A property that would be optional by convention can be configured to be required as follows:</span></span>

# <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="814af-122">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="814af-122">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]

# <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="814af-123">흐름 API</span><span class="sxs-lookup"><span data-stu-id="814af-123">Fluent API</span></span>](#tab/fluent-api) 

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

***