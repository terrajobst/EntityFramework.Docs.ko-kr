---
title: 기본 스키마-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655976"
---
# <a name="default-schema"></a><span data-ttu-id="65ff7-102">기본 스키마</span><span class="sxs-lookup"><span data-stu-id="65ff7-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="65ff7-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="65ff7-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="65ff7-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="65ff7-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="65ff7-105">기본 스키마는 해당 개체에 대해 스키마가 명시적으로 구성 되지 않은 경우 개체가 만들어지는 데이터베이스 스키마입니다.</span><span class="sxs-lookup"><span data-stu-id="65ff7-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="65ff7-106">규칙</span><span class="sxs-lookup"><span data-stu-id="65ff7-106">Conventions</span></span>

<span data-ttu-id="65ff7-107">규칙에 따라 데이터베이스 공급자가 가장 적합 한 기본 스키마를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="65ff7-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="65ff7-108">예를 들어 Microsoft SQL Server는 `dbo` 스키마를 사용 하 고 SQLite는 스키마를 사용 하지 않습니다. 즉, 스키마는 SQLite에서 지원 되지 않기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="65ff7-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="65ff7-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="65ff7-109">Data Annotations</span></span>

<span data-ttu-id="65ff7-110">데이터 주석을 사용 하 여 기본 스키마를 설정할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="65ff7-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="65ff7-111">흐름 API</span><span class="sxs-lookup"><span data-stu-id="65ff7-111">Fluent API</span></span>

<span data-ttu-id="65ff7-112">흐름 API를 사용 하 여 기본 스키마를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="65ff7-112">You can use the Fluent API to specify a default schema.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
