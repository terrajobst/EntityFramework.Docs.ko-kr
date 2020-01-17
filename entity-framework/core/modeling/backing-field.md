---
title: 필드 지원-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 20cf9dc9b0d556f29680bce588bcbdc4ea48fa74
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124381"
---
# <a name="backing-fields"></a><span data-ttu-id="91d6c-102">지원 필드</span><span class="sxs-lookup"><span data-stu-id="91d6c-102">Backing Fields</span></span>

<span data-ttu-id="91d6c-103">지원 필드를 사용 하면 EF가 속성이 아닌 필드를 읽고 쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-103">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="91d6c-104">이는 응용 프로그램 코드를 통해 데이터에 대 한 액세스와 관련 하 여 의미 체계를 사용 하는 것을 제한 하기 위해 클래스의 캡슐화를 사용 하는 데 사용 되는 경우에 유용할 수 있지만, 값은 이러한 제한 사항 및 향상 된 기능을 사용 하지 않고 데이터베이스에서 읽거나 데이터베이스에 써야 합니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-104">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="basic-configuration"></a><span data-ttu-id="91d6c-105">기본 구성</span><span class="sxs-lookup"><span data-stu-id="91d6c-105">Basic configuration</span></span>

<span data-ttu-id="91d6c-106">규칙에 따라 다음 필드는 지정 된 속성에 대 한 지원 필드로 검색 됩니다 (우선 순위에 따라 나열 됨).</span><span class="sxs-lookup"><span data-stu-id="91d6c-106">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> 

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

<span data-ttu-id="91d6c-107">다음 샘플에서 `Url` 속성은 `_url` 지원 필드로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-107">In the following sample, the `Url` property is configured to have `_url` as its backing field:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="91d6c-108">지원 필드는 모델에 포함 된 속성에 대해서만 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-108">Note that backing fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="91d6c-109">모델에 포함 된 속성에 대 한 자세한 내용은 [속성을 제외 하 고 & 포함](included-properties.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="91d6c-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

<span data-ttu-id="91d6c-110">필드 이름이 위의 규칙에 해당 하지 않는 경우와 같이 지원 필드를 명시적으로 구성할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-110">You can also configure backing fields explicitly, e.g. if the field name doesn't correspond to the above conventions:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs?name=BackingField&highlight=5)]

## <a name="field-and-property-access"></a><span data-ttu-id="91d6c-111">필드 및 속성 액세스</span><span class="sxs-lookup"><span data-stu-id="91d6c-111">Field and property access</span></span>

<span data-ttu-id="91d6c-112">기본적으로 EF는 항상 지원 필드를 읽고 씁니다. 하나는 제대로 구성 된 것으로 가정 하 고는 속성을 사용 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-112">By default, EF will always read and write to the backing field - assuming one has been properly configured - and will never use the property.</span></span> <span data-ttu-id="91d6c-113">그러나 EF는 다른 액세스 패턴도 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-113">However, EF also supports other access patterns.</span></span> <span data-ttu-id="91d6c-114">예를 들어 다음 샘플에서는 구체화 하는 동안에만 지원 필드에 쓰도록 지시 하 고 다른 모든 경우에는 속성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-114">For example, the following sample instructs EF to write to the backing field only while materializing, and to use the property in all other cases:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs?name=BackingFieldAccessMode&highlight=6)]

<span data-ttu-id="91d6c-115">지원 되는 전체 옵션 집합은 [Propertyaccessmode 열거형](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="91d6c-115">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the complete set of supported options.</span></span>

> [!NOTE]
> <span data-ttu-id="91d6c-116">EF Core 3.0에서는 기본 속성 액세스 모드가 `PreferFieldDuringConstruction`에서 `PreferField`로 변경 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-116">With EF Core 3.0, the default property access mode changed from `PreferFieldDuringConstruction` to `PreferField`.</span></span>

## <a name="field-only-properties"></a><span data-ttu-id="91d6c-117">필드 전용 속성</span><span class="sxs-lookup"><span data-stu-id="91d6c-117">Field-only properties</span></span>

<span data-ttu-id="91d6c-118">엔터티 클래스에 해당 CLR 속성이 없는 모델에서 개념적 속성을 만들 수도 있습니다. 대신 필드를 사용 하 여 엔터티에 데이터를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-118">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="91d6c-119">이는 데이터가 엔터티의 CLR 형식이 아닌 변경 추적 장치에 저장 되는 [섀도 속성과](shadow-properties.md)는 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-119">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker, rather than in the entity's CLR type.</span></span> <span data-ttu-id="91d6c-120">필드 전용 속성은 엔터티 클래스가 속성 대신 메서드를 사용 하 여 값을 가져오거나 설정 하거나, 필드가 도메인 모델 (예: 기본 키)의 모든에서 노출 되지 않아야 하는 경우에 일반적으로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-120">Field-only properties are commonly used when the entity class uses methods instead of properties to get/set values, or in cases where fields shouldn't be exposed at all in the domain model (e.g. primary keys).</span></span>

<span data-ttu-id="91d6c-121">`Property(...)` API에 이름을 제공 하 여 필드 전용 속성을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-121">You can configure a field-only property by providing a name in the `Property(...)` API:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="91d6c-122">EF는 지정 된 이름의 CLR 속성 또는 속성이 없는 경우 필드를 찾으려고 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-122">EF will attempt to find a CLR property with the given name, or a field if a property isn't found.</span></span> <span data-ttu-id="91d6c-123">속성 또는 필드를 찾을 수 없는 경우에는 그림자 속성이 대신 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-123">If neither a property nor a field are found, a shadow property will be set up instead.</span></span>

<span data-ttu-id="91d6c-124">LINQ 쿼리에서는 필드 전용 속성을 참조 해야 할 수 있지만 이러한 필드는 일반적으로 private입니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-124">You may need to refer to a field-only property from LINQ queries, but such fields are typically private.</span></span> <span data-ttu-id="91d6c-125">LINQ 쿼리에서 `EF.Property(...)` 메서드를 사용 하 여 필드를 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="91d6c-125">You can use the `EF.Property(...)` method in a LINQ query to refer to the field:</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
