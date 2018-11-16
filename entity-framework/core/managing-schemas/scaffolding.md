---
title: 리버스 엔지니어링-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/13/2018
ms.assetid: 6263EF7D-4989-42E6-BDEE-45DA770342FB
uid: core/managing-schemas/scaffolding
ms.openlocfilehash: ef729c0c26d5a1f57099f339eb51cda7e83289df
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688682"
---
# <a name="reverse-engineering"></a><span data-ttu-id="54334-102">리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="54334-102">Reverse Engineering</span></span>

<span data-ttu-id="54334-103">리버스 엔지니어링은 엔터티 형식 클래스 및 데이터베이스 스키마를 기반으로 하는 DbContext 클래스를 스 캐 폴딩의 프로세스입니다.</span><span class="sxs-lookup"><span data-stu-id="54334-103">Reverse engineering is the process of scaffolding entity type classes and a DbContext class based on a database schema.</span></span> <span data-ttu-id="54334-104">사용 하 여 수행할 수 있습니다 합니다 `Scaffold-DbContext` EF Core 패키지 관리자 콘솔 (PMC) 도구 명령 또는 `dotnet ef dbcontext scaffold` .NET CLI (명령줄 인터페이스) 도구 명령입니다.</span><span class="sxs-lookup"><span data-stu-id="54334-104">It can be performed using the `Scaffold-DbContext` command of the EF Core Package Manager Console (PMC) tools or the `dotnet ef dbcontext scaffold` command of the .NET Command-line Interface (CLI) tools.</span></span>

## <a name="installing"></a><span data-ttu-id="54334-105">설치</span><span class="sxs-lookup"><span data-stu-id="54334-105">Installing</span></span>

<span data-ttu-id="54334-106">리버스 엔지니어링, 하기 전에 설치 해야 합니다 [PMC 도구](xref:core/miscellaneous/cli/powershell) (Visual Studio에만 해당) 또는 [CLI 도구](xref:core/miscellaneous/cli/dotnet)합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-106">Before reverse engineering, you'll need to install either the [PMC tools](xref:core/miscellaneous/cli/powershell) (Visual Studio only) or the [CLI tools](xref:core/miscellaneous/cli/dotnet).</span></span> <span data-ttu-id="54334-107">세부 정보에 대 한 링크를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="54334-107">See links for details.</span></span>

<span data-ttu-id="54334-108">적절 한 설치도 해야 [데이터베이스 공급자](xref:core/providers/index) 리버스 엔지니어링 하려는 데이터베이스 스키마에 대 한 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-108">You'll also need to install an appropriate [database provider](xref:core/providers/index) for the database schema you want to reverse engineer.</span></span>

## <a name="connection-string"></a><span data-ttu-id="54334-109">연결 문자열</span><span class="sxs-lookup"><span data-stu-id="54334-109">Connection string</span></span>

<span data-ttu-id="54334-110">명령에 첫 번째 인수가 데이터베이스에 연결 문자열입니다.</span><span class="sxs-lookup"><span data-stu-id="54334-110">The first argument to the command is a connection string to the database.</span></span> <span data-ttu-id="54334-111">도구는 데이터베이스 스키마를 읽어올이 연결 문자열을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-111">The tools will use this connection string to read the database schema.</span></span>

<span data-ttu-id="54334-112">인용 하 고 연결 문자열을 이스케이프 하는 방법에 셸 명령을 실행 하는 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="54334-112">How you quote and escape the connection string depends on which shell you are using to execute the command.</span></span> <span data-ttu-id="54334-113">자세한 내용은 셸의 문서를 참조 하십시오.</span><span class="sxs-lookup"><span data-stu-id="54334-113">Refer to your shell's documentation for specifics.</span></span> <span data-ttu-id="54334-114">예를 들어 PowerShell에서는 이스케이프 해야 합니다 `$` 문자를 제외한 `\`합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-114">For example, PowerShell requires you to escape the `$` character, but not `\`.</span></span>

``` powershell
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer
```

``` Console
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="configuration-and-user-secrets"></a><span data-ttu-id="54334-115">구성 및 사용자 암호</span><span class="sxs-lookup"><span data-stu-id="54334-115">Configuration and User Secrets</span></span>

<span data-ttu-id="54334-116">ASP.NET Core 프로젝트를 만든 경우 사용할 수 있습니다는 `Name=<connection-string>` 구문을 구성에서 연결 문자열을 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-116">If you have an ASP.NET Core project, you can use the `Name=<connection-string>` syntax to read the connection string from configuration.</span></span>

<span data-ttu-id="54334-117">이 잘 작동 합니다 [암호 관리자 도구](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) 데이터베이스 암호를 코드 베이스에서 별도로 유지 하 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-117">This works well with the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager) to keep your database password separate from your codebase.</span></span>

``` Console
dotnet user-secrets set ConnectionStrings.Chinook "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook"
dotnet ef dbcontext scaffold Name=Chinook Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="provider-name"></a><span data-ttu-id="54334-118">공급자 이름</span><span class="sxs-lookup"><span data-stu-id="54334-118">Provider name</span></span>

<span data-ttu-id="54334-119">두 번째 인수에 공급자 이름입니다.</span><span class="sxs-lookup"><span data-stu-id="54334-119">The second argument is the provider name.</span></span> <span data-ttu-id="54334-120">공급자 이름은 일반적으로 공급자의 NuGet 패키지 이름과 동일 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-120">The provider name is typically the same as the provider's NuGet package name.</span></span>

## <a name="specifying-tables"></a><span data-ttu-id="54334-121">지정 된 테이블</span><span class="sxs-lookup"><span data-stu-id="54334-121">Specifying tables</span></span>

<span data-ttu-id="54334-122">데이터베이스 스키마의 모든 테이블은 기본적으로 엔터티 형식으로 엔지니어링 역방향입니다.</span><span class="sxs-lookup"><span data-stu-id="54334-122">All tables in the database schema are reverse engineered into entity types by default.</span></span> <span data-ttu-id="54334-123">테이블은 스키마 및 테이블을 지정 하 여 엔지니어링 역방향 제한할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-123">You can limit which tables are reverse engineered by specifying schemas and tables.</span></span>

<span data-ttu-id="54334-124">합니다 `-Schemas` PMC에서 매개 변수 및 `--schema` 스키마 내의 모든 테이블을 포함 하려면 CLI에서 옵션을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-124">The `-Schemas` parameter in PMC and the `--schema` option in the CLI can be used to include every table within a schema.</span></span>

<span data-ttu-id="54334-125">`-Tables` (PMC) 및 `--table` (CLI)를 사용 하 여 특정 테이블을 포함 하도록 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-125">`-Tables` (PMC) and `--table` (CLI) can be used to include specific tables.</span></span>

<span data-ttu-id="54334-126">PMC에서 여러 테이블에 포함 하려면 배열을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-126">To include multiple tables in PMC, use an array.</span></span>

``` powershell
Scaffold-DbContext ... -Tables Artist, Album
```

<span data-ttu-id="54334-127">CLI에서 여러 테이블에 포함 하려면 옵션을 여러 번 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-127">To include multiple tables in the CLI, specify the option multiple times.</span></span>

``` Console
dotnet ef dbcontext scaffold ... --table Artist --table Album
```

## <a name="preserving-names"></a><span data-ttu-id="54334-128">이름 유지</span><span class="sxs-lookup"><span data-stu-id="54334-128">Preserving names</span></span>

<span data-ttu-id="54334-129">테이블 및 열 이름은 기본적으로 형식 및 속성에 대 한.NET 명명 규칙을 더 잘 맞게 고정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-129">Table and column names are fixed up to better match the .NET naming conventions for types and properties by default.</span></span> <span data-ttu-id="54334-130">지정 된 `-UseDatabaseNames` PMC에서 전환 또는 `--use-database-names` 옵션 CLI에서 원래 데이터베이스 이름을 최대한 많이 보존 하는이 동작을 사용 하지 않도록 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-130">Specifying the `-UseDatabaseNames` switch in PMC or the `--use-database-names` option in the CLI will disable this behavior preserving the original database names as much as possible.</span></span> <span data-ttu-id="54334-131">잘못 된.NET 식별자 여전히 수정 될 예정 및 탐색 속성과 같이 합성 된 이름은.NET 명명 규칙을 따라야 계속 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-131">Invalid .NET identifiers will still be fixed and synthesized names like navigation properties will still conform to .NET naming conventions.</span></span>

## <a name="fluent-api-or-data-annotations"></a><span data-ttu-id="54334-132">데이터 주석 또는 Fluent API</span><span class="sxs-lookup"><span data-stu-id="54334-132">Fluent API or Data Annotations</span></span>

<span data-ttu-id="54334-133">엔터티 형식은 기본적으로 Fluent API를 사용 하 여 구성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-133">Entity types are configured using the Fluent API by default.</span></span> <span data-ttu-id="54334-134">지정할 `-DataAnnotations` (PMC) 또는 `--data-annotations` (CLI) 대신 사용 하 여 가능한 경우 데이터 주석입니다.</span><span class="sxs-lookup"><span data-stu-id="54334-134">Specify `-DataAnnotations` (PMC) or `--data-annotations` (CLI) to instead use data annotations when possible.</span></span>

<span data-ttu-id="54334-135">예를 들어, Fluent API를 사용 하는 스 캐 폴딩이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-135">For example, using the Fluent API will scaffold the this.</span></span>

``` csharp
entity.Property(e => e.Title)
    .IsRequired()
    .HasMaxLength(160);
```

<span data-ttu-id="54334-136">데이터 주석을 사용 하는 동안는 스 캐 폴딩이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-136">While using Data Annotations will scaffold this.</span></span>

``` csharp
[Required]
[StringLength(160)]
public string Title { get; set; }
```

## <a name="dbcontext-name"></a><span data-ttu-id="54334-137">DbContext 이름</span><span class="sxs-lookup"><span data-stu-id="54334-137">DbContext name</span></span>

<span data-ttu-id="54334-138">스 캐 폴드 된 DbContext 클래스 이름 접미사를 사용 하는 데이터베이스의 이름이 됩니다 *상황에 맞는* 기본적으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-138">The scaffolded DbContext class name will be the name of the database suffixed with *Context* by default.</span></span> <span data-ttu-id="54334-139">다른 이름을 지정 하려면 사용 `-Context` PMC에서 및 `--context` CLI에서.</span><span class="sxs-lookup"><span data-stu-id="54334-139">To specify a different one, use `-Context` in PMC and `--context` in the CLI.</span></span>

## <a name="directories-and-namespaces"></a><span data-ttu-id="54334-140">디렉터리 및 네임 스페이스</span><span class="sxs-lookup"><span data-stu-id="54334-140">Directories and namespaces</span></span>

<span data-ttu-id="54334-141">엔터티 클래스 및 DbContext 클래스는 프로젝트의 루트 디렉터리에 스 캐 폴드 된 프로젝트의 기본 네임 스페이스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-141">The entity classes and a DbContext class are scaffolded into the project's root directory and use the project's default namespace.</span></span> <span data-ttu-id="54334-142">디렉터리를 지정할 수 있는 클래스를 사용 하 여 스 캐 폴드는 `-OutputDir` (PMC) 또는 `--output-dir` (CLI).</span><span class="sxs-lookup"><span data-stu-id="54334-142">You can specify the directory where classes are scaffolded using `-OutputDir` (PMC) or `--output-dir` (CLI).</span></span> <span data-ttu-id="54334-143">네임 스페이스는 루트 네임 스페이스와 프로젝트의 루트 디렉터리 아래의 하위 디렉터리의 이름이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-143">The namespace will be the root namespace plus the names of any subdirectories under the project's root directory.</span></span>

<span data-ttu-id="54334-144">사용할 수도 있습니다 `-ContextDir` (PMC) 및 `--context-dir` (CLI) 엔터티 형식 클래스에서 별도 디렉터리에 DbContext 클래스를 스 캐 폴딩 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-144">You can also use `-ContextDir` (PMC) and `--context-dir` (CLI) to scaffold the DbContext class into a separate directory from the entity type classes.</span></span>

``` powershell
Scaffold-DbContext ... -ContextDir Data -OutputDir Models
```

``` Console
dotnet ef dbcontext scaffold ... --context-dir Data --output-dir Models
```

## <a name="how-it-works"></a><span data-ttu-id="54334-145">작동 방법</span><span class="sxs-lookup"><span data-stu-id="54334-145">How it works</span></span>

<span data-ttu-id="54334-146">리버스 엔지니어링 데이터베이스 스키마를 참조 하 여 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-146">Reverse engineering starts by reading the database schema.</span></span> <span data-ttu-id="54334-147">테이블, 열, 제약 조건 및 인덱스에 대 한 정보를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-147">It reads information about tables, columns, constraints, and indexes.</span></span>

<span data-ttu-id="54334-148">다음으로, EF Core 모델을 만들 스키마 정보를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-148">Next, it uses the schema information to create an EF Core model.</span></span> <span data-ttu-id="54334-149">테이블은 엔터티 형식을 만드는 데 사용 됩니다. 열 속성을 만드는 데 사용 됩니다. 및 외래 키 관계를 만드는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-149">Tables are used to create entity types; columns are used to create properties; and foreign keys are used to create relationships.</span></span>

<span data-ttu-id="54334-150">마지막으로 모델 코드 생성에 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-150">Finally, the model is used to generate code.</span></span> <span data-ttu-id="54334-151">앱에서 동일한 모델을 다시 만들려면 해당 엔터티 형식 클래스, Fluent API 및 데이터 주석을 스 캐 폴드 된 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-151">The corresponding entity type classes, Fluent API, and data annotations are scaffolded in order to re-create the same model from your app.</span></span>

## <a name="what-doesnt-work"></a><span data-ttu-id="54334-152">작동 하지 않는</span><span class="sxs-lookup"><span data-stu-id="54334-152">What doesn't work</span></span>

<span data-ttu-id="54334-153">모델에 대 한 것은 아닙니다 데이터베이스 스키마를 사용 하 여 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-153">Not everything about a model can be represented using a database schema.</span></span> <span data-ttu-id="54334-154">예를 들어,에 대 한 정보 **상속 계층 구조**, **소유 된 형식이**, 및 **분할 테이블** 데이터베이스 스키마에 없는 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-154">For example, information about **inheritance hierarchies**, **owned types**, and **table splitting** are not present in the database schema.</span></span> <span data-ttu-id="54334-155">이 인해 이러한 구문은 됩니다 하지 수 리버스 엔지니어링 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-155">Because of this, these constructs will never be reverse engineered.</span></span>

<span data-ttu-id="54334-156">또한 **몇 가지 열 형식을** EF Core 공급자가 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-156">In addition, **some column types** may not be supported by the EF Core provider.</span></span> <span data-ttu-id="54334-157">이러한 열은 모델에 포함 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-157">These columns won't be included in the model.</span></span>

<span data-ttu-id="54334-158">EF Core에는 키를 갖는 모든 엔터티 형식에 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-158">EF Core requires every entity type to have a key.</span></span> <span data-ttu-id="54334-159">그러나 테이블 기본 키를 지정 하려면 필요한 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-159">Tables, however, aren't required to specify a primary key.</span></span> <span data-ttu-id="54334-160">**기본 키가 없는 테이블** 은 리버스 엔지니어링 된 현재 없습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-160">**Tables without a primary key** are currently not reverse engineered.</span></span>

<span data-ttu-id="54334-161">정의할 수 있습니다 **동시성 토큰** EF Core 모델 두 사용자가 동시에 동일한 엔터티를 업데이트 하지 못하도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-161">You can define **concurrency tokens** in an EF Core model to prevent two users from updating the same entity at the same time.</span></span> <span data-ttu-id="54334-162">일부 데이터베이스를 되돌릴 수 있는 경우가이 정보를 엔지니어링에서이 형식의 열 (예: SQL Server에서 rowversion)을 나타내는 특수 형식이 그러나 다른 동시성 토큰은 수 리버스 엔지니어링 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-162">Some databases have a special type to represent this type of column (for example, rowversion in SQL Server) in which case we can reverse engineer this information; however, other concurrency tokens will not be reverse engineered.</span></span>

## <a name="customizing-the-model"></a><span data-ttu-id="54334-163">모델을 사용자 지정</span><span class="sxs-lookup"><span data-stu-id="54334-163">Customizing the model</span></span>

<span data-ttu-id="54334-164">EF Core에서 생성 된 코드는 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="54334-164">The code generated by EF Core is your code.</span></span> <span data-ttu-id="54334-165">자유롭게 변경할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-165">Feel free to change it.</span></span> <span data-ttu-id="54334-166">리버스 엔지니어링 하면 동일한 모델을 다시 하는 경우만 재생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-166">It will only be regenerated if you reverse engineer the same model again.</span></span> <span data-ttu-id="54334-167">스 캐 폴드 된 코드를 나타내는 *하나* 데이터베이스에 있지만 액세스할 수 있는 모델은 분명 하지는 *만* 사용할 수 있는 모델입니다.</span><span class="sxs-lookup"><span data-stu-id="54334-167">The scaffolded code represents *one* model that can be used to access the database, but it's certainly not the *only* model that can be used.</span></span>

<span data-ttu-id="54334-168">엔터티 형식 클래스 및 필요에 맞게 DbContext 클래스를 사용자 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-168">Customize the entity type classes and DbContext class to fit your needs.</span></span> <span data-ttu-id="54334-169">예를 들어, 형식 및 속성을 이름 바꾸기, 상속 계층 구조를 도입 또는 여러 엔터티를 테이블에 분할을 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-169">For example, you may choose to rename types and properties, introduce inheritance hierarchies, or split a table into to multiple entities.</span></span> <span data-ttu-id="54334-170">또한 비고유 인덱스, 사용 하지 않는 시퀀스 및 탐색 속성, 선택적 스칼라 속성 및 제약 조건 이름은 모델에서 제거할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-170">You can also remove non-unique indexes, unused sequences and navigation properties, optional scalar properties, and constraint names from the model.</span></span>

<span data-ttu-id="54334-171">또한 추가 생성자, 메서드, 속성 등을 추가할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-171">You can also add additional constructors, methods, properties, etc.</span></span> <span data-ttu-id="54334-172">별도 파일에서 다른 partial 클래스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-172">using another partial class in a separate file.</span></span> <span data-ttu-id="54334-173">이 방법은 다시 리버스 엔지니어링 모델 하려는 경우에 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-173">This approach works even when you intend to reverse engineer the model again.</span></span>

## <a name="updating-the-model"></a><span data-ttu-id="54334-174">모델 업데이트</span><span class="sxs-lookup"><span data-stu-id="54334-174">Updating the model</span></span>

<span data-ttu-id="54334-175">데이터베이스에 구성을 변경한 후 변경 내용을 반영 하도록 EF Core 모델을 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="54334-175">After making changes to the database, you may need to update your EF Core model to reflect those changes.</span></span> <span data-ttu-id="54334-176">데이터베이스 변경 내용을 간단한 경우 EF Core 모델을 수동으로 변경할 가장 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-176">If the database changes are simple, it may be easiest just to manually make the changes to your EF Core model.</span></span> <span data-ttu-id="54334-177">예를 들어, 테이블 또는 열 이름 바꾸기, 열을 제거 하거나 열의 형식을 업데이트 하는 코드에서 변경 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-177">For example, renaming a table or column, removing a column, or updating a column's type are trivial changes to make in code.</span></span>

<span data-ttu-id="54334-178">그러나 더 중요 한 변경 내용은 않습니다 쉽게 확인으로 수동으로.</span><span class="sxs-lookup"><span data-stu-id="54334-178">More significant changes, however, are not as easy make manually.</span></span> <span data-ttu-id="54334-179">다시 사용 하 여 데이터베이스에서 모델을 설계를 반대로 하려면 일반적인 워크플로 하나 `-Force` (PMC) 또는 `--force` (CLI) 기존 모델 업데이트 된 항목을 덮어쓸 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-179">One common workflow is to reverse engineer the model from the database again using `-Force` (PMC) or `--force` (CLI) to overwrite the existing model with an updated one.</span></span>

<span data-ttu-id="54334-180">다른 일반적으로 요청 된 기능은 이름 바꾸기, 형식 계층 구조 등 사용자 지정을 유지 하는 동안 데이터베이스에서 모델을 업데이트 하는 기능입니다. 문제를 사용 하 여 [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) 이 기능의 진행률을 추적할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="54334-180">Another commonly requested feature is the ability to update the model from the database while preserving customization like renames, type hierarchies, etc. Use issue [#831](https://github.com/aspnet/EntityFrameworkCore/issues/831) to track the progress of this feature.</span></span>

> [!WARNING]
> <span data-ttu-id="54334-181">를 리버스 엔지니어링 하면 데이터베이스에서 모델 다시 파일을 변경한 후 모든 변경 내용이 손실 됩니다.</span><span class="sxs-lookup"><span data-stu-id="54334-181">If you reverse engineer the model from the database again, any changes you've made to the files will be lost.</span></span>
