---
title: 별도의 마이그레이션 프로젝트 사용-EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 89b7f50fe750c2953aa75efcdffcb1a5199ce90c
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824416"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="6fa80-102">별도의 마이그레이션 프로젝트 사용</span><span class="sxs-lookup"><span data-stu-id="6fa80-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="6fa80-103">`DbContext`를 포함 하는 어셈블리와 다른 어셈블리에 마이그레이션을 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="6fa80-104">또한이 전략을 사용 하 여 여러 마이그레이션 집합을 유지 관리할 수 있습니다. 예를 들어 개발 및 릴리스 간 업그레이드를 위한 여러 마이그레이션 집합을 유지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="6fa80-105">수행 작업</span><span class="sxs-lookup"><span data-stu-id="6fa80-105">To do this...</span></span>

1. <span data-ttu-id="6fa80-106">새 클래스 라이브러리를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-106">Create a new class library.</span></span>

2. <span data-ttu-id="6fa80-107">DbContext 어셈블리에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="6fa80-108">마이그레이션 및 모델 스냅숏 파일을 클래스 라이브러리로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="6fa80-109">기존 마이그레이션이 없는 경우 DbContext를 포함 하는 프로젝트에서이를 생성 한 후 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="6fa80-110">마이그레이션 어셈블리에 기존 마이그레이션이 포함 되어 있지 않은 경우에는 마이그레이션 추가 명령이 DbContext을 찾을 수 없기 때문에이는 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="6fa80-111">마이그레이션 어셈블리를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="6fa80-112">시작 어셈블리에서 마이그레이션 어셈블리에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="6fa80-113">이로 인해 순환 종속성이 발생 하는 경우 클래스 라이브러리의 출력 경로를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="6fa80-114">모든 작업을 올바르게 수행한 경우 프로젝트에 새 마이그레이션을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6fa80-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="6fa80-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6fa80-115">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="6fa80-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6fa80-116">Visual Studio</span></span>](#tab/vs)

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
