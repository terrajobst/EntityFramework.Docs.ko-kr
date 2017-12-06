---
title: "SQLite 데이터베이스 공급자-도-전보다 EF 코어"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="f4640-102">SQLite EF 코어 데이터베이스 공급자의 제한 사항</span><span class="sxs-lookup"><span data-stu-id="f4640-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="f4640-103">SQLite 공급자 마이그레이션 제한 사항을 번호가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="f4640-104">대부분의 이러한 제한은 기본 SQLite 데이터베이스 엔진의 제한 한 결과 및 EF에 한정 되지 않는 합니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="f4640-105">제한 사항 모델링</span><span class="sxs-lookup"><span data-stu-id="f4640-105">Modeling limitations</span></span>

<span data-ttu-id="f4640-106">(Entity Framework 관계형 데이터베이스 공급자에 의해 공유) 일반적인 관계형 라이브러리는 대부분의 관계형 데이터베이스 엔진에 공통 된 개념 모델링에 대 한 Api를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="f4640-107">이러한 개념 중 몇 SQLite 공급자가 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="f4640-108">스키마</span><span class="sxs-lookup"><span data-stu-id="f4640-108">Schemas</span></span>
* <span data-ttu-id="f4640-109">시퀀스</span><span class="sxs-lookup"><span data-stu-id="f4640-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="f4640-110">마이그레이션 제한 사항</span><span class="sxs-lookup"><span data-stu-id="f4640-110">Migrations limitations</span></span>

<span data-ttu-id="f4640-111">SQLite 데이터베이스 엔진에서 대부분의 다른 관계형 데이터베이스에서 지원 되는 스키마 작업의 수를 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="f4640-112">SQLite 데이터베이스에 지원 되지 않는 작업 중 하나를 적용 하려는 경우는 `NotSupportedException` throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="f4640-113">작업</span><span class="sxs-lookup"><span data-stu-id="f4640-113">Operation</span></span>            | <span data-ttu-id="f4640-114">지원 되나요?</span><span class="sxs-lookup"><span data-stu-id="f4640-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="f4640-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="f4640-115">AddColumn</span></span>            | <span data-ttu-id="f4640-116">✔</span><span class="sxs-lookup"><span data-stu-id="f4640-116">✔</span></span>          |
| <span data-ttu-id="f4640-117">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="f4640-117">AddForeignKey</span></span>        | <span data-ttu-id="f4640-118">✗</span><span class="sxs-lookup"><span data-stu-id="f4640-118">✗</span></span>          |
| <span data-ttu-id="f4640-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="f4640-119">AddPrimaryKey</span></span>        | <span data-ttu-id="f4640-120">✗</span><span class="sxs-lookup"><span data-stu-id="f4640-120">✗</span></span>          |
| <span data-ttu-id="f4640-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="f4640-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="f4640-122">✗</span><span class="sxs-lookup"><span data-stu-id="f4640-122">✗</span></span>          |
| <span data-ttu-id="f4640-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="f4640-123">AlterColumn</span></span>          | <span data-ttu-id="f4640-124">✗</span><span class="sxs-lookup"><span data-stu-id="f4640-124">✗</span></span>          |
| <span data-ttu-id="f4640-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="f4640-125">CreateIndex</span></span>          | <span data-ttu-id="f4640-126">✔</span><span class="sxs-lookup"><span data-stu-id="f4640-126">✔</span></span>          |
| <span data-ttu-id="f4640-127">CreateTable</span><span class="sxs-lookup"><span data-stu-id="f4640-127">CreateTable</span></span>          | <span data-ttu-id="f4640-128">✔</span><span class="sxs-lookup"><span data-stu-id="f4640-128">✔</span></span>          |
| <span data-ttu-id="f4640-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="f4640-129">DropColumn</span></span>           | <span data-ttu-id="f4640-130">✗</span><span class="sxs-lookup"><span data-stu-id="f4640-130">✗</span></span>          |
| <span data-ttu-id="f4640-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="f4640-131">DropForeignKey</span></span>       | <span data-ttu-id="f4640-132">✗</span><span class="sxs-lookup"><span data-stu-id="f4640-132">✗</span></span>          |
| <span data-ttu-id="f4640-133">DropIndex</span><span class="sxs-lookup"><span data-stu-id="f4640-133">DropIndex</span></span>            | <span data-ttu-id="f4640-134">✔</span><span class="sxs-lookup"><span data-stu-id="f4640-134">✔</span></span>          |
| <span data-ttu-id="f4640-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="f4640-135">DropPrimaryKey</span></span>       | <span data-ttu-id="f4640-136">✗</span><span class="sxs-lookup"><span data-stu-id="f4640-136">✗</span></span>          |
| <span data-ttu-id="f4640-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="f4640-137">DropTable</span></span>            | <span data-ttu-id="f4640-138">✔</span><span class="sxs-lookup"><span data-stu-id="f4640-138">✔</span></span>          |
| <span data-ttu-id="f4640-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="f4640-139">DropUniqueConstraint</span></span> | <span data-ttu-id="f4640-140">✗</span><span class="sxs-lookup"><span data-stu-id="f4640-140">✗</span></span>          |
| <span data-ttu-id="f4640-141">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="f4640-141">RenameColumn</span></span>         | <span data-ttu-id="f4640-142">✗</span><span class="sxs-lookup"><span data-stu-id="f4640-142">✗</span></span>          |
| <span data-ttu-id="f4640-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="f4640-143">RenameIndex</span></span>          | <span data-ttu-id="f4640-144">✗</span><span class="sxs-lookup"><span data-stu-id="f4640-144">✗</span></span>          |
| <span data-ttu-id="f4640-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="f4640-145">RenameTable</span></span>          | <span data-ttu-id="f4640-146">✔</span><span class="sxs-lookup"><span data-stu-id="f4640-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="f4640-147">마이그레이션 제한 사항 해결 방법</span><span class="sxs-lookup"><span data-stu-id="f4640-147">Migrations limitations workaround</span></span>

<span data-ttu-id="f4640-148">일부를 방지할 수 있습니다 이러한 제한 사항을 수동으로 테이블을 수행 하 여 마이그레이션에 코드를 작성 하 여 다시 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="f4640-149">테이블 rebuild 기존 테이블 이름 바꾸기, 새 테이블 만들기, 새 테이블에 데이터를 복사 및 이전 테이블을 삭제 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="f4640-150">사용 해야 합니다는 `Sql(string)` 메서드를 다음이 단계 중 일부를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="f4640-151">참조 [다른 종류의 테이블 스키마 변경 사항을 편집](http://sqlite.org/lang_altertable.html#otheralter) 자세한 내용은 SQLite 설명서에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="f4640-152">나중에 EF 내부적 테이블 다시 작성 방법을 사용 하 여 이러한 작업 중 일부를 지원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="f4640-153">있습니다 수 [우리의 GitHub 프로젝트에서이 기능을 추적](https://github.com/aspnet/EntityFramework/issues/329)합니다.</span><span class="sxs-lookup"><span data-stu-id="f4640-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
