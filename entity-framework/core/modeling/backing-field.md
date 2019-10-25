---
title: 필드 지원-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
uid: core/modeling/backing-field
ms.openlocfilehash: 288440a4494117fe59d27187e24424c4d2fd44ab
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811881"
---
# <a name="backing-fields"></a><span data-ttu-id="482d0-102">지원 필드</span><span class="sxs-lookup"><span data-stu-id="482d0-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="482d0-103">이 기능은 EF Core 1.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="482d0-104">지원 필드를 사용 하면 EF가 속성이 아닌 필드를 읽고 쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="482d0-105">이는 클래스의 캡슐화를 사용 하 여 응용 프로그램 코드를 통해 데이터에 대 한 액세스를 제한 하는 기능을 사용 하는 것을 제한 하는 데 사용 되는 경우에 유용할 수 있지만, 이러한 제한을 사용 하지 않고 값을 데이터베이스에서 읽거나 데이터베이스에 써야 합니다. 된.</span><span class="sxs-lookup"><span data-stu-id="482d0-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="482d0-106">규칙</span><span class="sxs-lookup"><span data-stu-id="482d0-106">Conventions</span></span>

<span data-ttu-id="482d0-107">규칙에 따라 다음 필드는 지정 된 속성에 대 한 지원 필드로 검색 됩니다 (우선 순위에 따라 나열 됨).</span><span class="sxs-lookup"><span data-stu-id="482d0-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="482d0-108">모델에 포함 된 속성에 대해서만 필드가 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="482d0-109">모델에 포함 된 속성에 대 한 자세한 내용은 [속성을 제외 하 고 & 포함](included-properties.md)을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="482d0-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/BackingField.cs#Sample)]

<span data-ttu-id="482d0-110">지원 필드를 구성 하면 EF는 속성 setter를 사용 하는 대신 데이터베이스에서 엔터티 인스턴스를 구체화 하는 경우 해당 필드에 직접 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="482d0-111">EF가 다른 시간에 값을 읽거나 써야 하는 경우 가능 하면 속성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="482d0-112">예를 들어 EF가 속성의 값을 업데이트 해야 하는 경우 속성 setter (정의 된 경우)를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="482d0-113">속성이 읽기 전용 이면 필드에 기록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="482d0-114">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="482d0-114">Data Annotations</span></span>

<span data-ttu-id="482d0-115">데이터 주석으로는 지원 필드를 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="482d0-116">흐름 API</span><span class="sxs-lookup"><span data-stu-id="482d0-116">Fluent API</span></span>

<span data-ttu-id="482d0-117">흐름 API를 사용 하 여 속성에 대 한 지원 필드를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="482d0-118">필드가 사용 되는 경우 제어</span><span class="sxs-lookup"><span data-stu-id="482d0-118">Controlling when the field is used</span></span>

<span data-ttu-id="482d0-119">EF에서 필드 또는 속성을 사용 하는 경우을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="482d0-120">지원 되는 옵션은 [Propertyaccessmode 열거형](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="482d0-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="482d0-121">속성이 없는 필드</span><span class="sxs-lookup"><span data-stu-id="482d0-121">Fields without a property</span></span>

<span data-ttu-id="482d0-122">엔터티 클래스에 해당 CLR 속성이 없는 모델에서 개념적 속성을 만들 수도 있습니다. 대신 필드를 사용 하 여 엔터티에 데이터를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="482d0-123">이는 데이터가 변경 추적 장치에 저장 되는 [섀도 속성과](shadow-properties.md)는 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="482d0-124">일반적으로 엔터티 클래스에서 메서드를 사용 하 여 값을 가져오거나 설정 하는 경우에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="482d0-125">`Property(...)` API의 필드 이름을 EF에 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="482d0-126">지정 된 이름의 속성이 없으면 EF에서 필드를 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="482d0-127">엔터티 클래스에 속성이 없는 경우 LINQ 쿼리에서 `EF.Property(...)` 메서드를 사용 하 여 개념적으로 모델의 일부인 속성을 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="482d0-127">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "_validatedUrl"));
```
