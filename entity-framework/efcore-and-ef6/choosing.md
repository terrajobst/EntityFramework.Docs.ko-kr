---
title: "EF Core 및 EF6 - 내게 적합한 항목"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 288f825b-b3e6-4096-971b-d0a1cb96770e
uid: efcore-and-ef6/choosing
ms.openlocfilehash: 9a113e0965fa75a03510199fb75165f6e9be0bbd
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="ef-core-and-ef6-which-one-is-right-for-you"></a><span data-ttu-id="92db0-102">EF Core 및 EF6: 내게 적합한 항목</span><span class="sxs-lookup"><span data-stu-id="92db0-102">EF Core and EF6: Which One Is Right for You</span></span>

<span data-ttu-id="92db0-103">다음 정보를 바탕으로 Entity Framework Core 및 Entity Framework 6 중에 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="92db0-103">The following information will help you choose between Entity Framework Core and Entity Framework 6.</span></span>

## <a name="guidance-for-new-applications"></a><span data-ttu-id="92db0-104">새 응용 프로그램에 대한 지침</span><span class="sxs-lookup"><span data-stu-id="92db0-104">Guidance for new applications</span></span>

<span data-ttu-id="92db0-105">EF Core가 새로운 제품이지만 일부 중요한 O/RM 기능이 없기 때문에 EF6는 대부분의 응용 프로그램에 가장 적합한 선택입니다.</span><span class="sxs-lookup"><span data-stu-id="92db0-105">Because EF Core is a new product, and still lacks some critical O/RM features, EF6 will still be the most suitable choice for many applications.</span></span>

<span data-ttu-id="92db0-106">**다음 응용 프로그램 유형에는 EF Core를 사용하는 것이 좋습니다.**</span><span class="sxs-lookup"><span data-stu-id="92db0-106">**These are the types of applications we would recommend using EF Core for:**</span></span>

* <span data-ttu-id="92db0-107">EF Core에 구현되지 않은 기능이 필요하지 않은 새로운 응용 프로그램입니다.</span><span class="sxs-lookup"><span data-stu-id="92db0-107">New applications that do not require features that are not yet implemented in EF Core.</span></span> <span data-ttu-id="92db0-108">[기능 비교](features.md)를 검토하여 EF Core가 응용 프로그램에 적합한 선택이 될 수 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="92db0-108">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

* <span data-ttu-id="92db0-109">UWP(유니버설 Windows 플랫폼) 및 ASP.NET Core 응용 프로그램과 같은 .NET Core를 대상으로 하는 응용 프로그램입니다.</span><span class="sxs-lookup"><span data-stu-id="92db0-109">Applications that target .NET Core, such as Universal Windows Platform (UWP) and ASP.NET Core applications.</span></span> <span data-ttu-id="92db0-110">이러한 응용 프로그램에는 .NET Framework(.NET Framework 4.5)가 필요하므로 EF6를 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="92db0-110">These applications can not use EF6 as it requires the .NET Framework (i.e. .NET Framework 4.5).</span></span>

## <a name="guidance-for-existing-ef6-applications"></a><span data-ttu-id="92db0-111">기존 EF6 응용 프로그램에 대한 지침</span><span class="sxs-lookup"><span data-stu-id="92db0-111">Guidance for existing EF6 applications</span></span>

<span data-ttu-id="92db0-112">EF Core의 기본적인 변경 내용 때문에, 변경할 중요한 이유가 있는 경우에만 EF6 응용 프로그램을 EF Core로 이동하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="92db0-112">Because of the fundamental changes in EF Core we do not recommend attempting to move an EF6 application to EF Core unless you have a compelling reason to make the change.</span></span> <span data-ttu-id="92db0-113">새로운 기능을 사용하기 위해 EF Core로 이동하려면 시작하기 전에 제한 사항을 알고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="92db0-113">If you want to move to EF Core to make use of new features, then make sure you are aware of its limitations before you start.</span></span> <span data-ttu-id="92db0-114">[기능 비교](features.md)를 검토하여 EF Core가 응용 프로그램에 적합한 선택이 될 수 있는지 확인합니다.</span><span class="sxs-lookup"><span data-stu-id="92db0-114">Review [Feature Comparison](features.md) to see if EF Core may be a suitable choice for your application.</span></span>

<span data-ttu-id="92db0-115">**EF6에서 EF Core로의 이동은 업그레이드가 아닌 포트로 이해해야 합니다.** </span><span class="sxs-lookup"><span data-stu-id="92db0-115">**You should view the move from EF6 to EF Core as a port rather than an upgrade.**</span></span> <span data-ttu-id="92db0-116">자세한 내용은 [EF6에서 EF Core로 포팅](porting/index.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="92db0-116">See [Porting from EF6 to EF Core](porting/index.md) for more information.</span></span>
