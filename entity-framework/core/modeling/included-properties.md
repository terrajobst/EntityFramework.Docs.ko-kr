---
title: 속성-EF Core 포함 및 제외
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929826"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="4c0fc-102">속성 포함 및 제외</span><span class="sxs-lookup"><span data-stu-id="4c0fc-102">Including & Excluding Properties</span></span>

<span data-ttu-id="4c0fc-103">모델의 속성을 포함 하 여 EF에 해당 속성에 대 한 메타 데이터를 의미 하 고 읽기 및 쓰기를 데이터베이스 값을 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c0fc-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="4c0fc-104">규칙</span><span class="sxs-lookup"><span data-stu-id="4c0fc-104">Conventions</span></span>

<span data-ttu-id="4c0fc-105">규칙에 따라 공용 속성 getter 및 setter를 사용 하 여 모델에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4c0fc-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4c0fc-106">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="4c0fc-106">Data Annotations</span></span>

<span data-ttu-id="4c0fc-107">데이터 주석 모델에서 속성을 제외를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c0fc-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="4c0fc-108">Fluent API</span><span class="sxs-lookup"><span data-stu-id="4c0fc-108">Fluent API</span></span>

<span data-ttu-id="4c0fc-109">모델에서 속성을 제외 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c0fc-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
