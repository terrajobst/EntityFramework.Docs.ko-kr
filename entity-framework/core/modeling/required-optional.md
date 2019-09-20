---
title: 필수/선택적 속성-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149149"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="90cc3-102">필수 및 선택적 속성</span><span class="sxs-lookup"><span data-stu-id="90cc3-102">Required and Optional Properties</span></span>

<span data-ttu-id="90cc3-103">속성에 포함 `null`하기에 유효한 경우 속성은 선택 사항으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90cc3-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="90cc3-104">이 `null` 속성에 유효한 값이 아닌 경우에는 필수 속성으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90cc3-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="90cc3-105">규칙</span><span class="sxs-lookup"><span data-stu-id="90cc3-105">Conventions</span></span>

<span data-ttu-id="90cc3-106">규칙에 따라 .net 형식이 null을 포함할 수 있는 속성은 선택적 (`string`, `int?`, `byte[]`등)으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90cc3-106">By convention, a property whose .NET type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="90cc3-107">CLR 형식이 null을 포함할 수 없는 속성은 필수 (`int`, `decimal`, `bool`등)로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90cc3-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="90cc3-108">.NET 형식을 null을 포함할 수 없는 속성은 선택 사항으로 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="90cc3-108">A property whose .NET type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="90cc3-109">속성은 항상 Entity Framework에 필요한 것으로 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="90cc3-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="90cc3-110">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="90cc3-110">Data Annotations</span></span>

<span data-ttu-id="90cc3-111">데이터 주석을 사용 하 여 속성이 필요 함을 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="90cc3-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="90cc3-112">Fluent API</span><span class="sxs-lookup"><span data-stu-id="90cc3-112">Fluent API</span></span>

<span data-ttu-id="90cc3-113">흐름 API를 사용 하 여 속성이 필요 함을 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="90cc3-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

