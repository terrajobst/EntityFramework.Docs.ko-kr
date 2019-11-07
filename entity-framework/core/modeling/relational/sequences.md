---
title: 시퀀스-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656109"
---
# <a name="sequences"></a><span data-ttu-id="84f6f-102">시퀀스</span><span class="sxs-lookup"><span data-stu-id="84f6f-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="84f6f-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="84f6f-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="84f6f-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="84f6f-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="84f6f-105">시퀀스는 데이터베이스에 순차 숫자 값을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="84f6f-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="84f6f-106">시퀀스는 특정 테이블과 연결 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="84f6f-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="84f6f-107">규칙</span><span class="sxs-lookup"><span data-stu-id="84f6f-107">Conventions</span></span>

<span data-ttu-id="84f6f-108">규칙에 따라 시퀀스는 모델에 추가 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="84f6f-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="84f6f-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="84f6f-109">Data Annotations</span></span>

<span data-ttu-id="84f6f-110">데이터 주석을 사용 하 여 시퀀스를 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="84f6f-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="84f6f-111">흐름 API</span><span class="sxs-lookup"><span data-stu-id="84f6f-111">Fluent API</span></span>

<span data-ttu-id="84f6f-112">흐름 API를 사용 하 여 모델에서 시퀀스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="84f6f-112">You can use the Fluent API to create a sequence in the model.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

<span data-ttu-id="84f6f-113">또한 해당 스키마, 시작 값, 증가값 등 시퀀스의 추가 측면을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="84f6f-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

<span data-ttu-id="84f6f-114">시퀀스가 도입 되 면 모델에서 속성 값을 생성 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="84f6f-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="84f6f-115">예를 들어 [기본값](default-values.md) 을 사용 하 여 시퀀스에서 다음 값을 삽입할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="84f6f-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
