---
title: Entity Framework 6-EF6로 업그레이드
author: divega
ms.date: 10/23/2016
ms.assetid: 29958ae5-85d3-4585-9ba6-550b8ec9393a
ms.openlocfilehash: 4395a9c117a6cf38e7fc08f11ee689d6fffa6fed
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182106"
---
# <a name="upgrading-to-entity-framework-6"></a><span data-ttu-id="52ade-102">Entity Framework 6으로 업그레이드</span><span class="sxs-lookup"><span data-stu-id="52ade-102">Upgrading to Entity Framework 6</span></span>

<span data-ttu-id="52ade-103">이전 버전의 EF에서 코드는 NuGet 패키지에 제공 된 .NET Framework 및 대역 외 (주로 EntityFramework .dll)의 일부로 제공 되는 핵심 라이브러리 (주로 System.object) 간에 분할 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-103">In previous versions of EF the code was split between core libraries (primarily System.Data.Entity.dll) shipped as part of the .NET Framework and out-of-band (OOB) libraries (primarily EntityFramework.dll) shipped in a NuGet package.</span></span> <span data-ttu-id="52ade-104">EF6는 핵심 라이브러리의 코드를 가져와서 OOB 라이브러리에 통합 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-104">EF6 takes the code from the core libraries and incorporates it into the OOB libraries.</span></span> <span data-ttu-id="52ade-105">이는 EF가 오픈 소스를 만들 수 있도록 하 고 .NET Framework에서 다른 속도로 발전 시킬 수 있도록 하기 위해 필요 했습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-105">This was necessary in order to allow EF to be made open source and for it to be able to evolve at a different pace from .NET Framework.</span></span> <span data-ttu-id="52ade-106">이로 인해 이동한 형식에 대해 응용 프로그램을 다시 작성 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-106">The consequence of this is that applications will need to be rebuilt against the moved types.</span></span>

<span data-ttu-id="52ade-107">EF 4.1 이상에서 제공 되는 DbContext을 사용 하는 응용 프로그램에 대해서는이 작업을 수행 하는 것이 간단 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-107">This should be straightforward for applications that make use of DbContext as shipped in EF 4.1 and later.</span></span> <span data-ttu-id="52ade-108">ObjectContext를 사용 하는 응용 프로그램의 경우에는 더 많은 작업이 필요 하지만 여전히 사용 하기 어렵습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-108">A little more work is required for applications that make use of ObjectContext but it still isn’t hard to do.</span></span>

<span data-ttu-id="52ade-109">다음은 기존 응용 프로그램을 EF6로 업그레이드 하기 위해 수행 해야 하는 작업의 검사 목록입니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-109">Here is a checklist of the things you need to do to upgrade an existing application to EF6.</span></span>

## <a name="1-install-the-ef6-nuget-package"></a><span data-ttu-id="52ade-110">1. EF6 NuGet 패키지 설치</span><span class="sxs-lookup"><span data-stu-id="52ade-110">1. Install the EF6 NuGet package</span></span>

<span data-ttu-id="52ade-111">새 Entity Framework 6 런타임으로 업그레이드 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-111">You need to upgrade to the new Entity Framework 6 runtime.</span></span>

1. <span data-ttu-id="52ade-112">프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **NuGet 패키지 관리 ...** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-112">Right-click on your project and select **Manage NuGet Packages...**</span></span>  
2. <span data-ttu-id="52ade-113">**온라인** 탭에서 **entityframework** 를 선택 하 고 **설치** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-113">Under the **Online** tab select **EntityFramework** and click **Install**</span></span>  
   > [!NOTE]
   > <span data-ttu-id="52ade-114">이전 버전의 EntityFramework NuGet 패키지가 설치 되 면 EF6로 업그레이드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-114">If a previous version of the EntityFramework NuGet package was installed this will upgrade it to EF6.</span></span>

<span data-ttu-id="52ade-115">또는 패키지 관리자 콘솔에서 다음 명령을 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-115">Alternatively, you can run the following command from Package Manager Console:</span></span>

``` powershell
Install-Package EntityFramework
```

## <a name="2-ensure-that-assembly-references-to-systemdataentitydll-are-removed"></a><span data-ttu-id="52ade-116">2. System.object에 대 한 어셈블리 참조가 제거 되었는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-116">2. Ensure that assembly references to System.Data.Entity.dll are removed</span></span>

<span data-ttu-id="52ade-117">EF6 NuGet 패키지를 설치 하면 프로젝트에서 System.object에 대 한 참조가 자동으로 제거 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-117">Installing the EF6 NuGet package should automatically remove any references to System.Data.Entity from your project for you.</span></span>

## <a name="3-swap-any-ef-designer-edmx-models-to-use-ef-6x-code-generation"></a><span data-ttu-id="52ade-118">3. EF 2.x 코드 생성을 사용 하도록 EDMX (EF Designer) 모델 교환</span><span class="sxs-lookup"><span data-stu-id="52ade-118">3. Swap any EF Designer (EDMX) models to use EF 6.x code generation</span></span>

<span data-ttu-id="52ade-119">EF Designer를 사용 하 여 모델을 만든 경우에는 EF6 호환 코드를 생성 하도록 코드 생성 템플릿을 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-119">If you have any models created with the EF Designer, you will need to update the code generation templates to generate EF6 compatible code.</span></span>

> [!NOTE]
> <span data-ttu-id="52ade-120">현재는 Visual Studio 2012 및 2013에 대해 EF 6.x DbContext 생성기 템플릿만 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-120">There are currently only EF 6.x DbContext Generator templates available for Visual Studio 2012 and 2013.</span></span>

1. <span data-ttu-id="52ade-121">기존 코드 생성 템플릿을 삭제 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-121">Delete existing code-generation templates.</span></span> <span data-ttu-id="52ade-122">이러한 파일 이름은 일반적으로 **\<edmx_file_name\>.tt** 하 고 **\<edmx_file_name\>합니다. Context.tt** 및 솔루션 탐색기에서 edmx 파일을 중첩할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-122">These files will typically be named **\<edmx_file_name\>.tt** and **\<edmx_file_name\>.Context.tt** and be nested under your edmx file in Solution Explorer.</span></span> <span data-ttu-id="52ade-123">솔루션 탐색기에서 템플릿을 선택 하 고 **Del** 키를 눌러 항목을 삭제할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-123">You can select the templates in Solution Explorer and press the **Del** key to delete them.</span></span>  
   > [!NOTE]
   > <span data-ttu-id="52ade-124">웹 사이트 프로젝트에서는 템플릿이 edmx 파일 아래에 중첩 되지 않지만 솔루션 탐색기에 함께 나열 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-124">In Web Site projects the templates will not be nested under your edmx file, but listed alongside it in Solution Explorer.</span></span>  

   > [!NOTE]
   > <span data-ttu-id="52ade-125">VB.NET 프로젝트에서는 중첩 된 템플릿 파일을 볼 수 있도록 ' 모든 파일 표시 '를 사용 하도록 설정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-125">In VB.NET projects you will need to enable 'Show All Files' to be able to see the nested template files.</span></span>
2. <span data-ttu-id="52ade-126">적절 한 EF 6.x 코드 생성 템플릿을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-126">Add the appropriate EF 6.x code generation template.</span></span> <span data-ttu-id="52ade-127">EF 디자이너에서 모델을 열고 디자인 화면을 마우스 오른쪽 단추로 클릭 한 다음 **코드 생성 항목 추가** ...를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-127">Open your model in the EF Designer, right-click on the design surface and select **Add Code Generation Item...**</span></span>
    - <span data-ttu-id="52ade-128">DbContext API를 사용 하는 경우 (권장) **EF 6.X DbContext 생성기** 는 **데이터** 탭에서 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-128">If you are using the DbContext API (recommended) then **EF 6.x DbContext Generator** will be available under the **Data** tab.</span></span>  
      > [!NOTE]
      > <span data-ttu-id="52ade-129">Visual Studio 2012을 사용 하는 경우이 템플릿을 사용 하려면 EF 6 도구를 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-129">If you are using Visual Studio 2012, you will need to install the EF 6 Tools to have this template.</span></span> <span data-ttu-id="52ade-130">자세한 내용은 [Entity Framework 가져오기](~/ef6/fundamentals/install.md) 를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="52ade-130">See [Get Entity Framework](~/ef6/fundamentals/install.md) for details.</span></span>  

    - <span data-ttu-id="52ade-131">ObjectContext API를 사용 하는 경우 **온라인** 탭을 선택 하 고 **EF 6.X. x entityobject 생성기**를 검색 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-131">If you are using the ObjectContext API then you will need to select the **Online** tab and search for **EF 6.x EntityObject Generator**.</span></span>  
3. <span data-ttu-id="52ade-132">코드 생성 템플릿에 사용자 지정을 적용 한 경우 업데이트 된 템플릿에 다시 적용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-132">If you applied any customizations to the code generation templates you will need to re-apply them to the updated templates.</span></span>

## <a name="4-update-namespaces-for-any-core-ef-types-being-used"></a><span data-ttu-id="52ade-133">4. 사용 중인 모든 코어 EF 형식에 대 한 네임 스페이스 업데이트</span><span class="sxs-lookup"><span data-stu-id="52ade-133">4. Update namespaces for any core EF types being used</span></span>

<span data-ttu-id="52ade-134">DbContext 및 Code First 형식에 대 한 네임 스페이스는 변경 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-134">The namespaces for DbContext and Code First types have not changed.</span></span> <span data-ttu-id="52ade-135">즉, EF 4.1 이상을 사용 하는 많은 응용 프로그램에서 아무것도 변경할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-135">This means for many applications that use EF 4.1 or later you will not need to change anything.</span></span>

<span data-ttu-id="52ade-136">이전에 System.object에 있던 ObjectContext와 같은 형식이 새 네임 스페이스로 이동 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-136">Types like ObjectContext that were previously in System.Data.Entity.dll have been moved to new namespaces.</span></span> <span data-ttu-id="52ade-137">즉, EF6에 대해 빌드하기 위해 *using* 또는 *Import* 지시문을 업데이트 해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-137">This means you may need to update your *using* or *Import* directives to build against EF6.</span></span>

<span data-ttu-id="52ade-138">네임 스페이스 변경 내용에 대 한 일반 규칙 System.Data.\* 에서 모든 유형의 System.Data.Entity.Core.\* 이동 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-138">The general rule for namespace changes is that any type in System.Data.\* is moved to System.Data.Entity.Core.\*.</span></span> <span data-ttu-id="52ade-139">즉, 단순히 **Entity. Core** 를 삽입 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-139">In other words, just insert **Entity.Core.**</span></span> <span data-ttu-id="52ade-140">시스템 데이터 이후.</span><span class="sxs-lookup"><span data-stu-id="52ade-140">after System.Data.</span></span> <span data-ttu-id="52ade-141">예를 들어 다음과 같은 가치를 제공해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-141">For example:</span></span>

- <span data-ttu-id="52ade-142">System.object 예외 = >. 데이터입니다. **엔터티입니다**. EntityException</span><span class="sxs-lookup"><span data-stu-id="52ade-142">System.Data.EntityException => System.Data.**Entity.Core**.EntityException</span></span>  
- <span data-ttu-id="52ade-143">System.object = > 시스템 데이터입니다. **엔터티입니다**. Objects. ObjectContext</span><span class="sxs-lookup"><span data-stu-id="52ade-143">System.Data.Objects.ObjectContext => System.Data.**Entity.Core**.Objects.ObjectContext</span></span>  
- <span data-ttu-id="52ade-144">DataClasses. RelationshipManager = >. 데이터입니다. **엔터티입니다**. DataClasses. RelationshipManager</span><span class="sxs-lookup"><span data-stu-id="52ade-144">System.Data.Objects.DataClasses.RelationshipManager => System.Data.**Entity.Core**.Objects.DataClasses.RelationshipManager</span></span>  

<span data-ttu-id="52ade-145">이러한 형식은 대부분의 DbContext 기반 응용 프로그램에 직접 사용 되지 않으므로 *핵심* 네임 스페이스에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-145">These types are in the *Core* namespaces because they are not used directly for most DbContext-based applications.</span></span> <span data-ttu-id="52ade-146">System.object의 일부인 일부 형식은 여전히 DbContext 기반 응용 프로그램에 대해 자주 사용 되며 *핵심* 네임 스페이스로 이동 되지 않았습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-146">Some types that were part of System.Data.Entity.dll are still used commonly and directly for DbContext-based applications and so have not been moved into the *Core* namespaces.</span></span> <span data-ttu-id="52ade-147">이러한 항목은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-147">These are:</span></span>

- <span data-ttu-id="52ade-148">EntityState = > 시스템 데이터입니다. **엔터티입니다**. EntityState</span><span class="sxs-lookup"><span data-stu-id="52ade-148">System.Data.EntityState => System.Data.**Entity**.EntityState</span></span>  
- <span data-ttu-id="52ade-149">DataClasses는 >. m a m a. **엔터티인 DbFunctionAttribute**</span><span class="sxs-lookup"><span data-stu-id="52ade-149">System.Data.Objects.DataClasses.EdmFunctionAttribute => System.Data.**Entity.DbFunctionAttribute**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="52ade-150">이 클래스의 이름이 변경 되었습니다. 이전 이름을 가진 클래스가 여전히 존재 하 고 작동 하지만 이제는 사용 되지 않는 것으로 표시 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-150">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.</span></span>  
- <span data-ttu-id="52ade-151">System.object는 >을 (를) 나타내는 데이터입니다. **엔터티인 함수**</span><span class="sxs-lookup"><span data-stu-id="52ade-151">System.Data.Objects.EntityFunctions => System.Data.**Entity.DbFunctions**</span></span>  
  > [!NOTE]
  > <span data-ttu-id="52ade-152">이 클래스의 이름이 변경 되었습니다. 이전 이름을 가진 클래스가 여전히 존재 하 고 작동 하지만 이제 사용 되지 않는 것으로 표시 되었습니다.)</span><span class="sxs-lookup"><span data-stu-id="52ade-152">This class has been renamed; a class with the old name still exists and works, but it now marked as obsolete.)</span></span>  
- <span data-ttu-id="52ade-153">공간 클래스 (예: DbGeography, DbGeometry)는 > System.object에서 이동 되었습니다. **엔터티입니다**. 음</span><span class="sxs-lookup"><span data-stu-id="52ade-153">Spatial classes (for example, DbGeography, DbGeometry) have moved from System.Data.Spatial => System.Data.**Entity**.Spatial</span></span>

> [!NOTE]
> <span data-ttu-id="52ade-154">System.object 네임 스페이스의 일부 형식은 EF 어셈블리가 아닌 System.object에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-154">Some types in the System.Data namespace are in System.Data.dll which is not an EF assembly.</span></span> <span data-ttu-id="52ade-155">이러한 형식은 이동 되지 않으므로 네임 스페이스는 변경 되지 않은 상태로 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="52ade-155">These types have not moved and so their namespaces remain unchanged.</span></span>
