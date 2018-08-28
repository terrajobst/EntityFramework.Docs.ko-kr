---
title: EF6-종속성 확인
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 45681bb0cedecd502b1968b90b7f682d3257dd23
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998164"
---
# <a name="dependency-resolution"></a><span data-ttu-id="930b1-102">종속성 확인</span><span class="sxs-lookup"><span data-stu-id="930b1-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="930b1-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="930b1-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="930b1-105">EF6부터 Entity Framework는 필요한 서비스의 구현을 가져오기 위한 일반 용도의 메커니즘을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="930b1-106">즉, EF 일부 인터페이스 또는 기본 클래스의 인스턴스를 사용 하는 경우 묻습니다 인터페이스 또는 기본 클래스의 구체적 구현은 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="930b1-107">이 작업은 IDbDependencyResolver 인터페이스를 사용 하 여 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="930b1-108">GetService 메서드는 일반적으로 EF에서 호출 및 EF에서 또는 응용 프로그램에서 제공 하는 IDbDependencyResolver의 구현에 의해 처리 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="930b1-109">형식 인수는 요청 되는 서비스의 인터페이스 또는 기본 클래스 형식을 호출 하는 경우 및 키 개체가 null 이거나 요청된 된 서비스에 대 한 컨텍스트 정보를 제공 하는 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="930b1-110">이 문서에서는 IDbDependencyResolver를 구현 하는 방법에 대 한 자세한 내용은 포함 하지 않지만 대신 담아 두는 EF GetService 및 키 개체의 의미 체계에 대 한 호출의 이러한 각 서비스 형식 (즉, 인터페이스 및 기본 클래스 형식)에 대 한 참조 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-110">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span> <span data-ttu-id="930b1-111">이 문서는 추가 서비스를 추가할 때 최신 상태로 유지 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-111">This document will be kept up-to-date as additional services are added.</span></span>  

## <a name="services-resolved"></a><span data-ttu-id="930b1-112">서비스 확인</span><span class="sxs-lookup"><span data-stu-id="930b1-112">Services resolved</span></span>  

<span data-ttu-id="930b1-113">언급이 단일 항목으로 사용할 수 있으므로 반환 되는 모든 개체 스레드로부터 안전 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-113">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="930b1-114">대부분의 경우에 팩터리 개체를 반환 팩터리 자체 스레드로부터 안전 해야 하지만 공장에서 반환 되는 개체 스레드로부터 안전한 새 인스턴스는 각 사용에 대 한 공장에서 요청한 이후의 되도록 필요는 없습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-114">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>  

### <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="930b1-115">것과 < TContext\></span><span class="sxs-lookup"><span data-stu-id="930b1-115">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="930b1-116">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-116">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-117">**반환 된 개체**: 지정된 된 컨텍스트 형식에 대 한 데이터베이스 이니셜라이저</span><span class="sxs-lookup"><span data-stu-id="930b1-117">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="930b1-118">**키**: 하지 사용; null</span><span class="sxs-lookup"><span data-stu-id="930b1-118">**Key**: Not used; will be null</span></span>  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="930b1-119">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="930b1-119">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="930b1-120">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-120">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="930b1-121">**반환 된 개체**: 마이그레이션과 만들려는 데이터베이스 이니셜라이저를 사용 하 여 데이터베이스 생성과 같은 데이터베이스를 유발 하는 다른 작업에 사용할 수 있는 SQL 생성기를 만들기 위한 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-121">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="930b1-122">**키**: SQL 생성 될 데이터베이스의 유형을 지정 하는 ADO.NET 공급자 고정 이름을 포함 하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-122">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="930b1-123">예를 들어, SQL Server SQL 생성기 키 "System.Data.SqlClient"에 대 한 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-123">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="930b1-124">EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-124">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="930b1-125">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="930b1-125">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="930b1-126">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-126">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-127">**반환 된 개체**: 지정 된 공급자 고정 이름을 사용 하는 EF 공급자</span><span class="sxs-lookup"><span data-stu-id="930b1-127">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="930b1-128">**키**: ADO.NET 공급자 고정 이름에는 공급자가 필요한 데이터베이스의 형식 지정을 포함 하는 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-128">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="930b1-129">예를 들어, SQL Server 공급자는 키 "System.Data.SqlClient"에 대 한 반환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-129">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="930b1-130">EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-130">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="930b1-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="930b1-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="930b1-132">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-132">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-133">**반환 된 개체**: 규칙에 따라 EF가 데이터베이스 연결을 만들 때 사용 될 연결 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-133">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="930b1-134">즉, EF에 연결 하지 못하거나 연결 문자열 지정 app.config 또는 web.config에서 연결 문자열을 찾을 수 있습니다, 다음이 서비스는 하는 데 사용 연결을 만들 규칙에 따라 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-134">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="930b1-135">연결 팩터리를 변경 합니다. 기본적으로 다른 유형의 데이터베이스 (예를 들어, SQL Server Compact Edition)를 사용 하는 EF를 허용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-135">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="930b1-136">**키**: 하지 사용; null</span><span class="sxs-lookup"><span data-stu-id="930b1-136">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="930b1-137">EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-137">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="930b1-138">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="930b1-138">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="930b1-139">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-139">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-140">**반환 된 개체**: 연결에서 공급자 매니페스트 토큰을 생성할 수 있는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-140">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="930b1-141">이 서비스는 일반적으로 두 가지 방법으로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-141">This service is typically used in two ways.</span></span> <span data-ttu-id="930b1-142">첫째, Code First 모델을 빌드할 때 데이터베이스에 연결 하지 않으려면 사용 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-142">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="930b1-143">둘째, Code First SQL Server 2008을 사용 하는 경우에 따라 하는 경우에 SQL Server 2005에 대 한 모델을 적용할 특정 데이터베이스 버전-예를 들어 모델을 작성 하도록 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-143">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="930b1-144">**개체 수명**: 단일-동일한 개체에는 시간 및 다른 스레드에서 동시에 사용 되는 여러 수 있습니다</span><span class="sxs-lookup"><span data-stu-id="930b1-144">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="930b1-145">**키**: 하지 사용; null</span><span class="sxs-lookup"><span data-stu-id="930b1-145">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="930b1-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="930b1-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="930b1-147">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-147">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-148">**반환 된 개체**: 지정된 된 연결에서 공급자 팩터리를 얻을 수 있는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-148">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="930b1-149">.NET 4.5에서 공급자는 연결에서 공개적으로 액세스할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-149">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="930b1-150">.NET 4에서이 서비스의 기본 구현은 일부 추론을 사용 하 여 일치 하는 공급자를 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-150">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="930b1-151">적절 한 확인을 제공 하는 이러한 실패 한 후이 서비스의 새 구현을 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-151">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="930b1-152">**키**: 하지 사용; null</span><span class="sxs-lookup"><span data-stu-id="930b1-152">**Key**: Not used; will be null</span></span>  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="930b1-153">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="930b1-153">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="930b1-154">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-154">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-155">**반환 된 개체**: 지정 된 컨텍스트에 대 한 모델 캐시 키를 생성 하는 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-155">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="930b1-156">기본적으로 EF DbContext 형식 공급자별 당 하나의 모델을 캐시합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-156">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="930b1-157">캐시 키에 스키마 이름 등의 기타 정보를 추가 하려면이 서비스의 다른 구현을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-157">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="930b1-158">**키**: 하지 사용; null</span><span class="sxs-lookup"><span data-stu-id="930b1-158">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="930b1-159">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="930b1-159">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="930b1-160">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-160">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-161">**반환 된 개체**: geography 및 geometry 공간 형식에 대 한 기본 EF 공급자 지원을 추가 하는 공간 공급자는 EF 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-161">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="930b1-162">**키**: DbSptialServices에 대 한 두 가지 방법으로 라는 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-162">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="930b1-163">첫째, 공급자별 공간 서비스 DbProviderInfo 개체를 사용 하 여 요청 된 (invariant를 포함 하는 이름 및 매니페스트 토큰) 키로 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-163">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="930b1-164">둘째, 키가 없는 DbSpatialServices에 요청할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-164">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="930b1-165">해결 "전역 공간 공급자" 독립 실행형 DbGeography 또는 DbGeometry 유형을 만들 때 사용 되는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-165">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="930b1-166">EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-166">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="930b1-167">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="930b1-167">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="930b1-168">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-168">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-169">**반환 된 개체**: 쿼리와 명령을 데이터베이스에 대해 실행 될 때 다시 시도 하거나 다른 동작을 구현 하는 공급자 수 있는 서비스를 만들기 위한 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-169">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="930b1-170">구현이 제공 하는 경우 다음 EF 단순히 명령을 실행 하 여를 throw 된 예외를 전파 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-170">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="930b1-171">SQL Server에 대 한이 서비스는 SQL Azure 같은 클라우드 기반 데이터베이스 서버에 대해 실행할 때 특히 유용 하며 재시도 정책을 제공에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-171">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="930b1-172">**키**: 공급자 고정 이름 및 필요에 따라 실행 전략은 사용할 서버 이름을 포함 하는 ExecutionStrategyKey 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-172">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="930b1-173">EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-173">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="930b1-174">Func < DbConnection, 문자열, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="930b1-174">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="930b1-175">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-175">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-176">**반환 된 개체**: 공급자를 HistoryContext의 매핑을 구성할 수 있도록 하는 팩터리를 `__MigrationHistory` EF 마이그레이션을 사용 하는 테이블입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-176">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="930b1-177">HistoryContext 코드 첫 번째 DbContext 되며 테이블 및 열 매핑 사양 이름과 같은 항목을 변경 하려면 일반 흐름 API를 사용 하 여 구성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-177">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="930b1-178">**키**: 하지 사용; null</span><span class="sxs-lookup"><span data-stu-id="930b1-178">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="930b1-179">EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-179">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="930b1-180">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="930b1-180">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="930b1-181">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-181">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-182">**반환 된 개체**: 지정 된 공급자 고정 이름을 사용 하는 ADO.NET 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-182">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="930b1-183">**키**: ADO.NET 공급자 고정 이름을 포함 하는 문자열</span><span class="sxs-lookup"><span data-stu-id="930b1-183">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="930b1-184">이 서비스는 일반적으로 변경 되지 직접 기본 구현에서는 일반 ADO.NET 공급자 등록을 사용 하기 때문입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-184">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="930b1-185">EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-185">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="930b1-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="930b1-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="930b1-187">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-187">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-188">**반환 된 개체**: DbProviderFactory의 지정된 된 형식에 대 한 공급자 고정 이름을 확인 하는 데 사용 되는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-188">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="930b1-189">이 서비스의 기본 구현을 ADO.NET 공급자 등록을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-189">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="930b1-190">이 ADO.NET 공급자 DbProviderFactory를 확인 하는 EF에서 때문에 일반적인 방법으로 등록 되지 않은, 경우 다음도 해야이 서비스를 확인할 것을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-190">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="930b1-191">**키**: The DbProviderFactory 인스턴스 고정 이름이 필수입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-191">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="930b1-192">EF6에서 공급자 관련 서비스에 대 한 자세한 내용은 참조는 [EF6 공급자 모델](~/ef6/fundamentals/providers/provider-model.md) 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-192">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="930b1-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="930b1-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="930b1-194">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-194">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-195">**반환 된 개체**: 미리 생성 된 뷰가 포함 된 어셈블리를 캐시 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-195">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="930b1-196">대체 EF 모든 검색을 수행 하지 않고 미리 생성 된 뷰를 포함 하는 어셈블리를 알 수 있도록 일반적으로 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-196">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="930b1-197">**키**: 하지 사용; null</span><span class="sxs-lookup"><span data-stu-id="930b1-197">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="930b1-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="930b1-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="930b1-199">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-199">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-200">**반환 된 개체**: 복수화 이름을 단 수 화를 EF에서 사용 되는 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-200">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="930b1-201">기본적으로 영어 복수 적용 서비스를 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-201">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="930b1-202">**키**: 하지 사용; null</span><span class="sxs-lookup"><span data-stu-id="930b1-202">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="930b1-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="930b1-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="930b1-204">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-204">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="930b1-205">**반환 된 개체**: 시작 하면 응용 프로그램을 등록 해야 하는 모든 인터셉터입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-205">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="930b1-206">GetServices 호출을 사용 하 여 이러한 개체는 요청 및 모든 종속성 확인자를 반환한 모든 인터셉터는를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-206">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="930b1-207">**키**: 하지 사용; null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-207">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="930b1-208">Func < System.Data.Entity.DbContext, 작업 < 문자열\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="930b1-208">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="930b1-209">**도입 된 버전**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="930b1-209">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="930b1-210">**반환 된 개체**: 팩터리를 사용할 수 있는 데이터베이스 로그 포맷터를 만들 때 사용 되는 컨텍스트. 지정 된 컨텍스트에 따라 Database.Log 속성이 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-210">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="930b1-211">**키**: 하지 사용; null이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-211">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="930b1-212">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="930b1-212">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="930b1-213">**도입 된 버전**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="930b1-213">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="930b1-214">**반환 된 개체**: 컨텍스트 매개 변수가 없는 생성자를 액세스할 수 없는 경우 마이그레이션에 대 한 컨텍스트 인스턴스를 만드는 데 사용할 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-214">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="930b1-215">**키**: 파생 된 DbContext에는 팩터리를 필요한 형식의 Type 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-215">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="930b1-216">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="930b1-216">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="930b1-217">**도입 된 버전**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="930b1-217">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="930b1-218">**반환 된 개체**: 직렬화 및에서 Code First 마이그레이션을 사용 하기 위해 XML로 desterilized 수 있도록 강력한 사용자 지정 주석의 serialization에 대 한 serializer를 만드는 데 사용할 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-218">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="930b1-219">**키**: 되는 주석의 이름을 serialize 하거나 deserialize 합니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-219">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="930b1-220">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="930b1-220">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="930b1-221">**도입 된 버전**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="930b1-221">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="930b1-222">**반환 된 개체**: 커밋 실패를 처리 하는 등 상황에 대 한 특수 처리를 적용할 수 있도록 트랜잭션에 대 한 처리기를 만들려면 사용할 팩터리입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-222">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="930b1-223">**키**: 공급자 고정 이름 및 필요에 따라 트랜잭션 처리기를 사용할 서버 이름을 포함 하는 ExecutionStrategyKey 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="930b1-223">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
