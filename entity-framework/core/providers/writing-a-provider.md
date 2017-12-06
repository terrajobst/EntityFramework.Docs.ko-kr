---
title: "데이터베이스 공급자-EF 코어 작성"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="0e08b-102">데이터베이스 공급자 작성</span><span class="sxs-lookup"><span data-stu-id="0e08b-102">Writing a Database Provider</span></span>

<span data-ttu-id="0e08b-103">Entity Framework Core 데이터베이스 공급자를 작성 하는 방법에 대 한 정보를 참조 하십시오. [EF 핵심 공급자를 작성 하려고](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) 여 [Arthur Vickers](https://github.com/ajcvickers)합니다.</span><span class="sxs-lookup"><span data-stu-id="0e08b-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

<span data-ttu-id="0e08b-104">EF 핵심 코드 베이스는 오픈 소스 이며 대 한 참조로 사용할 수 있는 몇 가지 데이터베이스 공급자를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e08b-104">The EF Core code base is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="0e08b-105">Https://github.com/aspnet/EntityFramework에서 소스 코드를 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0e08b-105">You can find the source code at https://github.com/aspnet/EntityFramework.</span></span>

## <a name="the-providers-beware-label"></a><span data-ttu-id="0e08b-106">공급자 조심 레이블</span><span class="sxs-lookup"><span data-stu-id="0e08b-106">The providers-beware label</span></span>

<span data-ttu-id="0e08b-107">공급자에 대 한 작업을 시작한 경우에 대 한 감시는 [ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware) 우리의 GitHub 문제와 끌어오기 요청에는 레이블.</span><span class="sxs-lookup"><span data-stu-id="0e08b-107">Once you begin work on a provider, watch for the [`providers-beware`](https://github.com/aspnet/EntityFramework/labels/providers-beware) label on our GitHub issues and pull requests.</span></span> <span data-ttu-id="0e08b-108">공급자 기록기에 영향을 줄 수 있는 변경 내용을 식별이 레이블을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="0e08b-108">We use this label to identify changes that may impact provider writers.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="0e08b-109">타사 공급자의 이름을 지정 하는 제안 된</span><span class="sxs-lookup"><span data-stu-id="0e08b-109">Suggested naming of third party providers</span></span>

<span data-ttu-id="0e08b-110">NuGet 패키지에 대 한 다음과 같은 명명을 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="0e08b-110">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="0e08b-111">EF 핵심 팀에서 제공 하는 패키지의 이름으로 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0e08b-111">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="0e08b-112">예:</span><span class="sxs-lookup"><span data-stu-id="0e08b-112">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
