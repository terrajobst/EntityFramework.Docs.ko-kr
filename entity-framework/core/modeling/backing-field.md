---
title: 지원 필드-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 79221b6f7968675ff10f80d5df181b674b6a20c9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994095"
---
# <a name="backing-fields"></a><span data-ttu-id="2d4c9-102">지원 필드</span><span class="sxs-lookup"><span data-stu-id="2d4c9-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="2d4c9-103">이 기능은 EF Core 1.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="2d4c9-104">지원 필드 EF 읽거나 속성 보다는 필드에 쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="2d4c9-105">응용 프로그램 코드에 클래스에 캡슐화의 사용을 제한 하거나 데이터 액세스 관련 의미 체계를 개선에 사용 되 고 있지만 값은 될 파일에서 읽거나 이러한 제한을 사용 하지 않고 데이터베이스에 기록 하는 경우 유용할 수 있습니다이 / 향상 된 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="2d4c9-106">규칙</span><span class="sxs-lookup"><span data-stu-id="2d4c9-106">Conventions</span></span>

<span data-ttu-id="2d4c9-107">규칙에 따라 다음 필드를 지정된 된 속성 (우선 순위 대로 나열 됨)에 대 한 필드를 백업으로 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="2d4c9-108">필드는 모델에 포함 되는 속성에 대 한만 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="2d4c9-109">속성은 모델에 포함 하는 자세한 내용은 [속성 포함 및 제외](included-properties.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="2d4c9-110">지원 필드를 구성 하는 경우 EF는 데이터베이스 (사용 하는 대신 속성 setter)에서 엔터티 인스턴스를 구체화 하는 경우 해당 필드에 직접 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="2d4c9-111">EF 읽기 또는 다른 시간에 값을 작성 하는 경우 가능 하면 속성이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="2d4c9-112">예를 들어, EF를 속성에 대 한 값을 업데이트 해야 하는 경우 정의 된 경우 속성 setter를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="2d4c9-113">속성은 읽기 전용 필드를 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2d4c9-114">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="2d4c9-114">Data Annotations</span></span>

<span data-ttu-id="2d4c9-115">데이터 주석을 사용한 지원 필드를 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2d4c9-116">Fluent API</span><span class="sxs-lookup"><span data-stu-id="2d4c9-116">Fluent API</span></span>

<span data-ttu-id="2d4c9-117">속성에 대 한 지원 필드를 구성 하는 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="2d4c9-118">필드를 사용 하는 경우 제어</span><span class="sxs-lookup"><span data-stu-id="2d4c9-118">Controlling when the field is used</span></span>

<span data-ttu-id="2d4c9-119">EF는 필드 또는 속성을 사용 하는 경우 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="2d4c9-120">참조 된 [PropertyAccessMode 열거형](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) 지원 되는 옵션에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="2d4c9-121">속성 없이 필드</span><span class="sxs-lookup"><span data-stu-id="2d4c9-121">Fields without a property</span></span>

<span data-ttu-id="2d4c9-122">또한 엔터티 클래스에 해당 하는 CLR 속성이 없는 이지만 대신 엔터티에서 데이터를 저장할 필드를 사용 하는 모델의 개념적 속성을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="2d4c9-123">다릅니다 [섀도 속성](shadow-properties.md), 변경 추적 장치에서 데이터를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="2d4c9-124">이 일반적으로 사용할 엔터티 클래스의 메서드를 사용 하 여 get 값을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="2d4c9-125">EF에서 필드의 이름을 지정할 수 있습니다는 `Property(...)` API.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="2d4c9-126">지정 된 이름의 속성이 없는 경우 EF는 필드에 대해 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="2d4c9-127">속성 필드 이름 외에 다른 이름을 제공할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="2d4c9-128">이 이름은 다음 모델을 만들 때 사용 되는, 가장 주목할 만한 데이터베이스에 매핑되는 열 이름에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="2d4c9-129">엔터티 클래스의 속성이 없는 경우 사용할 수 있습니다는 `EF.Property(...)` 개념적 모델의 일부인 속성을 참조 하는 LINQ 쿼리에서 메서드.</span><span class="sxs-lookup"><span data-stu-id="2d4c9-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
