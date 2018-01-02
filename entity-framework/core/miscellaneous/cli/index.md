---
title: "명령줄 참조 - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 11/6/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 076e9251850ba10df323cd25922aa8b95b3a5491
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
<a name="entity-framework-core-tools"></a><span data-ttu-id="0e066-102">Entity Framework Core 도구</span><span class="sxs-lookup"><span data-stu-id="0e066-102">Entity Framework Core Tools</span></span>
===========================
<span data-ttu-id="0e066-103">Entity Framework Core 도구를 사용하면 EF Core 앱 개발 과정에 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-103">The Entity Framework Core Tools help you during the development of EF Core apps.</span></span> <span data-ttu-id="0e066-104">주로 데이터베이스의 스키마에 대한 리버스 엔지니어링을 통해 DbContext와 엔터티 형식을 스캐폴드하고 마이그레이션을 관리합니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-104">They're primarily used to scaffold a DbContext and entity types by reverse engineering the schema of a database, and to manage Migrations.</span></span>

<span data-ttu-id="0e066-105">[EF Core PMC(Package Manager Console) 도구][1]는 Visual Studio 내부에서 뛰어난 환경을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-105">The [EF Core Package Manager Console (PMC) Tools][1] provide a superior experience inside Visual Studio.</span></span> <span data-ttu-id="0e066-106">NuGet의 [PMC(Package Manager Console)][2]를 사용하여 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-106">Run them using NuGet's [Package Manager Console][2].</span></span> <span data-ttu-id="0e066-107">이러한 도구는 .NET Core와 .NET Framework 프로젝트 모두에서 작동합니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="0e066-108">[EF Core .NET 명령줄 도구][3]는 [.NET Core CLI(명령줄) 도구][4]의 확장으로, 여러 플랫폼에서 사용하고 Visual Studio 외부에서 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-108">The [EF Core .NET Command-line Tools][3] are an extension to the [.NET Core command-line interface (CLI) tools][4] that are cross-platform and can run outside of Visual Studio.</span></span> <span data-ttu-id="0e066-109">이러한 도구에는 .NET Core SDK 프로젝트(`Sdk="Microsoft.NET.Sdk"` 또는 프로젝트 파일에서 유사 항목)가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-109">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="0e066-110">두 도구 모두 동일한 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-110">Both tools expose the same functionality.</span></span> <span data-ttu-id="0e066-111">Visual Studio에서 개발할 경우 더 통합적인 환경을 제공하는 PMC 도구를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-111">If you're developing in Visual Studio, we recommend using the PMC Tools since they provide a more integrated experience.</span></span>

<a name="frameworks"></a><span data-ttu-id="0e066-112">프레임워크</span><span class="sxs-lookup"><span data-stu-id="0e066-112">Frameworks</span></span>
----------
<span data-ttu-id="0e066-113">이 도구는 .NET Framework 또는 .NET Core 대상 프로젝트를 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-113">The tools support projects targeting .NET Framework or .NET Core.</span></span>

<span data-ttu-id="0e066-114">다른 프레임워크(예: 유니버설 Windows 또는 Xamarin)를 대상으로 하는 프로젝트의 경우 별도의 .NET Standard 프로젝트를 만들고, 지원되는 프레임워크 중 하나에 대한 교차 대상을 지정하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-114">If your project targets another framework (for example, Universal Windows or Xamarin), we recommend creating a separate .NET Standard project and cross-targeting one of the supported frameworks.</span></span>

<span data-ttu-id="0e066-115">.NET Core에서 교차 대상을 지정하려면 프로젝트를 마우스 오른쪽 단추로 클릭하고 **\*.csproj 편집** 등을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-115">To cross-target .NET Core, for example, right-click on the project and select **Edit \*.csproj**.</span></span> <span data-ttu-id="0e066-116">다음과 같이 `TargetFramework` 속성을 업데이트합니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-116">Update the `TargetFramework` property as follows.</span></span> <span data-ttu-id="0e066-117">단, 속성 이름 복수가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-117">(Note, the property name becomes plural.)</span></span>

``` xml
<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>
```

<span data-ttu-id="0e066-118">.NET Standard 클래스 라이브러리를 사용할 경우 시작 프로젝트가 .NET Framework 또는 .NET Core를 대상으로 할 때 교차 대상을 지정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-118">If you're using a .NET Standard class library, you don't need to cross-target if your startup project targets .NET Framework or .NET Core.</span></span>

<a name="startup-and-target-projects"></a><span data-ttu-id="0e066-119">시작 및 대상 프로젝트</span><span class="sxs-lookup"><span data-stu-id="0e066-119">Startup and Target Projects</span></span>
---------------------------
<span data-ttu-id="0e066-120">명령을 호출할 때마다 대상 프로젝트와 시작 프로젝트 등의 두 프로젝트가 관련됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-120">Whenever you invoke a command, there are two projects involved: the target project and the startup project.</span></span>

<span data-ttu-id="0e066-121">대상 프로젝트에는 모든 파일이 추가됩니다(또는 일부 경우 제거됨).</span><span class="sxs-lookup"><span data-stu-id="0e066-121">The target project is where any files are added (or in some cases removed).</span></span>

<span data-ttu-id="0e066-122">시작 프로젝트는 프로젝트 코드를 실행할 때 도구가 에뮬레이트하는 프로젝트입니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-122">The startup project is the one emulated by the tools when executing your project's code.</span></span>

<span data-ttu-id="0e066-123">대상 프로젝트와 시작 포르젝트가 동일할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e066-123">Both the target project and the startup project can be the same.</span></span>


  [1]: powershell.md
  [2]: https://docs.microsoft.com/nuget/tools/package-manager-console
  [3]: dotnet.md
  [4]: https://docs.microsoft.com/dotnet/core/tools/
