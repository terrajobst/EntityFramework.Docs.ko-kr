---
title: ".NET Core에서 시작 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 35fc7f49-fda8-4d8d-822f-78dd2d484d79
ms.technology: entity-framework-core
uid: core/get-started/netcore/index
ms.openlocfilehash: 7b70fccd8aab6675d44cec457cc40f24d8e4db35
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="getting-started-with-ef-core-on-net-core"></a><span data-ttu-id="b596b-102">.NET Core에서 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="b596b-102">Getting Started with EF Core on .NET Core</span></span>

<span data-ttu-id="b596b-103">이 101이 자습서에는 Entity Framework Core나 Visual Studio에 대한 사전 지식이 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b596b-103">These 101 tutorials require no previous knowledge of Entity Framework Core or Visual Studio.</span></span> <span data-ttu-id="b596b-104">데이터베이스에서 데이터를 쿼리하여 저장하는 간단한 .NET Core 콘솔 응용 프로그램을 만들어가면서 단계별로 안내합니다.</span><span class="sxs-lookup"><span data-stu-id="b596b-104">They will take you step-by-step through creating a simple .NET Core Console Application that queries and saves data from a database.</span></span> <span data-ttu-id="b596b-105">이 자습서는 .NET Core에서 지원하는 모든 플랫폼(Windows, OSX, Linux 등)에서 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b596b-105">The tutorials can be completed on any platform supported by .NET Core (Windows, OSX, Linux, etc.).</span></span>

<span data-ttu-id="b596b-106">.NET Core 설명서는 [docs.microsoft.com/dotnet/articles/core](https://docs.microsoft.com/dotnet/articles/core/)에서 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="b596b-106">You can find the .NET Core documentation at [docs.microsoft.com/dotnet/articles/core](https://docs.microsoft.com/dotnet/articles/core/).</span></span>

> [!NOTE]  
> <span data-ttu-id="b596b-107">이 자습서와 해당 샘플은 EF Core 2.0을 사용하도록 업데이트되었습니다(EF Core 1.1을 계속 사용하는 UWP 제외).</span><span class="sxs-lookup"><span data-stu-id="b596b-107">These tutorials and the accompanying samples have been updated to use EF Core 2.0 (with the exception of the UWP tutorial, that still uses EF Core 1.1).</span></span> <span data-ttu-id="b596b-108">그러나 대부분의 경우 명령을 약간만 수정하면 이전 릴리스를 사용하는 응용 프로그램을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b596b-108">However, in the majority of cases it should be possible to create applications that use previous releases, with minimal modification to the instructions.</span></span>
