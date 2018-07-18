---
title: EF6 및 EF Core - 동일한 응용 프로그램에서 사용
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: ead251c5454473738c2f2bfdac6557aa3e1c5591
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949079"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="b6aa0-102">동일한 응용 프로그램에서 EF Core 및 EF6 사용</span><span class="sxs-lookup"><span data-stu-id="b6aa0-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="b6aa0-103">NuGet 패키지를 둘 다 설치하여 동일한 .NET Framework 응용 프로그램 또는 라이브러리에서 EF Core 및 EF6를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6aa0-103">It is possible to use EF Core and EF6 in the same .NET Framework application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="b6aa0-104">일부 유형은 EF Core 및 EF6에서 같은 이름을 가지며 네임스페이스만 다르므로 동일한 코드 파일에서 EF Core 및 EF6를 둘 다 사용하는 것이 이해하기 어려울 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6aa0-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="b6aa0-105">네임스페이스 별칭 지시문을 사용하면 모호함을 쉽게 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6aa0-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="b6aa0-106">예:</span><span class="sxs-lookup"><span data-stu-id="b6aa0-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="b6aa0-107">여러 EF 모델이 포함된 기존 응용 프로그램을 포팅하는 경우 모델 중 일부를 선택적으로 포팅하도록 선택하고 다른 모델에는 EF6를 계속 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b6aa0-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
