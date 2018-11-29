---
title: 연결 문자열-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 7bb39d260f700e5087673e92a50377dc68151710
ms.sourcegitcommit: 85ccc9ed42d4aaf7525c6312058c5c9ebdaed3ae
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2018
ms.locfileid: "50191344"
---
# <a name="connection-strings"></a><span data-ttu-id="b1de0-102">연결 문자열</span><span class="sxs-lookup"><span data-stu-id="b1de0-102">Connection Strings</span></span>

<span data-ttu-id="b1de0-103">대부분의 데이터베이스 공급자는 특정 형태의 데이터베이스에 연결할 연결 문자열을 요구합니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="b1de0-104">이 연결 문자열은 보호해야 하는 중요한 정보를 포함하는 경우도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="b1de0-105">응용 프로그램 개발, 테스트 및 프로덕션 등 환경 간에 이동하면 연결 문자열을 변경해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="b1de0-106">.NET Framework 응용 프로그램</span><span class="sxs-lookup"><span data-stu-id="b1de0-106">.NET Framework Applications</span></span>

<span data-ttu-id="b1de0-107">WinForms, WPF, 콘솔 및 ASP.NET 4 등의.NET framework 응용 프로그램 테스트를 거친 연결 문자열 패턴을 경우</span><span class="sxs-lookup"><span data-stu-id="b1de0-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="b1de0-108">연결 문자열은 응용 프로그램 App.config 파일 (Web.config ASP.NET을 사용 하는 경우)에 추가 되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-108">The connection string should be added to your applications App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="b1de0-109">연결 문자열에는 사용자 이름 및 암호와 같은 중요 한 정보를 포함 하는 경우 사용 하 여 구성 파일의 콘텐츠를 보호할 수 있습니다 [보호 되는 구성을](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration)합니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> <span data-ttu-id="b1de0-110">`providerName` 설정을 데이터베이스 공급자 코드를 통해 구성했기 때문에 App.config에 저장된 EF 코어 연결 문자열에 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="b1de0-111">그런 다음 컨텍스트의 `OnConfiguring` 메서드에서 `ConfigurationManager` API를 사용하여 연결 문자열을 읽을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="b1de0-112">이 API를 사용하려면 `System.Configuration` 프레임워크 어셈블리에 대한 참조를 추가해야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="b1de0-113">UWP(유니버설 Windows 플랫폼)</span><span class="sxs-lookup"><span data-stu-id="b1de0-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="b1de0-114">UWP 응용 프로그램의 연결 문자열은 일반적으로 로컬 파일 이름만 지정하는 SQLite 연결입니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="b1de0-115">일반적으로 민감한 정보는 포함되어 있지 않으므로 응용 프로그램이 배포될 때 변경하지 않아도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="b1de0-116">따라서 이러한 연결 문자열은 일반적으로 아래와 같이 코드에 남게 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="b1de0-117">코드를 코드 밖으로 이동하려면 UWP가 설정 개념을 지원합니다. 자세한 내용은 [UWP 설명서의 앱 설정 섹션](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="b1de0-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a><span data-ttu-id="b1de0-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1de0-118">ASP.NET Core</span></span>

<span data-ttu-id="b1de0-119">ASP.NET Core에서 구성 시스템은 매우 유연하며 연결 문자열은 `appsettings.json`, 환경 변수, 사용자 비밀 저장소 또는 다른 구성 소스에 저장될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="b1de0-120">자세한 내용은 [ASP.NET 설명서의 구성 섹션](https://docs.asp.net/en/latest/fundamentals/configuration.html)을 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="b1de0-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="b1de0-121">다음 예는 `appsettings.json`에 저장된 연결 문자열을 보여줍니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="b1de0-122">컨텍스트는 일반적으로 구성에서 읽는 연결 문자열로 `Startup.cs`에 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="b1de0-123">`GetConnectionString()` 메서드는 키가 `ConnectionStrings:<connection string name>`인 구성 값을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="b1de0-124">이 확장 메서드를 사용하려면 [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) 네임 스페이스를 가져와야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b1de0-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
