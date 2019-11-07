---
title: Foreign Key 제약 조건-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655993"
---
# <a name="foreign-key-constraints"></a><span data-ttu-id="34879-102">외래 키 제약 조건</span><span class="sxs-lookup"><span data-stu-id="34879-102">Foreign Key Constraints</span></span>

> [!NOTE]  
> <span data-ttu-id="34879-103">이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="34879-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="34879-104">여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).</span><span class="sxs-lookup"><span data-stu-id="34879-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="34879-105">외래 키 제약 조건은 모델의 각 관계에 대해 도입 됩니다.</span><span class="sxs-lookup"><span data-stu-id="34879-105">A foreign key constraint is introduced for each relationship in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="34879-106">규칙</span><span class="sxs-lookup"><span data-stu-id="34879-106">Conventions</span></span>

<span data-ttu-id="34879-107">규칙에 따라 foreign key 제약 조건의 이름은 `FK_<dependent type name>_<principal type name>_<foreign key property name>`입니다.</span><span class="sxs-lookup"><span data-stu-id="34879-107">By convention, foreign key constraints are named `FK_<dependent type name>_<principal type name>_<foreign key property name>`.</span></span> <span data-ttu-id="34879-108">복합 외래 키의 경우에는 쉼표로 구분 된 외래 키 속성 이름 목록이 `<foreign key property name>` 됩니다.</span><span class="sxs-lookup"><span data-stu-id="34879-108">For composite foreign keys `<foreign key property name>` becomes an underscore separated list of foreign key property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="34879-109">데이터 주석</span><span class="sxs-lookup"><span data-stu-id="34879-109">Data Annotations</span></span>

<span data-ttu-id="34879-110">외래 키 제약 조건 이름은 데이터 주석을 사용 하 여 구성할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="34879-110">Foreign key constraint names cannot be configured using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="34879-111">흐름 API</span><span class="sxs-lookup"><span data-stu-id="34879-111">Fluent API</span></span>

<span data-ttu-id="34879-112">흐름 API를 사용 하 여 관계에 대 한 foreign key 제약 조건 이름을 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="34879-112">You can use the Fluent API to configure the foreign key constraint name for a relationship.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
