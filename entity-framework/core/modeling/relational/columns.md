---
title: 열 매핑-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197208"
---
# <a name="column-mapping"></a><span data-ttu-id="8a186-102">열 매핑</span><span class="sxs-lookup"><span data-stu-id="8a186-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="8a186-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a186-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="8a186-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="8a186-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="8a186-105">열 매핑은 데이터베이스에서 쿼리하고 저장할 열 데이터를 식별 합니다.</span><span class="sxs-lookup"><span data-stu-id="8a186-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="8a186-106">규칙</span><span class="sxs-lookup"><span data-stu-id="8a186-106">Conventions</span></span>

<span data-ttu-id="8a186-107">규칙에 따라 각 속성은 속성과 이름이 같은 열에 매핑되도록 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="8a186-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="8a186-108">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="8a186-108">Data Annotations</span></span>

<span data-ttu-id="8a186-109">데이터 주석을 사용 하 여 속성이 매핑되는 열을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a186-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="8a186-110">Fluent API</span><span class="sxs-lookup"><span data-stu-id="8a186-110">Fluent API</span></span>

<span data-ttu-id="8a186-111">흐름 API를 사용 하 여 속성이 매핑되는 열을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="8a186-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
