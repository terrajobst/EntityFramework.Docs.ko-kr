---
title: 대체 키 (Unique 제약 조건)-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197609"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="cebc6-102">대체 키(고유 제약 조건)</span><span class="sxs-lookup"><span data-stu-id="cebc6-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="cebc6-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="cebc6-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="cebc6-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="cebc6-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="cebc6-105">모델의 각 대체 키에 대해 unique 제약 조건이 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="cebc6-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="cebc6-106">규칙</span><span class="sxs-lookup"><span data-stu-id="cebc6-106">Conventions</span></span>

<span data-ttu-id="cebc6-107">규칙에 따라 대체 키에 대해 도입 된 인덱스 및 제약 조건의 이름이로 지정 `AK_<type name>_<property name>`됩니다.</span><span class="sxs-lookup"><span data-stu-id="cebc6-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="cebc6-108">복합 대체 키 `<property name>` 의 경우 속성 이름의 밑줄로 구분 된 목록이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="cebc6-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="cebc6-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="cebc6-109">Data Annotations</span></span>

<span data-ttu-id="cebc6-110">데이터 주석을 사용 하 여 Unique 제약 조건을 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="cebc6-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="cebc6-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="cebc6-111">Fluent API</span></span>

<span data-ttu-id="cebc6-112">흐름 API를 사용 하 여 대체 키에 대 한 인덱스 및 제약 조건 이름을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="cebc6-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
