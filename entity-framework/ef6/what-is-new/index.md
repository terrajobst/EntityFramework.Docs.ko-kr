---
title: 새로운 기능 - EF6
author: divega
ms.date: 09/12/2019
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
uid: ef6/what-is-new/index
ms.openlocfilehash: c49f4cba0066d1e218f11c3959d96f9cafa913f4
ms.sourcegitcommit: 7bc43f21e7bdd64926314ea949aae689f1911956
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266790"
---
# <a name="whats-new-in-ef6"></a><span data-ttu-id="e6fa5-102">EF6의 새로운 기능</span><span class="sxs-lookup"><span data-stu-id="e6fa5-102">What's new in EF6</span></span>

<span data-ttu-id="e6fa5-103">최신 기능 및 높은 안정성을 얻을 수 있도록 Entity Framework 최신 릴리스 버전을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-103">We highly recommend that you use the latest released version of Entity Framework to ensure you get the latest features and the highest stability.</span></span>
<span data-ttu-id="e6fa5-104">하지만 이전 버전을 사용해야 하거나 최신 시험판 릴리스의 개선된 기능을 사용해 보려는 분들도 있다는 사실을 잘 알고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-104">However, we realize that you may need to use a previous version, or that you may want to experiment with new improvements in the latest pre-release.</span></span>
<span data-ttu-id="e6fa5-105">EF 특정 버전을 설치하는 방법은 [Entity Framework 받기](~/ef6/fundamentals/install.md)를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-105">To install specific versions of EF, see [Get Entity Framework](~/ef6/fundamentals/install.md).</span></span>

## <a name="ef-630"></a><span data-ttu-id="e6fa5-106">EF 6.3.0</span><span class="sxs-lookup"><span data-stu-id="e6fa5-106">EF 6.3.0</span></span>

<span data-ttu-id="e6fa5-107">EF 6.3.0 런타임은 2019년 9월에 NuGet에서 출시되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-107">The EF 6.3.0 runtime was released to NuGet in September 2019.</span></span> <span data-ttu-id="e6fa5-108">이 릴리스의 주요 목표는 EF 6을 사용하는 기존 애플리케이션을 .NET Core 3.0으로 손쉽게 마이그레이션하는 것이었습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-108">The main goal of this release was to facilitate migrating existing applications that use EF 6 to .NET Core 3.0.</span></span> <span data-ttu-id="e6fa5-109">커뮤니티 역시 여러 버그 수정과 기능 향상에 기여했습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-109">The community has also contributed several bug fixes and enhancements.</span></span> <span data-ttu-id="e6fa5-110">자세한 내용은 각 6.3.0 [마일스톤](https://github.com/aspnet/EntityFramework6/milestones?state=closed)에서 종결된 문제를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-110">See the issues closed in each 6.3.0 [milestone](https://github.com/aspnet/EntityFramework6/milestones?state=closed) for details.</span></span> <span data-ttu-id="e6fa5-111">다음은 몇 가지 주목할 만한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-111">Here are some of the more notable ones:</span></span>

- <span data-ttu-id="e6fa5-112">.NET Core 3.0 지원</span><span class="sxs-lookup"><span data-stu-id="e6fa5-112">Support for .NET Core 3.0</span></span>
  - <span data-ttu-id="e6fa5-113">이제 EntityFramework 패키지는 .NET Framework 4.x. 외에도 .NET Standard 2.1을 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-113">The EntityFramework package now targets .NET Standard 2.1 in addition to .NET Framework 4.x.</span></span>
  - <span data-ttu-id="e6fa5-114">즉, EF 6.3은 플랫폼 간이며 Windows 외에도 Linux 및 macOS와 같은 다른 운영 체제에서도 지원됨을 의미합니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-114">This means that EF 6.3 is cross-platform and supported on other operating systems besides Windows, like Linux and macOS.</span></span>
  - <span data-ttu-id="e6fa5-115">마이그레이션 명령이 프로세스에서 실행되고 SDK 스타일 프로젝트에서 사용할 수 있도록 다시 작성되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-115">The migrations commands have been rewritten to execute out of process and work with SDK-style projects.</span></span>
- <span data-ttu-id="e6fa5-116">SQL Server HierarchyId 지원</span><span class="sxs-lookup"><span data-stu-id="e6fa5-116">Support for SQL Server HierarchyId</span></span>
- <span data-ttu-id="e6fa5-117">Roslyn 및 NuGet PackageReference와의 호환성이 향상되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-117">Improved compatibility with Roslyn and NuGet PackageReference</span></span>
- <span data-ttu-id="e6fa5-118">어셈블리로부터 마이그레이션을 사용, 추가, 스크립팅 및 적용하기 위한 `ef6.exe` 유틸리티를 추가했습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-118">Added `ef6.exe` utility for enabling, adding, scripting, and applying migrations from assemblies.</span></span> <span data-ttu-id="e6fa5-119">이는 `migrate.exe`를 대체합니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-119">This replaces `migrate.exe`</span></span>

### <a name="ef-designer-support"></a><span data-ttu-id="e6fa5-120">EF 디자이너 지원</span><span class="sxs-lookup"><span data-stu-id="e6fa5-120">EF designer support</span></span>

<span data-ttu-id="e6fa5-121">현재 .NET Core 또는 .NET Standard 프로젝트에서 직접 EF 디자이너를 사용하는 것은 지원되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-121">There's currently no support for using the EF designer directly on .NET Core or .NET Standard projects.</span></span> 

<span data-ttu-id="e6fa5-122">동일한 솔루션의 .NET Core 3.0 또는 .NET Standard 2.1 프로젝트에 EDMX 파일 및 엔터티와 DbContext에 대해 생성된 클래스를 연결 파일로 추가하여 이 제한을 해결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-122">You can work around this limitation by adding the EDMX file and the generated classes for the entities and the DbContext as linked files to a .NET Core 3.0 or .NET Standard 2.1 project in the same solution.</span></span>

<span data-ttu-id="e6fa5-123">연결 파일은 프로젝트 파일에서 다음과 같이 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-123">The linked files will look like this in the project file:</span></span>

``` csproj 
<ItemGroup>
  <EntityDeploy Include="..\EdmxDesignHost\Entities.edmx" Link="Model\Entities.edmx" />
  <Compile Include="..\EdmxDesignHost\Entities.Context.cs" Link="Model\Entities.Context.cs" />
  <Compile Include="..\EdmxDesignHost\Thing.cs" Link="Model\Thing.cs" />
  <Compile Include="..\EdmxDesignHost\Person.cs" Link="Model\Person.cs" />
</ItemGroup>
```

<span data-ttu-id="e6fa5-124">EDMX 파일은 EntityDeploy 빌드 작업과 연결됩니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-124">Note that the EDMX file is linked with the EntityDeploy build action.</span></span> <span data-ttu-id="e6fa5-125">이 작업은 EF 모델을 대상 어셈블리에 포함 리소스로 추가하거나 EDMX의 메타데이터 아티팩트 처리 설정에 따라 출력 폴더의 파일로 복사하는 특수 MSBuild 작업(현재 EF 6.3 패키지에 포함됨)입니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-125">This is a special MSBuild task (now included in the EF 6.3 package) that takes care of adding the EF model into the target assembly as embedded resources (or copying it as files in the output folder, depending on the Metadata Artifact Processing setting in the EDMX).</span></span> <span data-ttu-id="e6fa5-126">설정하는 방법에 대한 자세한 내용은 [EDMX .NET Core 샘플](https://aka.ms/EdmxDotNetCoreSample)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-126">For more details on how to get this set up, see our [EDMX .NET Core sample](https://aka.ms/EdmxDotNetCoreSample).</span></span>

## <a name="past-releases"></a><span data-ttu-id="e6fa5-127">이전 릴리스</span><span class="sxs-lookup"><span data-stu-id="e6fa5-127">Past Releases</span></span>

<span data-ttu-id="e6fa5-128">[이전 릴리스](past-releases.md) 페이지에는 모든 EF 이전 버전 및 각 릴리스에 도입된 주요 기능 아카이브가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e6fa5-128">The [Past Releases](past-releases.md) page contains an archive of all previous versions of EF and the major features that were introduced on each release.</span></span>
