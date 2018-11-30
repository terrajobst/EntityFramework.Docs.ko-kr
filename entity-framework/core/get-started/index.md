---
title: 시작 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3c88427c-20c6-42ec-a736-22d3eccd5071
uid: core/get-started/index
ms.openlocfilehash: b846d63f2c285a43d60eecfb2be3d460a5d31924
ms.sourcegitcommit: 064b09431f05848830e145a6cd65cad58881557c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52552596"
---
# <a name="getting-started-with-entity-framework-core"></a><span data-ttu-id="1086e-102">Entity Framework Core 시작</span><span class="sxs-lookup"><span data-stu-id="1086e-102">Getting Started with Entity Framework Core</span></span>

## <a name="installing-ef-coreinstallindexmd"></a>[<span data-ttu-id="1086e-103">EF Core 설치</span><span class="sxs-lookup"><span data-stu-id="1086e-103">Installing EF Core</span></span>](install/index.md)

<span data-ttu-id="1086e-104">다양한 플랫폼과 널리 사용되는 IDE에서 응용 프로그램에 EF Core를 추가하는 데 필요한 단계를 요약합니다.</span><span class="sxs-lookup"><span data-stu-id="1086e-104">A summary of the steps necessary to add EF Core to your application in different platforms and popular IDEs.</span></span>

## <a name="step-by-step-tutorials"></a><span data-ttu-id="1086e-105">단계별 자습서</span><span class="sxs-lookup"><span data-stu-id="1086e-105">Step-by-step Tutorials</span></span>

<span data-ttu-id="1086e-106">이 기본 자습서에는 Entity Framework Core나 특정 IDE에 대한 사전 지식이 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="1086e-106">These introductory tutorials require no previous knowledge of Entity Framework Core or any particular IDE.</span></span> <span data-ttu-id="1086e-107">데이터베이스에서 데이터를 쿼리하여 저장하는 간단한 응용 프로그램을 만들어가면서 단계별로 안내합니다.</span><span class="sxs-lookup"><span data-stu-id="1086e-107">They will take you step-by-step through the creation of a simple application that queries and saves data from a database.</span></span> <span data-ttu-id="1086e-108">다양한 운영 체제와 응용 프로그램 유형에서 시작할 수 있는 자습서를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="1086e-108">We have provided tutorials to get you started on various operating systems and application types.</span></span>

<span data-ttu-id="1086e-109">Entity Framework Core는 기존 데이터베이스 기반 모델이나, 모델 기반 데이터베이스를 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1086e-109">Entity Framework Core can create a model based on an existing database, or create a database for you based on your model.</span></span> <span data-ttu-id="1086e-110">이 방법 모두 설명하는 자습서가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1086e-110">There are tutorials that demonstrate both of these approaches.</span></span>

* <span data-ttu-id="1086e-111">.NET Core 콘솔 앱</span><span class="sxs-lookup"><span data-stu-id="1086e-111">.NET Core Console Apps</span></span>
  * [<span data-ttu-id="1086e-112">새 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="1086e-112">New Database</span></span>](netcore/new-db-sqlite.md)
* <span data-ttu-id="1086e-113">ASP.NET Core 앱</span><span class="sxs-lookup"><span data-stu-id="1086e-113">ASP.NET Core Apps</span></span>
  * [<span data-ttu-id="1086e-114">새 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="1086e-114">New Database</span></span>](aspnetcore/new-db.md)
  * [<span data-ttu-id="1086e-115">기존 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="1086e-115">Existing Database</span></span>](aspnetcore/existing-db.md)
  * [<span data-ttu-id="1086e-116">EF Core 및 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="1086e-116">EF Core and Razor Pages</span></span>](/aspnet/core/data/ef-rp/intro)
* <span data-ttu-id="1086e-117">UWP(유니버설 Windows 플랫폼) 앱</span><span class="sxs-lookup"><span data-stu-id="1086e-117">Universal Windows Platform (UWP) Apps</span></span>
  * [<span data-ttu-id="1086e-118">새 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="1086e-118">New Database</span></span>](uwp/getting-started.md)
* <span data-ttu-id="1086e-119">.NET Framework 앱</span><span class="sxs-lookup"><span data-stu-id="1086e-119">.NET Framework Apps</span></span>
  * [<span data-ttu-id="1086e-120">새 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="1086e-120">New Database</span></span>](full-dotnet/new-db.md)
  * [<span data-ttu-id="1086e-121">기존 데이터베이스</span><span class="sxs-lookup"><span data-stu-id="1086e-121">Existing Database</span></span>](full-dotnet/existing-db.md)

> [!NOTE]  
> <span data-ttu-id="1086e-122">이 자습서와 함께 제공되는 샘플은 EF Core 2.1을 사용하도록 업데이트되었습니다.</span><span class="sxs-lookup"><span data-stu-id="1086e-122">These tutorials and the accompanying samples have been updated to use EF Core 2.1.</span></span> <span data-ttu-id="1086e-123">그러나 대부분의 경우 명령을 약간만 수정하면 이전 릴리스를 사용하는 응용 프로그램을 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1086e-123">However, in the majority of cases it should be possible to create applications that use previous releases, with minimal modification to the instructions.</span></span> 
