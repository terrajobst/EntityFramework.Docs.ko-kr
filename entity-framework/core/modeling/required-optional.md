---
title: 필수/선택적 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929812"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="85807-102">필수 및 선택적 속성</span><span class="sxs-lookup"><span data-stu-id="85807-102">Required and Optional Properties</span></span>

<span data-ttu-id="85807-103">속성에 포함 하려면 유효한 경우 선택적으로 간주 됩니다 `null`합니다.</span><span class="sxs-lookup"><span data-stu-id="85807-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="85807-104">경우 `null` 필수 속성으로 간주 됩니다 속성에 할당할 올바른 값이 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="85807-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="85807-105">규칙</span><span class="sxs-lookup"><span data-stu-id="85807-105">Conventions</span></span>

<span data-ttu-id="85807-106">규칙에 따라 CLR 형식이 null을 포함할 수 있습니다 속성 구성지 것입니다 옵션으로 (`string`, `int?`, `byte[]`등.).</span><span class="sxs-lookup"><span data-stu-id="85807-106">By convention, a property whose CLR type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="85807-107">CLR 형식이 null을 포함할 수 없습니다. 속성을 구성할 수는 필요에 따라 (`int`, `decimal`, `bool`등.).</span><span class="sxs-lookup"><span data-stu-id="85807-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="85807-108">CLR 형식이 null을 포함할 수 없습니다. 속성은 선택적으로 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="85807-108">A property whose CLR type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="85807-109">항상 Entity Framework에 필요한 속성을 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="85807-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="85807-110">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="85807-110">Data Annotations</span></span>

<span data-ttu-id="85807-111">필수 속성 임을 데이터 주석을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="85807-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="85807-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="85807-112">Fluent API</span></span>

<span data-ttu-id="85807-113">필수 속성 임을 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="85807-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

