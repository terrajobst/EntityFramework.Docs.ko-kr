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
# <a name="custom-migrations-operations"></a>사용자 지정 마이그레이션 작업

MigrationBuilder API를 사용 하면 마이그레이션 중에 다양 한 종류의 작업을 수행할 수 있지만이 작업은 완전 하지 않습니다. 그러나 API를 확장 하 여 사용자가 직접 작업을 정의할 수도 있습니다. API를 확장 하는 방법에는 `Sql()` 메서드를 사용 하거나 사용자 지정 `MigrationOperation` 개체를 정의 하는 두 가지 방법이 있습니다.

각 접근 방법을 사용 하 여 데이터베이스 사용자를 만드는 작업을 구현 하는 방법을 살펴보겠습니다. 이 마이그레이션에서는 다음 코드를 작성 하도록 설정 하려고 합니다.

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a>MigrationBuilder () 사용

사용자 지정 작업을 구현 하는 가장 쉬운 방법은 `MigrationBuilder.Sql()`를 호출 하는 확장 메서드를 정의 하는 것입니다. 다음은 적절 한 Transact-sql을 생성 하는 예제입니다.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

마이그레이션이 여러 데이터베이스 공급자를 지원 해야 하는 경우 `MigrationBuilder.ActiveProvider` 속성을 사용할 수 있습니다. Microsoft SQL Server 및 PostgreSQL를 모두 지 원하는 예제는 다음과 같습니다.

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

이 방법은 사용자 지정 작업이 적용 되는 모든 공급자를 알고 있는 경우에만 작동 합니다.

## <a name="using-a-migrationoperation"></a>MigrationOperation 사용

SQL에서 사용자 지정 작업을 분리 하기 위해 고유한 `MigrationOperation`를 정의 하 여이 작업을 나타낼 수 있습니다. 그런 다음 생성 될 적절 한 SQL을 확인할 수 있도록 작업을 공급자에 전달 합니다.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

이 방법을 사용 하는 경우 확장 메서드는 이러한 작업 중 하나를 `MigrationBuilder.Operations`에 추가 하기만 하면 됩니다.

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

이 접근 방식을 사용 하려면 각 공급자가 `IMigrationsSqlGenerator` 서비스에서이 작업에 대 한 SQL을 생성 하는 방법을 알아야 합니다. 새 작업을 처리 하기 위해 SQL Server의 생성기를 재정의 하는 예제는 다음과 같습니다.

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

기본 마이그레이션 sql 생성기 서비스를 업데이트 된 것으로 바꿉니다.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
