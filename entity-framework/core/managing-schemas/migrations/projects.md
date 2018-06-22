---
title: 여러 프로젝트-EF 코어 마이그레이션
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3684e86cce0005056380d89604d038c734054d14
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/22/2017
ms.locfileid: "27161229"
---
<a name="using-a-separate-project"></a><span data-ttu-id="1a67b-102">별도 프로젝트를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="1a67b-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="1a67b-103">마이그레이션을 포함 하는 하나의 보다 다른 어셈블리에 저장 하려는 경우 사용자 `DbContext`합니다.</span><span class="sxs-lookup"><span data-stu-id="1a67b-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="1a67b-104">또한 릴리스 간 업그레이드에 대 한 예를 들어 여러 집합이 마이그레이션 유지 하기 위해이 전략, 개발에 대 한 이며 다른 하나를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a67b-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="1a67b-105">수행 작업</span><span class="sxs-lookup"><span data-stu-id="1a67b-105">To do this...</span></span>

1. <span data-ttu-id="1a67b-106">새 클래스 라이브러리를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="1a67b-106">Create a new class library.</span></span>

2. <span data-ttu-id="1a67b-107">DbContext 어셈블리에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a67b-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="1a67b-108">클래스 라이브러리에는 마이그레이션 및 모델 스냅숏 파일을 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a67b-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="1a67b-109">모든 추가 않은 DbContext 프로젝트에 하나를 추가한 다음 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a67b-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="1a67b-110">마이그레이션 어셈블리를 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a67b-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="1a67b-111">시작 어셈블리에서 마이그레이션 어셈블리에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a67b-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="1a67b-112">이렇게 하면 순환 종속성을 하는 경우 클래스 라이브러리의 출력 경로 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="1a67b-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="1a67b-113">올바르게 모든 작업을 수행한 경우에 새 마이그레이션 프로젝트에 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1a67b-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
