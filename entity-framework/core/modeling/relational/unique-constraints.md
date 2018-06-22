---
title: 대체 키 (고유 제약 조건)-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052793"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="3f48e-102">대체 키 (고유 제약 조건)</span><span class="sxs-lookup"><span data-stu-id="3f48e-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="3f48e-103">이 섹션의 구성을 일반적 관계형 데이터베이스에 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f48e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="3f48e-104">여기에 표시 된 확장 메서드를 사용할 수 있는 관계형 데이터베이스 공급자를 설치할 때 (공유 인해 *Microsoft.EntityFrameworkCore.Relational* 패키지).</span><span class="sxs-lookup"><span data-stu-id="3f48e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="3f48e-105">모델의 각 대체 키에 대 한 unique 제약 조건을 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="3f48e-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="3f48e-106">규칙</span><span class="sxs-lookup"><span data-stu-id="3f48e-106">Conventions</span></span>

<span data-ttu-id="3f48e-107">규칙에 따라 인덱스와 대체 키에 대 한 소개 하는 제약 조건 이름은 `AK_<type name>_<property name>`합니다.</span><span class="sxs-lookup"><span data-stu-id="3f48e-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="3f48e-108">대체 복합 키에 대 한 `<property name>` 속성 이름의 밑줄 구분 된 목록이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3f48e-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3f48e-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="3f48e-109">Data Annotations</span></span>

<span data-ttu-id="3f48e-110">데이터 주석을 사용 하 여 unique 제약 조건은 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f48e-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3f48e-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="3f48e-111">Fluent API</span></span>

<span data-ttu-id="3f48e-112">대체 키에 대 한 인덱스 및 제약 조건 이름을 구성 하려면 Fluent API를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3f48e-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
