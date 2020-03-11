---
title: 코드 기반 구성-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: 079a4ab30af74eac8b1f51ece5801ff40a867a29
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414802"
---
# <a name="code-based-configuration"></a><span data-ttu-id="35d34-102">코드 기반 구성</span><span class="sxs-lookup"><span data-stu-id="35d34-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="35d34-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="35d34-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="35d34-105">구성 파일 (app.config/web.config) 또는 코드를 통해 Entity Framework 응용 프로그램에 대 한 구성을 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="35d34-106">후자를 코드 기반 구성 이라고 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="35d34-107">구성 파일의 구성은 [별도의 문서](config-file.md)에 설명 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="35d34-108">구성 파일이 코드 기반 구성 보다 우선적으로 적용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="35d34-109">즉, 구성 옵션이 코드와 구성 파일 모두에 설정 된 경우 구성 파일의 설정이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="35d34-110">DbConfiguration 사용</span><span class="sxs-lookup"><span data-stu-id="35d34-110">Using DbConfiguration</span></span>  

<span data-ttu-id="35d34-111">EF6 이상의 하위 클래스를 만들어 이러한 구성 요소의 코드 기반 구성을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="35d34-112">DbConfiguration를 서브클래싱 할 때 다음 지침을 따라야 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="35d34-113">응용 프로그램에 대해 DbConfiguration 클래스를 하나만 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="35d34-114">이 클래스는 앱 도메인 전체 설정을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="35d34-115">DbConfiguration 클래스를 DbContext 클래스와 동일한 어셈블리에 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="35d34-116">(이를 변경 하려는 경우 *이동 DbConfiguration* 섹션을 참조 하세요.)</span><span class="sxs-lookup"><span data-stu-id="35d34-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="35d34-117">DbConfiguration 클래스에 매개 변수가 없는 public 생성자를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="35d34-118">이 생성자 내에서 보호 된 DbConfiguration 메서드를 호출 하 여 구성 옵션을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="35d34-119">이러한 지침에 따라 EF는 모델에 액세스 해야 하는 도구와 응용 프로그램이 실행 될 때 모두 자동으로 구성을 검색 하 고 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="35d34-120">예제</span><span class="sxs-lookup"><span data-stu-id="35d34-120">Example</span></span>  

<span data-ttu-id="35d34-121">DbConfiguration에서 파생 된 클래스는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-121">A class derived from DbConfiguration might look like this:</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDbConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

<span data-ttu-id="35d34-122">이 클래스는 SQL Azure 실행 전략을 사용 하도록 EF를 설정 하 여 실패 한 데이터베이스 작업을 자동으로 다시 시도 하 고 Code First에서 규칙으로 만들어진 데이터베이스에 로컬 DB를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="35d34-123">DbConfiguration 이동</span><span class="sxs-lookup"><span data-stu-id="35d34-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="35d34-124">DbConfiguration 클래스를 DbContext 클래스와 동일한 어셈블리에 저장할 수 없는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="35d34-125">예를 들어 두 개의 DbContext 클래스가 서로 다른 어셈블리에 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="35d34-126">이를 처리 하는 두 가지 옵션이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-126">There are two options for handling this.</span></span>  

<span data-ttu-id="35d34-127">첫 번째 옵션은 구성 파일을 사용 하 여 사용할 DbConfiguration 인스턴스를 지정 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="35d34-128">이렇게 하려면 entityFramework 섹션의 codeConfigurationType 특성을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="35d34-129">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="35d34-130">CodeConfigurationType의 값은 DbConfiguration 클래스의 어셈블리 및 네임 스페이스 정규화 된 이름 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="35d34-131">두 번째 옵션은 컨텍스트 클래스에 DbConfigurationTypeAttribute를 추가 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="35d34-132">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="35d34-133">특성에 전달 된 값은 위에 표시 된 것 처럼 DbConfiguration 형식 이거나 어셈블리 및 네임 스페이스 정규화 된 형식 이름 문자열 일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="35d34-134">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="35d34-135">DbConfiguration 명시적 설정</span><span class="sxs-lookup"><span data-stu-id="35d34-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="35d34-136">DbContext 형식을 사용 하기 전에 구성이 필요할 수 있는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="35d34-137">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-137">Examples of this include:</span></span>  

- <span data-ttu-id="35d34-138">DbModelBuilder를 사용 하 여 컨텍스트가 없는 모델 빌드</span><span class="sxs-lookup"><span data-stu-id="35d34-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="35d34-139">응용 프로그램 컨텍스트를 사용 하기 전에 해당 컨텍스트가 사용 되는 DbContext를 활용 하는 다른 프레임 워크/유틸리티 코드 사용</span><span class="sxs-lookup"><span data-stu-id="35d34-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="35d34-140">이러한 상황에서 EF는 구성을 자동으로 검색할 수 없으며 대신 다음 중 하나를 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="35d34-141">위의 *dbconfiguration* 섹션에서 설명한 대로 구성 파일에서 dbconfiguration 유형을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="35d34-142">응용 프로그램 시작 중에 정적 DbConfiguration. SetConfiguration 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="35d34-143">DbConfiguration 재정의</span><span class="sxs-lookup"><span data-stu-id="35d34-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="35d34-144">DbConfiguration에서 구성 집합을 재정의 해야 하는 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="35d34-145">이는 일반적으로 응용 프로그램 개발자가 수행 하는 것이 아니라 파생 된 DbConfiguration 클래스를 사용할 수 없는 타사 공급자 및 플러그 인을 통해 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="35d34-146">이를 위해 EntityFramework는 잠금 해제 되기 직전에 기존 구성을 수정할 수 있는 이벤트 처리기를 등록할 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="35d34-147">또한 EF service locator에서 반환 하는 모든 서비스를 바꾸기 위한 sugar 메서드를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="35d34-148">이를 사용 하는 방법은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="35d34-149">EF를 사용 하기 전에 앱을 시작할 때 플러그 인 또는 공급자는이 이벤트에 대 한 이벤트 처리기 메서드를 등록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="35d34-150">이는 응용 프로그램이 EF를 사용 하기 전에 발생 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="35d34-151">이벤트 처리기는 교체 해야 하는 모든 서비스에 대해 ReplaceService를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="35d34-152">예를 들어 IDbConnectionFactory 및 DbProviderService를 대체 하려면 다음과 같이 처리기를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-152">For example, to replace IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="35d34-153">위의 코드에서 MyProviderServices 및 Myproviderservices는 서비스의 구현을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="35d34-154">다른 종속성 처리기를 추가 하 여 동일한 결과를 얻을 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="35d34-155">이러한 방식으로 DbProviderFactory 래핑할 수도 있지만이 경우 ef 외부의 DbProviderFactory를 사용 하는 것이 아니라 EF에만 영향을 줍니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="35d34-156">이러한 이유로 이전 처럼 DbProviderFactory 래핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="35d34-157">또한 패키지 관리자 콘솔에서 마이그레이션을 실행 하는 경우와 같이 응용 프로그램 외부에서 실행 하는 서비스를 염두에 두어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="35d34-158">콘솔에서 마이그레이션을 실행 하면 DbConfiguration 찾기가 시도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="35d34-159">그러나 래핑된 서비스를 가져올 것인지 여부는 등록 된 이벤트 처리기의 위치에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="35d34-160">DbConfiguration 생성의 일부로 등록 된 경우 코드를 실행 하 고 서비스를 래핑해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="35d34-161">일반적으로이 경우에는 이러한 경우가 발생 하지 않습니다. 즉, 도구는 래핑된 서비스를 가져오지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="35d34-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
