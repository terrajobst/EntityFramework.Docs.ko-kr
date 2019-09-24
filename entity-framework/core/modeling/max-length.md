---
title: 최대 길이-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197218"
---
# <a name="maximum-length"></a><span data-ttu-id="4cbec-102">최대 길이</span><span class="sxs-lookup"><span data-stu-id="4cbec-102">Maximum Length</span></span>

<span data-ttu-id="4cbec-103">최대 길이를 구성 하면 지정 된 속성에 사용할 적절 한 데이터 형식에 대 한 힌트를 데이터 저장소에 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="4cbec-104">최대 길이는 `string` 및 `byte[]`와 같은 배열 데이터 형식에만 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="4cbec-105">Entity Framework는 공급자에 게 데이터를 전달 하기 전에 최대 길이에 대 한 유효성 검사를 수행 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="4cbec-106">해당 하는 경우 공급자 또는 데이터 저장소에서 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="4cbec-107">예를 들어 SQL Server를 대상으로 지정 하는 경우 최대 길이를 초과 하면 기본 열의 데이터 형식에 따라 초과 되는 데이터가 저장 되는 것을 허용 하지 않기 때문에 예외가 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="4cbec-108">규칙</span><span class="sxs-lookup"><span data-stu-id="4cbec-108">Conventions</span></span>

<span data-ttu-id="4cbec-109">규칙에 따라 속성에 대 한 적절 한 데이터 형식을 선택 하기 위해 데이터베이스 공급자에 게 남아 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="4cbec-110">길이가 인 속성의 경우 데이터베이스 공급자가 일반적으로 가장 긴 데이터 길이를 허용 하는 데이터 형식을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="4cbec-111">예를 들어 Microsoft SQL Server는 속성 `nvarchar(max)` ( `string` 또는 `nvarchar(450)` 열이 키로 사용 되는 경우)에를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4cbec-112">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="4cbec-112">Data Annotations</span></span>

<span data-ttu-id="4cbec-113">데이터 주석을 사용 하 여 속성의 최대 길이를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="4cbec-114">이 예에서는이 `nvarchar(500)` SQL Server 대상으로 지정 하면 데이터 형식이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="4cbec-115">Fluent API</span><span class="sxs-lookup"><span data-stu-id="4cbec-115">Fluent API</span></span>

<span data-ttu-id="4cbec-116">흐름 API를 사용 하 여 속성에 대 한 최대 길이를 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="4cbec-117">이 예에서는이 `nvarchar(500)` SQL Server 대상으로 지정 하면 데이터 형식이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4cbec-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
