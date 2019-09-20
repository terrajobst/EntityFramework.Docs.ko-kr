---
title: 데이터 형식-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: d667cbcb821e321faed36d097b531c7c55b81248
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149168"
---
# <a name="data-types"></a><span data-ttu-id="c1743-102">데이터 형식</span><span class="sxs-lookup"><span data-stu-id="c1743-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="c1743-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="c1743-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="c1743-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="c1743-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="c1743-105">데이터 형식은 속성이 매핑되는 열의 데이터베이스 관련 유형을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="c1743-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="c1743-106">규칙</span><span class="sxs-lookup"><span data-stu-id="c1743-106">Conventions</span></span>

<span data-ttu-id="c1743-107">규칙에 따라 데이터베이스 공급자는 속성의 .NET 형식을 기반으로 데이터 형식을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1743-107">By convention, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="c1743-108">구성 된 [최대 길이](../max-length.md)와 같은 다른 메타 데이터 (속성이 기본 키의 일부 인지 여부 등)도 고려 합니다.</span><span class="sxs-lookup"><span data-stu-id="c1743-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="c1743-109">예를 들어 `DateTime` 속성에 `datetime2(7)` 는를 사용 하 `nvarchar(max)` 고 `string` 속성 (또는 `nvarchar(450)` `string` 키로 사용 되는 속성의 경우)에는를 사용 SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1743-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c1743-110">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="c1743-110">Data Annotations</span></span>

<span data-ttu-id="c1743-111">데이터 주석을 사용 하 여 열에 정확한 데이터 형식을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1743-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="c1743-112">예를 들어 다음 코드는 `Url` 의 최대 `200` 길이 `Rating` 와 소수 자릿수가의 전체 자릿수 `5` `2`와 소수 자릿수가 인 비유니코드 문자열로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c1743-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="c1743-113">Fluent API</span><span class="sxs-lookup"><span data-stu-id="c1743-113">Fluent API</span></span>

<span data-ttu-id="c1743-114">흐름 API를 사용 하 여 열에 대해 동일한 데이터 형식을 지정할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c1743-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
