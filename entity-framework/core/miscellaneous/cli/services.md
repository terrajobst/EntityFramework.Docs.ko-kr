---
title: 디자인 타임 서비스-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655830"
---
# <a name="design-time-services"></a><span data-ttu-id="a4c46-102">디자인 타임 서비스</span><span class="sxs-lookup"><span data-stu-id="a4c46-102">Design-time services</span></span>

<span data-ttu-id="a4c46-103">도구에서 사용 하는 일부 서비스는 디자인 타임에만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a4c46-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="a4c46-104">이러한 서비스는 앱과 함께 배포 되는 것을 방지 하기 위해 EF Core의 런타임 서비스와는 별도로 관리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a4c46-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="a4c46-105">이러한 서비스 중 하나를 재정의 하려면 (예: 마이그레이션 파일을 생성 하는 서비스) 시작 프로젝트에 `IDesignTimeServices`의 구현을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="a4c46-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
