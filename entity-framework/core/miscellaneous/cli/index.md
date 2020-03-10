---
title: Entity Framework Core 도구 참조 - EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 237192c55ea3542521a7a292ac8550d72e4ef82c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412818"
---
# <a name="entity-framework-core-tools-reference"></a><span data-ttu-id="25d9c-102">Entity Framework Core 도구 참조</span><span class="sxs-lookup"><span data-stu-id="25d9c-102">Entity Framework Core tools reference</span></span>

<span data-ttu-id="25d9c-103">Entity Framework Core 도구는 디자인 타임 개발 작업에 도움이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="25d9c-103">The Entity Framework Core tools help with design-time development tasks.</span></span> <span data-ttu-id="25d9c-104">주로 마이그레이션을 관리하고 데이터베이스의 스키마에 대한 리버스 엔지니어링을 통해 `DbContext`와 엔터티 형식을 스캐폴드하는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="25d9c-104">They're primarily used to manage Migrations and to scaffold a `DbContext` and entity types by reverse engineering the schema of a database.</span></span>

* <span data-ttu-id="25d9c-105">[EF Core 패키지 관리자 콘솔 도구](powershell.md)는 Visual Studio의 [패키지 관리자 콘솔](https://docs.microsoft.com/nuget/tools/package-manager-console)에서 실행됩니다.</span><span class="sxs-lookup"><span data-stu-id="25d9c-105">The [EF Core Package Manager Console tools](powershell.md) run in the [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console) in Visual Studio.</span></span>

* <span data-ttu-id="25d9c-106">[EF Core .NET CLI(명령줄 인터페이스) 도구](dotnet.md)는 플랫폼 간 [.NET Core CLI 도구](https://docs.microsoft.com/dotnet/core/tools/)의 확장입니다.</span><span class="sxs-lookup"><span data-stu-id="25d9c-106">The [EF Core .NET command-line interface (CLI) tools](dotnet.md) are an extension to the cross-platform [.NET Core CLI tools](https://docs.microsoft.com/dotnet/core/tools/).</span></span> <span data-ttu-id="25d9c-107">이러한 도구에는 .NET Core SDK 프로젝트(`Sdk="Microsoft.NET.Sdk"` 또는 프로젝트 파일에서 유사 항목)가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="25d9c-107">These tools require a .NET Core SDK project (one with `Sdk="Microsoft.NET.Sdk"` or similar in the project file).</span></span>

<span data-ttu-id="25d9c-108">두 도구 모두 동일한 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="25d9c-108">Both tools expose the same functionality.</span></span> <span data-ttu-id="25d9c-109">Visual Studio에서 개발할 경우 더 통합적인 환경을 제공하는 **패키지 관리자 콘솔** 도구를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="25d9c-109">If you're developing in Visual Studio, we recommend using the **Package Manager Console** tools since they provide a more integrated experience.</span></span>

## <a name="next-steps"></a><span data-ttu-id="25d9c-110">다음 단계</span><span class="sxs-lookup"><span data-stu-id="25d9c-110">Next steps</span></span>

* [<span data-ttu-id="25d9c-111">EF Core 패키지 관리자 콘솔 도구 참조</span><span class="sxs-lookup"><span data-stu-id="25d9c-111">EF Core Package Manager Console tools reference</span></span>](powershell.md)
* [<span data-ttu-id="25d9c-112">EF Core .NET CLI 도구 참조</span><span class="sxs-lookup"><span data-stu-id="25d9c-112">EF Core .NET CLI tools reference</span></span>](dotnet.md)
