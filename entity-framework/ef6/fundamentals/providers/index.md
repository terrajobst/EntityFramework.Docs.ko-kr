---
title: Entity Framework 공급자 - EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: 661398e7d6037875ce0cdb15c221a729d1f0c7d8
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413338"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="ae140-102">Entity Framework 6 공급자</span><span class="sxs-lookup"><span data-stu-id="ae140-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="ae140-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="ae140-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="ae140-105">현재 Entity Framework는 오픈 소스 라이선스 하에서 개발 중이며 EF6 이상은 .NET Framework의 일부로 제공되지 않을 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="ae140-106">이 방법은 여러 가지 장점이 있지만, EF6 어셈블리에 맞게 EF 공급자를 다시 빌드해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="ae140-107">즉, EF5 이하의 EF 공급자는 다시 빌드될 때까지 EF6를 사용하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="ae140-108">EF6에 어떤 공급자를 사용할 수 있나요?</span><span class="sxs-lookup"><span data-stu-id="ae140-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="ae140-109">EF6에 맞게 다시 빌드된 것으로 파악된 공급자는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="ae140-110">**Microsoft SQL Server 공급자**</span><span class="sxs-lookup"><span data-stu-id="ae140-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="ae140-111">[Entity Framework 오픈 소스 코드베이스](https://github.com/aspnet/EntityFramework6)로 빌드</span><span class="sxs-lookup"><span data-stu-id="ae140-111">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="ae140-112">[EntityFramework NuGet 패키지](https://nuget.org/packages/EntityFramework)의 일부로 제공</span><span class="sxs-lookup"><span data-stu-id="ae140-112">Shipped as part of the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="ae140-113">**Microsoft SQL Server Compact Edition 공급자**</span><span class="sxs-lookup"><span data-stu-id="ae140-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="ae140-114">[Entity Framework 오픈 소스 코드베이스](https://github.com/aspnet/EntityFramework6)로 빌드</span><span class="sxs-lookup"><span data-stu-id="ae140-114">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="ae140-115">[EntityFramework.SqlServerCompact NuGet 패키지](https://nuget.org/packages/EntityFramework.SqlServerCompact)에 포함</span><span class="sxs-lookup"><span data-stu-id="ae140-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="ae140-116">**Devart dotConnect Data 공급자**</span><span class="sxs-lookup"><span data-stu-id="ae140-116">**Devart dotConnect Data Providers**</span></span>](https://www.devart.com/dotconnect/)
    *   <span data-ttu-id="ae140-117">Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 및 SQL Server를 포함하여 다양한 데이터베이스에 대한 [Devart](https://www.devart.com/)의 타사 공급자가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-117">There are third-party providers from [Devart](https://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="ae140-118">**CData Software 공급자**</span><span class="sxs-lookup"><span data-stu-id="ae140-118">**CData Software providers**</span></span>](https://www.cdata.com/ado/)
    *   <span data-ttu-id="ae140-119">Salesforce, Azure Table Storage, MySql 등을 포함하여 다양한 데이터 저장소에 대한 [CData Software](https://www.cdata.com/ado/)의 타사 공급자가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-119">There are third-party providers from [CData Software](https://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="ae140-120">**Firebird 공급자**</span><span class="sxs-lookup"><span data-stu-id="ae140-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="ae140-121">[NuGet 패키지](https://www.nuget.org/packages/EntityFramework.Firebird/)의 일부로 제공</span><span class="sxs-lookup"><span data-stu-id="ae140-121">Available as a [NuGet Package](https://www.nuget.org/packages/EntityFramework.Firebird/)</span></span>
*   <span data-ttu-id="ae140-122">**Visual Fox Pro 공급자**</span><span class="sxs-lookup"><span data-stu-id="ae140-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="ae140-123">[NuGet 패키지](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)의 일부로 제공</span><span class="sxs-lookup"><span data-stu-id="ae140-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="ae140-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="ae140-124">**MySQL**</span></span>
    *   [<span data-ttu-id="ae140-125">Entity Framework용 MySQL 커넥터/NET</span><span class="sxs-lookup"><span data-stu-id="ae140-125">MySQL Connector/NET for Entity Framework</span></span>](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   <span data-ttu-id="ae140-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="ae140-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="ae140-127">Npgsql은 [NuGet 패키지](https://www.nuget.org/packages/EntityFramework6.Npgsql/)의 일부로 제공</span><span class="sxs-lookup"><span data-stu-id="ae140-127">Npgsql is available as a [NuGet package](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span></span>
*   <span data-ttu-id="ae140-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="ae140-128">**Oracle**</span></span>
    *   <span data-ttu-id="ae140-129">ODP.NET은 [NuGet 패키지](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)의 일부로 제공</span><span class="sxs-lookup"><span data-stu-id="ae140-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="ae140-130">이 목록에 포함된다는 것이 특정 공급자의 기능 또는 지원 수준을 나타내는 것은 아니며, EF6용 빌드를 사용할 수 있다는 것만 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="ae140-131">EF 공급자 등록</span><span class="sxs-lookup"><span data-stu-id="ae140-131">Registering EF providers</span></span>

<span data-ttu-id="ae140-132">Entity Framework 6부터 코드 기반 구성을 사용하여 또는 애플리케이션의 구성 파일에서 EF 공급자를 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="ae140-133">구성 파일 등록</span><span class="sxs-lookup"><span data-stu-id="ae140-133">Config file registration</span></span>

<span data-ttu-id="ae140-134">app.config 또는 web.config에서 EF 공급자를 등록하는 형식은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="ae140-135">EF 공급자가 NuGet에서 설치되는 경우 종종 NuGet 패키지가 자동으로 이 등록을 구성 파일에 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="ae140-136">앱의 시작 프로젝트가 아닌 프로젝트에 NuGet 패키지를 설치하는 경우 등록을 시작 프로젝트의 구성 파일에 복사해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="ae140-137">이 등록의 "invariantName"은 ADO.NET 공급자를 식별하는 데 사용되는 것과 동일한 고정 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="ae140-138">DbProviderFactories 등록의 "고정" 특성 및 연결 문자열 등록의 "providerName" 특성으로 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="ae140-139">사용할 고정 이름이 공급자에 대한 설명서에도 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="ae140-140">고정 이름의 예로 SQL Server의 “System.Data.SqlClient” 및 SQL Server Compact의 “System.Data.SqlServerCe.4.0”을 들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="ae140-141">이 등록의 "형식"은 "System.Data.Entity.Core.Common.DbProviderServices"에서 파생되는 공급자 형식의 정규화된 어셈블리 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="ae140-142">예를 들어 SQL Compact에 사용할 문자열은 “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”입니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="ae140-143">여기서 사용할 형식은 공급자에 대한 설명서에 포함되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="ae140-144">코드 기반 등록</span><span class="sxs-lookup"><span data-stu-id="ae140-144">Code-based registration</span></span>

<span data-ttu-id="ae140-145">Entity Framework 6부터 EF에 대한 애플리케이션 수준 구성을 코드에서 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="ae140-146">자세한 내용은 _[Entity Framework 코드 기반 구성](https://msdn.microsoft.com/data/jj680699)_ 을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="ae140-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="ae140-147">코드 기반 구성을 사용하여 EF 공급자를 등록하는 일반적인 방법은 System.Data.Entity.DbConfiguration에서 파생되는 새 클래스를 만들어서 DbContext 클래스와 동일한 어셈블리에 배치하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="ae140-148">그러면 DbConfiguration 클래스가 공급자를 생성자에 등록합니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="ae140-149">예를 들어 SQL Compact 공급자를 등록하려는 경우 DbConfiguration 클래스는 다음과 비슷합니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

<span data-ttu-id="ae140-150">이 코드에서 "SqlCeProviderServices.ProviderInvariantName"은 SQL Server Compact 공급자 고정 이름 문자열(“System.Data.SqlServerCe.4.0”)에 유용하고 SqlCeProviderServices.Instance는 SQL Compact EF 공급자의 singleton 인스턴스를 반환합니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="ae140-151">필요한 공급자를 사용할 수 없으면 어떻게 되나요?</span><span class="sxs-lookup"><span data-stu-id="ae140-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="ae140-152">이전 버전의 EF에 대한 공급자를 사용할 수 있는 경우 공급자의 소유자에게 연락하여 EF6 버전을 만들어 달라고 요청하세요.</span><span class="sxs-lookup"><span data-stu-id="ae140-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="ae140-153">[EF6 공급자 모델에 대한 설명서](~/ef6/fundamentals/providers/provider-model.md) 참조를 포함해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="ae140-154">공급자를 직접 작성할 수 있나요?</span><span class="sxs-lookup"><span data-stu-id="ae140-154">Can I write a provider myself?</span></span>

<span data-ttu-id="ae140-155">EF 공급자를 직접 만들 수 있지만, 쉬운 일은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="ae140-156">EF6 공급자 모델에 대한 위의 링크는 시작하기에 좋은 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="ae140-157">또한 [EF 오픈 소스 코드베이스](https://github.com/aspnet/EntityFramework6)에 포함된 SQL Server 및 SQL CE 공급자에 대한 코드를 시작 지점으로 또는 참조로 사용하면 도움이 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="ae140-158">EF6를 시작하면 EF 공급자가 기본 ADO.NET 공급자와 덜 긴밀하게 결합됩니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="ae140-159">따라서 ADO.NET 클래스를 작성 또는 래핑할 필요 없이 EF 공급자를 손쉽게 작성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ae140-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
