---
title: SQLite 데이터베이스 공급자-제한 사항-EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 2f80dc195265787318ac4925dd937da45ffad011
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414760"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="9f85c-102">SQLite EF Core 데이터베이스 공급자 제한 사항</span><span class="sxs-lookup"><span data-stu-id="9f85c-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="9f85c-103">SQLite 공급자에는 여러 마이그레이션 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="9f85c-104">이러한 제한 사항 중 대부분은 기본 SQLite 데이터베이스 엔진의 제한 사항 때문 이며 EF에 국한 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="9f85c-105">모델링 제한 사항</span><span class="sxs-lookup"><span data-stu-id="9f85c-105">Modeling limitations</span></span>

<span data-ttu-id="9f85c-106">공통 관계형 라이브러리 (Entity Framework 관계형 데이터베이스 공급자에서 공유)는 대부분의 관계형 데이터베이스 엔진에 공통적인 모델링 개념에 대 한 Api를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="9f85c-107">이러한 개념 중 몇 가지는 SQLite 공급자가 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="9f85c-108">스키마</span><span class="sxs-lookup"><span data-stu-id="9f85c-108">Schemas</span></span>
* <span data-ttu-id="9f85c-109">시퀀스</span><span class="sxs-lookup"><span data-stu-id="9f85c-109">Sequences</span></span>
* <span data-ttu-id="9f85c-110">계산 열</span><span class="sxs-lookup"><span data-stu-id="9f85c-110">Computed columns</span></span>

## <a name="query-limitations"></a><span data-ttu-id="9f85c-111">쿼리 제한 사항</span><span class="sxs-lookup"><span data-stu-id="9f85c-111">Query limitations</span></span>

<span data-ttu-id="9f85c-112">SQLite는 기본적으로 다음 데이터 형식을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-112">SQLite doesn't natively support the following data types.</span></span> <span data-ttu-id="9f85c-113">EF Core는 이러한 형식의 값을 읽고 쓸 수 있으며 같음 (`where e.Property == value`)에 대 한 쿼리도 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-113">EF Core can read and write values of these types, and querying for equality (`where e.Property == value`) is also supported.</span></span> <span data-ttu-id="9f85c-114">그러나 비교 및 정렬과 같은 다른 작업은 클라이언트에서 평가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-114">Other operations, however, like comparison and ordering will require evaluation on the client.</span></span>

* <span data-ttu-id="9f85c-115">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="9f85c-115">DateTimeOffset</span></span>
* <span data-ttu-id="9f85c-116">Decimal</span><span class="sxs-lookup"><span data-stu-id="9f85c-116">Decimal</span></span>
* <span data-ttu-id="9f85c-117">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="9f85c-117">TimeSpan</span></span>
* <span data-ttu-id="9f85c-118">UInt64</span><span class="sxs-lookup"><span data-stu-id="9f85c-118">UInt64</span></span>

<span data-ttu-id="9f85c-119">`DateTimeOffset`대신 DateTime 값을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-119">Instead of `DateTimeOffset`, we recommend using DateTime values.</span></span> <span data-ttu-id="9f85c-120">여러 표준 시간대를 처리 하는 경우에는 저장 하기 전에 값을 UTC로 변환한 다음 다시 적절 한 표준 시간대로 변환 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-120">When handling multiple time zones, we recommend converting the values to UTC before saving and then converting back to the appropriate time zone.</span></span>

<span data-ttu-id="9f85c-121">`Decimal` 형식은 높은 수준의 전체 자릿수를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-121">The `Decimal` type provides a high level of precision.</span></span> <span data-ttu-id="9f85c-122">그러나 정밀도 수준이 필요 하지 않은 경우에는 double을 대신 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-122">If you don't need that level of precision, however, we recommend using double instead.</span></span> <span data-ttu-id="9f85c-123">[값 변환기](../../modeling/value-conversions.md) 를 사용 하 여 클래스에서 decimal을 계속 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-123">You can use a [value converter](../../modeling/value-conversions.md) to continue using decimal in your classes.</span></span>

``` csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a><span data-ttu-id="9f85c-124">마이그레이션 제한 사항</span><span class="sxs-lookup"><span data-stu-id="9f85c-124">Migrations limitations</span></span>

<span data-ttu-id="9f85c-125">SQLite 데이터베이스 엔진은 대부분의 다른 관계형 데이터베이스에서 지원 되는 많은 스키마 작업을 지원 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-125">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="9f85c-126">지원 되지 않는 작업 중 하나를 SQLite 데이터베이스에 적용 하려고 하면 `NotSupportedException`이 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-126">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="9f85c-127">연산</span><span class="sxs-lookup"><span data-stu-id="9f85c-127">Operation</span></span>            | <span data-ttu-id="9f85c-128">지원 여부</span><span class="sxs-lookup"><span data-stu-id="9f85c-128">Supported?</span></span> | <span data-ttu-id="9f85c-129">버전 필요</span><span class="sxs-lookup"><span data-stu-id="9f85c-129">Requires version</span></span> |
|:---------------------|:-----------|:-----------------|
| <span data-ttu-id="9f85c-130">AddColumn</span><span class="sxs-lookup"><span data-stu-id="9f85c-130">AddColumn</span></span>            | <span data-ttu-id="9f85c-131">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-131">✔</span></span>          | <span data-ttu-id="9f85c-132">1.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-132">1.0</span></span>              |
| <span data-ttu-id="9f85c-133">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="9f85c-133">AddForeignKey</span></span>        | <span data-ttu-id="9f85c-134">✗</span><span class="sxs-lookup"><span data-stu-id="9f85c-134">✗</span></span>          |                  |
| <span data-ttu-id="9f85c-135">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="9f85c-135">AddPrimaryKey</span></span>        | <span data-ttu-id="9f85c-136">✗</span><span class="sxs-lookup"><span data-stu-id="9f85c-136">✗</span></span>          |                  |
| <span data-ttu-id="9f85c-137">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="9f85c-137">AddUniqueConstraint</span></span>  | <span data-ttu-id="9f85c-138">✗</span><span class="sxs-lookup"><span data-stu-id="9f85c-138">✗</span></span>          |                  |
| <span data-ttu-id="9f85c-139">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="9f85c-139">AlterColumn</span></span>          | <span data-ttu-id="9f85c-140">✗</span><span class="sxs-lookup"><span data-stu-id="9f85c-140">✗</span></span>          |                  |
| <span data-ttu-id="9f85c-141">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="9f85c-141">CreateIndex</span></span>          | <span data-ttu-id="9f85c-142">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-142">✔</span></span>          | <span data-ttu-id="9f85c-143">1.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-143">1.0</span></span>              |
| <span data-ttu-id="9f85c-144">CreateTable</span><span class="sxs-lookup"><span data-stu-id="9f85c-144">CreateTable</span></span>          | <span data-ttu-id="9f85c-145">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-145">✔</span></span>          | <span data-ttu-id="9f85c-146">1.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-146">1.0</span></span>              |
| <span data-ttu-id="9f85c-147">DropColumn</span><span class="sxs-lookup"><span data-stu-id="9f85c-147">DropColumn</span></span>           | <span data-ttu-id="9f85c-148">✗</span><span class="sxs-lookup"><span data-stu-id="9f85c-148">✗</span></span>          |                  |
| <span data-ttu-id="9f85c-149">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="9f85c-149">DropForeignKey</span></span>       | <span data-ttu-id="9f85c-150">✗</span><span class="sxs-lookup"><span data-stu-id="9f85c-150">✗</span></span>          |                  |
| <span data-ttu-id="9f85c-151">DropIndex</span><span class="sxs-lookup"><span data-stu-id="9f85c-151">DropIndex</span></span>            | <span data-ttu-id="9f85c-152">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-152">✔</span></span>          | <span data-ttu-id="9f85c-153">1.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-153">1.0</span></span>              |
| <span data-ttu-id="9f85c-154">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="9f85c-154">DropPrimaryKey</span></span>       | <span data-ttu-id="9f85c-155">✗</span><span class="sxs-lookup"><span data-stu-id="9f85c-155">✗</span></span>          |                  |
| <span data-ttu-id="9f85c-156">DropTable</span><span class="sxs-lookup"><span data-stu-id="9f85c-156">DropTable</span></span>            | <span data-ttu-id="9f85c-157">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-157">✔</span></span>          | <span data-ttu-id="9f85c-158">1.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-158">1.0</span></span>              |
| <span data-ttu-id="9f85c-159">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="9f85c-159">DropUniqueConstraint</span></span> | <span data-ttu-id="9f85c-160">✗</span><span class="sxs-lookup"><span data-stu-id="9f85c-160">✗</span></span>          |                  |
| <span data-ttu-id="9f85c-161">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="9f85c-161">RenameColumn</span></span>         | <span data-ttu-id="9f85c-162">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-162">✔</span></span>          | <span data-ttu-id="9f85c-163">2.2.2</span><span class="sxs-lookup"><span data-stu-id="9f85c-163">2.2.2</span></span>            |
| <span data-ttu-id="9f85c-164">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="9f85c-164">RenameIndex</span></span>          | <span data-ttu-id="9f85c-165">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-165">✔</span></span>          | <span data-ttu-id="9f85c-166">2.1</span><span class="sxs-lookup"><span data-stu-id="9f85c-166">2.1</span></span>              |
| <span data-ttu-id="9f85c-167">RenameTable</span><span class="sxs-lookup"><span data-stu-id="9f85c-167">RenameTable</span></span>          | <span data-ttu-id="9f85c-168">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-168">✔</span></span>          | <span data-ttu-id="9f85c-169">1.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-169">1.0</span></span>              |
| <span data-ttu-id="9f85c-170">EnsureSchema</span><span class="sxs-lookup"><span data-stu-id="9f85c-170">EnsureSchema</span></span>         | <span data-ttu-id="9f85c-171">✔ (작동 안 함)</span><span class="sxs-lookup"><span data-stu-id="9f85c-171">✔ (no-op)</span></span>  | <span data-ttu-id="9f85c-172">2.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-172">2.0</span></span>              |
| <span data-ttu-id="9f85c-173">DropSchema</span><span class="sxs-lookup"><span data-stu-id="9f85c-173">DropSchema</span></span>           | <span data-ttu-id="9f85c-174">✔ (작동 안 함)</span><span class="sxs-lookup"><span data-stu-id="9f85c-174">✔ (no-op)</span></span>  | <span data-ttu-id="9f85c-175">2.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-175">2.0</span></span>              |
| <span data-ttu-id="9f85c-176">삽입</span><span class="sxs-lookup"><span data-stu-id="9f85c-176">Insert</span></span>               | <span data-ttu-id="9f85c-177">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-177">✔</span></span>          | <span data-ttu-id="9f85c-178">2.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-178">2.0</span></span>              |
| <span data-ttu-id="9f85c-179">업데이트</span><span class="sxs-lookup"><span data-stu-id="9f85c-179">Update</span></span>               | <span data-ttu-id="9f85c-180">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-180">✔</span></span>          | <span data-ttu-id="9f85c-181">2.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-181">2.0</span></span>              |
| <span data-ttu-id="9f85c-182">삭제</span><span class="sxs-lookup"><span data-stu-id="9f85c-182">Delete</span></span>               | <span data-ttu-id="9f85c-183">✔</span><span class="sxs-lookup"><span data-stu-id="9f85c-183">✔</span></span>          | <span data-ttu-id="9f85c-184">2.0</span><span class="sxs-lookup"><span data-stu-id="9f85c-184">2.0</span></span>              |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="9f85c-185">마이그레이션 제한 해결 방법</span><span class="sxs-lookup"><span data-stu-id="9f85c-185">Migrations limitations workaround</span></span>

<span data-ttu-id="9f85c-186">마이그레이션에 수동으로 코드를 작성 하 여 테이블 다시 빌드를 수행 하면 이러한 제한 사항 중 일부를 해결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-186">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="9f85c-187">테이블 다시 빌드에는 기존 테이블 이름 바꾸기, 새 테이블 만들기, 새 테이블에 데이터 복사 및 이전 테이블 삭제가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-187">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="9f85c-188">이러한 단계 중 일부를 수행 하려면 `Sql(string)` 메서드를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-188">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="9f85c-189">자세한 내용은 SQLite 설명서에서 [다른 종류의 테이블 스키마 변경 만들기](https://sqlite.org/lang_altertable.html#otheralter) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="9f85c-189">See [Making Other Kinds Of Table Schema Changes](https://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="9f85c-190">앞으로 EF는 내부적으로 테이블 다시 작성 방법을 사용 하 여 이러한 작업 중 일부를 지원할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-190">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="9f85c-191">[GitHub 프로젝트에서이 기능을 추적할](https://github.com/aspnet/EntityFrameworkCore/issues/329)수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="9f85c-191">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFrameworkCore/issues/329).</span></span>
