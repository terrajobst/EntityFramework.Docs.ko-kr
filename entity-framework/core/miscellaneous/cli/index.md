---
title: 명령줄 참조 - EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/06/2017
ms.openlocfilehash: d43b01fc61bb1c9b678e12e41c27d7efe9a59fa5
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490365"
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="d89ab-102">Entity Framework Core 도구</span><span class="sxs-lookup"><span data-stu-id="d89ab-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="d89ab-103">Entity Framework Core 도구를 사용하면 EF Core 앱 개발 과정에 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="d89ab-104">주로 데이터베이스의 스키마에 대한 리버스 엔지니어링을 통해 DbContext와 엔터티 형식을 스캐폴드하고 마이그레이션을 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="d89ab-105">[EF Core PMC(Package Manager Console) 도구][1]는 Visual Studio 내부에서 뛰어난 환경을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="d89ab-106">NuGet의 [PMC(Package Manager Console)][2]를 사용하여 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="d89ab-107">이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="d89ab-108">[EF Core .NET 명령줄 도구][3]는 [.NET Core CLI(명령줄) 도구][4]의 확장으로, 여러 플랫폼에서 사용하고 Visual Studio 외부에서 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="d89ab-109">이러한 도구에는 .NET Core SDK 프로젝트(`Sdk="Microsoft.NET.Sdk"` 또는 프로젝트 파일에서 유사 항목)가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="d89ab-110">두 도구 모두 동일한 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="d89ab-111">Visual Studio에서 개발할 경우 더 통합적인 환경을 제공하는 PMC 도구를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="d89ab-112">프레임워크</span><span class="sxs-lookup"><span data-stu-id="d89ab-112">Frameworks</span></span>
----------
<span data-ttu-id="d89ab-113">이 도구는 .NET Framework 또는 .NET Core 대상 프로젝트를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="d89ab-114">클래스 라이브러리를 사용하려면 가능한 경우 .NET Core 또는 .NET Framework 클래스 라이브러리를 사용하는 것을 고려합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-114">If you want to use a class library, then consider using a .NET Core or .NET Framework class library if possible.</span></span> <span data-ttu-id="d89ab-115">그러면 .NET 도구와 관련된 문제가 최소화됩니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-115">This will result in the least issues with .NET tooling.</span></span> <span data-ttu-id="d89ab-116">대신 .NET Standard 클래스 라이브러리를 사용하려면 도구에서 사용자의 클래스 라이브러리를 로드할 수 있는 구체적인 대상 플랫폼을 갖도록 .NET Framework 또는 .NET Core를 대상으로 하는 시작 프로젝트를 사용해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-116">If instead you wish to use a .NET Standard class library, then you will need to use a startup project that targets .NET Framework or .NET Core so that the tooling has a conrete target platform into which it can load your class library.</span></span> <span data-ttu-id="d89ab-117">이 시작 프로젝트는 실제 코드가 없는 임시 프로젝트일 수 있으며, 도구에 대한 대상을 제공하는 데만 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-117">This startup project can be a dummy project with no real code--it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="d89ab-118">프로젝트가 다른 프레임워크(예: 유니버설 Windows 또는 Xamarin)를 대상으로 하는 경우, 별도의 .NET Standard 클래스 라이브러리를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-118">If your project targets another framework (for example, Universal Windows or Xamarin), then you will need to create a separate .NET Standard class library.</span></span> <span data-ttu-id="d89ab-119">이 경우 도구에서 사용할 수 있는 시작 프로젝트를 만들 수 있는 위의 지침을 따릅니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-119">In this case, follow the guidance above to also create a startup project that can be used by the tooling.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="d89ab-120">시작 및 대상 프로젝트</span><span class="sxs-lookup"><span data-stu-id="d89ab-120">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="d89ab-121">명령을 호출할 때마다 대상 프로젝트와 시작 프로젝트 등의 두 프로젝트가 관련됩니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-121">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="d89ab-122">대상 프로젝트에는 모든 파일이 추가됩니다(또는 일부 경우 제거됨).</span><span class="sxs-lookup"><span data-stu-id="d89ab-122">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="d89ab-123">시작 프로젝트는 프로젝트 코드를 실행할 때 도구가 에뮬레이트하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-123">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="d89ab-124">대상 프로젝트와 시작 포르젝트가 동일할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d89ab-124">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
