---
title: 여러 프로젝트-EF Core 마이그레이션
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997989"
---
<a name="using-a-separate-project"></a><span data-ttu-id="53c54-102">별도 프로젝트 사용</span><span class="sxs-lookup"><span data-stu-id="53c54-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="53c54-103">마이그레이션을 포함 하는 하나의 보다 다른 어셈블리에 저장 하려는 경우에 `DbContext`입니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="53c54-104">또한 릴리스 간 업그레이드에 대 한 예를 들어 여러 마이그레이션 집합을 유지 하기 위해이 전략, 개발에 대 한 용이고 다른 하나를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="53c54-105">수행 작업</span><span class="sxs-lookup"><span data-stu-id="53c54-105">To do this...</span></span>

1. <span data-ttu-id="53c54-106">새 클래스 라이브러리를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-106">Create a new class library.</span></span>

2. <span data-ttu-id="53c54-107">DbContext 어셈블리에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="53c54-108">클래스 라이브러리에는 마이그레이션 및 모델 스냅숏 파일을 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="53c54-109">기존 마이그레이션이 경우 DbContext를 포함 하는 프로젝트에서 생성 한 다음 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="53c54-110">추가 마이그레이션 명령을 DbContext를 찾을 수 없게 됩니다 마이그레이션 어셈블리에는 기존 마이그레이션을 찾을 수 없는 경우이 중요 합니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="53c54-111">마이그레이션 어셈블리를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="53c54-112">시작 어셈블리에서 마이그레이션 어셈블리에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="53c54-113">이렇게 하면 순환 종속성을 클래스 라이브러리의 출력 경로 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="53c54-114">올바르게 모든 작업을 수행한 경우 새 마이그레이션 프로젝트에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="53c54-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
