---
title: EF Core 데이터베이스 공급자-작성
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993668"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="af6af-102">데이터베이스 공급자 작성</span><span class="sxs-lookup"><span data-stu-id="af6af-102">Writing a Database Provider</span></span>

<span data-ttu-id="af6af-103">Entity Framework Core 데이터베이스 공급자를 작성 하는 방법에 대 한 내용은 [EF Core 공급자를 작성 하려고](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) 하 여 [Arthur Vickers](https://github.com/ajcvickers)합니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="af6af-104">이러한 게시물 EF Core 1.1부터 업데이트 되지 않은 하 고 해당 시점 이후에 중요 한 변경 내용이 [문제 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) 이 설명서에 업데이트를 추적 합니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="af6af-105">EF Core 코드 베이스는 오픈 소스 이며 참조로 사용할 수 있는 몇 가지 데이터베이스 공급자를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="af6af-106">소스 코드를 찾을 수 있습니다 https://github.com/aspnet/EntityFrameworkCore합니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-106">You can find the source code at https://github.com/aspnet/EntityFrameworkCore.</span></span> <span data-ttu-id="af6af-107">같은 자주 사용 되는 타사 공급자에 대 한 코드에서 확인 하면 도움이 될 수도 [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)하십시오 [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), 및 [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)합니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="af6af-108">특히, 이러한 프로젝트는 설치 프로그램에서 확장 하 고 NuGet에 게시 하는 기능 테스트를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="af6af-109">이 유형의 설치 프로그램을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="af6af-110">공급자 변경 내용을 최신 상태로 유지</span><span class="sxs-lookup"><span data-stu-id="af6af-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="af6af-111">2.1 릴리스 후 작업을 사용 하 여부터 만들었습니다를 [변경 로그](provider-log.md) 공급자 코드에서 해당 변경이 필요한 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="af6af-112">이 기존 공급자를 업데이트 하는 경우 EF Core의 새 버전으로 작업 하도록 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="af6af-113">2.1 이전 버전에서는 사용 합니다 [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) 하 고 [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) 이 GitHub 문제에 비슷한 목적을 위해 끌어오기 요청 레이블을 합니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="af6af-114">Continiue 특정 릴리스에서 어떤 작업 항목 공급자에서 수행 될 작업을 할 가늠할 수 있도록 문제에 이러한 레이블에 사용 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="af6af-115">A `providers-beware` 레이블을 작업 항목의 구현 공급자 중단 될 수는 일반적으로 의미 하는 동안는 `providers-fyi` 레이블 공급자 중단 되지 않지만 코드를 변경할지, 예를 들어, 새 기능을 사용 하도록 설정 해야 할 수는 일반적으로 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="af6af-116">타사 공급자의 이름 지정 제안</span><span class="sxs-lookup"><span data-stu-id="af6af-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="af6af-117">NuGet 패키지에 대 한 다음과 같은 명명을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="af6af-118">EF Core 팀에서 제공 하는 패키지의 이름으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af6af-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="af6af-119">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="af6af-119">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
