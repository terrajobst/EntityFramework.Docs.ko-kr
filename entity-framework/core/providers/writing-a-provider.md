---
title: 데이터베이스 공급자 작성-EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 2d9e4a6cdfda80d7dfcfb6e7bf0480eb49f8e057
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414808"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="5a795-102">데이터베이스 공급자 작성</span><span class="sxs-lookup"><span data-stu-id="5a795-102">Writing a Database Provider</span></span>

<span data-ttu-id="5a795-103">Entity Framework Core 데이터베이스 공급자를 작성 하는 방법에 대 한 자세한 내용은를 참조 하십시오 .이를 통해 [Arthur Vickers](https://github.com/ajcvickers)에서 [EF Core 공급자를 작성](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) 합니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="5a795-104">이러한 게시물은 1.1 EF Core 이후에 업데이트 되지 않으며, 해당 시간 [681](https://github.com/dotnet/EntityFramework.Docs/issues/681) 이이 설명서에 대 한 업데이트를 추적 하 고 있으므로 중요 한 변경 사항이 있었습니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="5a795-105">EF Core codebase는 오픈 소스 이며 참조로 사용할 수 있는 여러 데이터베이스 공급자를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="5a795-106"><https://github.com/aspnet/EntityFrameworkCore>에서 소스 코드를 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-106">You can find the source code at <https://github.com/aspnet/EntityFrameworkCore>.</span></span> <span data-ttu-id="5a795-107">[Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact)등 일반적으로 사용 되는 타사 공급자에 대 한 코드를 살펴보는 것도 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="5a795-108">특히, 이러한 프로젝트는 NuGet에 게시 하는 기능 테스트를 확장 하 고 실행 하도록 설정 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="5a795-109">이러한 종류의 설정은 적극 권장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="5a795-110">공급자 변경 내용에 대 한 최신 상태 유지</span><span class="sxs-lookup"><span data-stu-id="5a795-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="5a795-111">2\.1 릴리스 후의 작업부터 공급자 코드에서 해당 변경 내용이 필요할 수 있는 [변경 로그](provider-log.md) 를 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="5a795-112">이는 EF Core의 새 버전에서 작동 하도록 기존 공급자를 업데이트할 때 도움을 주기 위한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="5a795-113">2\.1 이전에는 [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) 를 사용 하 고 GitHub 문제에 대 한 레이블 및 [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) 유사한 목적으로 요청을 끌어옵니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="5a795-114">지정 된 릴리스의 작업 항목에는 공급자에서 작업을 수행 해야 할 수도 있음을 나타내는 데 이러한 레이블에 on 문제를 continiue 합니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="5a795-115">`providers-beware` 레이블은 일반적으로 작업 항목의 구현이 공급자를 중단할 수 있는 반면, `providers-fyi` 레이블은 일반적으로 공급자가 손상 되지 않는다는 것을 의미 하지만 새 기능을 사용 하도록 설정 하는 등의 방법으로 코드를 변경 해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="5a795-116">타사 공급자의 제안 된 이름 지정</span><span class="sxs-lookup"><span data-stu-id="5a795-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="5a795-117">NuGet 패키지에 다음 이름을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="5a795-118">이는 EF Core 팀에서 제공 하는 패키지의 이름과 일치 합니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="5a795-119">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="5a795-119">For example:</span></span>

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
