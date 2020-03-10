---
title: 도구 및 확장 - EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 99f59153a452a2f4aad5811110ebc5b5da7717ef
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412998"
---
# <a name="ef-core-tools--extensions"></a><span data-ttu-id="7ea67-102">EF Core 도구 및 확장</span><span class="sxs-lookup"><span data-stu-id="7ea67-102">EF Core Tools & Extensions</span></span>

<span data-ttu-id="7ea67-103">이러한 도구 및 확장은 Entity Framework Core 2.1 이상에 대한 추가 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-103">These tools and extensions provide additional functionality for Entity Framework Core 2.1 and later.</span></span>

> [!IMPORTANT]  
> <span data-ttu-id="7ea67-104">확장은 다양한 원본을 바탕으로 작성되며 Entity Framework Core 프로젝트의 일부로 유지되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-104">Extensions are built by a variety of sources and aren't maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="7ea67-105">타사 확장을 고려할 때는 품질, 라이선싱, 호환성, 지원 등을 평가하여 요구 사항에 적합한지를 확인해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-105">When considering a third party extension, be sure to evaluate its quality, licensing, compatibility, support, etc. to ensure it meets your requirements.</span></span> <span data-ttu-id="7ea67-106">특히 이전 버전의 EF Core용으로 빌드된 확장은 최신 버전에서 작동하기 전에 업데이트해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-106">In particular, an extension built for an older version of EF Core may need updating before it will work with the latest versions.</span></span>

## <a name="tools"></a><span data-ttu-id="7ea67-107">도구</span><span class="sxs-lookup"><span data-stu-id="7ea67-107">Tools</span></span>

### <a name="llblgen-pro"></a><span data-ttu-id="7ea67-108">LLBLGen Pro</span><span class="sxs-lookup"><span data-stu-id="7ea67-108">LLBLGen Pro</span></span>

<span data-ttu-id="7ea67-109">LLBLGen Pro는 Entity Framework 및 Entity Framework Core 지원을 함께 제공하는 엔터티 모델링 솔루션입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-109">LLBLGen Pro is an entity modeling solution with support for Entity Framework and Entity Framework Core.</span></span> <span data-ttu-id="7ea67-110">즉시 쿼리 작성을 시작할 수 있도록 데이터베이스를 우선 사용하거나 모델을 우선 사용하여 쉽게 엔터티 모델을 정의하고 데이터베이스에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-110">It lets you easily define your entity model and map it to your database, using database first or model first, so you can get started writing queries right away.</span></span> <span data-ttu-id="7ea67-111">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-111">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-112">웹 사이트</span><span class="sxs-lookup"><span data-stu-id="7ea67-112">Website</span></span>](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a><span data-ttu-id="7ea67-113">Devart Entity Developer</span><span class="sxs-lookup"><span data-stu-id="7ea67-113">Devart Entity Developer</span></span>

<span data-ttu-id="7ea67-114">Entity Developer는 ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access 및 LINQ to SQL을 위한 강력한 ORM 디자이너입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-114">Entity Developer is a powerful ORM designer for ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access, and LINQ to SQL.</span></span> <span data-ttu-id="7ea67-115">모델 우선 접근법 또는 데이터베이스 우선 접근법 및 C# 또는 Visual Basic Code 생성을 사용하여 EF Core 모델을 시각적으로 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-115">It supports designing EF Core models visually, using model first or database first approaches, and C# or Visual Basic code generation.</span></span> <span data-ttu-id="7ea67-116">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-116">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-117">웹 사이트</span><span class="sxs-lookup"><span data-stu-id="7ea67-117">Website</span></span>](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a><span data-ttu-id="7ea67-118">Entity Framework에 대한 ORM nHydrate</span><span class="sxs-lookup"><span data-stu-id="7ea67-118">nHydrate ORM for Entity Framework</span></span>

<span data-ttu-id="7ea67-119">Entity Framework용 강력한 형식의 확장 가능한 클래스를 만드는 ORM입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-119">An ORM that creates strongly-typed, extendable classes for Entity Framework.</span></span> <span data-ttu-id="7ea67-120">생성된 코드는 Entity Framework Core입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-120">The generated code is Entity Framework Core.</span></span> <span data-ttu-id="7ea67-121">차이가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-121">There is no difference.</span></span> <span data-ttu-id="7ea67-122">이것은 EF 또는 사용자 지정 ORM을 대체하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-122">This is not a replacement for EF or a custom ORM.</span></span> <span data-ttu-id="7ea67-123">팀이 복잡한 데이터베이스 스키마를 관리할 수 있는 시각적 모델링 계층입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-123">It is a visual, modeling layer that allows a team to manage complex database schemas.</span></span> <span data-ttu-id="7ea67-124">Git 같은 SCM 소프트웨어에서 잘 작동하므로 충돌을 최소화하면서 모델에 대한 다중 사용자 액세스를 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-124">It works well with SCM software like Git, allowing multi-user access to your model with minimal conflicts.</span></span> <span data-ttu-id="7ea67-125">설치 프로그램이 모델 변경 내용을 추적하고 업그레이드 스크립트를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-125">The installer tracks model changes and creates upgrade scripts.</span></span> <span data-ttu-id="7ea67-126">EF Core용: 3.</span><span class="sxs-lookup"><span data-stu-id="7ea67-126">For EF Core: 3.</span></span>

[<span data-ttu-id="7ea67-127">Github 사이트</span><span class="sxs-lookup"><span data-stu-id="7ea67-127">Github site</span></span>](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a><span data-ttu-id="7ea67-128">EF Core Power Tools</span><span class="sxs-lookup"><span data-stu-id="7ea67-128">EF Core Power Tools</span></span>

<span data-ttu-id="7ea67-129">EF Core Power Tools는 간단한 사용자 인터페이스에서 다양한 EF Core 디자인 타임 작업을 노출하는 Visual Studio의 확장 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-129">EF Core Power Tools is a Visual Studio extension that exposes various EF Core design-time tasks in a simple user interface.</span></span> <span data-ttu-id="7ea67-130">여기에는 기존 데이터베이스에서 DbContext와 엔터티 클래스의 리버스 엔지니어링 및 [SQL Server Dacpac](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), 데이터베이스 마이그레이션의 관리 및 모델 시각화가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-130">It includes reverse engineering of DbContext and entity classes from existing databases and [SQL Server DACPACs](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), management of database migrations, and model visualizations.</span></span> <span data-ttu-id="7ea67-131">EF Core용: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="7ea67-131">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="7ea67-132">GitHub Wiki</span><span class="sxs-lookup"><span data-stu-id="7ea67-132">GitHub wiki</span></span>](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a><span data-ttu-id="7ea67-133">Entity Framework Visual Editor</span><span class="sxs-lookup"><span data-stu-id="7ea67-133">Entity Framework Visual Editor</span></span>

<span data-ttu-id="7ea67-134">Entity Framework Visual Editor는 EF 6 및 EF Core 클래스의 시각적 개체 디자인을 위해 ORM 디자이너를 추가한 Visual Studio의 확장 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-134">Entity Framework Visual Editor is a Visual Studio extension that adds an ORM designer for visual design of EF 6, and EF Core classes.</span></span> <span data-ttu-id="7ea67-135">코드는 T4 템플릿을 사용하여 생성되므로 필요에 맞게 사용자 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-135">Code is generated using T4 templates so can be customized to suit any needs.</span></span> <span data-ttu-id="7ea67-136">상속, 단방향 및 양방향 연결, 열거형 및 클래스를 색으로 구분하는 기능을 지원하고, 디자인의 잠재적으로 난해한 부분을 설명하기 위한 텍스트 블록을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-136">It supports inheritance, unidirectional and bidirectional associations, enumerations, and the ability to color-code your classes and add text blocks to explain potentially arcane parts of your design.</span></span> <span data-ttu-id="7ea67-137">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-137">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-138">Marketplace</span><span class="sxs-lookup"><span data-stu-id="7ea67-138">Marketplace</span></span>](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a><span data-ttu-id="7ea67-139">CatFactory</span><span class="sxs-lookup"><span data-stu-id="7ea67-139">CatFactory</span></span>

<span data-ttu-id="7ea67-140">CatFactory는 SQL Server 데이터베이스에서 DbContext 클래스, 엔터티, 매핑 구성 및 리포지토리 클래스의 생성을 자동화할 수 있는 .NET Core에 대한 스캐폴딩 엔진입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-140">CatFactory is a scaffolding engine for .NET Core that can automate the generation of DbContext classes, entities, mapping configurations, and repository classes from a SQL Server database.</span></span> <span data-ttu-id="7ea67-141">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-141">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-142">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-142">GitHub repository</span></span>](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a><span data-ttu-id="7ea67-143">LoreSoft의 Entity Framework Core 생성기</span><span class="sxs-lookup"><span data-stu-id="7ea67-143">LoreSoft's Entity Framework Core Generator</span></span>

<span data-ttu-id="7ea67-144">Entity Framework Core 생성기(efg)는 `dotnet ef dbcontext scaffold`와 비슷하게 기존 데이터베이스에서 EF Core 모델을 생성할 수 있는 .NET Core CLI 도구이지만 지역 교체를 통하거나 매핑 파일을 구문 분석하여 안전한 코드 [재생성](https://efg.loresoft.com/en/latest/regeneration/)도 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-144">Entity Framework Core Generator (efg) is a .NET Core CLI tool that can generate EF Core models from an existing database, much like `dotnet ef dbcontext scaffold`, but it also supports safe code [regeneration](https://efg.loresoft.com/en/latest/regeneration/) via region replacement or by parsing mapping files.</span></span> <span data-ttu-id="7ea67-145">이 도구는 보기 모델, 유효성 검사 및 개체 매퍼 코드를 생성하도록 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-145">This tool supports generating view models, validation, and object mapper code.</span></span> <span data-ttu-id="7ea67-146">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-146">For EF Core: 2.</span></span>

<span data-ttu-id="7ea67-147">[자습서](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[설명서](https://efg.loresoft.com/en/latest/)</span><span class="sxs-lookup"><span data-stu-id="7ea67-147">[Tutorial](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Documentation](https://efg.loresoft.com/en/latest/)</span></span>

## <a name="extensions"></a><span data-ttu-id="7ea67-148">확장</span><span class="sxs-lookup"><span data-stu-id="7ea67-148">Extensions</span></span>

### <a name="microsoftentityframeworkcoreautohistory"></a><span data-ttu-id="7ea67-149">Microsoft.EntityFrameworkCore.AutoHistory</span><span class="sxs-lookup"><span data-stu-id="7ea67-149">Microsoft.EntityFrameworkCore.AutoHistory</span></span>

<span data-ttu-id="7ea67-150">기록 테이블에 EF Core에서 수행된 데이터 변경 내용을 자동으로 기록할 수 있는 플러그 인 라이브러리입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-150">A plugin library that enables automatically recording the data changes performed by EF Core into a history table.</span></span> <span data-ttu-id="7ea67-151">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-151">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-152">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-152">GitHub repository</span></span>](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a><span data-ttu-id="7ea67-153">EFSecondLevelCache.Core</span><span class="sxs-lookup"><span data-stu-id="7ea67-153">EFSecondLevelCache.Core</span></span>

<span data-ttu-id="7ea67-154">이후에 동일한 쿼리를 실행할 경우 데이터베이스에 대한 액세스를 방지하고 캐시에서 직접 데이터를 검색할 수 있도록 두 번째 수준의 캐시에 EF Core 쿼리의 결과를 저장할 수 있는 확장 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-154">An extension that enables storing the results of EF Core queries into a second-level cache, so that subsequent executions of the same queries can avoid accessing the database and retrieve the data directly from the cache.</span></span> <span data-ttu-id="7ea67-155">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-155">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-156">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-156">GitHub repository</span></span>](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a><span data-ttu-id="7ea67-157">Geco</span><span class="sxs-lookup"><span data-stu-id="7ea67-157">Geco</span></span>

<span data-ttu-id="7ea67-158">Geco(생성기 콘솔)는 .NET Core에서 실행되고 콘솔 생성을 위해 C# 보간 문자열을 사용하는 콘솔 프로젝트에 기반한 간단한 코드 생성기입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-158">Geco (Generator Console) is a simple code generator based on a console project, that runs on .NET Core and uses C# interpolated strings for code generation.</span></span> <span data-ttu-id="7ea67-159">Geco에는 복수형, 단수형 및 편집 가능한 템플릿에 대한 지원과 함께 EF Core용 리버스 모델 생성기가 포함됩니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-159">Geco includes a reverse model generator for EF Core with support for pluralization, singularization, and editable templates.</span></span> <span data-ttu-id="7ea67-160">시드 데이터 스크립트 생성기, 스크립트 실행기 및 데이터베이스 클리너도 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-160">It also provides a seed data script generator, a script runner, and a database cleaner.</span></span> <span data-ttu-id="7ea67-161">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-161">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-162">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-162">GitHub repository</span></span>](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a><span data-ttu-id="7ea67-163">EntityFrameworkCore.Scaffolding.Handlebars</span><span class="sxs-lookup"><span data-stu-id="7ea67-163">EntityFrameworkCore.Scaffolding.Handlebars</span></span> 

<span data-ttu-id="7ea67-164">Handlebars 템플릿이 있는 Entity Framework Core 도구 체인을 사용하여 기존 데이터베이스에서 리버스 엔지니어링된 클래스를 사용자 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-164">Allows customization of classes reverse engineered from an existing database using the Entity Framework Core toolchain with Handlebars templates.</span></span> <span data-ttu-id="7ea67-165">EF Core용: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="7ea67-165">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="7ea67-166">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-166">GitHub repository</span></span>](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a><span data-ttu-id="7ea67-167">NeinLinq.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="7ea67-167">NeinLinq.EntityFrameworkCore</span></span> 

<span data-ttu-id="7ea67-168">NeinLinq는 함수를 재사용할 수 있도록 설정하기 위해 Entity Framework와 같은 LINQ 공급 기업을 확장하여 쿼리를 다시 작성하고, 변환 가능한 조건자 및 선택기를 사용하는 동적 쿼리를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-168">NeinLinq extends LINQ providers such as Entity Framework to enable reusing functions, rewriting queries, and building dynamic queries using translatable predicates and selectors.</span></span> <span data-ttu-id="7ea67-169">EF Core용: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="7ea67-169">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="7ea67-170">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-170">GitHub repository</span></span>](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a><span data-ttu-id="7ea67-171">Microsoft.EntityFrameworkCore.UnitOfWork</span><span class="sxs-lookup"><span data-stu-id="7ea67-171">Microsoft.EntityFrameworkCore.UnitOfWork</span></span>

<span data-ttu-id="7ea67-172">지원되는 분산된 트랜잭션을 사용한 리포지토리, 작업 패턴의 단위 및 여러 데이터베이스를 지원하기 위한 Microsoft.EntityFrameworkCore에 대한 플러그 인입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-172">A plugin for Microsoft.EntityFrameworkCore to support repository, unit of work patterns, and multiple databases with distributed transaction supported.</span></span> <span data-ttu-id="7ea67-173">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-173">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-174">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-174">GitHub repository</span></span>](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a><span data-ttu-id="7ea67-175">EFCore.BulkExtensions</span><span class="sxs-lookup"><span data-stu-id="7ea67-175">EFCore.BulkExtensions</span></span>

<span data-ttu-id="7ea67-176">대량 작업(삽입, 업데이트, 삭제)을 위한 EF Core 확장입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-176">EF Core extensions for Bulk operations (Insert, Update, Delete).</span></span> <span data-ttu-id="7ea67-177">EF Core용: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="7ea67-177">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="7ea67-178">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-178">GitHub repository</span></span>](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a><span data-ttu-id="7ea67-179">Bricelam.EntityFrameworkCore.Pluralizer</span><span class="sxs-lookup"><span data-stu-id="7ea67-179">Bricelam.EntityFrameworkCore.Pluralizer</span></span>

<span data-ttu-id="7ea67-180">디자인 타임 복수화를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-180">Adds design-time pluralization.</span></span> <span data-ttu-id="7ea67-181">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-181">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-182">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-182">GitHub repository</span></span>](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a><span data-ttu-id="7ea67-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="7ea67-183">Toolbelt.EntityFrameworkCore.IndexAttribute</span></span>

<span data-ttu-id="7ea67-184">[인덱스] 특성의 반복입니다(모델 빌드를 위한 확장 기능을 포함).</span><span class="sxs-lookup"><span data-stu-id="7ea67-184">Revival of [Index] attribute (with extension for model building).</span></span> <span data-ttu-id="7ea67-185">EF Core용: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="7ea67-185">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="7ea67-186">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-186">GitHub repository</span></span>](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a><span data-ttu-id="7ea67-187">EfCore.InMemoryHelpers</span><span class="sxs-lookup"><span data-stu-id="7ea67-187">EfCore.InMemoryHelpers</span></span>

<span data-ttu-id="7ea67-188">EF Core 메모리 내 데이터베이스 공급 기업에 대한 래퍼를 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-188">Provides a wrapper around the EF Core In-Memory Database Provider.</span></span> <span data-ttu-id="7ea67-189">관계형 공급 기업과 같이 잘 작동할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-189">Makes it act more like a relational provider.</span></span> <span data-ttu-id="7ea67-190">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-190">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-191">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-191">GitHub repository</span></span>](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a><span data-ttu-id="7ea67-192">EFCore.TemporalSupport</span><span class="sxs-lookup"><span data-stu-id="7ea67-192">EFCore.TemporalSupport</span></span>

<span data-ttu-id="7ea67-193">임시 지원을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-193">An implementation of temporal support.</span></span> <span data-ttu-id="7ea67-194">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-194">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-195">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-195">GitHub repository</span></span>](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a><span data-ttu-id="7ea67-196">EfCoreTemporalTable</span><span class="sxs-lookup"><span data-stu-id="7ea67-196">EfCoreTemporalTable</span></span>

<span data-ttu-id="7ea67-197">`AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`의 도입된 확장 메서드를 사용하여 즐겨 찾는 데이터베이스에서 임시 쿼리를 쉽게 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-197">Easily perform temporal queries on your favourite database using introduced extension methods: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`.</span></span> <span data-ttu-id="7ea67-198">EF Core용: 3.</span><span class="sxs-lookup"><span data-stu-id="7ea67-198">For EF Core: 3.</span></span>

[<span data-ttu-id="7ea67-199">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-199">GitHub repository</span></span>](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a><span data-ttu-id="7ea67-200">EFCore.TimeTraveler</span><span class="sxs-lookup"><span data-stu-id="7ea67-200">EFCore.TimeTraveler</span></span>

<span data-ttu-id="7ea67-201">이미 정의된 EF Core 코드, 엔터티 및 매핑을 사용하여 [SQL Server 임시 기록](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel)에 대한 모든 기능을 갖춘 Entity Framework Core 쿼리를 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-201">Allow full-featured Entity Framework Core queries against [SQL Server Temporal History](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) using the EF Core code, entities, and mappings you already have defined.</span></span>  <span data-ttu-id="7ea67-202">`using (TemporalQuery.AsOf(targetDateTime)) {...}`에서 코드를 래핑하여 시간을 통해 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-202">Travel through time by wrapping your code in `using (TemporalQuery.AsOf(targetDateTime)) {...}`.</span></span> <span data-ttu-id="7ea67-203">EF Core용: 3.</span><span class="sxs-lookup"><span data-stu-id="7ea67-203">For EF Core: 3.</span></span>

[<span data-ttu-id="7ea67-204">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-204">GitHub repository</span></span>](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a><span data-ttu-id="7ea67-205">EntityFrameworkCore.TemporalTables</span><span class="sxs-lookup"><span data-stu-id="7ea67-205">EntityFrameworkCore.TemporalTables</span></span>

<span data-ttu-id="7ea67-206">SQL Server를 사용하여 임시 테이블을 쉽게 사용할 수 있도록 하는 Entity Framework Core에 대한 확장 라이브러리입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-206">Extension library for Entity Framework Core which allows developers who use SQL Server to easily use temporal tables.</span></span> <span data-ttu-id="7ea67-207">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-207">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-208">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-208">GitHub repository</span></span>](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a><span data-ttu-id="7ea67-209">EntityFrameworkCore.Cacheable</span><span class="sxs-lookup"><span data-stu-id="7ea67-209">EntityFrameworkCore.Cacheable</span></span>

<span data-ttu-id="7ea67-210">두 번째 수준 고성능 쿼리 캐시입니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-210">A high-performance second-level query cache.</span></span> <span data-ttu-id="7ea67-211">EF Core용: 2.</span><span class="sxs-lookup"><span data-stu-id="7ea67-211">For EF Core: 2.</span></span>

[<span data-ttu-id="7ea67-212">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-212">GitHub repository</span></span>](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a><span data-ttu-id="7ea67-213">Entity Framework Plus</span><span class="sxs-lookup"><span data-stu-id="7ea67-213">Entity Framework Plus</span></span>

<span data-ttu-id="7ea67-214">다음과 같은 기능으로 DbContext를 확장합니다. 필터, 감사, 캐싱, 향후 쿼리, 일괄 삭제, 일괄 업데이트 등을 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-214">Extends your DbContext with features such as: Include Filter, Auditing, Caching, Query Future, Batch Delete, Batch Update, and more.</span></span> <span data-ttu-id="7ea67-215">EF Core용: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="7ea67-215">For EF Core: 2, 3.</span></span>

<span data-ttu-id="7ea67-216">[웹 사이트](https://entityframework-plus.net/)
[GitHub 리포지토리](https://github.com/zzzprojects/EntityFramework-Plus)</span><span class="sxs-lookup"><span data-stu-id="7ea67-216">[Website](https://entityframework-plus.net/)
[GitHub repository](https://github.com/zzzprojects/EntityFramework-Plus)</span></span>

### <a name="entity-framework-extensions"></a><span data-ttu-id="7ea67-217">Entity Framework Extensions</span><span class="sxs-lookup"><span data-stu-id="7ea67-217">Entity Framework Extensions</span></span>

<span data-ttu-id="7ea67-218">고성능 대량 작업을 통해 DbContext를 확장합니다. BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge 등이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-218">Extends your DbContext with high-performance bulk operations: BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge, and more.</span></span> <span data-ttu-id="7ea67-219">EF Core용: 2, 3.</span><span class="sxs-lookup"><span data-stu-id="7ea67-219">For EF Core: 2, 3.</span></span>

[<span data-ttu-id="7ea67-220">웹 사이트</span><span class="sxs-lookup"><span data-stu-id="7ea67-220">Website</span></span>](https://entityframework-extensions.net/)

### <a name="expressionify"></a><span data-ttu-id="7ea67-221">Expressionify</span><span class="sxs-lookup"><span data-stu-id="7ea67-221">Expressionify</span></span>

<span data-ttu-id="7ea67-222">LINQ 람다에서 확장 메서드를 호출하기 위한 지원을 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="7ea67-222">Add support for calling extension methods in linq lambdas.</span></span> <span data-ttu-id="7ea67-223">EF Core용: 3.1</span><span class="sxs-lookup"><span data-stu-id="7ea67-223">For EF Core: 3.1</span></span>

[<span data-ttu-id="7ea67-224">GitHub 리포지토리</span><span class="sxs-lookup"><span data-stu-id="7ea67-224">GitHub repository</span></span>](https://github.com/ClaveConsulting/Expressionify)
