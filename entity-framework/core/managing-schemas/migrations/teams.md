---
title: 팀 환경-EF Core 마이그레이션
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: e8ff7f468d5ab6dbd6285f1abf9199e413288d10
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997697"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="e3af1-102">팀 환경에서 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="e3af1-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="e3af1-103">마이그레이션 팀 환경에서에서 작업할 때에 모델 스냅숏 파일에 추가 주의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="e3af1-104">이 파일에 팀 동료의 마이그레이션 기업과 완전히 병합 또는 공유 하기 전에 먼저 마이그레이션을 다시 만들어 충돌을 해결 해야 할 경우 알려 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="e3af1-105">병합</span><span class="sxs-lookup"><span data-stu-id="e3af1-105">Merging</span></span>
-------
<span data-ttu-id="e3af1-106">마이그레이션에서 동료를 병합 하면 모델 스냅숏 파일에서 충돌을 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="e3af1-107">변경 내용을 모두 관련 없으면 병합 trivial 이며 두 마이그레이션 공존할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="e3af1-108">예를 들어, 다음과 같은 고객 엔터티 유형의 구성에 병합 충돌이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="e3af1-109">이러한 속성을 모두 최종 모델에 존재 해야, 하므로 두 속성을 추가 하 여 병합을 완료 합니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="e3af1-110">대부분의 경우에서 버전 제어 시스템 수에 대 한 이러한 변경 내용을 자동으로 병합 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="e3af1-111">이러한 경우 팀 동료의 마이그레이션하고 마이그레이션에는 서로 독립적입니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="e3af1-112">둘 중 하나가 먼저 적용할 수 없습니다, 이후 마이그레이션 팀과 공유 하기 전에 추가로 변경할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="e3af1-113">충돌 해결</span><span class="sxs-lookup"><span data-stu-id="e3af1-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="e3af1-114">경우에 따라 스냅숏 model 병합할 때 true 충돌이 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="e3af1-115">예를 들어 하 고 동료가 각 바꿀 수도 동일한 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="e3af1-116">이러한 종류의 충돌을 발생 하는 경우에 마이그레이션을 다시 만들어 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="e3af1-117">아래 단계를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-117">Follow these steps:</span></span>

1. <span data-ttu-id="e3af1-118">병합 및 병합 하기 전에 작업 디렉터리에 롤백 중단</span><span class="sxs-lookup"><span data-stu-id="e3af1-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="e3af1-119">마이그레이션 제거 (하지만 모델 변경 내용을 유지)</span><span class="sxs-lookup"><span data-stu-id="e3af1-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="e3af1-120">동료가 변경 내용을 작업 디렉터리에 병합</span><span class="sxs-lookup"><span data-stu-id="e3af1-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="e3af1-121">마이그레이션을 다시 추가</span><span class="sxs-lookup"><span data-stu-id="e3af1-121">Re-add your migration</span></span>

<span data-ttu-id="e3af1-122">그러면 두 마이그레이션이 올바른 순서로 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="e3af1-123">해당 마이그레이션 먼저 적용 된 열 이름 바꾸기 *별칭*, 이후 마이그레이션 이름을 바꿉니다 하 *Username*합니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="e3af1-124">마이그레이션 팀의 나머지 부분을 사용 하 여 안전 하 게 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e3af1-124">Your migration can safely be shared with the rest of the team.</span></span>
