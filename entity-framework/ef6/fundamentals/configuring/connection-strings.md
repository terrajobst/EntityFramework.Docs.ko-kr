---
title: 연결 문자열 및 EF6-모델
author: divega
ms.date: 10/23/2016
ms.assetid: 294bb138-978f-4fe2-8491-fdf3cd3c60c4
ms.openlocfilehash: 2c9f084107e4de7f5439bf0082b46a3b538496e0
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490749"
---
# <a name="connection-strings-and-models"></a><span data-ttu-id="4c84a-102">연결 문자열 및 모델</span><span class="sxs-lookup"><span data-stu-id="4c84a-102">Connection strings and models</span></span>
<span data-ttu-id="4c84a-103">이 항목에서는 Entity Framework를 사용 하려면 데이터베이스 연결을 검색 하는 방법 및이 변경 하는 방법을 설명 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-103">This topic covers how Entity Framework discovers which database connection to use, and how you can change it.</span></span> <span data-ttu-id="4c84a-104">Code First 및 EF 디자이너를 사용 하 여 만든 모델은 모두이 항목에서 설명 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-104">Models created with Code First and the EF Designer are both covered in this topic.</span></span>  

<span data-ttu-id="4c84a-105">일반적으로 Entity Framework 응용 프로그램 DbContext에서 파생 된 클래스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-105">Typically an Entity Framework application uses a class derived from DbContext.</span></span> <span data-ttu-id="4c84a-106">이 파생된 클래스는 컨트롤에 기본 DbContext 클래스의 생성자 중 하나를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-106">This derived class will call one of the constructors on the base DbContext class to control:</span></span>  

- <span data-ttu-id="4c84a-107">컨텍스트 데이터베이스에 연결 하는 방법을-즉, 어떻게 연결 문자열은 발견 하 고 사용</span><span class="sxs-lookup"><span data-stu-id="4c84a-107">How the context will connect to a database — that is, how a connection string is found/used</span></span>  
- <span data-ttu-id="4c84a-108">Code First를 사용 하 여 모델 계산 또는 EF 디자이너를 사용 하 여 만든 모델을 로드 컨텍스트를 사용할지 여부를</span><span class="sxs-lookup"><span data-stu-id="4c84a-108">Whether the context will use calculate a model using Code First or load a model created with the EF Designer</span></span>  
- <span data-ttu-id="4c84a-109">추가 고급 옵션</span><span class="sxs-lookup"><span data-stu-id="4c84a-109">Additional advanced options</span></span>  

<span data-ttu-id="4c84a-110">다음 코드 조각이 표시 일부의 DbContext 생성자를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-110">The following fragments show some of the ways the DbContext constructors can be used.</span></span>  

## <a name="use-code-first-with-connection-by-convention"></a><span data-ttu-id="4c84a-111">규칙에 따라 연결을 사용 하 여 Code First 사용</span><span class="sxs-lookup"><span data-stu-id="4c84a-111">Use Code First with connection by convention</span></span>  

<span data-ttu-id="4c84a-112">응용 프로그램에서 다른 구성을 수행 하지 않은 경우 DbContext에서 매개 변수가 없는 생성자를 호출 하는 다음 규칙을 만든 데이터베이스 연결을 사용 하 여 Code First 모드에서 실행 되도록 DbContext 발생 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-112">If you have not done any other configuration in your application, then calling the parameterless constructor on DbContext will cause DbContext to run in Code First mode with a database connection created by convention.</span></span> <span data-ttu-id="4c84a-113">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="4c84a-113">For example:</span></span>  

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

<span data-ttu-id="4c84a-114">이 예에서 DbContext에 파생된 컨텍스트 class—Demo.EF.BloggingContext—as 데이터베이스 이름은 네임 스페이스 정규화 된 이름을 사용 하 고 SQL Express 또는 LocalDB를 사용 하 여이 데이터베이스에 대 한 연결 문자열을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-114">In this example DbContext uses the namespace qualified name of your derived context class—Demo.EF.BloggingContext—as the database name and creates a connection string for this database using either SQL Express or LocalDB.</span></span> <span data-ttu-id="4c84a-115">둘 다 설치 된 SQL Express 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-115">If both are installed, SQL Express will be used.</span></span>  

<span data-ttu-id="4c84a-116">Visual Studio 2010 기본 및 Visual Studio 2012에서 SQL Express를 포함 하 고 나중에 LocalDB를 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-116">Visual Studio 2010 includes SQL Express by default and Visual Studio 2012 and later includes LocalDB.</span></span> <span data-ttu-id="4c84a-117">EntityFramework NuGet 패키지를 설치 하는 동안 데이터베이스 서버를 사용할 수를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-117">During installation, the EntityFramework NuGet package checks which database server is available.</span></span> <span data-ttu-id="4c84a-118">다음 NuGet 패키지 규칙에 따라 연결을 만들 때에 Code First를 사용 하 여 기본 데이터베이스 서버를 설정 하 여 구성 파일을 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-118">The NuGet package will then update the configuration file by setting the default database server that Code First uses when creating a connection by convention.</span></span> <span data-ttu-id="4c84a-119">SQL Express를 실행 하는 경우 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-119">If SQL Express is running, it will be used.</span></span> <span data-ttu-id="4c84a-120">SQL Express 사용할 수 없는 경우 다음 LocalDB은 등록할 기본값으로 대신 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-120">If SQL Express is not available then LocalDB will be registered as the default instead.</span></span> <span data-ttu-id="4c84a-121">기본 연결 팩터리가 대 한 설정이 포함 되어 있으면 해당 구성 파일에 변경 된 내용이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-121">No changes are made to the configuration file if it already contains a setting for the default connection factory.</span></span>  

## <a name="use-code-first-with-connection-by-convention-and-specified-database-name"></a><span data-ttu-id="4c84a-122">규칙 및 지정 된 데이터베이스 이름으로 연결을 사용 하 여 Code First 사용</span><span class="sxs-lookup"><span data-stu-id="4c84a-122">Use Code First with connection by convention and specified database name</span></span>  

<span data-ttu-id="4c84a-123">응용 프로그램에서 다른 구성을 수행 하지 않은 경우 다음 사용 하려는 데이터베이스 이름으로 DbContext에서 string 생성자를 호출 하면 규칙 데이터베이스를 만든 데이터베이스 연결을 사용 하 여 Code First 모드에서 실행 되도록 DbContext 해당 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-123">If you have not done any other configuration in your application, then calling the string constructor on DbContext with the database name you want to use will cause DbContext to run in Code First mode with a database connection created by convention to the database of that name.</span></span> <span data-ttu-id="4c84a-124">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="4c84a-124">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingDatabase")
    {
    }
}
```  

<span data-ttu-id="4c84a-125">이 예제의 DbContext 데이터베이스 이름으로 "BloggingDatabase"를 사용 하 고 (Visual Studio 2010을 사용 하 여 설치) SQL Express 또는 LocalDB (Visual Studio 2012와 함께 설치 됨)를 사용 하 여이 데이터베이스에 대 한 연결 문자열을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-125">In this example DbContext uses “BloggingDatabase” as the database name and creates a connection string for this database using either SQL Express (installed with Visual Studio 2010) or LocalDB (installed with Visual Studio 2012).</span></span> <span data-ttu-id="4c84a-126">둘 다 설치 된 SQL Express 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-126">If both are installed, SQL Express will be used.</span></span>  

## <a name="use-code-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="4c84a-127">App.config/web.config 파일에서 연결 문자열에 Code First 사용</span><span class="sxs-lookup"><span data-stu-id="4c84a-127">Use Code First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="4c84a-128">연결 문자열을 app.config 또는 web.config 파일에 삽입할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-128">You may choose to put a connection string in your app.config or web.config file.</span></span> <span data-ttu-id="4c84a-129">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="4c84a-129">For example:</span></span>  

``` xml  
<configuration>
  <connectionStrings>
    <add name="BloggingCompactDatabase"
         providerName="System.Data.SqlServerCe.4.0"
         connectionString="Data Source=Blogging.sdf"/>
  </connectionStrings>
</configuration>
```  

<span data-ttu-id="4c84a-130">이것은 DbContext 이외의 SQL Express 또는 LocalDB 데이터베이스 서버를 사용 하도록 지시 하는 쉬운 방법을-위의 예제에서는 SQL Server Compact Edition 데이터베이스를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-130">This is an easy way to tell DbContext to use a database server other than SQL Express or LocalDB — the example above specifies a SQL Server Compact Edition database.</span></span>  

<span data-ttu-id="4c84a-131">연결 문자열의 이름 (사용 하 여 또는 네임 스페이스 한정자 없이) 컨텍스트의 이름과 일치 하는 경우 다음 있습니다 DbContext에서 매개 변수가 없는 생성자를 사용 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="4c84a-131">If the name of the connection string matches the name of your context (either with or without namespace qualification) then it will be found by DbContext when the parameterless constructor is used.</span></span> <span data-ttu-id="4c84a-132">연결 문자열 이름이 컨텍스트의 이름과 다른 경우 연결 문자열 이름을 DbContext 생성자에 전달 하 여 Code First 모드에서이 연결을 사용 하는 DbContext를 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-132">If the connection string name is different from the name of your context then you can tell DbContext to use this connection in Code First mode by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="4c84a-133">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="4c84a-133">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="4c84a-134">폼을 사용할 수 또는 "이름 =\<연결 문자열 이름\>" DbContext 생성자에 전달 된 문자열에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-134">Alternatively, you can use the form “name=\<connection string name\>” for the string passed to the DbContext constructor.</span></span> <span data-ttu-id="4c84a-135">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="4c84a-135">For example:</span></span>  

``` csharp  
public class BloggingContext : DbContext
{
    public BloggingContext()
        : base("name=BloggingCompactDatabase")
    {
    }
}
```  

<span data-ttu-id="4c84a-136">이 폼을 사용 하면 구성 파일에 대 한 연결 문자열을 예상 되는 명시적입니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-136">This form makes it explicit that you expect the connection string to be found in your config file.</span></span> <span data-ttu-id="4c84a-137">지정 된 이름의 연결 문자열을 찾을 수 없으면 예외가 throw 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-137">An exception will be thrown if a connection string with the given name is not found.</span></span>  

## <a name="databasemodel-first-with-connection-string-in-appconfigwebconfig-file"></a><span data-ttu-id="4c84a-138">Database/Model First app.config/web.config 파일에서 연결 문자열을 사용 하 여</span><span class="sxs-lookup"><span data-stu-id="4c84a-138">Database/Model First with connection string in app.config/web.config file</span></span>  

<span data-ttu-id="4c84a-139">EF 디자이너를 사용 하 여 만든 모델 다릅니다 Code First는 모델에 이미 있으며 응용 프로그램을 실행 하는 경우 코드에서 생성 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-139">Models created with the EF Designer are different from Code First in that your model already exists and is not generated from code when the application runs.</span></span> <span data-ttu-id="4c84a-140">모델은 프로젝트에서 EDMX 파일을 일반적으로 존재합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-140">The model typically exists as an EDMX file in your project.</span></span>  

<span data-ttu-id="4c84a-141">디자이너는 EF 연결 문자열을 app.config 또는 web.config 파일에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-141">The designer will add an EF connection string to your app.config or web.config file.</span></span> <span data-ttu-id="4c84a-142">이 연결 문자열은 EDMX 파일에서 정보를 찾는 방법에 대 한 정보를 포함 한다는 점에서 특별 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-142">This connection string is special in that it contains information about how to find the information in your EDMX file.</span></span> <span data-ttu-id="4c84a-143">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="4c84a-143">For example:</span></span>  

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

<span data-ttu-id="4c84a-144">또한 EF 디자이너의 연결 문자열 이름을 DbContext 생성자에 전달 하 여이 연결을 사용 하려면 DbContext를 지시 하는 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-144">The EF Designer will also generate code that tells DbContext to use this connection by passing the connection string name to the DbContext constructor.</span></span> <span data-ttu-id="4c84a-145">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="4c84a-145">For example:</span></span>  

``` csharp  
public class NorthwindContext : DbContext
{
    public NorthwindContext()
        : base("name=Northwind_Entities")
    {
    }
}
```  

<span data-ttu-id="4c84a-146">기존 모델 대신 코드에서 계산 하려면 Code First를 사용 하 여 로드에 DbContext를 알고 있는 연결 문자열은 사용 하도록 모델의 세부 정보를 포함 하는 EF 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-146">DbContext knows to load the existing model (rather than using Code First to calculate it from code) because the connection string is an EF connection string containing details of the model to use.</span></span>  

## <a name="other-dbcontext-constructor-options"></a><span data-ttu-id="4c84a-147">다른 DbContext 생성자 옵션</span><span class="sxs-lookup"><span data-stu-id="4c84a-147">Other DbContext constructor options</span></span>  

<span data-ttu-id="4c84a-148">DbContext 클래스는 다른 생성자 및 일부 고급 시나리오를 사용 하도록 설정 하는 사용 패턴을 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-148">The DbContext class contains other constructors and usage patterns that enable some more advanced scenarios.</span></span> <span data-ttu-id="4c84a-149">이 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-149">Some of these are:</span></span>  

- <span data-ttu-id="4c84a-150">DbContext 인스턴스를 인스턴스화하지 않고도 Code First 모델을 작성 하는 DbModelBuilder 클래스를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-150">You can use the DbModelBuilder class to build a Code First model without instantiating a DbContext instance.</span></span> <span data-ttu-id="4c84a-151">이 결과 DbModel 개체입니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-151">The result of this is a DbModel object.</span></span> <span data-ttu-id="4c84a-152">다음에 DbContext 인스턴스를 만들 준비가 되었을 때이 DbModel 개체 DbContext 생성자 중 하나에 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-152">You can then pass this DbModel object to one of the DbContext constructors when you are ready to create your DbContext instance.</span></span>  
- <span data-ttu-id="4c84a-153">데이터베이스 또는 연결 문자열 이름만 대신 dbcontext 전체 연결 문자열을 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-153">You can pass a full connection string to DbContext instead of just the database or connection string name.</span></span> <span data-ttu-id="4c84a-154">기본적으로이 연결 문자열은 System.Data.SqlClient 공급자로 사용 이 컨텍스트에 다른 구현의 IConnectionFactory 설정 하 여 변경할 수 있습니다. Database.DefaultConnectionFactory 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-154">By default this connection string is used with the System.Data.SqlClient provider; this can be changed by setting a different implementation of IConnectionFactory onto context.Database.DefaultConnectionFactory.</span></span>  
- <span data-ttu-id="4c84a-155">DbContext 생성자에 전달 하 여 기존 DbConnection 개체를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-155">You can use an existing DbConnection object by passing it to a DbContext constructor.</span></span> <span data-ttu-id="4c84a-156">연결 개체의 인스턴스가 EntityConnection을 계산 하는 대신 사용 되는 연결에 지정 된 모델 됩니다 하는 경우 먼저 코드를 사용 하 여 모델을 합니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-156">If the connection object is an instance of EntityConnection, then the model specified in the connection will be used rather than calculating a model using Code First.</span></span> <span data-ttu-id="4c84a-157">경우 개체는 다른 형식의 인스턴스-예를 들어 SqlConnection-컨텍스트에서 Code First 모드에 대 한 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-157">If the object is an instance of some other type—for example, SqlConnection—then the context will use it for Code First mode.</span></span>  
- <span data-ttu-id="4c84a-158">기존 ObjectContext 기존 컨텍스트를 래핑 DbContext를 만들려면 DbContext 생성자에 전달할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-158">You can pass an existing ObjectContext to a DbContext constructor to create a DbContext wrapping the existing context.</span></span> <span data-ttu-id="4c84a-159">이 ObjectContext를 사용 하는 싶지만 DbContext 응용 프로그램의 일부를 활용 하기 위해 기존 응용 프로그램에 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4c84a-159">This can be used for existing applications that use ObjectContext but which want to take advantage of DbContext in some parts of the application.</span></span>  
