---
title: 코드 기반 구성-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
ms.openlocfilehash: c317f112f713612f7b9aef3764a0bd004fef5424
ms.sourcegitcommit: 735715f10cc8a231c213e4f055d79f0effd86570
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/16/2019
ms.locfileid: "56325355"
---
# <a name="code-based-configuration"></a><span data-ttu-id="3526a-102">코드 기반 구성</span><span class="sxs-lookup"><span data-stu-id="3526a-102">Code-based configuration</span></span>
> [!NOTE]
> <span data-ttu-id="3526a-103">**EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="3526a-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="3526a-105">Entity Framework 응용 프로그램에 대 한 구성은 구성 파일 (app.config/web.config) 또는 코드를 통해 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-105">Configuration for an Entity Framework application can be specified in a config file (app.config/web.config) or through code.</span></span> <span data-ttu-id="3526a-106">후자는 라고 코드 기반 구성 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-106">The latter is known as code-based configuration.</span></span>  

<span data-ttu-id="3526a-107">구성 파일에서 구성에 설명 되어는 [별도 문서](config-file.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-107">Configuration in a config file is described in a [separate article](config-file.md).</span></span> <span data-ttu-id="3526a-108">구성 파일 코드 기반 구성 보다 우선합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-108">The config file takes precedence over code-based configuration.</span></span> <span data-ttu-id="3526a-109">즉, 구성 옵션 모두 코드 및 config 파일을 설정 하는 경우 구성 파일의 설정이 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-109">In other words, if a configuration option is set in both code and in the config file, then the setting in the config file is used.</span></span>  

## <a name="using-dbconfiguration"></a><span data-ttu-id="3526a-110">DbConfiguration를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="3526a-110">Using DbConfiguration</span></span>  

<span data-ttu-id="3526a-111">코드 기반 구성 이상 EF6에서 System.Data.Entity.Config.DbConfiguration의 서브 클래스를 만들어서 이루어집니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-111">Code-based configuration in EF6 and above is achieved by creating a subclass of System.Data.Entity.Config.DbConfiguration.</span></span> <span data-ttu-id="3526a-112">DbConfiguration 서브클래싱 할 경우 다음 지침을 따라야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-112">The following guidelines should be followed when subclassing DbConfiguration:</span></span>  

- <span data-ttu-id="3526a-113">응용 프로그램에 대 한 하나의 DbConfiguration 클래스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-113">Create only one DbConfiguration class for your application.</span></span> <span data-ttu-id="3526a-114">이 클래스는 응용 프로그램 도메인 전체 설정을 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-114">This class specifies app-domain wide settings.</span></span>  
- <span data-ttu-id="3526a-115">DbConfiguration 클래스 DbContext 클래스와 동일한 어셈블리에 배치 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-115">Place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="3526a-116">(참조를 *DbConfiguration 이동* 변경 하려는 경우 섹션입니다.)</span><span class="sxs-lookup"><span data-stu-id="3526a-116">(See the *Moving DbConfiguration* section if you want to change this.)</span></span>  
- <span data-ttu-id="3526a-117">DbConfiguration 클래스에 매개 변수가 없는 public 생성자를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-117">Give your DbConfiguration class a public parameterless constructor.</span></span>  
- <span data-ttu-id="3526a-118">이 생성자 내에서 보호 된 DbConfiguration 메서드를 호출 하 여 구성 옵션을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-118">Set configuration options by calling protected DbConfiguration methods from within this constructor.</span></span>  

<span data-ttu-id="3526a-119">다음 지침에 따라 EF를 검색 하 고 모델에 액세스 해야 하 고 응용 프로그램이 실행 될 때 도구 모두에서 구성을 자동으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-119">Following these guidelines allows EF to discover and use your configuration automatically by both tooling that needs to access your model and when your application is run.</span></span>  

## <a name="example"></a><span data-ttu-id="3526a-120">예제</span><span class="sxs-lookup"><span data-stu-id="3526a-120">Example</span></span>  

<span data-ttu-id="3526a-121">DbConfiguration에서 파생 된 클래스는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-121">A class derived from DbConfiguration might look like this:</span></span>  

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

<span data-ttu-id="3526a-122">이 클래스 EF를 자동으로 실패 한 데이터베이스 작업-다시 시도 하 고 Code First에서 규칙에 따라 생성 된 데이터베이스에 대 한 로컬 DB를 사용 하려면 SQL Azure 실행 전략-을 사용 하도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-122">This class sets up EF to use the SQL Azure execution strategy - to automatically retry failed database operations - and to use Local DB for databases that are created by convention from Code First.</span></span>  

## <a name="moving-dbconfiguration"></a><span data-ttu-id="3526a-123">DbConfiguration 이동</span><span class="sxs-lookup"><span data-stu-id="3526a-123">Moving DbConfiguration</span></span>  

<span data-ttu-id="3526a-124">DbConfiguration 클래스 DbContext 클래스와 동일한 어셈블리에 적용할 수 없는 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-124">There are cases where it is not possible to place your DbConfiguration class in the same assembly as your DbContext class.</span></span> <span data-ttu-id="3526a-125">예를 들어 두 DbContext 클래스는 각각 서로 다른 어셈블리에서 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-125">For example, you may have two DbContext classes each in different assemblies.</span></span> <span data-ttu-id="3526a-126">이 처리 하기 위한 두 가지 옵션이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-126">There are two options for handling this.</span></span>  

<span data-ttu-id="3526a-127">첫 번째 옵션 DbConfiguration 사용할 인스턴스를 지정 하려면 구성 파일을 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-127">The first option is to use the config file to specify the DbConfiguration instance to use.</span></span> <span data-ttu-id="3526a-128">이 작업을 수행 하려면 entityFramework 구역의 codeConfigurationType 특성을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-128">To do this, set the codeConfigurationType attribute of the entityFramework section.</span></span> <span data-ttu-id="3526a-129">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="3526a-129">For example:</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

<span data-ttu-id="3526a-130">어셈블리 및 네임 스페이스 DbConfiguration 클래스의 정규화 된 이름을 codeConfigurationType 값 이어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-130">The value of codeConfigurationType must be the assembly and namespace qualified name of your DbConfiguration class.</span></span>  

<span data-ttu-id="3526a-131">두 번째 방법은 DbConfigurationTypeAttribute 컨텍스트 클래스에 배치 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-131">The second option is to place DbConfigurationTypeAttribute on your context class.</span></span> <span data-ttu-id="3526a-132">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="3526a-132">For example:</span></span>  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

<span data-ttu-id="3526a-133">수 DbConfiguration 형식--위에 표시 된 대로 특성에 전달 된 값 또는 어셈블리와 네임 스페이스 정규화 된 형식 이름 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-133">The value passed to the attribute can either be your DbConfiguration type - as shown above - or the assembly and namespace qualified type name string.</span></span> <span data-ttu-id="3526a-134">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="3526a-134">For example:</span></span>  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a><span data-ttu-id="3526a-135">DbConfiguration를 명시적으로 설정</span><span class="sxs-lookup"><span data-stu-id="3526a-135">Setting DbConfiguration explicitly</span></span>  

<span data-ttu-id="3526a-136">DbContext 형식일이 사용 하기 전에 구성 필요할 수 있는 몇 가지 경우가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-136">There are some situations where configuration may be needed before any DbContext type has been used.</span></span> <span data-ttu-id="3526a-137">이 예제:</span><span class="sxs-lookup"><span data-stu-id="3526a-137">Examples of this include:</span></span>  

- <span data-ttu-id="3526a-138">DbModelBuilder를 사용 하 여 컨텍스트 없이 모델 작성</span><span class="sxs-lookup"><span data-stu-id="3526a-138">Using DbModelBuilder to build a model without a context</span></span>  
- <span data-ttu-id="3526a-139">응용 프로그램 컨텍스트를 사용 하기 전에 해당 컨텍스트를 사용 하는 위치 DbContext를 사용 하는 다른 프레임 워크/유틸리티 코드를 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="3526a-139">Using some other framework/utility code that utilizes a DbContext where that context is used before your application context is used</span></span>  

<span data-ttu-id="3526a-140">이러한 상황에서는 EF 구성을 자동으로 검색할 수 있도록 아니며 다음 중 하나를 대신 수행 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-140">In such situations EF is unable to discover the configuration automatically and you must instead do one of the following:</span></span>  

- <span data-ttu-id="3526a-141">에 설명 된 대로 구성 파일에서 DbConfiguration 형식을 설정 합니다 *DbConfiguration 이동* 위의 섹션</span><span class="sxs-lookup"><span data-stu-id="3526a-141">Set the DbConfiguration type in the config file, as described in the *Moving DbConfiguration* section above</span></span>
- <span data-ttu-id="3526a-142">응용 프로그램 시작 하는 동안 정적 DbConfiguration.SetConfiguration 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-142">Call the static DbConfiguration.SetConfiguration method during application startup</span></span>  

## <a name="overriding-dbconfiguration"></a><span data-ttu-id="3526a-143">DbConfiguration 재정의</span><span class="sxs-lookup"><span data-stu-id="3526a-143">Overriding DbConfiguration</span></span>  

<span data-ttu-id="3526a-144">DbConfiguration에서 설정 된 구성을 재정의 해야 하는 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-144">There are some situations where you need to override the configuration set in the DbConfiguration.</span></span> <span data-ttu-id="3526a-145">이렇게 하지 않으면 일반적으로 응용 프로그램 개발자가 아니라 대신 타사 공급자 및 플러그 인 파생된 DbConfiguration 클래스를 사용할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-145">This is not typically done by application developers but rather by third party providers and plug-ins that cannot use a derived DbConfiguration class.</span></span>  

<span data-ttu-id="3526a-146">EntityFramework는이 위해 잠기기 직전 기존 구성을 수정할 수 있는 이벤트 처리기를를 등록할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-146">For this, EntityFramework allows an event handler to be registered that can modify existing configuration just before it is locked down.</span></span>  <span data-ttu-id="3526a-147">또한 대체 EF 서비스 로케이터를 반환한 모든 서비스에 맞게 sugar 메서드를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-147">It also provides a sugar method specifically for replacing any service returned by the EF service locator.</span></span> <span data-ttu-id="3526a-148">다음은 방법을 사용할 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-148">This is how it is intended to be used:</span></span>  

- <span data-ttu-id="3526a-149">(EF가 사용 됨) 전에 앱 시작 시 플러그 인 또는 공급자는이 이벤트에 대 한 이벤트 처리기 메서드를 등록 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-149">At app startup (before EF is used) the plug-in or provider should register the event handler method for this event.</span></span> <span data-ttu-id="3526a-150">(응용 프로그램 EF를 사용 하기 전에 발생 해야이 note 합니다.)</span><span class="sxs-lookup"><span data-stu-id="3526a-150">(Note that this must happen before the application uses EF.)</span></span>  
- <span data-ttu-id="3526a-151">이벤트 처리기를 호출 ReplaceService 교체 해야 하는 모든 서비스에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-151">The event handler makes a call to ReplaceService for every service that needs to be replaced.</span></span>  

<span data-ttu-id="3526a-152">예를 들어 repalce IDbConnectionFactory DbProviderService를 다음과 같이 결과 처리기 등록:</span><span class="sxs-lookup"><span data-stu-id="3526a-152">For example, to repalce IDbConnectionFactory and DbProviderService you would register a handler something like this:</span></span>  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

<span data-ttu-id="3526a-153">MyProviderServices 및 MyConnectionFactory 위의 코드에서 서비스의 구현을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-153">In the code above MyProviderServices and MyConnectionFactory represent your implementations of the service.</span></span>  

<span data-ttu-id="3526a-154">또한 동일한 효과를 얻을 때 추가 종속성 처리기를 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-154">You can also add additional dependency handlers to get the same effect.</span></span>  

<span data-ttu-id="3526a-155">따라서에서 DbProviderFactory를 래핑할 수도 있습니다 하지만 수행 하므로 영향을 받습니다 EF 및 EF 외부 DbProviderFactory 사용 하지는 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-155">Note that you could also wrap DbProviderFactory in this way, but doing so will only affect EF and not uses of the DbProviderFactory outside of EF.</span></span> <span data-ttu-id="3526a-156">이러한 이유로 원할 것 계속 하기 전에 동일 DbProviderFactory를 래핑합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-156">For this reason you’ll probably want to continue to wrap DbProviderFactory as you have before.</span></span>  

<span data-ttu-id="3526a-157">또한 유지 해야 염두에서 실행 하는 응용 프로그램 외부에 예를 들어, 패키지 관리자 콘솔에서 마이그레이션을 실행할 때 서비스입니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-157">You should also keep in mind the services that you run externally to your application - for example, when running migrations from the Package Manager Console.</span></span> <span data-ttu-id="3526a-158">실행 하는 경우에 DbConfiguration을 찾으려고 시도 하는 콘솔에서 마이그레이션하십시오.</span><span class="sxs-lookup"><span data-stu-id="3526a-158">When you run migrate from the console it will attempt to find your DbConfiguration.</span></span> <span data-ttu-id="3526a-159">그러나 래핑된를 가져오게 됩니다 여부에 따라 달라 집니다 where 이벤트 처리기를 등록 합니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-159">However, whether or not it will get the wrapped service depends on where the event handler it registered.</span></span> <span data-ttu-id="3526a-160">프로그램 DbConfiguration 생성의 일부로 등록 된 경우 그런 다음 코드를 실행 해야 하 고 서비스 래핑해야 가져옵니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-160">If it is registered as part of the construction of your DbConfiguration then the code should execute and the service should get wrapped.</span></span> <span data-ttu-id="3526a-161">일반적으로이 그렇지 않을 고 즉, 도구가 래핑된 서비스를 가져오지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3526a-161">Usually this won’t be the case and this means that tooling won’t get the wrapped service.</span></span>  
