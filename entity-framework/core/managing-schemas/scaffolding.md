---
title: 리버스 엔지니어링-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: 775a929982b9f4fb10aad9cd43bbb555ce632ad1
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149017"
---
# <a name="reverse-engineering"></a><span data-ttu-id="e4733-102">리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="e4733-102">Reverse Engineering</span></span>

<span data-ttu-id="e4733-103">리버스 엔지니어링은 엔터티 형식 클래스 및 데이터베이스 스키마 기반의 DbContext 클래스를 스캐폴딩하는 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="e4733-104">EF Core 패키지 관리자 콘솔(PMC) 도구의 `Scaffold-DbContext` 명령이나 .NET 명령줄 인터페이스(CLI) 도구의 `dotnet ef dbcontext scaffold` 명령을 사용하여 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="e4733-105">설치</span><span class="sxs-lookup"><span data-stu-id="e4733-105">Installing</span></span>

<span data-ttu-id="e4733-106">리버스 엔지니어링을 하기 전에 [PMC 도구](xref:core/miscellaneous/cli/powershell)(Visual Studio에만 해당) 또는 [CLI 도구](xref:core/miscellaneous/cli/dotnet) 중 하나를 설치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="e4733-107">자세한 내용은 링크를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="e4733-107">See links for details.</span></span>

<span data-ttu-id="e4733-108">또한 리버스 엔지니어링하려는 데이터베이스 스키마에 적절한 [데이터베이스 공급자](xref:core/providers/index)를 설치해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="e4733-109">연결 문자열</span><span class="sxs-lookup"><span data-stu-id="e4733-109">Connection string</span></span>

<span data-ttu-id="e4733-110">명령의 첫 번째 인수는 데이터베이스에 대한 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="e4733-111">도구는 이 연결 문자열을 사용하여 데이터베이스 스키마를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="e4733-112">연결 문자열을 인용하고 이스케이프하는 방법은 명령을 실행하는 데 사용 중인 셸에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="e4733-113">자세한 내용은 셸 설명서를 참조하십시오.</span><span class="sxs-lookup"><span data-stu-id="e4733-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="e4733-114">예를 들어 PowerShell에서는 `$` 문자를 이스케이프해야 하지만 `\`는 이스케이프하지 않아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="e4733-115">구성 및 사용자 암호</span><span class="sxs-lookup"><span data-stu-id="e4733-115">Configuration and User Secrets</span></span>

<span data-ttu-id="e4733-116">ASP.NET Core 프로젝트를 만든 경우 `Name=<connection-string>` 구문을 사용하여 구성에서 연결 문자열을 읽을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="e4733-117">이 기능은 [암호 관리자 도구](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager)와 잘 작동하여 데이터베이스 암호를 코드베이스와 별도로 유지합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="e4733-118">공급자 이름</span><span class="sxs-lookup"><span data-stu-id="e4733-118">Provider name</span></span>

<span data-ttu-id="e4733-119">두 번째 인수는 공급자 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-119">The second argument is the provider name.</span></span> <span data-ttu-id="e4733-120">공급자 이름은 일반적으로 공급자의 NuGet 패키지 이름과 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="e4733-121">지정 된 테이블</span><span class="sxs-lookup"><span data-stu-id="e4733-121">Specifying tables</span></span>

<span data-ttu-id="e4733-122">데이터베이스 스키마의 모든 테이블은 기본적으로 엔터티 형식으로 리버스 엔지니어링됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="e4733-123">스키마와 테이블을 지정하여 리버스 엔지니어링되는 테이블을 제한할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="e4733-124">PMC의 `-Schemas` 매개 변수와 CLI의 `--schema` 옵션을 사용하여 스키마의 모든 테이블을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="e4733-125">`-Tables` (PMC) 및 `--table` (CLI)를 사용하여 특정 테이블을 포함할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="e4733-126">PMC에서 여러 테이블을 포함하려면 배열을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="e4733-127">CLI에서 여러 테이블을 포함하려면 옵션을 여러 번 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="e4733-128">이름 유지</span><span class="sxs-lookup"><span data-stu-id="e4733-128">Preserving names</span></span>

<span data-ttu-id="e4733-129">테이블 및 열 이름은 기본적으로 형식 및 속성에 대한 .NET 명명 규칙과 보다 잘 일치하도록 수정됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="e4733-130">PMC에서 지정된 `-UseDatabaseNames` 또는 CLI에서 지정된 `--use-database-names` 옵션을 지정하면 가능한 한 원본 데이터베이스 이름을 그대로 유지하면서 이 동작이 비활성화됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="e4733-131">잘못된 .NET 식별자는 여전히 수정되며 탐색 속성과 같은 합성된 이름은 .NET 명명 규칙을 계속 준수합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="e4733-132">데이터 주석 또는 Fluent API</span><span class="sxs-lookup"><span data-stu-id="e4733-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="e4733-133">엔터티 형식은 기본적으로 Fluent API를 사용하여 구성됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="e4733-134">가능한 경우 데이터 주석을 대신 사용하려면 `-DataAnnotations`(PMC) 또는 `--data-annotations`(CLI)를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="e4733-135">예를 들어, 흐름 API를 사용 하면 다음과 같이 스 캐 폴드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-135">For example, using the Fluent API will scaffold this:</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="e4733-136">데이터 주석을 사용 하는 동안 다음과 같은 스 캐 폴드 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-136">While using Data Annotations will scaffold this:</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="e4733-137">DbContext 이름</span><span class="sxs-lookup"><span data-stu-id="e4733-137">DbContext name</span></span>

<span data-ttu-id="e4733-138">스캐폴딩된 DbContext 클래스 이름은 기본적으로 *Context* 접미사가 붙은 데이터베이스의 이름이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="e4733-139">다른 이름을 지정하려면 PMC에서 `-Context`를 사용하고 CLI에서 `--context`를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="e4733-140">디렉터리 및 네임 스페이스</span><span class="sxs-lookup"><span data-stu-id="e4733-140">Directories and namespaces</span></span>

<span data-ttu-id="e4733-141">엔터티 클래스 및 DbContext 클래스는 프로젝트의 루트 디렉터리로 스캐폴딩되어 프로젝트의 기본 네임스페이스를 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="e4733-142">`-OutputDir`(PMC) 또는 `--output-dir`(CLI)를 사용하여 클래스가 스캐폴딩되는 디렉터리를 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="e4733-143">네임스페이스는 루트 네임스페이스와 프로젝트의 루트 디렉터리에 있는 하위 디렉터리의 이름이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="e4733-144">`-ContextDir`(PMC) 및 `--context-dir`(CLI)를 사용하여 엔터티 형식 클래스와 별도의 디렉터리에 DbContext 클래스를 스캐폴딩할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="e4733-145">작동 방법</span><span class="sxs-lookup"><span data-stu-id="e4733-145">How it works</span></span>

<span data-ttu-id="e4733-146">리버스 엔지니어링은 데이터베이스 스키마를 읽음으로써 시작됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="e4733-147">테이블, 열, 제약 조건 및 인덱스에 대한 정보를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="e4733-148">그런 다음 스키마 정보를 사용하여 EF Core 모델을 작성합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="e4733-149">테이블은 엔터티 형식을 만드는 데 사용됩니다. 열은 속성을 만드는 데 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="e4733-150">관계를 작성하기 위해 외래 키가 사용됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="e4733-151">해당 엔터티 형식 클래스, Fluent API 및 데이터 주석은 앱에서 동일한 모델을 재생성하기 위해 스캐폴딩됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="e4733-152">작동 하지 않는</span><span class="sxs-lookup"><span data-stu-id="e4733-152">What doesn't work</span></span>

<span data-ttu-id="e4733-153">모델에 대한 모든 것이 데이터베이스 스키마를 사용하여 표현될 수 있는 것은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="e4733-154">예를 들어 [**상속 계층 구조**](../modeling/inheritance.md), [**소유 된 형식**](../modeling/owned-entities.md)및 [**테이블 분할**](../modeling/table-splitting.md) 에 대 한 정보는 데이터베이스 스키마에 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-154">For example, information about [**inheritance hierarchies**](../modeling/inheritance.md), [**owned types**](../modeling/owned-entities.md), and [**table splitting**](../modeling/table-splitting.md) are not present in the database schema.</span></span> <span data-ttu-id="e4733-155">이 때문에 이러한 구조는 결코 리버스 엔지니어링되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="e4733-156">또한 **몇 가지 열 형식**은 EF Core 공급자가 지원되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="e4733-157">이러한 열은 모델에 포함되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="e4733-158">EF Core 모델에서 [**동시성 토큰**](../modeling/concurrency.md)을 정의 하 여 두 사용자가 동시에 동일한 엔터티를 업데이트 하지 못하게 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-158">You can define [**concurrency tokens**](../modeling/concurrency.md), in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="e4733-159">일부 데이터베이스에는 이 형식의 열(예: SQL Server의 rowversion)을 나타내는 특수한 형식이 있습니다. 이 경우 이 정보를 리버스 엔지니어링할 수 있습니다. 그러나 다른 동시성 토큰은 리버스 엔지니어링되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-159">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="e4733-160">모델을 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="e4733-160">Customizing the model</span></span>

<span data-ttu-id="e4733-161">EF Core에서 생성된 코드는 자유롭게 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-161">The code generated by EF Core is your code.</span></span> <span data-ttu-id="e4733-162">자유롭게 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-162">Feel free to change it.</span></span> <span data-ttu-id="e4733-163">리버스 엔지니어링 하면 동일한 모델을 다시 하는 경우만 재생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-163">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="e4733-164">스캐폴딩된 코드는 데이터베이스에 액세스하는 데 사용할 수 있는 *하나*의 모델을 나타내지만 확실히 사용할 수 있는 *유일한* 모델은 아닙니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-164">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="e4733-165">필요에 맞게 엔터티 형식 클래스 및 DbContext 클래스를 사용자 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-165">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="e4733-166">예를 들어 형식 및 속성의 이름을 바꾸거나 상속 계층 구조를 도입하거나 테이블을 여러 엔터티로 분할하도록 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-166">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="e4733-167">또한 비고유 인덱스, 사용하지 않는 시퀀스 및 탐색 속성, 선택적 스칼라 속성 및 제약 조건 이름은 모델에서 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-167">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="e4733-168">또한 추가 생성자, 메서드, 속성 등을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-168">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="e4733-169">별도 파일에서 다른 partial 클래스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-169">using another partial class in a separate file.</span></span> <span data-ttu-id="e4733-170">이 방법은 다시 리버스 엔지니어링 모델 하려는 경우에 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-170">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="e4733-171">모델 업데이트</span><span class="sxs-lookup"><span data-stu-id="e4733-171">Updating the model</span></span>

<span data-ttu-id="e4733-172">데이터베이스를 변경한 후에는 변경 사항을 반영하도록 EF Core 모델을 업데이트해야 할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-172">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="e4733-173">데이터베이스 변경이 간단하면 EF Core 모델을 수동으로 변경하는 것이 가장 쉽습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-173">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="e4733-174">예를 들어, 테이블 또는 열 이름 바꾸기, 열을 제거하거나 열의 형식을 업데이트 하는 코드에서 변경됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-174">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="e4733-175">그러나 더 중요한 변경 사항은 수동으로 작성하기가 쉽지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-175">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="e4733-176">일반적인 워크플로 중 하나는 `-Force`(PMC) 또는 `--force`(CLI)를 사용하여 데이터베이스에서 모델을 다시 리버스 엔지니어링하여 기존 모델을 업데이트된 모델로 덮어 쓰는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-176">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="e4733-177">다른 일반적으로 요구되는 또 다른 기능은 이름 변경, 형식 계층 구조 등과 같은 사용자 정의를 유지하면서 데이터베이스에서 모델을 업데이트하는 기능입니다. 이 기능의 진행 상황을 추적하려면 이슈 [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831)을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-177">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="e4733-178">모델을 데이터베이스에서 다시 리버스 엔지니어링하면 파일에 대한 모든 변경 사항이 손실됩니다.</span><span class="sxs-lookup"><span data-stu-id="e4733-178">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
