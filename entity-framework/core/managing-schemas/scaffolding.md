---
title: 리버스 엔지니어링-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 1ba9352d261f1c131b0d70f8cdad2128d9afaefe
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414238"
---
# <a name="reverse-engineering"></a><span data-ttu-id="4ef00-102">리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="4ef00-102">Reverse Engineering</span></span>

<span data-ttu-id="4ef00-103">리버스 엔지니어링은 엔터티 형식 클래스 및 데이터베이스 스키마 기반의 DbContext 클래스를 스캐폴딩하는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="4ef00-104">PMC (EF Core 패키지 관리자 콘솔) 도구의 `Scaffold-DbContext` 명령을 사용 하거나 .NET CLI (명령줄 인터페이스) 도구의 `dotnet ef dbcontext scaffold` 명령을 사용 하 여이를 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="4ef00-105">설치</span><span class="sxs-lookup"><span data-stu-id="4ef00-105">Installing</span></span>

<span data-ttu-id="4ef00-106">리버스 엔지니어링 하기 전에 [PMC 도구](xref:core/miscellaneous/cli/powershell) (Visual Studio에만 해당) 또는 [CLI 도구](xref:core/miscellaneous/cli/dotnet)를 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="4ef00-107">자세한 내용은 링크를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="4ef00-107">See links for details.</span></span>

<span data-ttu-id="4ef00-108">또한 리버스 엔지니어링할 데이터베이스 스키마에 적합 한 [데이터베이스 공급자](xref:core/providers/index) 를 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="4ef00-109">연결 문자열</span><span class="sxs-lookup"><span data-stu-id="4ef00-109">Connection string</span></span>

<span data-ttu-id="4ef00-110">명령의 첫 번째 인수는 데이터베이스에 대한 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="4ef00-111">도구는 이 연결 문자열을 사용하여 데이터베이스 스키마를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="4ef00-112">연결 문자열을 인용하고 이스케이프하는 방법은 명령을 실행하는 데 사용 중인 셸에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="4ef00-113">자세한 내용은 셸 설명서를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="4ef00-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="4ef00-114">예를 들어 PowerShell을 사용 하려면 `$` 문자를 이스케이프 해야 하지만 `\`는 해서는 안 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

```dotnetcli
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="4ef00-115">구성 및 사용자 암호</span><span class="sxs-lookup"><span data-stu-id="4ef00-115">Configuration and User Secrets</span></span>

<span data-ttu-id="4ef00-116">ASP.NET Core 프로젝트를 사용 하는 경우 `Name=<connection-string>` 구문을 사용 하 여 구성에서 연결 문자열을 읽을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="4ef00-117">이는 [암호 관리자 도구](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) 를 사용 하 여 코드 베이스와 별도로 데이터베이스 암호를 유지 하는 데 효과적입니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

```dotnetcli
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="4ef00-118">공급자 이름</span><span class="sxs-lookup"><span data-stu-id="4ef00-118">Provider name</span></span>

<span data-ttu-id="4ef00-119">두 번째 인수는 공급자 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-119">The second argument is the provider name.</span></span> <span data-ttu-id="4ef00-120">공급자 이름은 일반적으로 공급자의 NuGet 패키지 이름과 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="4ef00-121">지정 된 테이블</span><span class="sxs-lookup"><span data-stu-id="4ef00-121">Specifying tables</span></span>

<span data-ttu-id="4ef00-122">데이터베이스 스키마의 모든 테이블은 기본적으로 엔터티 형식으로 리버스 엔지니어링됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="4ef00-123">스키마와 테이블을 지정하여 리버스 엔지니어링되는 테이블을 제한할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="4ef00-124">PMC의 `-Schemas` 매개 변수와 CLI의 `--schema` 옵션은 스키마 내의 모든 테이블을 포함 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="4ef00-125">`-Tables` (PMC) 및 `--table` (CLI)는 특정 테이블을 포함 하는 데 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="4ef00-126">PMC에서 여러 테이블을 포함하려면 배열을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="4ef00-127">CLI에서 여러 테이블을 포함하려면 옵션을 여러 번 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

```dotnetcli
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="4ef00-128">이름 유지</span><span class="sxs-lookup"><span data-stu-id="4ef00-128">Preserving names</span></span>

<span data-ttu-id="4ef00-129">테이블 및 열 이름은 기본적으로 형식 및 속성에 대한 .NET 명명 규칙과 보다 잘 일치하도록 수정됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="4ef00-130">PMC에 `-UseDatabaseNames` 스위치를 지정 하거나 CLI에서 `--use-database-names` 옵션을 지정 하면 원래 데이터베이스 이름을 최대한 유지 하면서이 동작을 사용 하지 않도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="4ef00-131">잘못된 .NET 식별자는 여전히 수정되며 탐색 속성과 같은 합성된 이름은 .NET 명명 규칙을 계속 준수합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="4ef00-132">데이터 주석 또는 Fluent API</span><span class="sxs-lookup"><span data-stu-id="4ef00-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="4ef00-133">엔터티 형식은 기본적으로 Fluent API를 사용하여 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="4ef00-134">가능 하면 `-DataAnnotations` (PMC) 또는 `--data-annotations` (CLI)를 지정 하 여 데이터 주석을 대신 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="4ef00-135">예를 들어, 흐름 API를 사용 하면 다음과 같이 스 캐 폴드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="4ef00-136">데이터 주석을 사용 하는 동안 다음과 같은 스 캐 폴드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="4ef00-137">DbContext 이름</span><span class="sxs-lookup"><span data-stu-id="4ef00-137">DbContext name</span></span>

<span data-ttu-id="4ef00-138">스 캐 폴드 DbContext 클래스 이름은 기본적으로 *컨텍스트가* 접미사로 사용 되는 데이터베이스의 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="4ef00-139">다른 항목을 지정 하려면 PMC의 `-Context` 및 CLI의 `--context`를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="4ef00-140">디렉터리 및 네임 스페이스</span><span class="sxs-lookup"><span data-stu-id="4ef00-140">Directories and namespaces</span></span>

<span data-ttu-id="4ef00-141">엔터티 클래스 및 DbContext 클래스는 프로젝트의 루트 디렉터리로 스캐폴딩되어 프로젝트의 기본 네임스페이스를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="4ef00-142">`-OutputDir` (PMC) 또는 `--output-dir` (CLI)를 사용 하 여 클래스가 스 캐 폴드 디렉터리를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="4ef00-143">네임스페이스는 루트 네임스페이스와 프로젝트의 루트 디렉터리에 있는 하위 디렉터리의 이름이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="4ef00-144">`-ContextDir` (PMC) 및 `--context-dir` (CLI)를 사용 하 여 DbContext 클래스를 엔터티 형식 클래스와는 별도의 디렉터리로 스 캐 폴드 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

```dotnetcli
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="4ef00-145">작동 방법</span><span class="sxs-lookup"><span data-stu-id="4ef00-145">How it works</span></span>

<span data-ttu-id="4ef00-146">리버스 엔지니어링은 데이터베이스 스키마를 읽음으로써 시작됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="4ef00-147">테이블, 열, 제약 조건 및 인덱스에 대한 정보를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="4ef00-148">그런 다음 스키마 정보를 사용하여 EF Core 모델을 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="4ef00-149">테이블은 엔터티 형식을 만드는 데 사용됩니다. 열은 속성을 만드는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="4ef00-150">관계를 작성하기 위해 외래 키가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="4ef00-151">해당 엔터티 형식 클래스, Fluent API 및 데이터 주석은 앱에서 동일한 모델을 재생성하기 위해 스캐폴딩됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="limitations"></a><span data-ttu-id="4ef00-152">제한 사항</span><span class="sxs-lookup"><span data-stu-id="4ef00-152">Limitations</span></span>

* <span data-ttu-id="4ef00-153">모델에 대한 모든 것이 데이터베이스 스키마를 사용하여 표현될 수 있는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="4ef00-154">예를 들어 [**상속 계층 구조**](../modeling/inheritance.md), [**소유 된 형식**](../modeling/owned-entities.md)및 [**테이블 분할**](../modeling/table-splitting.md) 에 대 한 정보는 데이터베이스 스키마에 없습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-154">For example, information about [**inheritance hierarchies**](../modeling/inheritance.md), [**owned types**](../modeling/owned-entities.md), and [**table splitting**](../modeling/table-splitting.md) are not present in the database schema.</span></span> <span data-ttu-id="4ef00-155">이 때문에 이러한 구조는 결코 리버스 엔지니어링되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-155">Because of this, these constructs will never be reverse engineered.</span></span>
* <span data-ttu-id="4ef00-156">또한 **일부 열 형식은** EF Core 공급자에서 지원 되지 않을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="4ef00-157">이러한 열은 모델에 포함되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-157">These columns won't be included in the model.</span></span>
* <span data-ttu-id="4ef00-158">EF Core 모델에서 [**동시성 토큰**](../modeling/concurrency.md)을 정의 하 여 두 사용자가 동시에 동일한 엔터티를 업데이트 하지 못하게 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-158">You can define [**concurrency tokens**](../modeling/concurrency.md), in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="4ef00-159">일부 데이터베이스에는 이 형식의 열(예: SQL Server의 rowversion)을 나타내는 특수한 형식이 있습니다. 이 경우 이 정보를 리버스 엔지니어링할 수 있습니다. 그러나 다른 동시성 토큰은 리버스 엔지니어링되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-159">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>
* <span data-ttu-id="4ef00-160">[8 C# nullable 참조 형식 기능은](/dotnet/csharp/tutorials/nullable-reference-types) 현재 리버스 엔지니어링에서 지원 되지 않습니다. EF Core 항상 기능 C# 을 사용할 수 없는 것으로 가정 하는 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-160">[The C# 8 nullable reference type feature](/dotnet/csharp/tutorials/nullable-reference-types) is currently unsupported in reverse engineering: EF Core always generates C# code that assumes the feature is disabled.</span></span> <span data-ttu-id="4ef00-161">예를 들어 nullable 텍스트 열은 속성이 필수 인지 여부를 구성 하는 데 사용 되는 흐름 API 또는 데이터 주석을 사용 하 여 `string?`아닌 `string` 형식의 속성으로 스 캐 폴드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-161">For example, nullable text columns will be scaffolded as a property with type `string` , not `string?`, with either the Fluent API or Data Annotations used to configure whether a property is required or not.</span></span> <span data-ttu-id="4ef00-162">스 캐 폴드 코드를 편집 하 여 null 허용 여부 주석 C# 으로 바꿀 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-162">You can edit the scaffolded code and replace these with C# nullability annotations.</span></span> <span data-ttu-id="4ef00-163">Nullable 참조 형식에 대 한 스 캐 폴딩 지원은 [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520)문제에 의해 추적 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-163">Scaffolding support for nullable reference types is tracked by issue [#15520](https://github.com/aspnet/EntityFrameworkCore/issues/15520).</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="4ef00-164">모델을 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="4ef00-164">Customizing the model</span></span>

<span data-ttu-id="4ef00-165">EF Core에서 생성된 코드는 자유롭게 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-165">The code generated by EF Core is your code.</span></span> <span data-ttu-id="4ef00-166">자유롭게 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-166">Feel free to change it.</span></span> <span data-ttu-id="4ef00-167">리버스 엔지니어링 하면 동일한 모델을 다시 하는 경우만 재생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-167">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="4ef00-168">스 캐 폴드 코드는 데이터베이스에 액세스 하는 데 사용할 수 있는 모델 *하나* 를 나타내지만 *유일* 하 게 사용할 수 있는 모델은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-168">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="4ef00-169">필요에 맞게 엔터티 형식 클래스 및 DbContext 클래스를 사용자 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-169">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="4ef00-170">예를 들어 형식 및 속성의 이름을 바꾸거나 상속 계층 구조를 도입하거나 테이블을 여러 엔터티로 분할하도록 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-170">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="4ef00-171">또한 비고유 인덱스, 사용하지 않는 시퀀스 및 탐색 속성, 선택적 스칼라 속성 및 제약 조건 이름은 모델에서 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-171">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="4ef00-172">또한 추가 생성자, 메서드, 속성 등을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-172">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="4ef00-173">별도 파일에서 다른 partial 클래스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-173">using another partial class in a separate file.</span></span> <span data-ttu-id="4ef00-174">이 방법은 다시 리버스 엔지니어링 모델 하려는 경우에 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-174">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="4ef00-175">모델 업데이트</span><span class="sxs-lookup"><span data-stu-id="4ef00-175">Updating the model</span></span>

<span data-ttu-id="4ef00-176">데이터베이스를 변경한 후에는 변경 사항을 반영하도록 EF Core 모델을 업데이트해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-176">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="4ef00-177">데이터베이스 변경이 간단하면 EF Core 모델을 수동으로 변경하는 것이 가장 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-177">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="4ef00-178">예를 들어, 테이블 또는 열 이름 바꾸기, 열을 제거하거나 열의 형식을 업데이트 하는 코드에서 변경됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-178">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="4ef00-179">그러나 더 중요한 변경 사항은 수동으로 작성하기가 쉽지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-179">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="4ef00-180">한 가지 일반적인 워크플로는 `-Force` (PMC) 또는 `--force` (CLI)를 사용 하 여 데이터베이스에서 모델을 다시 리버스 엔지니어링 하 여 기존 모델을 업데이트 된 모델로 덮어쓰는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-180">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="4ef00-181">일반적으로 요청 하는 또 다른 기능은 이름 바꾸기, 형식 계층 등의 사용자 지정을 유지 하면서 데이터베이스에서 모델을 업데이트 하는 기능입니다. 문제 [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) 를 사용 하 여이 기능의 진행률을 추적할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-181">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="4ef00-182">모델을 데이터베이스에서 다시 리버스 엔지니어링하면 파일에 대한 모든 변경 사항이 손실됩니다.</span><span class="sxs-lookup"><span data-stu-id="4ef00-182">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
