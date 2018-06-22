---
title: EF 핵심 필드-백업
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a628795e-64df-4f24-a5e8-76bc261e7ed8
ms.technology: entity-framework-core
uid: core/modeling/backing-field
ms.openlocfilehash: e95417b3368d09a64f9ec02ae40c38dc770c2e6b
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053463"
---
# <a name="backing-fields"></a><span data-ttu-id="199a1-102">지원 필드</span><span class="sxs-lookup"><span data-stu-id="199a1-102">Backing Fields</span></span>

> [!NOTE]  
> <span data-ttu-id="199a1-103">이 기능은 EF 코어 1.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-103">This feature is new in EF Core 1.1.</span></span>

<span data-ttu-id="199a1-104">지원 필드는 읽기 및/또는 속성 보다는 필드에 쓸 EF를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-104">Backing fields allow EF to read and/or write to a field rather than a property.</span></span> <span data-ttu-id="199a1-105">응용 프로그램 코드에서 클래스에 캡슐화의 사용이 제한 및/또는 향상 의미 체계 데이터에 대 한 액세스 주위에 사용 되 고 있지만 값에서 읽은 되거나 이러한 제한을 사용 하지 않고 데이터베이스에 기록 되어야 하는 경우 유용할 수 있습니다 / 향상 된 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-105">This can be useful when encapsulation in the class is being used to restrict the use of and/or enhance the semantics around access to the data by application code, but the value should be read from and/or written to the database without using those restrictions/enhancements.</span></span>

## <a name="conventions"></a><span data-ttu-id="199a1-106">규칙</span><span class="sxs-lookup"><span data-stu-id="199a1-106">Conventions</span></span>

<span data-ttu-id="199a1-107">일반적으로 다음 필드를 필드 (우선 순위 대로 나열) 지정된 된 속성에 대 한 백업으로 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-107">By convention, the following fields will be discovered as backing fields for a given property (listed in precedence order).</span></span> <span data-ttu-id="199a1-108">필드는 모델에 포함 된 속성에 대 한만 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-108">Fields are only discovered for properties that are included in the model.</span></span> <span data-ttu-id="199a1-109">모델에 포함 된 속성에 자세한 내용은 참조 하십시오. [속성 제외 및 포함 하 여](included-properties.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-109">For more information on which properties are included in the model, see [Including & Excluding Properties](included-properties.md).</span></span>

* `_<camel-cased property name>`
* `_<property name>`
* `m_<camel-cased property name>`
* `m_<property name>`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/BackingField.cs#Sample)]

<span data-ttu-id="199a1-110">지원 필드로 구성 된 경우 EF (대신 데이터베이스 속성 setter를 사용 하 여)에서 엔터티 인스턴스를 구체화할 때 해당 필드에 직접 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-110">When a backing field is configured, EF will write directly to that field when materializing entity instances from the database (rather than using the property setter).</span></span> <span data-ttu-id="199a1-111">EF를 읽기 또는 다른 시간에 값을 기록 하는 경우 가능 하면 속성이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-111">If EF needs to read or write the value at other times, it will use the property if possible.</span></span> <span data-ttu-id="199a1-112">예를 들어 EF를 속성에 대 한 값을 업데이트 하는 경우 정의 된 경우 속성 setter를 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-112">For example, if EF needs to update the value for a property, it will use the property setter if one is defined.</span></span> <span data-ttu-id="199a1-113">속성은 읽기 전용 필드에 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-113">If the property is read-only, then it will write to the field.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="199a1-114">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="199a1-114">Data Annotations</span></span>

<span data-ttu-id="199a1-115">데이터 주석을 사용한 백업 하는 필드를 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-115">Backing fields cannot be configured with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="199a1-116">Fluent API</span><span class="sxs-lookup"><span data-stu-id="199a1-116">Fluent API</span></span>

<span data-ttu-id="199a1-117">속성에 대 한 지원 필드를 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-117">You can use the Fluent API to configure a backing field for a property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingField.cs#Sample)]

### <a name="controlling-when-the-field-is-used"></a><span data-ttu-id="199a1-118">필드가 사용 되는 경우를 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-118">Controlling when the field is used</span></span>

<span data-ttu-id="199a1-119">EF 필드 또는 속성을 사용 하는 경우를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-119">You can configure when EF uses the field or property.</span></span> <span data-ttu-id="199a1-120">참조는 [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) 지원 되는 옵션에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-120">See the [PropertyAccessMode enum](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.propertyaccessmode) for the supported options.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldAccessMode.cs#Sample)]

### <a name="fields-without-a-property"></a><span data-ttu-id="199a1-121">속성이 없는 필드</span><span class="sxs-lookup"><span data-stu-id="199a1-121">Fields without a property</span></span>

<span data-ttu-id="199a1-122">또한 엔터티 클래스에서 해당 CLR 속성이 되어 있지 않지만 대신 필드를 사용 하 여 엔터티의 데이터를 저장 하는 모델의 개념적 속성을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-122">You can also create a conceptual property in your model that does not have a corresponding CLR property in the entity class, but instead uses a field to store the data in the entity.</span></span> <span data-ttu-id="199a1-123">다릅니다 [그림자 속성](shadow-properties.md), 데이터가 변경 추적 장치에 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-123">This is different from [Shadow Properties](shadow-properties.md), where the data is stored in the change tracker.</span></span> <span data-ttu-id="199a1-124">이 엔터티 클래스의 메서드를 사용 하 여 get 값을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-124">This would typically be used if the entity class uses methods to get/set values.</span></span>

<span data-ttu-id="199a1-125">에 있는 필드의 이름을 EF를 지정할 수 있습니다는 `Property(...)` API입니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-125">You can give EF the name of the field in the `Property(...)` API.</span></span> <span data-ttu-id="199a1-126">지정 된 이름의 속성이 없습니다 EF 필드에 대해 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-126">If there is no property with the given name, then EF will look for a field.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldNoProperty.cs#Sample)]

<span data-ttu-id="199a1-127">또한 속성 필드 이름 외에 다른 이름을 제공할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-127">You can also choose to give the property a name, other than the field name.</span></span> <span data-ttu-id="199a1-128">이 이름은 다음 모델을 만들 때 사용 되는, 데이터베이스에 매핑되는 열 이름에 대 한 가장 주목할 만한 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="199a1-128">This name is then used when creating the model, most notably it will be used for the column name that is mapped to in the database.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/BackingFieldConceptualProperty.cs#Sample)]

<span data-ttu-id="199a1-129">엔터티 클래스에 없는 속성 인 경우 사용할 수 있습니다는 `EF.Property(...)` 개념적 모델의 일부인 속성을 참조 하는 LINQ 쿼리에서 메서드.</span><span class="sxs-lookup"><span data-stu-id="199a1-129">When there is no property in the entity class, you can use the `EF.Property(...)` method in a LINQ query to refer to the property that is conceptually part of the model.</span></span>

``` csharp
var blogs = db.blogs.OrderBy(b => EF.Property<string>(b, "Url"));
```
