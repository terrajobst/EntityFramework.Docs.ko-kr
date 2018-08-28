---
title: ASP.NET Core에서 시작 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bcf6d28a-5a2a-40b9-87ea-19ed9ef2e555
uid: core/get-started/aspnetcore/index
ms.openlocfilehash: b676f84218af77e51fb0558d04b775dda0375847
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993549"
---
# <a name="getting-started-with-ef-core-on-aspnet-core"></a><span data-ttu-id="875ae-102">ASP.NET Core에서 EF Core 시작</span><span class="sxs-lookup"><span data-stu-id="875ae-102">Getting Started with EF Core on ASP.NET Core</span></span>

<span data-ttu-id="875ae-103">이 101이 자습서에는 Entity Framework Core나 Visual Studio에 대한 사전 지식이 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="875ae-103">These 101 tutorials require no previous knowledge of Entity Framework Core or Visual Studio.</span></span> <span data-ttu-id="875ae-104">데이터베이스에서 데이터를 쿼리하여 저장하는 간단한 ASP.NET Core 응용 프로그램을 만들어가면서 단계별로 안내합니다.</span><span class="sxs-lookup"><span data-stu-id="875ae-104">They will take you step-by-step through creating a simple ASP.NET Core application that queries and saves data from a database.</span></span> <span data-ttu-id="875ae-105">기존 데이터베이스를 기반으로 모델을 만드는 자습서 또는 사용자의 모델을 기반으로 자동으로 데이터베이스를 만드는 자습서를 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="875ae-105">You can choose a tutorial that creates a model based on an existing database, or creates a database for you based on your model.</span></span>

<span data-ttu-id="875ae-106">ASP.NET Core 설명서는 [ASP.NET Core 소개](/aspnet/core/)에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="875ae-106">You can find the ASP.NET Core documentation at [Introduction to ASP.NET Core](/aspnet/core/).</span></span>

> [!NOTE]  
> <span data-ttu-id="875ae-107">이 자습서와 해당 샘플은 EF Core 2.0을 사용하도록 업데이트되었습니다(EF Core 1.1을 계속 사용하는 UWP 제외).</span><span class="sxs-lookup"><span data-stu-id="875ae-107">These tutorials and the accompanying samples have been updated to use EF Core 2.0 (with the exception of the UWP tutorial, that still uses EF Core 1.1).</span></span> <span data-ttu-id="875ae-108">그러나 대부분의 경우 명령을 약간만 수정하면 이전 릴리스를 사용하는 응용 프로그램을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="875ae-108">However, in the majority of cases it should be possible to create applications that use previous releases, with minimal modification to the instructions.</span></span>
