---
title: 팀 환경-EF 코어에서 마이그레이션
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053803"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="03332-102">팀 환경에서 마이그레이션</span><span class="sxs-lookup"><span data-stu-id="03332-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="03332-103">팀 환경에서 마이그레이션 작업을 하는 경우 모델 스냅숏 파일에 부가적인 주의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="03332-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="03332-104">이 파일 하는 경우 이렇게 마이그레이션 병합 완전히의 기업과 것을 공유 하기 전에 마이그레이션을 다시 만들어서 충돌을 해결 하는 경우를 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="03332-104">This file can tell you if your teammate's migration merges cleanly with yours of if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="03332-105">병합</span><span class="sxs-lookup"><span data-stu-id="03332-105">Merging</span></span>
-------
<span data-ttu-id="03332-106">다른 팀 멤버에서 마이그레이션을 병합할 때 모델 스냅숏 파일에서 충돌을 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="03332-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="03332-107">변경 내용이 모두 관련 없는 경우 병합 trivial 이며 두 마이그레이션 공존할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="03332-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="03332-108">예를 들어 다음과 같은 고객 엔터티 유형의 구성에 병합 충돌이 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="03332-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="03332-109">최종 모델에 있는 필요가 이러한 두 속성이 없기 때문에 두 속성을 추가 하 여 병합을 완료 합니다.</span><span class="sxs-lookup"><span data-stu-id="03332-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="03332-110">대부분의 경우에서 버전 제어 시스템을 이러한 변경 내용이 자동으로 병합 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="03332-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="03332-111">이러한 경우 마이그레이션 및 팀 동료의 마이그레이션은 서로 독립적입니다.</span><span class="sxs-lookup"><span data-stu-id="03332-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="03332-112">둘 중 하나가 적용 되지 않아 먼저 하므로 팀과 공유 하기 전에 마이그레이션에 추가로 변경할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="03332-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="03332-113">충돌 해결</span><span class="sxs-lookup"><span data-stu-id="03332-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="03332-114">경우에 따라 모델 스냅숏 모델 병합할 때 true 충돌이 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="03332-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="03332-115">예를 들어 사용자와 동료가 각 바꿀 수도 동일한 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="03332-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="03332-116">이러한 종류의 충돌을 발생 하는 경우에 마이그레이션을 다시 만들어서 해결 합니다.</span><span class="sxs-lookup"><span data-stu-id="03332-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="03332-117">아래 단계를 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="03332-117">Follow these steps:</span></span>

1. <span data-ttu-id="03332-118">병합 및 rollback을 병합 하기 전에 작업 디렉터리를 중단 합니다.</span><span class="sxs-lookup"><span data-stu-id="03332-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="03332-119">마이그레이션 제거 (하지만 모델 변경 내용을 유지)</span><span class="sxs-lookup"><span data-stu-id="03332-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="03332-120">이렇게 변경 내용을 작업 디렉터리에 병합</span><span class="sxs-lookup"><span data-stu-id="03332-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="03332-121">마이그레이션을 다시 추가</span><span class="sxs-lookup"><span data-stu-id="03332-121">Re-add your migration</span></span>

<span data-ttu-id="03332-122">이 작업을 수행한 후 두 마이그레이션 올바른 순서로 적용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="03332-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="03332-123">마이그레이션을 먼저 적용 된 다음에 열 이름을 바꾸면 *별칭*, 그 후 마이그레이션 이름을 바꾸거나를 *사용자 이름*합니다.</span><span class="sxs-lookup"><span data-stu-id="03332-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="03332-124">마이그레이션은 팀의 나머지와 안전 하 게 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="03332-124">Your migration can safely be shared with the rest of the team.</span></span>
