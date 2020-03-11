---
title: 연결 문자열 및 모델-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415954"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="3e663-102">연결 문자열 및 모델</span><span class="sxs-lookup"><span data-stu-id="3e663-102">Connection strings and models</span></span>
<span data-ttu-id="3e663-103">이 항목에서는 Entity Framework에서 사용할 데이터베이스 연결을 검색 하는 방법 및 변경할 수 있는 방법에 대해 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="3e663-104">Code First 및 EF Designer를 사용 하 여 만든 모델은 모두이 항목에서 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="3e663-105">일반적으로 Entity Framework 응용 프로그램은 DbContext에서 파생 된 클래스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="3e663-106">이 파생 클래스는 기본 DbContext 클래스의 생성자 중 하나를 호출 하 여를 제어 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="3e663-107">컨텍스트에서 데이터베이스에 연결 하는 방법, 즉 연결 문자열을 찾거나 사용 하는 방법</span><span class="sxs-lookup"><span data-stu-id="3e663-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="3e663-108">컨텍스트에서 Code First를 사용 하 여 모델을 계산 하거나 EF Designer를 사용 하 여 만든 모델을 로드할지 여부</span><span class="sxs-lookup"><span data-stu-id="3e663-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="3e663-109">추가 고급 옵션</span><span class="sxs-lookup"><span data-stu-id="3e663-109">Additional advanced options</span></span>  

<span data-ttu-id="3e663-110">다음 조각은 DbContext 생성자를 사용 하는 몇 가지 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="3e663-111">규칙에 따라 연결 Code First 사용</span><span class="sxs-lookup"><span data-stu-id="3e663-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="3e663-112">응용 프로그램에서 다른 구성을 수행 하지 않은 경우 DbContext에서 매개 변수가 없는 생성자를 호출 하면 DbContext가 규칙에 따라 생성 된 데이터베이스 연결을 사용 하 여 Code First 모드로 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="3e663-113">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-113">For example:</span></span>  

``` csharp  
namespace Demo.EF
{
    public class BloggingContext : DbContext
    {
        public BloggingContext()
        // C# will call base class parameterless constructor by default
        {
        }
    }
}
```  

<span data-ttu-id="3e663-114">이 예제에서 DbContext는 파생 컨텍스트 클래스 (BloggingContext)의 네임 스페이스로 정규화 된 이름을 데이터베이스 이름으로 사용 하 고 SQL Express 또는 LocalDB를 사용 하 여이 데이터베이스에 대 한 연결 문자열을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="3e663-115">둘 다 설치 된 경우 SQL Express가 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="3e663-116">Visual Studio 2010에는 기본적으로 SQL Express가 포함 되 고 Visual Studio 2012 이상에는 LocalDB가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="3e663-117">설치 하는 동안 EntityFramework NuGet 패키지는 사용할 수 있는 데이터베이스 서버를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="3e663-118">그런 다음 NuGet 패키지는 규칙에 따라 연결을 만들 때 Code First 사용 하는 기본 데이터베이스 서버를 설정 하 여 구성 파일을 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="3e663-119">SQL Express를 실행 하는 경우 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="3e663-120">SQL Express를 사용할 수 없는 경우 LocalDB가 기본값으로 등록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="3e663-121">기본 연결 팩터리의 설정이 이미 포함 되어 있으면 구성 파일이 변경 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="3e663-122">규칙 및 지정 된 데이터베이스 이름으로 연결 Code First 사용</span><span class="sxs-lookup"><span data-stu-id="3e663-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="3e663-123">응용 프로그램에서 다른 구성을 수행 하지 않은 경우 사용 하려는 데이터베이스 이름으로 DbContext의 문자열 생성자를 호출 하면 DbContext가 데이터베이스에 대해 규칙에 따라 생성 된 데이터베이스 연결을 사용 하 여 Code First 모드에서 실행 됩니다. 해당 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="3e663-124">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="3e663-125">이 예제에서 DbContext는 데이터베이스 이름으로 "BloggingDatabase"를 사용 하 고 SQL Express (Visual Studio 2010와 함께 설치 됨) 또는 LocalDB (Visual Studio 2012과 함께 설치 됨)를 사용 하 여이 데이터베이스에 대 한 연결 문자열을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="3e663-126">둘 다 설치 된 경우 SQL Express가 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="3e663-127">App.config/web.config 파일에 연결 문자열을 사용 하 여 Code First 사용</span><span class="sxs-lookup"><span data-stu-id="3e663-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="3e663-128">App.config 또는 web.config 파일에 연결 문자열을 배치 하도록 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="3e663-129">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="3e663-130">이 방법은 SQL Express 또는 LocalDB 이외의 데이터베이스 서버를 사용 하도록 DbContext에 지시 하는 쉬운 방법입니다. 위의 예에서는 SQL Server Compact Edition 데이터베이스를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="3e663-131">연결 문자열의 이름이 네임 스페이스 정규화를 사용 하거나 사용 하지 않는 컨텍스트 이름과 일치 하는 경우에는 매개 변수가 없는 생성자가 사용 될 때 DbContext에 의해 검색 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="3e663-132">연결 문자열 이름이 컨텍스트 이름과 다른 경우 연결 문자열 이름을 DbContext 생성자에 전달 하 여 Code First 모드에서이 연결을 사용 하도록 DbContext에 지시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="3e663-133">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="3e663-134">또는 DbContext 생성자에 전달 된 문자열에 대해 "name =\<연결 문자열 이름\>" 형식을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="3e663-135">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="3e663-136">이 폼을 사용 하면 연결 문자열을 구성 파일에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="3e663-137">지정 된 이름을 가진 연결 문자열을 찾을 수 없는 경우 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="3e663-138">App.config/web.config 파일에 연결 문자열을 사용 하는 데이터베이스/Model First</span><span class="sxs-lookup"><span data-stu-id="3e663-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="3e663-139">EF Designer를 사용 하 여 만든 모델은 모델이 이미 존재 하 고 응용 프로그램이 실행 될 때 코드에서 생성 되지 않기 때문에 Code First와 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="3e663-140">모델은 일반적으로 프로젝트의 EDMX 파일로 존재 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="3e663-141">디자이너는 app.config 또는 web.config 파일에 EF 연결 문자열을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="3e663-142">이 연결 문자열은 EDMX 파일에서 정보를 찾는 방법에 대 한 정보를 포함 하 고 있으므로 특수 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="3e663-143">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-143">For example:</span></span>  

``` xml  
<configuration>  
  <connectionStrings>  
    <add name="Northwind_Entities"  
         connectionString="metadata=res://*/Northwind.csdl|  
                                    res://*/Northwind.ssdl|  
                                    res://*/Northwind.msl;  
                           provider=System.Data.SqlClient;  
                           provider connection string=  
                               &quot;Data Source=.\sqlexpress;  
                                     Initial Catalog=Northwind;  
                                     Integrated Security=True;  
                                     MultipleActiveResultSets=True&quot;"  
         providerName="System.Data.EntityClient"/>  
  </connectionStrings>  
</configuration>
```  

<span data-ttu-id="3e663-144">또한 EF 디자이너는 연결 문자열 이름을 DbContext 생성자에 전달 하 여이 연결을 사용 하도록 DbContext에 지시 하는 코드도 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="3e663-145">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="3e663-146">연결 문자열은 사용할 모델의 세부 정보를 포함 하는 EF 연결 문자열 이므로 DbContext은 코드에서 계산 하는 데 Code First를 사용 하는 대신 기존 모델을 로드 하는 것을 알고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="3e663-147">기타 DbContext 생성자 옵션</span><span class="sxs-lookup"><span data-stu-id="3e663-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="3e663-148">DbContext 클래스에는 고급 시나리오를 사용할 수 있는 다른 생성자 및 사용 패턴이 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="3e663-149">그 중 일부는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-149">Some of these are:</span></span>  

- <span data-ttu-id="3e663-150">DbModelBuilder 클래스를 사용 하 여 DbContext 인스턴스를 인스턴스화하지 않고 Code First 모델을 빌드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="3e663-151">이 결과는 DbModel 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="3e663-152">그런 다음 DbContext 인스턴스를 만들 준비가 되 면 DbContext 생성자 중 하나에이 DbModel 개체를 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="3e663-153">데이터베이스 또는 연결 문자열 이름 뿐만 아니라 전체 연결 문자열을 DbContext에 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="3e663-154">기본적으로이 연결 문자열은 System.web. SqlClient 공급자와 함께 사용 됩니다. 이는 IConnectionFactory의 다른 구현을 컨텍스트로 설정 하 여 변경할 수 있습니다. 데이터베이스. DefaultConnectionFactory.</span><span class="sxs-lookup"><span data-stu-id="3e663-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="3e663-155">기존 DbConnection 개체를 DbContext 생성자에 전달 하 여 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="3e663-156">연결 개체가 EntityConnection 인스턴스이면 연결에 지정 된 모델이 Code First를 사용 하 여 모델을 계산 하는 대신 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="3e663-157">개체가 다른 형식의 인스턴스인 경우 (예: SqlConnection) 컨텍스트는이를 Code First 모드에 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="3e663-158">기존 ObjectContext를 DbContext 생성자에 전달 하 여 기존 컨텍스트를 DbContext 래핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="3e663-159">이는 ObjectContext를 사용 하지만 응용 프로그램의 일부 부분에서 DbContext를 활용 하려는 기존 응용 프로그램에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3e663-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
