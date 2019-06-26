---
title: SQLite 데이터베이스 공급자-제한 사항-EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: eaa7d5b1496172e4f3821433a1cd098ee7e8b737
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394796"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="4f484-102">SQLite EF Core 데이터베이스 공급자의 제한 사항</span><span class="sxs-lookup"><span data-stu-id="4f484-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="4f484-103">SQLite 공급자에 많은 마이그레이션 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="4f484-104">대부분의 이러한 제한 기본 SQLite 데이터베이스 엔진에서 제한 사항의 결과 되며 EF와 관련이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="4f484-105">모델링 제한 사항</span><span class="sxs-lookup"><span data-stu-id="4f484-105">Modeling limitations</span></span>

<span data-ttu-id="4f484-106">대부분의 관계형 데이터베이스 엔진에 공통 되는 개념을 모델링 하는 것에 대 한 Api를 정의 하는 일반적인 관계형 라이브러리 (Entity Framework 관계형 데이터베이스 공급자에 의해 공유).</span><span class="sxs-lookup"><span data-stu-id="4f484-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="4f484-107">이러한 개념 중 몇 SQLite 공급자가 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="4f484-108">스키마</span><span class="sxs-lookup"><span data-stu-id="4f484-108">Schemas</span></span>
* <span data-ttu-id="4f484-109">시퀀스</span><span class="sxs-lookup"><span data-stu-id="4f484-109">Sequences</span></span>
* <span data-ttu-id="4f484-110">계산 열</span><span class="sxs-lookup"><span data-stu-id="4f484-110">Computed columns</span></span>

## <a name="query-limitations"></a><span data-ttu-id="4f484-111">쿼리 제한 사항</span><span class="sxs-lookup"><span data-stu-id="4f484-111">Query limitations</span></span>

<span data-ttu-id="4f484-112">SQLite는 데이터 형식은 기본적으로 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-112">SQLite doesn't natively support the following data types.</span></span> <span data-ttu-id="4f484-113">EF Core 읽고 이러한 형식 및 같음 쿼리 값을 쓸 수 있습니다 (`where e.Property == value`)도 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-113">EF Core can read and write values of these types, and querying for equality (`where e.Property == value`) is also support.</span></span> <span data-ttu-id="4f484-114">그러나 다른 작업을 비교와 클라이언트에서 평가 순서 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-114">Other operations, however, like comparison and ordering will require evaluation on the client.</span></span>

* <span data-ttu-id="4f484-115">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="4f484-115">DateTimeOffset</span></span>
* <span data-ttu-id="4f484-116">Decimal</span><span class="sxs-lookup"><span data-stu-id="4f484-116">Decimal</span></span>
* <span data-ttu-id="4f484-117">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="4f484-117">TimeSpan</span></span>
* <span data-ttu-id="4f484-118">UInt64</span><span class="sxs-lookup"><span data-stu-id="4f484-118">UInt64</span></span>

<span data-ttu-id="4f484-119">대신 `DateTimeOffset`, 날짜/시간 값을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-119">Instead of `DateTimeOffset`, we recommend using DateTime values.</span></span> <span data-ttu-id="4f484-120">여러 표준 시간대를 처리 하는 경우에 값을 저장 하 고 적절 한 표준 시간대로 다시 변환 전에 UTC로 변환 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-120">When handling multiple time zones, we recommend converting the values to UTC before saving and then converting back to the appropriate time zone.</span></span>

<span data-ttu-id="4f484-121">`Decimal` 형식은 높은 수준의 전체 자릿수를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-121">The `Decimal` type provides a high level of precision.</span></span> <span data-ttu-id="4f484-122">그러나 전체 자릿수는 수준이 필요 하지 않으면, 좋습니다 double을 대신 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-122">If you don't need that level of precision, however, we recommend using double instead.</span></span> <span data-ttu-id="4f484-123">사용할 수는 [값 변환기](../../modeling/value-conversions.md) 소수 클래스에서 사용 하 여 계속 합니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-123">You can use a [value converter](../../modeling/value-conversions.md) to continue using decimal in your classes.</span></span>

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a><span data-ttu-id="4f484-124">마이그레이션 제한 사항</span><span class="sxs-lookup"><span data-stu-id="4f484-124">Migrations limitations</span></span>

<span data-ttu-id="4f484-125">SQLite 데이터베이스 엔진이 다양 한 대부분의 기타 관계형 데이터베이스에서 지원 되는 스키마 작업을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-125">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="4f484-126">SQLite 데이터베이스에 지원 되지 않는 작업 중 하나를 적용 하려고 하면 다음을 `NotSupportedException` throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-126">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="4f484-127">작업</span><span class="sxs-lookup"><span data-stu-id="4f484-127">Operation</span></span>            | <span data-ttu-id="4f484-128">지원 되나요?</span><span class="sxs-lookup"><span data-stu-id="4f484-128">Supported?</span></span> | <span data-ttu-id="4f484-129">버전 필요</span><span class="sxs-lookup"><span data-stu-id="4f484-129">Requires version</span></span> |
|:---------------------|:-----------|:-----------------|
| <span data-ttu-id="4f484-130">AddColumn</span><span class="sxs-lookup"><span data-stu-id="4f484-130">AddColumn</span></span>            | <span data-ttu-id="4f484-131">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-131">✔</span></span>          | <span data-ttu-id="4f484-132">1.0</span><span class="sxs-lookup"><span data-stu-id="4f484-132">1.0</span></span>              |
| <span data-ttu-id="4f484-133">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="4f484-133">AddForeignKey</span></span>        | <span data-ttu-id="4f484-134">✗</span><span class="sxs-lookup"><span data-stu-id="4f484-134">✗</span></span>          |                  |
| <span data-ttu-id="4f484-135">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="4f484-135">AddPrimaryKey</span></span>        | <span data-ttu-id="4f484-136">✗</span><span class="sxs-lookup"><span data-stu-id="4f484-136">✗</span></span>          |                  |
| <span data-ttu-id="4f484-137">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="4f484-137">AddUniqueConstraint</span></span>  | <span data-ttu-id="4f484-138">✗</span><span class="sxs-lookup"><span data-stu-id="4f484-138">✗</span></span>          |                  |
| <span data-ttu-id="4f484-139">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="4f484-139">AlterColumn</span></span>          | <span data-ttu-id="4f484-140">✗</span><span class="sxs-lookup"><span data-stu-id="4f484-140">✗</span></span>          |                  |
| <span data-ttu-id="4f484-141">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="4f484-141">CreateIndex</span></span>          | <span data-ttu-id="4f484-142">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-142">✔</span></span>          | <span data-ttu-id="4f484-143">1.0</span><span class="sxs-lookup"><span data-stu-id="4f484-143">1.0</span></span>              |
| <span data-ttu-id="4f484-144">CreateTable</span><span class="sxs-lookup"><span data-stu-id="4f484-144">CreateTable</span></span>          | <span data-ttu-id="4f484-145">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-145">✔</span></span>          | <span data-ttu-id="4f484-146">1.0</span><span class="sxs-lookup"><span data-stu-id="4f484-146">1.0</span></span>              |
| <span data-ttu-id="4f484-147">DropColumn</span><span class="sxs-lookup"><span data-stu-id="4f484-147">DropColumn</span></span>           | <span data-ttu-id="4f484-148">✗</span><span class="sxs-lookup"><span data-stu-id="4f484-148">✗</span></span>          |                  |
| <span data-ttu-id="4f484-149">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="4f484-149">DropForeignKey</span></span>       | <span data-ttu-id="4f484-150">✗</span><span class="sxs-lookup"><span data-stu-id="4f484-150">✗</span></span>          |                  |
| <span data-ttu-id="4f484-151">DropIndex</span><span class="sxs-lookup"><span data-stu-id="4f484-151">DropIndex</span></span>            | <span data-ttu-id="4f484-152">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-152">✔</span></span>          | <span data-ttu-id="4f484-153">1.0</span><span class="sxs-lookup"><span data-stu-id="4f484-153">1.0</span></span>              |
| <span data-ttu-id="4f484-154">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="4f484-154">DropPrimaryKey</span></span>       | <span data-ttu-id="4f484-155">✗</span><span class="sxs-lookup"><span data-stu-id="4f484-155">✗</span></span>          |                  |
| <span data-ttu-id="4f484-156">DropTable</span><span class="sxs-lookup"><span data-stu-id="4f484-156">DropTable</span></span>            | <span data-ttu-id="4f484-157">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-157">✔</span></span>          | <span data-ttu-id="4f484-158">1.0</span><span class="sxs-lookup"><span data-stu-id="4f484-158">1.0</span></span>              |
| <span data-ttu-id="4f484-159">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="4f484-159">DropUniqueConstraint</span></span> | <span data-ttu-id="4f484-160">✗</span><span class="sxs-lookup"><span data-stu-id="4f484-160">✗</span></span>          |                  |
| <span data-ttu-id="4f484-161">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="4f484-161">RenameColumn</span></span>         | <span data-ttu-id="4f484-162">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-162">✔</span></span>          | <span data-ttu-id="4f484-163">2.2.2</span><span class="sxs-lookup"><span data-stu-id="4f484-163">2.2.2</span></span>            |
| <span data-ttu-id="4f484-164">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="4f484-164">RenameIndex</span></span>          | <span data-ttu-id="4f484-165">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-165">✔</span></span>          | <span data-ttu-id="4f484-166">2.1</span><span class="sxs-lookup"><span data-stu-id="4f484-166">2.1</span></span>              |
| <span data-ttu-id="4f484-167">RenameTable</span><span class="sxs-lookup"><span data-stu-id="4f484-167">RenameTable</span></span>          | <span data-ttu-id="4f484-168">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-168">✔</span></span>          | <span data-ttu-id="4f484-169">1.0</span><span class="sxs-lookup"><span data-stu-id="4f484-169">1.0</span></span>              |
| <span data-ttu-id="4f484-170">EnsureSchema</span><span class="sxs-lookup"><span data-stu-id="4f484-170">EnsureSchema</span></span>         | <span data-ttu-id="4f484-171">✔ (아무)</span><span class="sxs-lookup"><span data-stu-id="4f484-171">✔ (no-op)</span></span>  | <span data-ttu-id="4f484-172">2.0</span><span class="sxs-lookup"><span data-stu-id="4f484-172">2.0</span></span>              |
| <span data-ttu-id="4f484-173">DropSchema</span><span class="sxs-lookup"><span data-stu-id="4f484-173">DropSchema</span></span>           | <span data-ttu-id="4f484-174">✔ (아무)</span><span class="sxs-lookup"><span data-stu-id="4f484-174">✔ (no-op)</span></span>  | <span data-ttu-id="4f484-175">2.0</span><span class="sxs-lookup"><span data-stu-id="4f484-175">2.0</span></span>              |
| <span data-ttu-id="4f484-176">Insert</span><span class="sxs-lookup"><span data-stu-id="4f484-176">Insert</span></span>               | <span data-ttu-id="4f484-177">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-177">✔</span></span>          | <span data-ttu-id="4f484-178">2.0</span><span class="sxs-lookup"><span data-stu-id="4f484-178">2.0</span></span>              |
| <span data-ttu-id="4f484-179">업데이트</span><span class="sxs-lookup"><span data-stu-id="4f484-179">Update</span></span>               | <span data-ttu-id="4f484-180">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-180">✔</span></span>          | <span data-ttu-id="4f484-181">2.0</span><span class="sxs-lookup"><span data-stu-id="4f484-181">2.0</span></span>              |
| <span data-ttu-id="4f484-182">삭제</span><span class="sxs-lookup"><span data-stu-id="4f484-182">Delete</span></span>               | <span data-ttu-id="4f484-183">✔</span><span class="sxs-lookup"><span data-stu-id="4f484-183">✔</span></span>          | <span data-ttu-id="4f484-184">2.0</span><span class="sxs-lookup"><span data-stu-id="4f484-184">2.0</span></span>              |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="4f484-185">마이그레이션 제한 사항 해결</span><span class="sxs-lookup"><span data-stu-id="4f484-185">Migrations limitations workaround</span></span>

<span data-ttu-id="4f484-186">일부 해결 방법을 사용 하면 수동으로 테이블을 수행 하 여 마이그레이션에 코드를 작성 하 여 이러한 한계를 다시 작성 합니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-186">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="4f484-187">테이블 다시 빌드에는 기존 테이블 이름 바꾸기, 새 테이블 만들기, 새 테이블에 데이터 복사 및 이전 테이블 삭제가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-187">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="4f484-188">사용 해야 합니다는 `Sql(string)` 일부이 단계를 수행 하는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-188">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="4f484-189">참조 [다른 종류의 테이블 스키마 변경 사항을 편집](http://sqlite.org/lang_altertable.html#otheralter) 자세한 SQLite 설명서.</span><span class="sxs-lookup"><span data-stu-id="4f484-189">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="4f484-190">나중에 EF 내부적으로 테이블 다시 빌드 방법을 사용 하 여 이러한 작업 일부를 지원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-190">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="4f484-191">할 수 있습니다 [GitHub 프로젝트에서이 기능은 추적](https://github.com/aspnet/EntityFrameworkCore/issues/329)합니다.</span><span class="sxs-lookup"><span data-stu-id="4f484-191">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFrameworkCore/issues/329).</span></span>
