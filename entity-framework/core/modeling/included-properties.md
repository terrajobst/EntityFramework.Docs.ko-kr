---
title: 속성을 제외 하 고 & 포함-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197418"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="a8e44-102">속성 포함 및 제외</span><span class="sxs-lookup"><span data-stu-id="a8e44-102">Including & Excluding Properties</span></span>

<span data-ttu-id="a8e44-103">모델에 속성을 포함 하면 EF는 해당 속성에 대 한 메타 데이터를 포함 하 고 데이터베이스의 값을 읽고 쓰는 것을 시도 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8e44-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="a8e44-104">규칙</span><span class="sxs-lookup"><span data-stu-id="a8e44-104">Conventions</span></span>

<span data-ttu-id="a8e44-105">규칙에 따라 getter 및 setter가 있는 공용 속성은 모델에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a8e44-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a8e44-106">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="a8e44-106">Data Annotations</span></span>

<span data-ttu-id="a8e44-107">데이터 주석을 사용 하 여 모델에서 속성을 제외할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a8e44-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="a8e44-108">Fluent API</span><span class="sxs-lookup"><span data-stu-id="a8e44-108">Fluent API</span></span>

<span data-ttu-id="a8e44-109">흐름 API를 사용 하 여 모델에서 속성을 제외할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a8e44-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
