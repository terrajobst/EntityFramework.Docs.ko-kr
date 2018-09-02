---
title: EF Core 및 EF6 - 내게 적합한 항목
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 83ae754f899b624d322c48e8de8432b65519546e
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993835"
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="d55a5-102">EF Core 및 EF6: 내게 적합한 항목</span><span class="sxs-lookup"><span data-stu-id="d55a5-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="d55a5-103">다음 정보를 바탕으로 Entity Framework Core 및 Entity Framework 6 중에 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d55a5-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="d55a5-104">새 응용 프로그램에 대한 지침</span><span class="sxs-lookup"><span data-stu-id="d55a5-104">Guidance for new applications</span></span>

<span data-ttu-id="d55a5-105">EF Core의 모든 기능을 활용하려는 경우 새 응용 프로그램에 대한 EF Core를 사용하는 것이 좋으며 응용 프로그램은 EF Core에서 아직 구현되지 않은 기능이 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d55a5-105">Consider using EF Core for new applications if you want to take advantage of the all the capabilities of EF Core and your application does not require any features that are not yet implemented in EF Core.</span></span>

<span data-ttu-id="d55a5-106">EF6은 .NET Framework 4.0(또는 이상 버전)이 필요하며 Windows에서만 지원되지만(즉, EF6은 현재 .NET Core에서 실행되지 않으며 다른 운영 체제에서 지원되지 않음) 해당 제약 조건이 허용되는 한 새 응용 프로그램에 여전히 유용한 선택이며 응용 프로그램은 EF6에 사용할 수 없는 EF Core의 새 기능이 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="d55a5-106">EF6 requires .NET Framework 4.0 (or a later version) and is only supported on Windows (that is, EF6 does not currently run on .NET Core and is not supported in other operating systems), but it is still a viable choice for new applications as long those constraints are acceptable and the application does not require new features in EF Core that are not available to EF6.</span></span>

<span data-ttu-id="d55a5-107">[기능 비교](features.md)를 검토하여 EF Core가 응용 프로그램에 적합한 선택이 될 수 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="d55a5-107">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="d55a5-108">기존 EF6 응용 프로그램에 대한 지침</span><span class="sxs-lookup"><span data-stu-id="d55a5-108">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="d55a5-109">EF Core의 기본적인 변경 내용 때문에, 변경할 중요한 이유가 있는 경우에만 EF6 응용 프로그램을 EF Core로 이동하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d55a5-109">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="d55a5-110">새로운 기능을 사용하기 위해 EF Core로 이동하려면 시작하기 전에 제한 사항을 알고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d55a5-110">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="d55a5-111">[기능 비교](features.md)를 검토하여 EF Core가 응용 프로그램에 적합한 선택이 될 수 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="d55a5-111">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="d55a5-112">**EF6에서 EF Core로의 이동은 업그레이드가 아닌 포트로 이해해야 합니다.** </span><span class="sxs-lookup"><span data-stu-id="d55a5-112">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="d55a5-113">자세한 내용은 [EF6에서 EF Core로 포팅](porting/index.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="d55a5-113">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
