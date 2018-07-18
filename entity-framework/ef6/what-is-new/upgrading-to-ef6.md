---
title: Entity Framework 6으로 업그레이드
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
caps.latest.revision: 3
ms.openlocfilehash: d22f0d686e6b8e3922d37245386af7723bf08ba1
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122783"
---
# <a name="upgrading-to-entity-framework-6"></a><span data-ttu-id="1fe92-102">Entity Framework 6으로 업그레이드</span><span class="sxs-lookup"><span data-stu-id="1fe92-102">Upgrading to Entity Framework 6</span></span>

<span data-ttu-id="1fe92-103">이전 버전의 EF 코드.NET Framework의 일부로 제공 되는 핵심 라이브러리 (주로 System.Data.Entity.dll)와 대역의 (OOB) 라이브러리 (주로 EntityFramework.dll) NuGet 패키지에 제공 간에 분할 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-103">In previous versions of EF the code was split between core libraries (primarily System.Data.Entity.dll) shipped as part of the .NET Framework and out-of-band (OOB) libraries (primarily EntityFramework.dll) shipped in a NuGet package.</span></span> <span data-ttu-id="1fe92-104">EF6 핵심 라이브러리에서 코드 가져와 OOB 라이브러리에 통합 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-104">EF6 takes the code from the core libraries and incorporates it into the OOB libraries.</span></span> <span data-ttu-id="1fe92-105">이 오픈 소스 되도록 EF를 허용 하는 데 필요한 및.NET Framework에서 서로 다른 속도로 진화 할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-105">This was necessary in order to allow EF to be made open source and for it to be able to evolve at a different pace from .NET Framework.</span></span> <span data-ttu-id="1fe92-106">이 응용 프로그램 이동된 형식에 대해 다시 작성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-106">The consequence of this is that applications will need to be rebuilt against the moved types.</span></span>

<span data-ttu-id="1fe92-107">이 EF 4.1 이상 출시 될 때 DbContext를 사용 하는 응용 프로그램에 대 한 간단 하 게 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-107">This should be straightforward for applications that make use of DbContext as shipped in EF 4.1 and later.</span></span> <span data-ttu-id="1fe92-108">좀 더 많은 작업 ObjectContext를 사용 하는 응용 프로그램에 대 한 필요 하지만 것도 어렵지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-108">A little more work is required for applications that make use of ObjectContext but it still isn’t hard to do.</span></span>

<span data-ttu-id="1fe92-109">기존 EF6 응용 프로그램을 업그레이드 하기 위해 필요한 검사는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-109">Here is a checklist of the things you need to do to upgrade an existing application to EF6.</span></span>

## <a name="1-install-the-ef6-nuget-package"></a><span data-ttu-id="1fe92-110">1. EF6 NuGet 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-110">1. Install the EF6 NuGet package</span></span>

<span data-ttu-id="1fe92-111">새 Entity Framework 6 런타임에 업그레이드 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-111">You need to upgrade to the new Entity Framework 6 runtime.</span></span>

1. <span data-ttu-id="1fe92-112">선택한 서버 프로젝트를 마우스 오른쪽 단추로 클릭 **NuGet 패키지 관리...**</span><span class="sxs-lookup"><span data-stu-id="1fe92-112">Right-click on your project and select **Manage NuGet Packages...**</span></span>  
2. <span data-ttu-id="1fe92-113">아래는 **Online** 탭을 선택 **EntityFramework** 를 클릭 하 고 **설치**</span><span class="sxs-lookup"><span data-stu-id="1fe92-113">Under the **Online** tab select **EntityFramework** and click **Install**</span></span>  
   > [!NOTE]
   > <span data-ttu-id="1fe92-114">이전 버전의 EntityFramework NuGet 패키지를 설치 하는 경우 그러면 EF6에 업그레이드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-114">If a previous version of the EntityFramework NuGet package was installed this will upgrade it to EF6.</span></span>

<span data-ttu-id="1fe92-115">또는, 패키지 관리자 콘솔에서 다음 명령을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-115">Alternatively, you can run the following command from Package Manager Console:</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a><span data-ttu-id="1fe92-116">2. System.Data.Entity.dll에 대 한 어셈블리 참조를 제거 해야</span><span class="sxs-lookup"><span data-stu-id="1fe92-116">2. Ensure that assembly references to System.Data.Entity.dll are removed</span></span>

<span data-ttu-id="1fe92-117">EF6 NuGet 패키지를 설치 하면에 대 한 프로젝트에서 System.Data.Entity에 대 한 참조를 자동으로 제거 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-117">Installing the EF6 NuGet package should automatically remove any references to System.Data.Entity from your project for you.</span></span>

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a><span data-ttu-id="1fe92-118">3. EF 6.x 코드 생성을 사용 하 여 모든 EF 디자이너 (EDMX) 모델을 교환</span><span class="sxs-lookup"><span data-stu-id="1fe92-118">3. Swap any EF Designer (EDMX) models to use EF 6.x code generation</span></span>

<span data-ttu-id="1fe92-119">EF 디자이너를 사용 하 여 만든 모델에 있는 경우에 EF6 호환 되는 코드를 생성 하도록 코드 생성 템플릿을 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-119">If you have any models created with the EF Designer, you will need to update the code generation templates to generate EF6 compatible code.</span></span>

> [!NOTE]
> <span data-ttu-id="1fe92-120">현재 가지 EF 6.x DbContext 생성기 템플릿만 Visual Studio 2012 및 2013 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-120">There are currently only EF 6.x DbContext Generator templates available for Visual Studio 2012 and 2013.</span></span>

1. <span data-ttu-id="1fe92-121">기존 코드 생성 템플릿을 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-121">Delete existing code-generation templates.</span></span> <span data-ttu-id="1fe92-122">이러한 파일 이름은 일반적으로  **\<edmx_file_name\>.tt** 하 고  **\<edmx_file_name\>합니다. Context.tt** 및 솔루션 탐색기에서 edmx 파일을 중첩할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-122">These files will typically be named **\<edmx_file_name\>.tt** and **\<edmx_file_name\>.Context.tt** and be nested under your edmx file in Solution Explorer.</span></span> <span data-ttu-id="1fe92-123">키를 눌러 솔루션 탐색기에서 템플릿을 선택할 수 있습니다 합니다 **Del** 삭제할 키입니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-123">You can select the templates in Solution Explorer and press the **Del** key to delete them.</span></span>  
   > [!NOTE]
   > <span data-ttu-id="1fe92-124">웹 사이트 프로젝트에서 하지 템플릿은 edmx 파일을 아래에 중첩 하 하지만 솔루션 탐색기에서 함께 나열 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-124">In Web Site projects the templates will not be nested under your edmx file, but listed alongside it in Solution Explorer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="1fe92-125">VB.NET 프로젝트는 중첩 된 템플릿 파일을 참조 하려면 ' 모든 파일 표시 '를 사용 하도록 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-125">In VB.NET projects you will need to enable 'Show All Files' to be able to see the nested template files.</span></span>
2. <span data-ttu-id="1fe92-126">적절 한 EF 6.x 코드 생성 템플릿을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-126">Add the appropriate EF 6.x code generation template.</span></span> <span data-ttu-id="1fe92-127">디자인 화면에 열려 EF 디자이너에서 모델 단추로 **코드 생성 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="1fe92-127">Open your model in the EF Designer, right-click on the design surface and select **Add Code Generation Item...**</span></span>
    - <span data-ttu-id="1fe92-128">(권장) 한 다음 DbContext API를 사용 하는 경우 **EF 6.x DbContext 생성기** 에서 사용할 수 있습니다 합니다 **데이터** 탭 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-128">If you are using the DbContext API (recommended) then **EF 6.x DbContext Generator** will be available under the **Data** tab.</span></span>  
      > [!NOTE]
      > <span data-ttu-id="1fe92-129">Visual Studio 2012를 사용 하는 경우이 템플릿이를 EF 6 도구를 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-129">If you are using Visual Studio 2012, you will need to install the EF 6 Tools to have this template.</span></span> <span data-ttu-id="1fe92-130">참조 [Entity Framework 가져오기](~/ef6/fundamentals/install.md) 세부 정보에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-130">See [Get Entity Framework](~/ef6/fundamentals/install.md) for details.</span></span>  

    - <span data-ttu-id="1fe92-131">ObjectContext API를 사용 하는 경우 선택 해야 합니다 **온라인** 탭 및 검색 **EF 6.x EntityObject 생성기**합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-131">If you are using the ObjectContext API then you will need to select the **Online** tab and search for **EF 6.x EntityObject Generator**.</span></span>  
3. <span data-ttu-id="1fe92-132">코드 생성 템플릿을 모든 사용자 지정을 적용 하는 경우 업데이트 된 템플릿을 다시 적용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-132">If you applied any customizations to the code generation templates you will need to re-apply them to the updated templates.</span></span>

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a><span data-ttu-id="1fe92-133">4. 사용 중인 모든 core EF 형식의 네임 스페이스를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-133">4. Update namespaces for any core EF types being used</span></span>

<span data-ttu-id="1fe92-134">DbContext와 Code First 형식에 대 한 네임 스페이스는 변경 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-134">The namespaces for DbContext and Code First types have not changed.</span></span> <span data-ttu-id="1fe92-135">즉, EF 4.1을 사용 하는 대부분의 응용 프로그램 또는 나중에 아무 것도 변경 해야 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-135">This means for many applications that use EF 4.1 or later you will not need to change anything.</span></span>

<span data-ttu-id="1fe92-136">System.Data.Entity.dll에 이전에 있던 ObjectContext과 같은 형식 새 네임 스페이스로 이동 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-136">Types like ObjectContext that were previously in System.Data.Entity.dll have been moved to new namespaces.</span></span> <span data-ttu-id="1fe92-137">즉, 업데이트 해야 하 *를 사용 하 여* 또는 *가져오기* EF6 빌드 지시문입니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-137">This means you may need to update your *using* or *Import* directives to build against EF6.</span></span>

<span data-ttu-id="1fe92-138">네임 스페이스 변경 내용에 대 한 일반 규칙 System.Data.*에서 모든 유형의 System.Data.Entity.Core.* 이동 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-138">The general rule for namespace changes is that any type in System.Data.* is moved to System.Data.Entity.Core.*.</span></span> <span data-ttu-id="1fe92-139">즉, 방금 삽입 **Entity.Core 합니다.**</span><span class="sxs-lookup"><span data-stu-id="1fe92-139">In other words, just insert **Entity.Core.**</span></span> <span data-ttu-id="1fe92-140">System.Data 후.</span><span class="sxs-lookup"><span data-stu-id="1fe92-140">after System.Data.</span></span> <span data-ttu-id="1fe92-141">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="1fe92-141">For example:</span></span>

- <span data-ttu-id="1fe92-142">System.Data.EntityException = > System.Data. **Entity.Core 합니다.** EntityException</span><span class="sxs-lookup"><span data-stu-id="1fe92-142">System.Data.EntityException => System.Data.**Entity.Core.** EntityException</span></span>  
- <span data-ttu-id="1fe92-143">System.Data.Objects.ObjectContext = > System.Data. **Entity.Core 합니다.** Objects.ObjectContext</span><span class="sxs-lookup"><span data-stu-id="1fe92-143">System.Data.Objects.ObjectContext => System.Data.**Entity.Core.** Objects.ObjectContext</span></span>  
- <span data-ttu-id="1fe92-144">System.Data.Objects.DataClasses.RelationshipManager = > System.Data. **Entity.Core 합니다.** Objects.DataClasses.RelationshipManager</span><span class="sxs-lookup"><span data-stu-id="1fe92-144">System.Data.Objects.DataClasses.RelationshipManager => System.Data.**Entity.Core.** Objects.DataClasses.RelationshipManager</span></span>  

<span data-ttu-id="1fe92-145">이러한 형식에는 *Core* 네임 스페이스는 대부분의 DbContext 기반 응용 프로그램에 대 한 직접 사용 되지 않으므로 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-145">These types are in the *Core* namespaces because they are not used directly for most DbContext-based applications.</span></span> <span data-ttu-id="1fe92-146">일부 System.Data.Entity.dll의 일부인 계속 되는 직접 및 일반적으로 DbContext 기반 응용 프로그램에 대 한 형식과 따라서 이동 되지 않은에 *Core* 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-146">Some types that were part of System.Data.Entity.dll are still used commonly and directly for DbContext-based applications and so have not been moved into the *Core* namespaces.</span></span> <span data-ttu-id="1fe92-147">이러한 항목은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-147">These are:</span></span>

- <span data-ttu-id="1fe92-148">System.Data.EntityState = > System.Data. **엔터티.** EntityState</span><span class="sxs-lookup"><span data-stu-id="1fe92-148">System.Data.EntityState => System.Data.**Entity.** EntityState</span></span>  
- <span data-ttu-id="1fe92-149">System.Data.Objects.DataClasses.EdmFunctionAttribute = > System.Data. **Entity.DbFunctionAttribute**</span><span class="sxs-lookup"><span data-stu-id="1fe92-149">System.Data.Objects.DataClasses.EdmFunctionAttribute => System.Data.**Entity.DbFunctionAttribute**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="1fe92-150">이 클래스의 이름이 바뀌었습니다. 이전 이름 가진 클래스에서 여전히 존재 하 고 작동 있지만 이제 되지 않음으로 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-150">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.</span></span>  
- <span data-ttu-id="1fe92-151">System.Data.Objects.EntityFunctions = > System.Data. **Entity.DbFunctions**</span><span class="sxs-lookup"><span data-stu-id="1fe92-151">System.Data.Objects.EntityFunctions => System.Data.**Entity.DbFunctions**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="1fe92-152">이 클래스의 이름이 바뀌었습니다. 이전 이름 가진 클래스 여전히 존재 하 고 작동 있지만 이제 사용 되지 않음으로 표시 합니다.)</span><span class="sxs-lookup"><span data-stu-id="1fe92-152">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.)</span></span>  
- <span data-ttu-id="1fe92-153">System.Data.Spatial에서 이동한 공간 클래스 (예: DbGeography, DbGeometry) = > System.Data. **엔터티.** 공간</span><span class="sxs-lookup"><span data-stu-id="1fe92-153">Spatial classes (for example, DbGeography, DbGeometry) have moved from System.Data.Spatial => System.Data.**Entity.** Spatial</span></span>

> [!NOTE]
> <span data-ttu-id="1fe92-154">System.Data 네임 스페이스의 형식은 EF 어셈블리로 되지 않는 System.Data.dll의 일부입니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-154">Some types in the System.Data namespace are in System.Data.dll which is not an EF assembly.</span></span> <span data-ttu-id="1fe92-155">이러한 형식으로 이동 하지 하 고 있으므로 해당 네임 스페이스가 그대로 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="1fe92-155">These types have not moved and so their namespaces remain unchanged.</span></span>
