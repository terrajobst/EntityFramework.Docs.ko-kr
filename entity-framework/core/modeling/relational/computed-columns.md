---
title: 계산 열-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655922"
---
# <a name="computed-columns"></a><span data-ttu-id="f3cd8-102">계산된 열</span><span class="sxs-lookup"><span data-stu-id="f3cd8-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="f3cd8-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="f3cd8-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="f3cd8-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="f3cd8-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="f3cd8-105">계산 열은 해당 값이 데이터베이스에서 계산 되는 열입니다.</span><span class="sxs-lookup"><span data-stu-id="f3cd8-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="f3cd8-106">계산 열은 테이블의 다른 열을 사용 하 여 해당 값을 계산할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f3cd8-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="f3cd8-107">규칙</span><span class="sxs-lookup"><span data-stu-id="f3cd8-107">Conventions</span></span>

<span data-ttu-id="f3cd8-108">규칙에 따라 계산 열은 모델에 생성 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f3cd8-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f3cd8-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="f3cd8-109">Data Annotations</span></span>

<span data-ttu-id="f3cd8-110">데이터 주석으로 계산 열을 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="f3cd8-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="f3cd8-111">흐름 API</span><span class="sxs-lookup"><span data-stu-id="f3cd8-111">Fluent API</span></span>

<span data-ttu-id="f3cd8-112">흐름 API를 사용 하 여 속성이 계산 열에 매핑되도록 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f3cd8-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
