---
title: 사용자 지정 마이그레이션 작업-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2017
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: bd2bfdc24977a47eaf7a6756a88b758b563d818a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812051"
---
# <a name="custom-migrations-operations"></a><span data-ttu-id="09426-102">사용자 지정 마이그레이션 작업</span><span class="sxs-lookup"><span data-stu-id="09426-102">Custom Migrations Operations</span></span>

<span data-ttu-id="09426-103">MigrationBuilder API를 사용 하면 마이그레이션 중에 다양 한 종류의 작업을 수행할 수 있지만이 작업은 완전 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="09426-103">The MigrationBuilder API allows you to perform many different kinds of operations during a migration, but it's far from exhaustive.</span></span> <span data-ttu-id="09426-104">그러나 API를 확장 하 여 사용자가 직접 작업을 정의할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="09426-104">However, the API is also extensible allowing you to define your own operations.</span></span> <span data-ttu-id="09426-105">API를 확장 하는 방법에는 `Sql()` 메서드를 사용 하거나 사용자 지정 `MigrationOperation` 개체를 정의 하는 두 가지 방법이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="09426-105">There are two ways to extend the API: Using the `Sql()` method, or by defining custom `MigrationOperation` objects.</span></span>

<span data-ttu-id="09426-106">각 접근 방법을 사용 하 여 데이터베이스 사용자를 만드는 작업을 구현 하는 방법을 살펴보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="09426-106">To illustrate, let's look at implementing an operation that creates a database user using each approach.</span></span> <span data-ttu-id="09426-107">이 마이그레이션에서는 다음 코드를 작성 하도록 설정 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="09426-107">In our migrations, we want to enable writing the following code:</span></span>

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a><span data-ttu-id="09426-108">MigrationBuilder () 사용</span><span class="sxs-lookup"><span data-stu-id="09426-108">Using MigrationBuilder.Sql()</span></span>

<span data-ttu-id="09426-109">사용자 지정 작업을 구현 하는 가장 쉬운 방법은 `MigrationBuilder.Sql()`를 호출 하는 확장 메서드를 정의 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="09426-109">The easiest way to implement a custom operation is to define an extension method that calls `MigrationBuilder.Sql()`.</span></span> <span data-ttu-id="09426-110">다음은 적절 한 Transact-sql을 생성 하는 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="09426-110">Here is an example that generates the appropriate Transact-SQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

<span data-ttu-id="09426-111">마이그레이션이 여러 데이터베이스 공급자를 지원 해야 하는 경우 `MigrationBuilder.ActiveProvider` 속성을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="09426-111">If your migrations need to support multiple database providers, you can use the `MigrationBuilder.ActiveProvider` property.</span></span> <span data-ttu-id="09426-112">Microsoft SQL Server 및 PostgreSQL를 모두 지 원하는 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="09426-112">Here's an example supporting both Microsoft SQL Server and PostgreSQL.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }

    return migrationBuilder;
}
```

<span data-ttu-id="09426-113">이 방법은 사용자 지정 작업이 적용 되는 모든 공급자를 알고 있는 경우에만 작동 합니다.</span><span class="sxs-lookup"><span data-stu-id="09426-113">This approach only works if you know every provider where your custom operation will be applied.</span></span>

## <a name="using-a-migrationoperation"></a><span data-ttu-id="09426-114">MigrationOperation 사용</span><span class="sxs-lookup"><span data-stu-id="09426-114">Using a MigrationOperation</span></span>

<span data-ttu-id="09426-115">SQL에서 사용자 지정 작업을 분리 하기 위해 고유한 `MigrationOperation`를 정의 하 여이 작업을 나타낼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="09426-115">To decouple the custom operation from the SQL, you can define your own `MigrationOperation` to represent it.</span></span> <span data-ttu-id="09426-116">그런 다음 생성 될 적절 한 SQL을 확인할 수 있도록 작업을 공급자에 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="09426-116">The operation is then passed to the provider so it can determine the appropriate SQL to generate.</span></span>

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

<span data-ttu-id="09426-117">이 방법을 사용 하는 경우 확장 메서드는 이러한 작업 중 하나를 `MigrationBuilder.Operations`에 추가 하기만 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="09426-117">With this approach, the extension method just needs to add one of these operations to `MigrationBuilder.Operations`.</span></span>

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

<span data-ttu-id="09426-118">이 접근 방식을 사용 하려면 각 공급자가 `IMigrationsSqlGenerator` 서비스에서이 작업에 대 한 SQL을 생성 하는 방법을 알아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="09426-118">This approach requires each provider to know how to generate SQL for this operation in their `IMigrationsSqlGenerator` service.</span></span> <span data-ttu-id="09426-119">새 작업을 처리 하기 위해 SQL Server의 생성기를 재정의 하는 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="09426-119">Here is an example overriding the SQL Server's generator to handle the new operation.</span></span>

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMappingSource.FindMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(operation.Name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(operation.Password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

<span data-ttu-id="09426-120">기본 마이그레이션 sql 생성기 서비스를 업데이트 된 것으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="09426-120">Replace the default migrations sql generator service with the updated one.</span></span>

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
